# Hytale Dedicated Server - Pterodactyl Egg

This Pterodactyl Egg installs and runs a **Hytale Dedicated Server** using the official Hytale CLI Downloader. It supports the required **Java 25** environment and handles the **Device Authentication** flow required by Hypixel Studios.

> [!IMPORTANT]
>
> ### Action Required:
>
> This pterodactyl hytale server egg is still experimental and not purposed to be used directly in a production environment. For any issues make sure to reach out to me at [micheal.cc/discord](https://micheal.cc/discord).

## 📋 Prerequisites

Before importing this egg, ensure your Pterodactyl node meets the following requirement:

1.  **Custom Java 25 Docker Image**: 
    The server **must** use a Docker image containing Java 25 (OpenJDK or Oracle). 
    * **Image Name**: `michealc/hytale-java25:latest` (or your private registry equivalent).
    * *Note: Standard Pterodactyl images (Java 17/21) will NOT work and will cause a crash on startup.*

---

## 🛠️ Installation Guide

### 1. Import the Egg
1.  Go to your **Pterodactyl Admin Panel**.
2.  Navigate to **Nests** > **Import Egg**.
3.  Upload the `hytale_downloader_egg.json` file.
4.  Select the Nest you want to group it under (e.g., "Minecraft" or create a new "Hytale" nest).

### 2. Create the Server
1.  Create a new server using the imported Egg.
2.  **Resources**:
    * **RAM**: Minimum **4GB** recommended (8GB+ for production).
    * **Disk**: Allocate at least **5GB** for the game files + backups.
    * **Port**: The default Hytale port is **UDP 5520**. Ensure this port is allocated.

### 3. Configure Variables
* **Server Jar File**: Leave as `HytaleServer.jar`.
* **Patchline**: Set to `live` for the stable release, or `pre-release` / `beta` if instructed by Hytale devs.
* **Download URL**: Do not change unless the official downloader URL changes.

---

## 🚀 First-Time Setup (Authentication)

**Crucial Step:** Hytale requires you to authenticate the server with your Hytale account on the first install.

1.  **Start the Server**: The server will enter the "Installing" state.
2.  **Open the Console**: Immediately click the server name to view the console output.
3.  **Watch for the Auth Code**:
    The installer will download the tools and then pause. You will see a message like:
    ```text
    ------------------------------------------------------
    STARTING DOWNLOADER...
    Please visit [https://hytale.com/activate](https://hytale.com/activate) and enter code: ABCD-1234
    ------------------------------------------------------
    ```
4.  **Activate**:
    * Go to the URL shown in your browser.
    * Log in with your Hytale account.
    * Enter the code (`ABCD-1234`).
    * Click **Approve**.
5.  **Finish**:
    * Once approved, the console will automatically detect the success.
    * It will download the remaining game files (`HytaleServer.jar`, `assets/`).
    * The server will finish installing and start up automatically.

> **Note:** Credentials are saved in `.hytale-downloader-credentials.json` in the server files. You will not need to re-authenticate for future updates unless you delete this file.

---

## 🔄 How to Update the Server

To update the Hytale server to the latest version:

1.  Go to the Server Console.
2.  Click **"Reinstall Server"** (under Settings).
3.  The installation script will run again.
    * Because you have already authenticated, **it will skip the code check**.
    * It will automatically fetch the latest files for your `Patchline`.
    * The server will restart with the new version.

---

## ⚠️ Troubleshooting

**Server crashes immediately with "Unsupported Class Version" or "Java Version Error"**
* **Cause**: You are not using the Java 25 Docker image.
* **Fix**: Go to the **Startup** tab in Pterodactyl. Ensure the **Docker Image** field is set to `michealc/hytale-java25:latest`.

**"Authentication Timed Out" or Installer Failed**
* **Cause**: You didn't enter the code in the browser fast enough.
* **Fix**: Click **Reinstall Server** to try again. Keep the console open and be ready to enter the code.

**Server stuck on "Starting"**
* **Cause**: First run sometimes takes longer to generate the world.
* **Fix**: Check the console. If you see "World Generated", the server is up. Ensure you are connecting with the correct IP and **Port 5520**.