# Authentication System Architecture

**Last Updated:** 2025-10-09  
**Status:** ✅ PRODUCTION READY

---

## Table of Contents

1. [Overview](#overview)
2. [Authentication Flow](#authentication-flow)
3. [Architecture Components](#architecture-components)
4. [Critical Implementation Details](#critical-implementation-details)
5. [Common Issues & Solutions](#common-issues--solutions)
6. [Testing Guide](#testing-guide)

---

## Overview

The Moj AI authentication system uses **Google OAuth 2.0** for user authentication with **JWT tokens** for session management. The system supports multi-tenant architecture with role-based access control (RBAC).

### Key Technologies

- **OAuth Provider:** Google OAuth 2.0
- **Token Format:** JWT (JSON Web Tokens)
- **Token Algorithm:** HS256
- **Session Storage:** localStorage (frontend) + JWT verification (backend)
- **Security:** HTTPBearer with custom logging wrapper

### User Roles

- **ADMIN:** Full access to admin dashboard and AI configuration
- **USER:** Access to chat interface and user features

### Admin Email Allowlist

The following emails are automatically granted ADMIN role:
- `mojaixyz@gmail.com`
- `produkcija97@gmail.com`

---

## Authentication Flow

### 1. Login Initiation

```
User clicks "Login with Google"
    ↓
Frontend redirects to: /api/v1/auth/google/login
    ↓
Backend redirects to: Google OAuth consent screen
```

### 2. Google OAuth Callback

```
User approves on Google
    ↓
Google redirects to: /api/v1/auth/google/callback?code=...
    ↓
Backend exchanges code for Google tokens
    ↓
Backend fetches user info from Google
    ↓
Backend creates/updates user in database
    ↓
Backend generates JWT access_token + refresh_token
    ↓
Backend redirects to: http://localhost:3000/auth/callback?access_token=...&refresh_token=...
```

### 3. Frontend Token Storage

```
Frontend callback page receives tokens
    ↓
Stores in localStorage:
  - access_token
  - refresh_token
    ↓
Calls /api/v1/auth/me to fetch user data
    ↓
Decodes JWT to determine user role
    ↓
Redirects to:
  - /admin (if role === "ADMIN")
  - /chat (if role === "USER")
```

### 4. Authenticated Requests

```
Frontend makes API request
    ↓
Adds header: Authorization: Bearer <access_token>
    ↓
Backend HTTPBearer extracts token
    ↓
Backend verifies JWT signature
    ↓
Backend queries database for user
    ↓
Backend returns user data or 401 Unauthorized
```

---

## Architecture Components

### Backend Components

#### 1. **`backend/app/api/v1/auth.py`** - Authentication Endpoints

**Purpose:** Main authentication API endpoints

**Key Endpoints:**

- `GET /api/v1/auth/google/login` - Initiates Google OAuth flow
- `GET /api/v1/auth/google/callback` - Handles Google OAuth callback
- `GET /api/v1/auth/me` - Returns current user information

**Critical Code - User Creation:**

```python
ADMIN_EMAILS = {"mojaixyz@gmail.com", "produkcija97@gmail.com"}

if not user_obj:
    # Create tenant
    tenant = Tenant(
        name=f"{name}'s Workspace",
        subdomain=f"tenant-{uuid_lib.uuid4().hex[:8]}",
    )
    db.add(tenant)
    await db.flush()

    # Create user with enum role
    role = UserRole.ADMIN if email in ADMIN_EMAILS else UserRole.USER
    user_obj = User(
        tenant_id=tenant.id,
        email=email,
        google_id=google_id,
        name=name,
        role=role,  # Must be UserRole enum, not string
    )
    db.add(user_obj)
    await db.flush()
```

**Critical Code - JWT Token Creation:**

```python
# Create a simple user object for JWT creation
user = {
    "id": user_obj.id,
    "tenant_id": user_obj.tenant_id,
    "email": user_obj.email,
    "name": user_obj.name,
    "role": user_obj.role.value if isinstance(user_obj.role, UserRole) else user_obj.role
}

access_token = create_access_token({
    "sub": str(user["id"]),
    "tenant": str(user["tenant_id"]),
    "email": user["email"],
    "role": user["role"]  # Must be string in JWT
})
```

**Critical Code - /me Endpoint:**

```python
@router.get("/me", response_model=UserResponse)
async def get_current_user_info(
    current_user: User = Depends(get_current_user),  # Uses CustomHTTPBearer
    db: AsyncSession = Depends(get_db)
):
    """Get current user information"""
    # Get tenant info
    result = await db.execute(
        select(Tenant).where(Tenant.id == current_user.tenant_id)
    )
    tenant = result.scalar_one_or_none()

    if not tenant:
        raise HTTPException(status_code=404, detail="Tenant not found")
    
    return UserResponse(
        id=str(current_user.id),
        email=current_user.email,
        name=current_user.name,
        role=current_user.role,
        tenant={
            "id": str(tenant.id),
            "name": tenant.name,
            "subdomain": tenant.subdomain
        }
    )
```

#### 2. **`backend/app/core/auth.py`** - Authentication Dependencies

**Purpose:** FastAPI dependencies for authentication and authorization

**Key Components:**

- `CustomHTTPBearer` - Custom HTTPBearer with logging
- `get_current_user()` - Extracts and verifies JWT token
- `require_admin()` - Ensures user has ADMIN role

**Critical Code - CustomHTTPBearer:**

```python
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials

class CustomHTTPBearer(HTTPBearer):
    async def __call__(self, request: Request):
        print("=" * 80)
        print("[CUSTOM_HTTP_BEARER] Called!")
        print(f"[CUSTOM_HTTP_BEARER] Headers: {dict(request.headers)}")
        print(f"[CUSTOM_HTTP_BEARER] Authorization: {request.headers.get('authorization', 'MISSING')}")
        print("=" * 80)
        try:
            result = await super().__call__(request)
            print(f"[CUSTOM_HTTP_BEARER] Success! Credentials: {result}")
            return result
        except Exception as e:
            print(f"[CUSTOM_HTTP_BEARER] FAILED! Error: {type(e).__name__}: {str(e)}")
            raise

security = CustomHTTPBearer()
```

**Critical Code - get_current_user:**

```python
async def get_current_user(
    credentials: Annotated[HTTPAuthorizationCredentials, Depends(security)],
    db: AsyncSession = Depends(get_db),
    request: Request = None
) -> User:
    """Get current authenticated user from JWT token"""
    
    # Extract token from credentials
    token = credentials.credentials
    
    # Verify token signature and expiration
    payload = verify_token(token)
    if not payload:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid authentication credentials",
            headers={"WWW-Authenticate": "Bearer"},
        )
    
    # Extract user_id and tenant_id from payload
    user_id = payload.get("sub")
    tenant_id = payload.get("tenant")
    
    if not user_id or not tenant_id:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid token payload"
        )

    # Convert string IDs to UUIDs
    user_id_uuid = uuid.UUID(user_id)
    tenant_id_uuid = uuid.UUID(tenant_id)

    # Query database for user
    result = await db.execute(
        select(User).where(
            User.id == user_id_uuid,
            User.tenant_id == tenant_id_uuid
        )
    )
    user = result.scalar_one_or_none()
    
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="User not found"
        )
    
    return user
```

#### 3. **`backend/app/utils/jwt.py`** - JWT Token Management

**Purpose:** Create and verify JWT tokens

**Key Functions:**

- `create_access_token()` - Creates JWT access token
- `create_refresh_token()` - Creates JWT refresh token
- `verify_token()` - Verifies and decodes JWT token

**Critical Code - Token Creation:**

```python
def create_access_token(data: dict, expires_delta: Optional[timedelta] = None) -> str:
    """Create a JWT access token"""
    to_encode = data.copy()
    
    if expires_delta:
        expire = datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(minutes=settings.jwt_expiration_minutes)
    
    to_encode.update({"exp": expire, "iat": datetime.utcnow()})
    
    encoded_jwt = jwt.encode(
        to_encode, 
        settings.jwt_secret, 
        algorithm=settings.jwt_algorithm
    )
    
    return encoded_jwt
```

**Critical Code - Token Verification:**

```python
def verify_token(token: str) -> Optional[Dict[str, Any]]:
    """Verify and decode a JWT token"""
    try:
        payload = jwt.decode(
            token, 
            settings.jwt_secret, 
            algorithms=[settings.jwt_algorithm]
        )
        return payload
    except JWTError as e:
        return None
```

#### 4. **`backend/app/main_db.py`** - Main Application

**Purpose:** FastAPI application setup and middleware configuration

**⚠️ CRITICAL: NO DEPENDENCY OVERRIDES**

```python
# ❌ WRONG - DO NOT DO THIS:
# app.dependency_overrides[get_current_user] = some_other_function

# ✅ CORRECT - Use the auth dependencies directly from core.auth:
from app.core.auth import get_current_user, require_admin
```

**Why This Matters:**

FastAPI's `dependency_overrides` replaces the dependency globally. If you override `get_current_user` with a function that uses a different `HTTPBearer` instance, your `CustomHTTPBearer` will never be called, and authentication will fail silently.

**CORS Configuration:**

```python
allowed_origins = [
    "http://localhost:3000",
    "http://localhost:3001",
    "https://slovenian-ai.com",
    "https://www.slovenian-ai.com"
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=allowed_origins,
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "DELETE", "PATCH"],
    allow_headers=["Authorization", "Content-Type", "X-Session-ID"],
)
```

### Frontend Components

#### 1. **`frontend/contexts/auth-context-safe.tsx`** - Authentication Context

**Purpose:** React context for managing authentication state

**Key Functions:**

- `checkAuth()` - Checks if user is authenticated
- `refreshUser()` - Fetches current user data from backend
- `logout()` - Clears tokens and redirects to login

**Critical Code - refreshUser:**

```typescript
const refreshUser = useCallback(async () => {
  try {
    const token = localStorage.getItem('access_token');
    if (!token) {
      console.log('No token found in refreshUser');
      return;
    }

    const base = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:8000';
    const response = await fetch(`${base}/api/v1/auth/me`, {
      method: 'GET',
      headers: {
        'Authorization': `Bearer ${token}`,
        'Content-Type': 'application/json'
      }
    });

    if (response.ok) {
      const userData = await response.json();
      setUser(userData);
      setLoading(false);
    } else {
      localStorage.removeItem('access_token');
      localStorage.removeItem('refresh_token');
      setUser(null);
      setLoading(false);
    }
  } catch (error) {
    console.error('refreshUser: Error fetching user data:', error);
    localStorage.removeItem('access_token');
    localStorage.removeItem('refresh_token');
    setUser(null);
    setLoading(false);
  }
}, []);
```

#### 2. **`frontend/app/auth/callback/page.tsx`** - OAuth Callback Handler

**Purpose:** Handles OAuth callback and token storage

**Critical Code:**

```typescript
useEffect(() => {
  const handleCallback = async () => {
    const access_token = searchParams.get('access_token');
    const refresh_token = searchParams.get('refresh_token');
    const error = searchParams.get('error');

    if (error) {
      toast.error('Authentication failed: ' + error);
      router.push('/');
      return;
    }

    if (access_token && refresh_token) {
      // Store tokens
      localStorage.setItem('access_token', access_token);
      localStorage.setItem('refresh_token', refresh_token);

      // Wait for localStorage to be ready
      await new Promise(resolve => setTimeout(resolve, 100));

      // Fetch user data to populate auth context
      await refreshUser();

      // Decode JWT to get user role
      const payload = JSON.parse(atob(access_token.split('.')[1]));
      const userRole = payload.role;

      toast.success('Successfully logged in!');

      // Redirect based on role
      if (userRole && (userRole.toLowerCase() === 'admin' || userRole === 'ADMIN')) {
        router.push('/admin');
      } else {
        router.push('/chat');
      }
    } else {
      toast.error('Authentication failed: Missing tokens');
      router.push('/');
    }
  };

  handleCallback();
}, [router, searchParams, refreshUser]);
```

---

## Critical Implementation Details

### 1. JWT Token Structure

**Payload Fields:**

```json
{
  "sub": "a1502ba2-13f4-4999-801c-28efca3cadce",  // User ID (UUID)
  "tenant": "c6bd390d-1cf5-4cac-a568-f604a7f447af",  // Tenant ID (UUID)
  "email": "produkcija97@gmail.com",
  "role": "ADMIN",  // String: "ADMIN" or "USER"
  "exp": 1760601959,  // Expiration timestamp
  "iat": 1759997159   // Issued at timestamp
}
```

**Token Expiration:**

- **Access Token:** 10,080 minutes (7 days) - configured in `JWT_EXPIRATION_MINUTES`
- **Refresh Token:** 30 days

### 2. Database Schema

**Users Table:**

```python
class User(Base):
    __tablename__ = "users"
    
    id = Column(UUID(as_uuid=True), primary_key=True, default=uuid.uuid4)
    tenant_id = Column(UUID(as_uuid=True), ForeignKey("tenants.id"), nullable=False)
    email = Column(String, unique=True, nullable=False, index=True)
    google_id = Column(String, unique=True, nullable=False, index=True)
    name = Column(String, nullable=False)
    role = Column(Enum(UserRole), nullable=False, default=UserRole.USER)  # ENUM, not string!
    is_active = Column(Boolean, default=True)
    created_at = Column(DateTime(timezone=True), server_default=func.now())
    updated_at = Column(DateTime(timezone=True), onupdate=func.now())
```

**Tenants Table:**

```python
class Tenant(Base):
    __tablename__ = "tenants"
    
    id = Column(UUID(as_uuid=True), primary_key=True, default=uuid.uuid4)
    name = Column(String, nullable=False)
    subdomain = Column(String, unique=True, nullable=False)
    created_at = Column(DateTime(timezone=True), server_default=func.now())
```

### 3. Environment Variables

**Required in `backend/.env`:**

```bash
# JWT Configuration
JWT_SECRET=your-secret-key-here-change-in-production-min-32-chars-long
JWT_ALGORITHM=HS256
JWT_EXPIRATION_MINUTES=10080

# Google OAuth
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
GOOGLE_REDIRECT_URI=http://localhost:8000/api/v1/auth/google/callback

# Frontend URL
FRONTEND_URL=http://localhost:3000
```

**Required in `frontend/.env.local`:**

```bash
NEXT_PUBLIC_API_URL=http://localhost:8000
```

---

## Common Issues & Solutions

### Issue 1: Login Returns 401 Unauthorized

**Symptoms:**
- User completes Google OAuth successfully
- Backend creates JWT tokens
- Frontend calls `/api/v1/auth/me` and gets 401
- User is redirected back to login screen

**Root Cause:**
FastAPI `dependency_overrides` is replacing `get_current_user` with a broken function that uses a different `HTTPBearer` instance.

**Solution:**
Remove all `dependency_overrides` from `backend/app/main_db.py`:

```python
# ❌ DELETE THIS:
app.dependency_overrides[get_current_user] = get_current_user_compat

# ✅ USE THIS INSTEAD:
from app.core.auth import get_current_user, require_admin
# No overrides needed!
```

### Issue 2: CustomHTTPBearer Logging Never Appears

**Symptoms:**
- Added logging to `CustomHTTPBearer.__call__()`
- Logging never appears in backend logs
- 401 errors occur without any custom logging

**Root Cause:**
A different `HTTPBearer` instance is being used (e.g., from `main_db.py` instead of `core/auth.py`).

**Solution:**
Ensure all endpoints use `Depends(get_current_user)` from `app.core.auth`, which uses the `CustomHTTPBearer` instance.

### Issue 3: Role Mismatch (Enum vs String)

**Symptoms:**
- User role is stored as string in database instead of enum
- Role comparisons fail
- Admin users cannot access admin endpoints

**Root Cause:**
Mixing enum and string types when creating/updating users.

**Solution:**
Always use `UserRole` enum when setting role:

```python
# ✅ CORRECT:
user.role = UserRole.ADMIN

# ❌ WRONG:
user.role = "ADMIN"
```

When creating JWT payload, convert enum to string:

```python
# ✅ CORRECT:
"role": user_obj.role.value if isinstance(user_obj.role, UserRole) else user_obj.role

# ❌ WRONG:
"role": user_obj.role  # This would be an enum object, not a string
```

### Issue 4: CORS Errors

**Symptoms:**
- Browser console shows CORS errors
- OPTIONS preflight requests fail
- Authorization header is blocked

**Solution:**
Ensure `Authorization` is in `allow_headers`:

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=allowed_origins,
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "DELETE", "PATCH"],
    allow_headers=["Authorization", "Content-Type", "X-Session-ID"],  # Must include Authorization
)
```

### Issue 5: Port 3000 Required for Google OAuth

**Symptoms:**
- Google OAuth fails when frontend runs on port 3001
- "Redirect URI mismatch" error

**Root Cause:**
Google OAuth is configured with `http://localhost:3000` as the authorized redirect URI.

**Solution:**
Always use port 3000 for frontend. If port 3000 is occupied, kill the process:

```bash
lsof -ti:3000 | xargs kill -9
```

---

## Testing Guide

### Manual Testing

**Test Accounts:**

- **Admin:** produkcija97@gmail.com
- **User:** blocklabstech@gmail.com

**Test Steps:**

1. **Start Backend:**
   ```bash
   cd backend
   source venv/bin/activate
   python3 -m uvicorn app.main_db:app --host 0.0.0.0 --port 8000 --reload
   ```

2. **Start Frontend:**
   ```bash
   cd frontend
   npm run dev
   ```

3. **Test Admin Login:**
   - Navigate to http://localhost:3000
   - Click "Login with Google"
   - Select produkcija97@gmail.com
   - Should redirect to http://localhost:3000/admin
   - Verify admin dashboard loads

4. **Test User Login:**
   - Logout
   - Click "Login with Google"
   - Select blocklabstech@gmail.com
   - Should redirect to http://localhost:3000/chat
   - Verify chat interface loads

5. **Test Token Verification:**
   ```bash
   # Get token from localStorage in browser console
   TOKEN=$(pbpaste)  # Paste token from clipboard
   
   # Test /me endpoint
   curl -H "Authorization: Bearer $TOKEN" http://localhost:8000/api/v1/auth/me
   ```

### Automated Testing with Playwright

```typescript
// Test admin login
test('admin can login and access admin dashboard', async ({ page }) => {
  await page.goto('http://localhost:3000');
  await page.click('text=Login with Google');
  
  // Complete Google OAuth (requires test account setup)
  // ...
  
  await expect(page).toHaveURL('http://localhost:3000/admin');
  await expect(page.locator('h1')).toContainText('Admin Dashboard');
});

// Test user login
test('user can login and access chat', async ({ page }) => {
  await page.goto('http://localhost:3000');
  await page.click('text=Login with Google');
  
  // Complete Google OAuth
  // ...
  
  await expect(page).toHaveURL('http://localhost:3000/chat');
  await expect(page.locator('h1')).toContainText('Chat');
});
```

---

## Security Best Practices

### 1. JWT Secret Management

- **Development:** Use a strong random string (min 32 characters)
- **Production:** Use environment-specific secrets, rotate regularly
- **Never commit:** Add `.env` to `.gitignore`

### 2. Token Expiration

- **Access Token:** Short-lived (7 days for development, 1 hour for production)
- **Refresh Token:** Longer-lived (30 days)
- **Implement refresh:** Use refresh token to get new access token before expiration

### 3. HTTPS in Production

- **Always use HTTPS** in production
- **Update Google OAuth redirect URI** to use HTTPS
- **Set secure cookie flags** for production

### 4. Admin Email Allowlist

- **Keep list minimal:** Only add trusted admin emails
- **Use environment variable:** Move allowlist to `.env` for production
- **Audit regularly:** Review admin access periodically

---

## Troubleshooting Checklist

When authentication breaks, check these in order:

1. ✅ **Backend is running** on port 8000
2. ✅ **Frontend is running** on port 3000 (not 3001!)
3. ✅ **No dependency overrides** in `main_db.py`
4. ✅ **CustomHTTPBearer is used** in `core/auth.py`
5. ✅ **CORS allows Authorization header**
6. ✅ **JWT_SECRET is set** in backend `.env`
7. ✅ **Google OAuth credentials** are correct
8. ✅ **Database is running** and accessible
9. ✅ **User exists in database** with correct role
10. ✅ **Token is not expired** (check `exp` field)

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER BROWSER                             │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Frontend (Next.js on port 3000)                           │ │
│  │  - Login button                                            │ │
│  │  - Auth callback handler                                   │ │
│  │  - Auth context (manages user state)                       │ │
│  │  - localStorage (stores tokens)                            │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ 1. Click "Login with Google"
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                    Backend (FastAPI on port 8000)                │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  /api/v1/auth/google/login                                 │ │
│  │  - Redirects to Google OAuth                               │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ 2. Redirect to Google
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                      Google OAuth Server                         │
│  - User approves access                                          │
│  - Returns authorization code                                    │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ 3. Callback with code
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                    Backend (FastAPI on port 8000)                │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  /api/v1/auth/google/callback                              │ │
│  │  - Exchange code for Google tokens                         │ │
│  │  - Fetch user info from Google                             │ │
│  │  - Create/update user in PostgreSQL                        │ │
│  │  - Generate JWT tokens                                     │ │
│  │  - Redirect to frontend with tokens                        │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ 4. Redirect with tokens
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                         USER BROWSER                             │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  /auth/callback?access_token=...&refresh_token=...         │ │
│  │  - Store tokens in localStorage                            │ │
│  │  - Call /api/v1/auth/me to fetch user data                 │ │
│  │  - Decode JWT to get role                                  │ │
│  │  - Redirect to /admin or /chat                             │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ 5. GET /api/v1/auth/me
                              │    Authorization: Bearer <token>
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                    Backend (FastAPI on port 8000)                │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  CustomHTTPBearer                                          │ │
│  │  - Extract token from Authorization header                 │ │
│  └────────────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  get_current_user()                                        │ │
│  │  - Verify JWT signature                                    │ │
│  │  - Decode payload                                          │ │
│  │  - Query PostgreSQL for user                               │ │
│  │  - Return User object                                      │ │
│  └────────────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  /api/v1/auth/me endpoint                                  │ │
│  │  - Return user data with tenant info                       │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ 6. Return user data
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                         USER BROWSER                             │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Auth Context                                              │ │
│  │  - Set user state                                          │ │
│  │  - User is now authenticated                               │ │
│  │  - All future requests include Authorization header        │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

---

## Maintenance Notes

### When to Update This Document

- ✅ After fixing authentication bugs
- ✅ After changing JWT configuration
- ✅ After modifying Google OAuth setup
- ✅ After adding new admin emails
- ✅ After changing database schema
- ✅ After updating security middleware

### Related Documentation

- `docs/architecture/database-schema.md` - Database structure
- `docs/architecture/system-overview.md` - Overall system architecture
- `docs/admin/admin-guide.md` - Admin user guide
- `docs/LOGIN_FIX_FINAL.md` - Historical login fix documentation

---

**Document Version:** 1.0  
**Last Verified Working:** 2025-10-09  
**Next Review Date:** 2025-11-09

