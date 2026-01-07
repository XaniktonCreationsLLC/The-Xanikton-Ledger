
# Deploying Xanikton Ledger Forms with GitHub Pages

This guide will walk you through the process of hosting your secure Xanikton Ledger forms (Client Intake Form, Partnership Agreement, and Admin Panel) using GitHub Pages. GitHub Pages is a static site hosting service that takes HTML, CSS, and JavaScript files straight from a repository on GitHub, optionally runs the files through a build process, and publishes a website directly.

---

## Prerequisites

*   A GitHub account.
*   The Xanikton Ledger forms code (the `client-intake-form.html`, `partnership-agreement.html`, `admin.html` and the walkthrough `.md` files).
*   Your Supabase Project URL and Anon Key (already obtained and updated in your HTML files).

---

## Step 1: Create a New GitHub Repository

1.  Go to [GitHub](https://github.com/) and log in to your account.
2.  Click the **"+"** icon in the top right corner and select **"New repository"**.
3.  **Repository name:** Choose a descriptive name, e.g., `xanikton-ledger-forms` or `client-onboarding-portal`.
    *   **Important:** If you want your site to be hosted at `yourusername.github.io`, the repository name MUST be `yourusername.github.io`. For other names, it will be `yourusername.github.io/repository-name`.
4.  **Description (Optional):** Add a brief description of your project.
5.  **Public/Private:** Choose **"Public"** if you want your forms to be accessible by anyone with the link (standard for client-facing forms). If you choose "Private," you will need to configure GitHub Pages for private repositories, which is typically a feature of GitHub Enterprise or requires more advanced setup outside the scope of basic GitHub Pages.
6.  **Initialize this repository with:** Do **NOT** check "Add a README file," "Add .gitignore," or "Choose a license" if you plan to push your existing files directly. If you do, you'll need to handle merging later. It's usually cleaner to start with an empty repo.
7.  Click **"Create repository"**.

---

## Step 2: Upload Your Project Files to GitHub

There are a few ways to do this:

### Option A: Using the GitHub Web Interface (Simplest for a few files)

1.  On your new repository page, click the **"uploading an existing file"** link.
2.  Drag and drop all your HTML files (`client-intake-form.html`, `partnership-agreement.html`, `admin.html`), markdown files (`client-walkthrough.md`, `bookkeeper-walkthrough.md`, `supabase-setup-guide.md`, `github-pages-deployment.md`), and `metadata.json` directly into the upload area.
3.  Add a commit message (e.g., "Initial commit of Xanikton Ledger Forms").
4.  Click **"Commit changes"**.

### Option B: Using Git Command Line (Recommended for ongoing development)

1.  **Initialize Git in your local project folder:**
    Open your terminal or command prompt, navigate to the folder containing your project files.
    ```bash
    cd /path/to/your/xanikton-forms-folder
    git init
    ```
2.  **Add your files:**
    ```bash
    git add .
    ```
3.  **Commit your files:**
    ```bash
    git commit -m "Initial commit of Xanikton Ledger Forms"
    ```
4.  **Link to your GitHub repository:**
    Replace `yourusername` and `yourrepositoryname` with your actual GitHub username and repository name.
    ```bash
    git remote add origin https://github.com/yourusername/yourrepositoryname.git
    ```
5.  **Push your files to GitHub:**
    ```bash
    git push -u origin main
    # Or if your default branch is 'master': git push -u origin master
    ```

---

## Step 3: Configure GitHub Pages

1.  On your GitHub repository page, click on **"Settings"** (the gear icon near the top right).
2.  In the left sidebar, click on **"Pages"** (under the "Code and automation" section).
3.  Under **"Build and deployment"**:
    *   **Source:** Select **"Deploy from a branch"**.
    *   **Branch:**
        *   Choose your main branch (usually `main` or `master`).
        *   Select the `/ (root)` folder.
    *   Click **"Save"**.

4.  **Wait for Deployment:** GitHub Pages will now start building and deploying your site. This usually takes a few minutes.
    *   You'll see a message like "Your GitHub Pages site is currently being built from the `main` branch."
    *   Refresh the page after a couple of minutes. Once deployment is successful, you'll see a message: **"Your site is live at `https://yourusername.github.io/yourrepositoryname/`"** (or `https://yourusername.github.io/` if it's a user/organization page).

---

## Step 4: Access Your Live Forms

1.  Click the provided link on the GitHub Pages settings page.
2.  Test `https://yourusername.github.io/yourrepositoryname/client-intake-form.html`
3.  Test `https://yourusername.github.io/yourrepositoryname/partnership-agreement.html`
4.  Test `https://yourusername.github.io/yourrepositoryname/admin.html`

**Important Notes:**

*   **Supabase URLs in HTML:** Ensure you have correctly replaced `YOUR_SUPABASE_URL` and `YOUR_SUPABASE_ANON_KEY` in `client-intake-form.html`, `partnership-agreement.html`, and `admin.html` with your actual Supabase project credentials.
*   **Redirect URL for Supabase Auth:** If you encounter issues with magic link or password reset redirects, double-check your Supabase project's **Authentication > URL Configuration** settings. You might need to add your GitHub Pages URL (e.g., `https://yourusername.github.io/yourrepositoryname/`) to the "Site URL" or "Redirect URLs" list.
*   **Domain:** If you want to use a custom domain (e.g., `forms.xaniktonledger.com`), you can configure that in the "Custom domain" section of the GitHub Pages settings. This requires DNS changes with your domain provider.
*   **SSL (HTTPS):** GitHub Pages automatically provides HTTPS for your deployed site, ensuring secure communication for your forms.

You now have your Xanikton Ledger Forms securely hosted and accessible online!
