# Publish the Documentation on GitHub and Build a Website

## Repository
Suggested name: `hadoop-bigdata-training`.

## Upload using the browser
1. Create the repository on GitHub.
2. Extract the project ZIP locally.
3. Upload the *contents* of the extracted folder, not the ZIP itself.
4. Commit the initial files.
5. Confirm `README.md` renders.

## Publish with MkDocs Material + GitHub Pages
This repository includes `mkdocs.yml` and `.github/workflows/deploy-docs.yml`.

1. Push all files to the `main` branch.
2. Open **Actions** and inspect the `Deploy training documentation` workflow.
3. After the workflow succeeds and creates/updates the `gh-pages` branch, open **Settings → Pages**.
4. Select deploy from branch `gh-pages`, folder `/ (root)`, if that is the configured publishing output.
5. Wait for deployment and open the Pages URL shown in Settings.

Official references:
- GitHub Pages: https://docs.github.com/en/pages/quickstart
- MkDocs Material: https://squidfunk.github.io/mkdocs-material/
- GitHub file upload: https://docs.github.com/en/repositories/working-with-files/managing-files/adding-a-file-to-a-repository

## Git from WSL/terminal (optional)
From the extracted project folder:
```bash
git init
git add .
git commit -m "Initial CDP training documentation"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/hadoop-bigdata-training.git
git push -u origin main
```
Replace `YOUR-USERNAME`. If the remote already contains commits, reconcile histories rather than overwriting blindly. Use GitHub's browser sign-in/credential manager; never put a token in a committed file.

## Safety before publishing
- Remove real company/customer names, internal IPs, hostnames, URLs, screenshots, logs, and architecture details.
- Never commit passwords, API tokens, personal access tokens, keytabs, private keys, or certificates.
- Review copied text/images for licensing and attribution.
