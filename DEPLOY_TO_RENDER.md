# How to Deploy HealthDashboard to Render.com

Since we've created a `Dockerfile`, you can easily deploy your app to Render.

## Prerequisites
1.  **GitHub Account**: Your code must be pushed to a GitHub repository.
2.  **Render Account**: [Create a free account](https://render.com/).

---

## Step 1: Push Code to GitHub
If you haven't already, push your code to a new GitHub repository:
1.  Initialize Git: `git init`
2.  Add files: `git add .`
3.  Commit: `git commit -m "Ready for deployment"`
4.  Create a repo on GitHub.com.
5.  Link and push:
    ```bash
    git remote add origin <your-github-repo-url>
    git push -u origin main
    ```

## Step 2: Create Web Service on Render
1.  Go to the [Render Dashboard](https://dashboard.render.com/).
2.  Click **"New +"** -> **"Web Service"**.
3.  Connect your GitHub account and select your `HealthDashboard` repository.
4.  **Configure the Service**:
    *   **Name**: `health-dashboard` (or unique name)
    *   **Region**: Closest to you (e.g., Frankfurt for EU)
    *   **Runtime**: Select **Docker** (Critical step!)
    *   **Instance Type**: Free
5.  **Environment Variables** (Optional):
    *   Render usually handles the port automatically (`8080` is exposed in our Dockerfile).
6.  Click **"Create Web Service"**.

## Step 3: Wait & Verify
1.  Render will verify the Dockerfile, build the image, and deploy it.
2.  This might take 3-5 minutes.
3.  Once the status says **Live**, click the URL (e.g., `https://health-dashboard.onrender.com`) to view your app.

---

## Important Notes
*   **Startup Time**: The Free tier on Render "spins down" after 15 minutes of inactivity. The first request after a break might take 50 seconds to load.
*   **Data Persistence**: Just like Azure, the **in-memory data will be lost** every time the server restarts or spins down.
