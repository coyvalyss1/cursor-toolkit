---
name: firebase-functions-v2-export
description: Correct export pattern for Firebase Functions v2 (onCall, onRequest, onSchedule). Use when creating or modifying Firebase Cloud Functions, especially when deployment succeeds but functions return 404 or don't appear in firebase functions:list.
---

# Firebase Functions v2 Export Pattern

## Critical Rule

**NEVER export Cloud Functions directly from service files. ALWAYS use the two-file pattern.**

## The Problem

Firebase Functions v2 (`onCall`, `onRequest`, `onSchedule`) require inline handler functions. Exporting them directly from service files causes:
- ✅ Deployment succeeds
- ❌ Function returns 404
- ❌ Function doesn't appear in `firebase functions:list`
- ❌ Silent failures with no error logs

## Correct Pattern

### Step 1: Service File (e.g., `myService.js`)

Export a **plain async function** (no Firebase wrappers):

```javascript
/**
 * Service function: My Service
 * 
 * @param {Object} data - Request data
 * @param {Object} context - Request context with auth
 * @returns {Object} Response data
 */
async function myServiceHandler(data, context) {
  // Your logic here
  return { success: true };
}

// Export ONLY the service function
module.exports = {
  myServiceHandler
};
```

### Step 2: Index File (`index.js`)

Import with **renamed alias** and wrap with inline handler:

```javascript
// At top of index.js - RENAME on import to avoid collision
const { myServiceHandler: myServiceLogic } = require('./myService.js');

// Define secrets if needed
const mySecret = defineSecret('MY_SECRET');

// In exports section - wrap with inline async function
exports.myServiceFunction = onCall({
  secrets: [mySecret],
  enforceAppCheck: false,
  cors: [
    'http://localhost:5173',
    'https://yourdomain.com',
    'https://your-project.web.app'
  ],
  region: 'us-central1',
  timeoutSeconds: 60,
  memory: '512MiB'
}, async (request) => {
  // Adapt v2 request format to service function signature
  const data = request.data;
  const context = { auth: request.auth };
  
  // Call the renamed imported function
  return await myServiceLogic(data, context);
});
```

## Common Function Types

### onCall (Callable Function)

```javascript
exports.myCallableFunction = onCall({
  secrets: [apiKey],
  region: 'us-central1',
  memory: '512MiB'
}, async (request) => {
  const data = request.data;
  const auth = request.auth;
  
  return await serviceFunction(data, { auth });
});
```

### onRequest (HTTP Function)

```javascript
exports.myHttpFunction = onRequest({
  region: 'us-central1',
  memory: '512MiB',
  timeoutSeconds: 300
}, async (req, res) => {
  const { logger } = require('firebase-functions/v2');
  
  try {
    const result = await serviceFunction(logger);
    res.status(200).json(result);
  } catch (error) {
    logger.error('Function failed:', error);
    res.status(500).json({ success: false, error: error.message });
  }
});
```

### onSchedule (Scheduled Function)

```javascript
exports.myScheduledFunction = onSchedule({
  schedule: 'every sunday 02:00',
  timeZone: 'America/Los_Angeles',
  region: 'us-central1',
  memory: '512MiB',
  timeoutSeconds: 540
}, async (event) => {
  const { logger } = require('firebase-functions/v2');
  
  return await serviceFunction(logger);
});
```

## Deployment Checklist

Before deploying a new Cloud Function:

- [ ] Service file exports plain async function (no Firebase wrappers)
- [ ] Import in `index.js` with **renamed alias**
- [ ] All required **secrets** defined at top of `index.js` using `defineSecret()`
- [ ] Export uses `onCall`/`onRequest`/`onSchedule` with **inline async handler**
- [ ] Handler adapts `request.data` and `request.auth` to service signature
- [ ] CORS includes all production domains if HTTP/callable
- [ ] Deploy: `firebase deploy --only functions:functionName`
- [ ] Verify: `firebase functions:list` shows the function
- [ ] Test: Function endpoint returns 200, not 404

## CORS Configuration Example

Always include all your domains:

```javascript
cors: [
  'http://localhost:5173',           // Local development
  'https://yourdomain.com',          // Production domain
  'https://your-project.web.app',    // Firebase hosting
  'https://app.yourdomain.com'       // App subdomain
]
```

## Debugging Firebase Function Errors

When a Firebase function issue occurs, check in this order:

### 1. CORS Errors

CORS is handled automatically by Firebase when the callable signature is correct. Do NOT try `cors: true`, manual CORS configuration, or installing the cors package for callable functions.

Use the `(request)` signature pattern. Read existing functions in `index.js` to match:
```javascript
exports.myFunction = onCall({ ... }, async (request) => {
  const data = request.data;
  const auth = request.auth;
  return await serviceLogic(data, { auth });
});
```

If CORS errors persist, the function likely isn't deploying correctly -- check export location next.

### 2. 404 / Function Not Found

The function is defined AFTER `module.exports = {}`, so it's silently ignored. `module.exports = {}` overwrites the entire exports object; any `exports.functionName` after it is lost. Firebase CLI does not warn about this.

Fix: Define the function variable BEFORE `module.exports = {}` and include it in the exports object.

### 3. Symptoms of Incorrect Pattern

- Deployment succeeds but function returns 404
- "ReferenceError: X is not defined" during deployment
- CORS errors even after deployment
- "Skipping unchanged functions" but function doesn't exist
- Function missing from `firebase functions:list`

### Quick Fix

1. Move Firebase wrapper (`onCall`/`onRequest`/`onSchedule`) from service file to `index.js`
2. Rename import in `index.js` to avoid name collision
3. Wrap with inline async function in `index.js`
4. Verify function is included in `module.exports = {}` (not after it)
5. Redeploy: `firebase deploy --only functions:functionName`
