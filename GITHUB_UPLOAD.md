# Uploading this project to GitHub

## GitHub website

1. Sign in to GitHub and choose **New repository**.
2. Name it `vidyakosha` and choose Public or Private.
3. Do not add another README, `.gitignore` or license during repository creation.
4. Create the repository, then use **uploading an existing file**.
5. Upload the contents of the `vidyakosha-github` folder, preserving the folders shown in the README.
6. Commit the files to the `main` branch.

GitHub's browser uploader does not unpack ZIP files automatically. Extract `vidyakosha-github-ready.zip` first, then upload the extracted contents.

## Git command line

From inside the extracted project folder:

```bash
git init
git add .
git commit -m "Initial Vidyakosha source"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/vidyakosha.git
git push -u origin main
```

Replace `YOUR-USERNAME` with your GitHub username. Do not commit `.env.local`, deployment tokens, database exports or uploaded student files.
