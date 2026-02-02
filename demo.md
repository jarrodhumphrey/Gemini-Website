# Hosting a Static Website on GitHub Pages

Follow these steps to host your static website on GitHub Pages:

## 1. Create a GitHub Repository

*   Go to [github.com/new](https://github.com/new) and create a new public repository.
*   The repository name should be `<your-username>.github.io`.

## 2. Initialize a Git Repository Locally

*   Open your terminal or command prompt.
*   Run the following commands:

    ```bash
    git init
    git add .
    git commit -m "Initial commit"
    ```

## 3. Connect to Your GitHub Repository

*   Run the following commands, replacing `<your-username>` with your GitHub username:

    ```bash
    git remote add origin https://github.com/<your-username>/<your-username>.github.io.git
    git branch -M main
    git push -u origin main
    ```

## 4. Enable GitHub Pages

*   Go to your repository's settings on GitHub.
*   In the "Pages" section, select the `main` branch as the source and click "Save".
*   Your website will be available at `https://<your-username>.github.io` in a few minutes.
