# How to Deploy HealthDashboard to Azure

Since your project is built with **ASP.NET Core**, the best and easiest place to host it is **Microsoft Azure App Service**.

## Prerequisites
1.  **Azure Account**: [Create a free account](https://azure.microsoft.com/free/) if you don't have one.
2.  **Azure Tools for VS Code**: Install the [Azure App Service extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).

---

## Method 1: Deploy from VS Code (Easiest)

This method is recommended because it handles all the configuration for you visually.

1.  **Open Project**: Open the `Web-Final` folder in VS Code.
2.  **Sign In**: Click the Azure icon in the sidebar and sign in to your account.
3.  **Deploy**:
    *   Expand the **"App Service"** section.
    *   Right-click in the empty space or on your subscription and select **"Create New Web App... (Advanced)"**.
    *   *Tip: Do not choose "Advanced" if you want the absolute fastest path, but "Advanced" lets you pick the Free tier explicitly.*
4.  **Follow the Prompts**:
    *   **Name**: Enter a unique name (e.g., `health-dashboard-elia`).
    *   **Runtime Stack**: Select **.NET 8 (LTS)** or **.NET 10** (whichever matches your local setup, you previously updated to .NET 10).
    *   **OS**: Select **Linux** (usually cheaper/faster for simple apps) or **Windows**.
    *   **Resource Group**: Create new (e.g., `HealthDashboardGroup`).
    *   **App Service Plan**: Create new. **CRITICAL**: Select **Free (F1)** to ensure you are not charged.
    *   **Location**: Choose the region closest to you (e.g., `West Europe`).
5.  **Finish**: Wait for the resources to be created.
6.  **Deploy Code**: 
    *   Once created, a notification will appear: *"Deploy to..."*. Click **Deploy**.
    *   Select the `HealthDashboard` folder (the one containing `.csproj`).
    *   Click **"Add Config"** if asked to update settings for deployment.
7.  **Browse**: Once finished, click **"Browse Website"** in the notification.

---

## Method 2: Deploy via Terminal (CLI)

If you prefer using the command line or don't want to use VS Code extensions.

1.  **Install Azure CLI**: [Download and install](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).
2.  **Login**:
    ```bash
    az login
    ```
3.  **Deploy Command**:
    Navigate to your project folder:
    ```bash
    cd HealthDashboard
    ```
    Run the "magic" command    # Replace 'health-dashboard-unique-name' with your own unique name (e.g. your-name-health-app)
    # If 'eastus' fails, try 'westeurope' or 'centralus'
    az webapp up --sku F1 --name health-dashboard-unique-name --os-type linux --location westeurope
    ```
    *   `--sku F1`: Tells Azure to use the **Free** tier.
    *   `--os-type linux`: Uses a Linux container.

---

## Method 3: Azure Portal (Failsafe)

**Use this if the terminals commands give you "Policy" or "Region" errors.**

1.  Log in to [portal.azure.com](https://portal.azure.com).
2.  Search for **"App Services"** -> Click **"Create"** -> **"Web App"**.
3.  **Basics Tab**:
    *   **Subscription**: Select your student/free subscription.
    *   **Resource Group**: Create new (e.g. `MyProjectGroup`).
    *   **Name**: Enter a unique name.
    *   **Publish**: Select **Code**.
    *   **Runtime stack**: Select **.NET 8 (LTS)** or **.NET 10**.
    *   **Operating System**: Linux.
    *   **Region**: Click the dropdown. **Only select regions that don't have a warning icon**. This avoids the policy error.
    *   **Pricing Plan**: Ensure it says **Free F1** (Change size if needed).
4.  Click **Review + Create** -> **Create**.
5.  **Deploy Code**:
    *   Once the resource is created, go to **Deployment Center** (in the left menu).
    *   Select **Local Git** or **GitHub** to push your code manually.


Your application currently stores data in a list:
```csharp
private static List<HealthReadingDto> _readings = new();
```

**What this means for deployment:**
Because you are not using a database (like SQL Server), your data (Heart Rate readings, etc.) will **disappear** every time the website restarts or "goes to sleep" (which happens frequently on the Free tier to save energy).

**Is this okay?**
*   **For a Demo/Presentation**: YES. It is perfectly fine. Just know that if you show it to someone 10 minutes later, the data might be gone.
*   **For Production**: You would eventually need to connect a database.
