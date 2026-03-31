# Getting Wired Up on Windows: VS Code + Node + GitHub + Gemini CLI

This guide will walk you through setting up your Windows development environment for programming, version control, and using the Gemini CLI.

## 1. Verify Installations in PowerShell

Ensure Node.js, npm, Git, and Gemini CLI are correctly installed and accessible from PowerShell.

1.  Open PowerShell by searching for "PowerShell" in the Start Menu.
2.  Run the following commands and check their output:

    ```powershell
    node -v
    npm -v
    git --version
    gemini --version
    ```

**What success looks like:**
You should see version numbers displayed for each command (e.g., `v20.11.0` for Node, `10.2.4` for npm, `git version 2.43.0.windows.1` for Git, and `1.0.0` for Gemini). If any command returns an error like "command not found" or "is not recognized," revisit the installation steps for that specific tool.

---

### Quick Test: Verify all versions

```powershell
node -v && npm -v && git --version && gemini --version
```
**Expected Output:** All version numbers displayed without errors.

---

## 2. Configure Git (first-time setup)

Configure your Git identity, which will be associated with your commits.

1.  In PowerShell, set your global Git username:

    ```powershell
    git config --global user.name "YOUR_GITHUB_USERNAME"
    ```
    *(Replace `YOUR_GITHUB_USERNAME` with your actual GitHub username)*

2.  Set your global Git email:

    ```powershell
    git config --global user.email "YOUR_GITHUB_EMAIL"
    ```
    *(Replace `YOUR_GITHUB_EMAIL` with the email associated with your GitHub account)*

3.  Set the default branch name for new repositories to `main`:

    ```powershell
    git config --global init.defaultBranch main
    ```

---

### Quick Test: Check Git Configuration

```powershell
git config --list
```
**Expected Output:** Look for `user.name` and `user.email` entries matching your configured values, and `init.defaultBranch=main`.

---

## 3. Create and Connect SSH key to GitHub (Windows)

Use an SSH key for secure and password-less interaction with GitHub repositories.

