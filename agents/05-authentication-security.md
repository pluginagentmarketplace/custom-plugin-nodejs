---
description: Implement secure authentication and authorization in Node.js applications with JWT, OAuth, sessions, and security best practices
capabilities: ["jwt-authentication", "oauth-integration", "session-management", "security-hardening", "password-hashing"]
---

# Authentication & Security Agent

Master authentication, authorization, and security in Node.js applications. Learn JWT, OAuth, session management, and implement production-grade security practices.

## Capabilities

### 1. **JWT (JSON Web Tokens) Authentication**
- JWT structure and signing
- Access and refresh tokens
- Token expiration and rotation
- Secure token storage
- JWT middleware implementation
- Blacklisting and revocation
- Token payload best practices

### 2. **OAuth 2.0 Integration**
- OAuth flow implementation
- Google, GitHub, Facebook integration
- Passport.js strategies
- Social login implementation
- Scope and permissions
- Token exchange
- Provider-specific considerations

### 3. **Session Management**
- Express-session configuration
- Session stores (Redis, MongoDB)
- Cookie security (httpOnly, secure, sameSite)
- Session expiration
- CSRF protection
- Session hijacking prevention

### 4. **Password Security**
- bcrypt hashing
- Salt generation
- Password strength validation
- Password reset flows
- Account lockout mechanisms
- Rate limiting login attempts

### 5. **Authorization Patterns**
- Role-based access control (RBAC)
- Permission-based access
- Middleware authorization
- Route protection
- Resource ownership validation
- API key authentication

### 6. **Security Best Practices**
- Helmet.js configuration
- CORS setup
- Input validation and sanitization
- SQL injection prevention
- XSS protection
- CSRF tokens
- Rate limiting
- Security headers
- Environment variable protection

## JWT Authentication Implementation

### JWT Setup and Generation
```javascript
const jwt = require('jsonwebtoken');
const bcrypt = require('bcryptjs');

// Generate JWT
function generateToken(userId) {
  const payload = {
    id: userId,
    iat: Date.now()
  };

  return jwt.sign(payload, process.env.JWT_SECRET, {
    expiresIn: '1h'  // Access token expires in 1 hour
  });
}

// Generate refresh token
function generateRefreshToken(userId) {
  return jwt.sign(
    { id: userId },
    process.env.JWT_REFRESH_SECRET,
    { expiresIn: '7d' }  // Refresh token expires in 7 days
  );
}

// Verify JWT
function verifyToken(token) {
  try {
    return jwt.verify(token, process.env.JWT_SECRET);
  } catch (error) {
    if (error.name === 'TokenExpiredError') {
      throw new Error('Token expired');
    }
    throw new Error('Invalid token');
  }
}
```

### Registration and Login
```javascript
// User registration
async function register(req, res) {
  try {
    const { email, password, name } = req.body;

    // Validate input
    if (!email || !password) {
      return res.status(400).json({ error: 'Email and password required' });
    }

    // Check if user exists
    const existingUser = await User.findOne({ email });
    if (existingUser) {
      return res.status(409).json({ error: 'User already exists' });
    }

    // Hash password
    const hashedPassword = await bcrypt.hash(password, 10);

    // Create user
    const user = await User.create({
      email,
      password: hashedPassword,
      name
    });

    // Generate tokens
    const accessToken = generateToken(user._id);
    const refreshToken = generateRefreshToken(user._id);

    // Store refresh token
    await RefreshToken.create({ token: refreshToken, userId: user._id });

    res.status(201).json({
      user: {
        id: user._id,
        email: user.email,
        name: user.name
      },
      accessToken,
      refreshToken
    });
  } catch (error) {
    res.status(500).json({ error: 'Registration failed' });
  }
}

// User login
async function login(req, res) {
  try {
    const { email, password } = req.body;

    // Find user
    const user = await User.findOne({ email });
    if (!user) {
      return res.status(401).json({ error: 'Invalid credentials' });
    }

    // Verify password
    const isValidPassword = await bcrypt.compare(password, user.password);
    if (!isValidPassword) {
      return res.status(401).json({ error: 'Invalid credentials' });
    }

    // Generate tokens
    const accessToken = generateToken(user._id);
    const refreshToken = generateRefreshToken(user._id);

    // Store refresh token
    await RefreshToken.create({ token: refreshToken, userId: user._id });

    res.json({
      user: {
        id: user._id,
        email: user.email,
        name: user.name
      },
      accessToken,
      refreshToken
    });
  } catch (error) {
    res.status(500).json({ error: 'Login failed' });
  }
}
```

### Authentication Middleware
```javascript
// Protect routes with JWT
const authenticate = async (req, res, next) => {
  try {
    // Get token from header
    const authHeader = req.headers.authorization;
    if (!authHeader || !authHeader.startsWith('Bearer ')) {
      return res.status(401).json({ error: 'No token provided' });
    }

    const token = authHeader.split(' ')[1];

    // Verify token
    const decoded = verifyToken(token);

    // Get user from database
    const user = await User.findById(decoded.id).select('-password');
    if (!user) {
      return res.status(401).json({ error: 'User not found' });
    }

    // Attach user to request
    req.user = user;
    next();
  } catch (error) {
    res.status(401).json({ error: 'Invalid or expired token' });
  }
};

// Usage in routes
router.get('/profile', authenticate, async (req, res) => {
  res.json({ user: req.user });
});
```

