# AWS Amplify Deployment & CORS Fix Guide

This guide helps resolve the **CORS policy** and **400 Bad Request** errors you're seeing in your deployed AWS Amplify application.

## 1. Authorize Your Domain in Vapi Dashboard

Vapi requires you to explicitly allow the domains that will call their API.

1.  Log in to your [Vapi Dashboard](https://dashboard.vapi.ai).
2.  Navigate to **Settings** -> **API Keys**.
3.  Find the **Allowed Origins** (or Web Origins) section.
4.  Add your Amplify URL:
    `https://main.d3p0zwxum23f16.amplifyapp.com`
5.  Click **Save**.

> [!IMPORTANT]
> Make sure there are no trailing slashes and the protocol (`https://`) is included.

---

## 2. Configure Environment Variables in AWS Amplify

Your application uses Vite, which bakes environment variables into the code *at build time*. If these are missing in Amplify, the API calls will fail with a 400 error.

1.  Open the [AWS Amplify Console](https://console.aws.amazon.com/amplify).
2.  Select your project (**main.d3p0zwxum23f16**).
3.  Go to **App settings** -> **Environment variables**.
4.  Click **Manage variables** and add the following:

| Variable Name | Value |
| :--- | :--- |
| `VITE_VOICE_AGENT_PUBLIC_KEY` | `480036f2-79db-46e7-8d3a-e4857c11f4fd` |
| `VITE_VAPI_ASSISTANT_ID` | `b971ee04-3880-4638-8b68-ba2a29633583` |

5.  Click **Save**.

---

## 3. Trigger a New Build

Since Vite injects these variables during the build process, you **must redeploy** your application for the changes to take effect.

1.  In the Amplify Console, go to your **Branch** (e.g., `main`).
2.  Click **Redeploy this version** or push a tiny change to your repository (e.g., a space in a README) to trigger an automatic build.
3.  Wait for the build and deployment to complete.

---

## 4. Troubleshooting

If it still doesn't work after redeploying:

*   **Check the Console**: Open your browser dev tools (F12) and check the "🔑 Vapi Configuration" log. It should show `hasPublicKey: true` and `hasAssistantId: true`.
*   **Public Key Match**: Ensure you are using the **Public Key** in `VITE_VOICE_AGENT_PUBLIC_KEY`, not the Private Key.
*   **Browser Cache**: Try opening the site in an Incognito window to rule out caching issues.
