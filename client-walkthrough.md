# Client Experience Walkthrough: Secure Onboarding Forms

This document outlines the step-by-step experience for a client, "Alex," when interacting with The Xanikton Ledger's secure onboarding forms, now enhanced with Supabase Magic Link authentication.

---

### Phase 1: Accessing the First Form (Client Intake Form)

1.  **You receive a link:** Your bookkeeper, Xanikton Creations LLC, sends you an email with a link like: `https://forms.yourcompany.com/client-intake-form.html` (or `yourusername.github.io/your-repo-name/client-intake-form.html`). This is your secure link to the Client Intake Form.

2.  **You click the link:** When you click the link, instead of seeing the Intake Form immediately, you are greeted with a simple login page *hosted by The Xanikton Ledger*. This page will look clean and branded, and it will have a clear message like:

    > **Welcome to The Xanikton Ledger Client Portal!**
    > Please enter your email address to securely access your forms. We'll send you a magic link to log in instantly.

    There's an input field for your email address and a button that says "Send Magic Link".

3.  **You enter your email:** You type in `alex@amazingbusiness.com` (the email address Xanikton Creations LLC has on file for you) and click "Send Magic Link".

4.  **Confirmation message:** The page immediately updates with a message:

    > **Check your inbox!**
    > We've sent a magic login link to `alex@amazingbusiness.com`. Please click the link in the email to proceed.

5.  **You check your email:** You open your email client and find an email from "The Xanikton Ledger" (or "notifications@supabase.com" if not customized). The subject might be "Your Login Link for The Xanikton Ledger". Inside, there's a button or link saying "Log in to Xanikton Forms" or "Confirm your Email".

6.  **You click the magic link:** You click the link in the email. This opens a new browser tab.
    *   Briefly, you might see a quick redirect through a Supabase URL (it's very fast).
    *   Then, you are securely redirected back to the **Client Intake Form** (`client-intake-form.html`)!

7.  **Access Granted & Form Appears:** Now, the full "The Xanikton Ledger Client Intake Form" loads. You can see all the fields, header, and content. Supabase has set a secure session cookie in your browser, so the application knows you are `alex@amazingbusiness.com`.
    *   **Security check in action:** If, for some reason, your email `alex@amazingbusiness.com` wasn't on Xanikton's "authorized client" list in their Supabase database, even after clicking the magic link, the form's JavaScript would detect this and display an "Access Denied" message instead of the form. This is the "email-locked" security.

8.  **You complete the Intake Form:** You fill out all the necessary business information, and the autosave feature continues to work, saving your progress locally in your browser.

9.  **You submit the Intake Form:** Once complete, you click "Submit Form".
    *   The form validates, and if all looks good, the PDF of your completed Intake Form is generated and downloaded to your computer (e.g., `20240726_Alexs_Amazing_Business_Intake_Form.pdf`).
    *   Your browser then attempts to open a `mailto:` email draft, pre-addressed to `info@xaniktoncreations.com`, with a subject like "Completed Bookkeeping Client Intake Form".
    *   A success message appears on the form page, reminding you to attach the downloaded PDF to the email draft and send it.

---

### Phase 2: Accessing the Second Form (Partnership Agreement)

1.  **You receive a new link:** After your bookkeeper reviews the Intake Form, they send you another secure email, this time with a link to the "Bookkeeping Partnership Agreement" form, e.g., `https://forms.yourcompany.com/partnership-agreement.html`.

2.  **You click the Partnership Agreement link:**
    *   **Scenario A (Session Active):** If it hasn't been too long since you logged in for the Intake Form (typically, Supabase sessions last for hours or even days unless explicitly logged out), your browser still has an active session. The Partnership Agreement Form loads *immediately*, without asking for your email again.
    *   **Scenario B (Session Expired):** If it's been a while, your Supabase session might have expired. In this case, you'll be redirected back to the same "Welcome to The Xanikton Ledger Client Portal!" login page (step 2 from above). You'd re-enter your email, receive and click another magic link, and then land directly on the Partnership Agreement Form.

3.  **Prefilled Information:** Once the Partnership Agreement Form loads, you notice that some of your company's information (like "Client Company Name," "Client Contact Person," "Email," and "Phone") is already filled in and marked as read-only. This data was securely transferred from your Intake Form submission (via local storage on your browser).

4.  **You complete the Partnership Agreement:** You review the terms, fill in any remaining details (like the number of bank accounts to reconcile, fees, dates), and then use the integrated signature pad to either draw or type your digital signature. You check the "I agree to the terms..." checkbox.

5.  **You submit the Partnership Agreement:** You click "Submit Agreement".
    *   The PDF of the signed Partnership Agreement is generated and downloaded (e.g., `20240726_Xanikton_Partnership_Agreement.pdf`).
    *   A `mailto:` email draft opens, pre-addressed to `xaniktoncreations@gmail.com`, with a subject like "Completed Bookkeeping Partnership Agreement".
    *   A success message appears on the form page, prompting you to attach the PDF and send the email.

---

**Summary for Alex:**

The process is streamlined. You only need to enter your email and click a magic link when your session isn't active. Once authenticated, the forms become accessible, and some data even carries over between them. This provides a secure, yet user-friendly, way to handle your onboarding documentation.