1.  **Generate a new SSH key:**
    Open PowerShell and run:

    ```powershell
    ssh-keygen -t ed25519 -C "YOUR_GITHUB_EMAIL"
    ```
    *(Replace `YOUR_GITHUB_EMAIL` with your GitHub email. Press Enter when prompted for a file to save the key, accepting the default location. You can set a passphrase, but it's optional for convenience.)*

2.  **Start the ssh-agent service:**
    The `ssh-agent` stores your private keys and manages authentication.

    ```powershell
    Get-Service ssh-agent | Set-Service -StartupType Automatic
    Start-Service ssh-agent
    ```

3.  **Add your SSH key to the ssh-agent:**

    ```powersera
    ssh-add ~/.ssh/id_ed25519
    ```
    *(If you chose a different name or path for your key, adjust `id_ed25519` accordingly.)*

4.  **Copy your public SSH key:**
    The public key needs to be added to your GitHub account.

    ```powershell
    Get-Content ~/.ssh/id_ed25519.pub | Set-Clipboard
    ```
    *(This command copies the content of your public key file to your clipboard.)*

5.  **Add the public key to GitHub:**
    *   Go to GitHub.com and log in.
    *   Click your profile picture in the top-right corner, then click **Settings**.
    *   In the left sidebar, click **SSH and GPG keys**.
    *   Click the **New SSH key** or **Add SSH key** button.
    *   Provide a descriptive title (e.g., "My Windows Laptop").
    *   Paste the copied public key into the "Key" field.
    *   Click **Add SSH key**.

---

### Quick Test: Test SSH Connection to GitHub

```powershell
ssh -T git@github.com
```
**Expected Output:** You might see a warning about host authenticity. Type `yes` and press Enter. A successful connection will display a message like: `Hi YOUR_GITHUB_USERNAME! You've successfully authenticated...`

**Note:** If you face persistent issues with SSH, you can always fall back to using HTTPS for Git operations, though SSH is generally preferred for convenience.

---

## 4. Sign into GitHub in VS Code (Windows)

Connect VS Code to your GitHub account for seamless integration with Git and GitHub features.

1.  Open VS Code.
2.  Click the **Accounts** icon (looks like a person) in the bottom-left corner of the sidebar.
3.  Click **Turn on Setting Sync...** (if prompted) or **Sign in to Sync Settings**. If already signed in, ensure it's the correct account.
4.  Choose **Sign in with GitHub**. Your browser will open to authenticate.
5.  Follow the prompts in your browser to authorize VS Code.
6.  Once authenticated, you will see your GitHub username next to the Accounts icon.
7.  Open the **Source Control** panel (Ctrl+Shift+G) to see Git integration features.

---

### Quick Test: Verify GitHub Sign-in

Check the bottom-left corner of VS Code; your GitHub username should be displayed when you hover over the Accounts icon.

---

## 5. Create a project folder and initialize Git

Set up a new local project, ready for version control.

1.  Open PowerShell.
2.  Create a new directory for your project:

    ```powershell
    mkdir MyNewProject
    cd MyNewProject
    ```
    *(Replace `MyNewProject` with your desired project name.)*

3.  Initialize a new Git repository in this folder:

    ```powershell
    git init
    ```
    **Expected Output:** `Initialized empty Git repository in C:/path/to/MyNewProject/.git/`

4.  Create a `README.md` file. You can do this directly in PowerShell:

    ```powershell
    New-Item -ItemType File README.md
    ```

5.  Open the project folder in VS Code:

    ```powershell
    code .
    ```
    *(This command opens the current directory in VS Code.)*

6.  In VS Code, add some content to `README.md` (e.g., `# My New Project`).
7.  Save the `README.md` file (Ctrl+S).
8.  Back in PowerShell (or VS Code's integrated terminal), add the `README.md` file to Git's staging area:

    ```powershell
    git add README.md
    ```

9.  Commit the file:

    ```powershell
    git commit -m "Initial commit: Add README"
    ```

---

### Quick Test: Check Git Status

```powershell
git status
```
**Expected Output:** `On branch main` and `nothing to commit, working tree clean`.

---

## 6. Create GitHub repo and push code

Create a remote repository on GitHub and push your local code to it.

1.  Go to GitHub.com and log in.
2.  Click the **+** icon in the top-right corner, then select **New repository**.
3.  For "Repository name," enter `MyNewProject` (or your project's name).
4.  Choose **Private** or **Public** as desired.
5.  **Do NOT** check "Add a README file" or any other options, as we already have a local README.
6.  Click **Create repository**.
7.  On the next page, under "Quick setup — if you've done this kind of thing before," copy the SSH remote URL. It will look something like `git@github.com:YOUR_GITHUB_USERNAME/MyNewProject.git`.
8.  Back in your project's PowerShell terminal, add this as your remote origin:

    ```powershell
    git remote add origin git@github.com:YOUR_GITHUB_USERNAME/MyNewProject.git
    ```
    *(Replace the URL with the one you copied from GitHub.)*

9.  Push your local `main` branch to the remote GitHub repository:

    ```powershell
    git push -u origin main
    ```
    **Expected Output:** Messages indicating the push was successful, like `branch 'main' set up to track 'origin/main'`.

---

### Quick Test: Verify Push to GitHub

Go to your repository on GitHub.com and refresh the page. You should see your `README.md` file.

---

## 7. Run Gemini CLI inside the project

Use the Gemini CLI to interact with your project directly from VS Code.

1.  In VS Code, open the integrated terminal (Ctrl+`).
2.  Ensure your current directory is your project folder (`MyNewProject`).
3.  Run a simple Gemini CLI command to summarize your `README.md`:

    ```powershell
    gemini summarize README.md
    ```
    **Expected Output:** Gemini CLI should process the `README.md` and provide a summary.

4.  Create a `TODO.md` file using Gemini CLI. This demonstrates Gemini's ability to create files based on prompts.

    ```powershell
    gemini create file TODO.md --prompt "Create a markdown TODO list for project setup. Include items like 'Set up CI/CD', 'Add project dependencies', 'Write unit tests'."
    ```
    **Expected Output:** Gemini CLI will create `TODO.md` with the requested content.

---

### Quick Test: Check Gemini CLI output and new file

Verify the summary of `README.md` was displayed, and check that `TODO.md` now exists in your project folder with generated content.

---

## 8. Troubleshooting (Windows)

Common issues and their solutions.

### `git` not recognized or similar command not found errors

*   **Cause:** The executable's directory is not in your system's PATH environment variable.
*   **Solution:**
    1.  Re-run the installer for Git, Node.js, or Gemini CLI. During installation, ensure you select the option to add the tool to your system's PATH.
    2.  Manually add the installation directory (e.g., `C:\Program Files\Git\bin` for Git, or `C:\Users\YOUR_USERNAME\AppData\Local\Programs\gemini-cli\bin` for Gemini CLI) to your system's PATH environment variable. After modifying PATH, close and reopen PowerShell.

### `permission denied (publickey)` when using SSH

*   **Cause:** Your SSH key isn't correctly set up, or GitHub isn't recognizing it.
*   **Solution:**
    1.  Ensure `ssh-agent` is running and your key is added (Section 3, steps 2 & 3).
    2.  Verify your public key is correctly added to your GitHub account (Section 3, step 5). Check for typos or extra spaces.
    3.  Confirm you're using the SSH URL (`git@github.com:user/repo.git`) and not the HTTPS URL.
    4.  If all else fails, consider using HTTPS for Git operations (e.g., `git remote set-url origin https://github.com/YOUR_USERNAME/MyNewProject.git`).

### `fatal: remote origin already exists.`

*   **Cause:** You've already configured a remote named `origin` for your repository.
*   **Solution:**
    1.  To see existing remotes: `git remote -v`
    2.  To remove the existing origin: `git remote remove origin`
    3.  Then, re-add your desired remote origin (Section 6, step 8).

### Gemini authentication issues (e.g., "Authentication required")

*   **Cause:** Your Gemini CLI token is missing or expired.
*   **Solution:**
    1.  Log in again using `gemini auth login` (if applicable to your Gemini CLI version).
    2.  Ensure your `GEMINI_API_KEY` environment variable is correctly set if you are using an API key.
    3.  Refer to the Gemini CLI documentation for specific authentication instructions.

---

## Final Checklist: You're Wired Up!

You should now have a fully functional development environment.

- [ ] Node.js and npm are installed and version-verified.
- [ ] Git is installed, configured with your identity, and version-verified.
- [ ] SSH key is generated, added to `ssh-agent`, and connected to GitHub.
- [ ] VS Code is signed into GitHub.
- [ ] You can create local Git repositories and push them to GitHub.
- [ ] Gemini CLI runs successfully within your project.
- [ ] You can troubleshoot common issues.

Congratulations! You're ready to start building and collaborating.