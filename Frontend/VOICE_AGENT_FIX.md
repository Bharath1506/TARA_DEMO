# Voice Agent Error Fix Guide

## Problem
You're getting a **400 Bad Request** error from Vapi API with error type `start-method-error`.

## Root Cause
The 400 error typically indicates one of these issues:
1. ❌ Invalid or expired API keys
2. ❌ Invalid Assistant ID
3. ❌ API key doesn't have permission to access the assistant
4. ❌ Missing environment variables

## Solution Steps

### Step 1: Verify Your .env File
Make sure your `.env` file in `f:\Tara\Frontend\.env` contains these exact variables:

```env
VITE_VOICE_AGENT_PRIVATE_KEY=f25480d6-3b64-4a86-9fc4-2039ff0a28ef
VITE_VOICE_AGENT_PUBLIC_KEY=480036f2-79db-46e7-8d3a-e4857c11f4fd
VITE_VAPI_ASSISTANT_ID=b971ee04-3880-4638-8b68-ba2a29633583
```

**Important:** Make sure there are NO spaces around the `=` sign and NO quotes around the values.

### Step 2: Verify Your Vapi Account
1. Go to https://vapi.ai/dashboard
2. Check that your API keys are valid and active
3. Verify that the assistant ID `b971ee04-3880-4638-8b68-ba2a29633583` exists
4. Make sure the public key has permission to use this assistant

### Step 3: Get Fresh API Credentials (If Needed)
If the above keys don't work:
1. Log into your Vapi dashboard
2. Go to **Settings** → **API Keys**
3. Create a new **Public Key**
4. Go to **Assistants** and copy your assistant ID
5. Update your `.env` file with the new credentials

### Step 4: Restart the Dev Server
After updating the `.env` file:
```bash
# Stop the current dev server (Ctrl+C)
# Then restart it
npm run dev
```

### Step 5: Check Browser Console
After restarting, open the browser console (F12) and look for:
```
🔑 Vapi Configuration: {
  hasPublicKey: true,
  publicKeyPrefix: "480036f2...",
  hasAssistantId: true,
  assistantIdPrefix: "b971ee04..."
}
```

If you see `false` for any of these, your environment variables aren't loading correctly.

## Common Issues

### Issue 1: Environment Variables Not Loading
**Symptom:** Console shows `❌ VITE_VOICE_AGENT_PUBLIC_KEY is not set in .env file`

**Fix:** 
- Make sure the `.env` file is in the `Frontend` folder (same level as `package.json`)
- Restart the dev server after creating/editing `.env`
- Vite only loads `.env` files on startup

### Issue 2: Invalid API Keys
**Symptom:** 400 error persists even with correct `.env` file

**Fix:**
- Your API keys might be expired or invalid
- Generate new keys from Vapi dashboard
- Make sure you're using the **Public Key**, not the Private Key

### Issue 3: Assistant Not Found
**Symptom:** Error mentions assistant ID

**Fix:**
- Verify the assistant exists in your Vapi dashboard
- Make sure the assistant is published/active
- Check that your API key has access to this assistant

## Testing
After making changes:
1. Restart the dev server
2. Open the app in browser
3. Click "Connect with Tara"
4. Check the browser console for detailed error logs

## Need More Help?
Check the browser console for these new detailed logs:
- `🔑 Vapi Configuration:` - Shows if env vars are loaded
- `🚀 Starting Vapi call with assistant ID:` - Shows the call is starting
- `❌ Failed to start call:` - Shows detailed error information

The error logs now include:
- Error message
- Error type
- Stage where it failed
- Context information