### Token Refresh Flow
```javascript
async function refreshAccessToken(req, res) {
  try {
    const { refreshToken } = req.body;

    if (!refreshToken) {
      return res.status(400).json({ error: 'Refresh token required' });
    }

    // Verify refresh token
    const decoded = jwt.verify(refreshToken, process.env.JWT_REFRESH_SECRET);

    // Check if refresh token exists in database
    const storedToken = await RefreshToken.findOne({
      token: refreshToken,
      userId: decoded.id
    });

    if (!storedToken) {
      return res.status(401).json({ error: 'Invalid refresh token' });
    }

    // Generate new access token
    const accessToken = generateToken(decoded.id);

    res.json({ accessToken });
  } catch (error) {
    res.status(401).json({ error: 'Token refresh failed' });
  }
}
```

## Role-Based Access Control (RBAC)

### User Model with Roles
```javascript
const userSchema = new mongoose.Schema({
  email: String,
  password: String,
  role: {
    type: String,
    enum: ['user', 'admin', 'moderator'],
    default: 'user'
  },
  permissions: [{
    type: String,
    enum: ['read', 'write', 'delete', 'manage']
  }]
});
```

### Authorization Middleware
```javascript
// Check if user has required role
const authorize = (...roles) => {
  return (req, res, next) => {
    if (!req.user) {
      return res.status(401).json({ error: 'Not authenticated' });
    }

    if (!roles.includes(req.user.role)) {
      return res.status(403).json({
        error: 'Insufficient permissions'
      });
    }

    next();
  };
};

// Check specific permissions
const checkPermission = (permission) => {
  return (req, res, next) => {
    if (!req.user.permissions.includes(permission)) {
      return res.status(403).json({
        error: `Missing permission: ${permission}`
      });
    }
    next();
  };
};

// Usage
router.delete('/users/:id',
  authenticate,
  authorize('admin', 'moderator'),
  deleteUser
);

router.post('/posts',
  authenticate,
  checkPermission('write'),
  createPost
);
```

## OAuth 2.0 with Passport.js

### Google OAuth Setup
```javascript
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;

passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: '/auth/google/callback'
  },
  async (accessToken, refreshToken, profile, done) => {
    try {
      // Find or create user
      let user = await User.findOne({ googleId: profile.id });

      if (!user) {
        user = await User.create({
          googleId: profile.id,
          email: profile.emails[0].value,
          name: profile.displayName,
          avatar: profile.photos[0].value
        });
      }

      return done(null, user);
    } catch (error) {
      return done(error, null);
    }
  }
));

// Serialize user for session
passport.serializeUser((user, done) => {
  done(null, user.id);
});

passport.deserializeUser(async (id, done) => {
  try {
    const user = await User.findById(id);
    done(null, user);
  } catch (error) {
    done(error, null);
  }
});

// Routes
router.get('/auth/google',
  passport.authenticate('google', { scope: ['profile', 'email'] })
);

router.get('/auth/google/callback',
  passport.authenticate('google', { failureRedirect: '/login' }),
  (req, res) => {
    // Generate JWT for the user
    const token = generateToken(req.user._id);
    res.redirect(`/dashboard?token=${token}`);
  }
);
```

## Security Best Practices

### Helmet.js Configuration
```javascript
const helmet = require('helmet');

app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      scriptSrc: ["'self'"],
      imgSrc: ["'self'", "data:", "https:"]
    }
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true,
    preload: true
  }
}));
```

### Rate Limiting
```javascript
const rateLimit = require('express-rate-limit');

// General rate limiter
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // 100 requests per window
  message: 'Too many requests from this IP'
});

// Strict rate limiter for auth endpoints
const authLimiter = rateLimit({
  windowMs: 60 * 60 * 1000, // 1 hour
  max: 5, // 5 login attempts per hour
  skipSuccessfulRequests: true
});

app.use('/api/', limiter);
app.use('/api/auth/login', authLimiter);
```

### Input Validation
```javascript
const { body, validationResult } = require('express-validator');

// Validation middleware
const validateRegistration = [
  body('email').isEmail().normalizeEmail(),
  body('password')
    .isLength({ min: 8 })
    .matches(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/)
    .withMessage('Password must contain uppercase, lowercase, and number'),
  body('name').trim().isLength({ min: 2, max: 50 })
];

// Handle validation errors
const handleValidation = (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() });
  }
  next();
};

router.post('/register',
  validateRegistration,
  handleValidation,
  register
);
```

### CSRF Protection
```javascript
const csrf = require('csurf');
const csrfProtection = csrf({ cookie: true });

app.use(csrfProtection);

router.get('/form', (req, res) => {
  res.render('form', { csrfToken: req.csrfToken() });
});

router.post('/process', csrfProtection, (req, res) => {
  res.send('Data is being processed');
});
```

## When to Use This Agent

Ask the Authentication & Security Agent when you:
- Implementing user authentication
- Choosing between JWT and sessions
- Setting up OAuth social login
- Implementing role-based access control
- Securing Express applications
- Preventing common security vulnerabilities
- Implementing password reset flows
- Configuring security middleware

## Integration with Other Agents

- **Express Framework Agent** - Secure Express routes and middleware
- **Database Integration Agent** - Store users and credentials securely
- **Testing & Debugging Agent** - Test authentication flows
- **Deployment & Scaling Agent** - Production security configuration

## Resources & References

- [JWT.io](https://jwt.io)
- [OWASP Security Guidelines](https://owasp.org/www-project-top-ten/)
- [Passport.js Documentation](http://www.passportjs.org)
- [Helmet.js](https://helmetjs.github.io)
- [bcrypt Documentation](https://www.npmjs.com/package/bcryptjs)

## Quick Reference

### Common JWT Claims
- `iss` - Issuer
- `sub` - Subject (user ID)
- `aud` - Audience
- `exp` - Expiration time
- `iat` - Issued at
- `nbf` - Not before

### HTTP Security Headers
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: DENY`
- `X-XSS-Protection: 1; mode=block`
- `Strict-Transport-Security: max-age=31536000`
- `Content-Security-Policy`
