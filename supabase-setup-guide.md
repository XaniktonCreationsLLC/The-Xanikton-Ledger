# Supabase Setup Guide for Xanikton Ledger Forms

This guide details the necessary steps to configure your Supabase project to enable secure authentication and authorization for your client-facing forms (`client-intake-form.html` and `partnership-agreement.html`) and the Bookkeeper Admin Panel (`admin.html`).

---

## Prerequisites

*   A Supabase account.
*   A new or existing Supabase project.
*   The code for your forms (`client-intake-form.html`, `partnership-agreement.html`, `admin.html`).

---

## Step 1: Get Your Supabase Project URL and Anon Key

These are essential for connecting your frontend applications to your Supabase backend.

1.  Log in to your [Supabase Dashboard](https://app.supabase.com/).
2.  Select your project.
3.  In the sidebar, navigate to **Project Settings** (the gear icon) > **API**.
4.  You will find:
    *   **Project URL:** Copy this (e.g., `https://abcdefg12345.supabase.co`).
    *   **`anon` (public) key:** Copy this (e.g., `eyJhbGciOiJIUzI1Ni...`).
5.  **Update your HTML files:** Replace `'YOUR_SUPABASE_URL'` and `'YOUR_SUPABASE_ANON_KEY'` placeholders in `client-intake-form.html`, `partnership-agreement.html`, and `admin.html` with these values.

    ```javascript
    // Example:
    const SUPABASE_URL = 'https://abcdefg12345.supabase.co'; 
    const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1Ni...'; 
    const supabase = Supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);
    ```

---

## Step 2: Configure Authentication Methods

You'll enable two types of authentication:
*   **Email Magic Link** for your clients (passwordless, user-friendly).
*   **Email/Password** for yourself (the bookkeeper) to access the Admin Panel.

1.  In your Supabase project dashboard, navigate to **Authentication** (the shield icon) > **Settings**.
2.  Under **Authentication Providers**:
    *   Ensure **Email** is toggled **ON**.
    *   For the **Client Forms (Magic Link)**: The `signInWithOtp` method used in `client-intake-form.html` and `partnership-agreement.html` relies on this. No further specific settings are typically needed here unless you want to customize email templates.
    *   For the **Bookkeeper Admin Panel (Email/Password)**: The `signInWithPassword` method in `admin.html` relies on this. Make sure "Email Sign-up" is enabled to allow you to create your admin user.
3.  **Email Templates (Optional but Recommended):** While still in **Authentication** > **Settings**, scroll down to **Email Templates**. You can customize the "Magic Link" and "Password Reset" email templates to match your branding (e.g., "The Xanikton Ledger").

---

## Step 3: Create Your Bookkeeper Admin User

You need a user account to log into the `admin.html` panel.

1.  In your Supabase project dashboard, navigate to **Authentication** > **Users**.
2.  Click the **"Invite user"** button.
3.  Enter your bookkeeper email address (e.g., `rochelle@xaniktonledger.com`).
4.  Provide a strong password.
5.  Click "Invite user".
6.  You will receive an invitation email. Click the link in the email to confirm your account.
7.  Once confirmed, you can log in to your `admin.html` panel using this email and password.

---

## Step 4: Create the `authorized_client_emails` Table

This table will store the email addresses of clients who are authorized to view your forms.

1.  In your Supabase project dashboard, navigate to **Table Editor** (the spreadsheet icon).
2.  Click the **"New table"** button.
3.  Configure the table as follows:
    *   **Name:** `authorized_client_emails`
    *   **Description:** Stores emails of clients authorized to access Xanikton Ledger forms.
    *   **Columns:**
        *   `id`:
            *   Type: `uuid`
            *   Default value: `gen_random_uuid()`
            *   Primary Key: **Yes**
        *   `email`:
            *   Type: `text`
            *   Default value: (leave blank)
            *   Is Nullable: **No** (i.e., `NOT NULL`)
            *   Is Unique: **Yes** (to prevent duplicate client emails)
        *   `created_at`:
            *   Type: `timestamp with time zone`
            *   Default value: `now()`
            *   Is Nullable: **No**
        *   `created_by`:
            *   Type: `uuid`
            *   Default value: (leave blank)
            *   Is Nullable: **No**
            *   **Foreign Key:** Click "Add Foreign Key".
                *   Table: `auth.users`
                *   Column: `id`
                *   This links each authorized email to the bookkeeper user who added it.
4.  Click **"Save"**.

---

## Step 5: Configure Row Level Security (RLS) for `authorized_client_emails`

RLS is crucial for ensuring that clients can only verify their own email, and you can manage all emails, securely.

1.  In the **Table Editor**, select your `authorized_client_emails` table.
2.  Click the **"Row Level Security"** tab.
3.  Toggle **"Enable Row Level Security (RLS)"** to **ON**.
4.  Click **"New policy"**.

### Policy 1: Bookkeeper Admin - Full Access (SELECT, INSERT)

This policy allows authenticated bookkeepers to view all authorized emails and add new ones.

*   **Policy Name:** `Bookkeeper Admin - Full Access` (or similar)
*   **Target Roles:** `authenticated` (your bookkeeper user is an `authenticated` user)
*   **Permissions:**
    *   `SELECT` (check this)
    *   `INSERT` (check this)
*   **Using expression (for SELECT):** `true` (allows all authenticated users to select all rows in this table for your admin panel)
*   **With check expression (for INSERT):** `auth.uid() = created_by` (ensures that when you add an email, your user ID is recorded as `created_by`, and only the `created_by` user (or someone with broader permissions) can modify/delete it later. For simplicity in this panel, we're only INSERTing.)
*   Click **"Review"** and then **"Save policy"**.

### Policy 2: Client Forms - Self-Email Check (SELECT)

This policy allows any authenticated client to check if *their own email address* exists in the `authorized_client_emails` table. They cannot see other clients' emails.

*   **Policy Name:** `Client Forms - Self-Email Check` (or similar)
*   **Target Roles:** `authenticated` (any logged-in client user is an `authenticated` user)
*   **Permissions:**
    *   `SELECT` (check this)
*   **Using expression:** `email = (SELECT email FROM auth.users WHERE id = auth.uid())`
    *   **Explanation:**
        *   `auth.uid()`: Gets the UUID of the currently authenticated user.
        *   `auth.users`: This is a built-in Supabase table containing user information, including their email.
        *   This policy ensures that a user can only `SELECT` (read) rows from `authorized_client_emails` where the `email` column matches their own authenticated email address.
*   Click **"Review"** and then **"Save policy"**.

---

## Step 6: Test Your Setup

1.  **Bookkeeper Admin Panel:**
    *   Go to `admin.html`.
    *   Log in with your bookkeeper credentials.
    *   You should see your email address, a logout button, the form to add new client emails, and a list of existing authorized emails.
    *   Add a new test client email (e.g., `testclient@example.com`).
    *   Verify it appears in the list.
2.  **Client Forms:**
    *   Go to `client-intake-form.html`.
    *   Enter the test client email (`testclient@example.com`) and click "Send Magic Link".
    *   Check `testclient@example.com`'s inbox for the magic link.
    *   Click the magic link.
    *   You should now see the full client intake form.
    *   If you try with an *unauthorized* email, you should see the "Access Denied!" message.
    *   Repeat for `partnership-agreement.html`.

---

By following these steps, you'll have a fully functional and secure system for managing client access to your forms.