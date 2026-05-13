

## Prerequisites[](https://ucr-research-computing.github.io/Knowledge_Base/how_to_mount_google_drive.html#prerequisites)

- A Google account with access to Google Drive
- A Linux machine with administrative access Rclone installed on your machine
    
    ## Step 1: Install Rclone[](https://ucr-research-computing.github.io/Knowledge_Base/how_to_mount_google_drive.html#step-1-install-rclone)
    
    If Rclone is not already installed on your machine, you can install it by running the following command:
    

```
curl https://rclone.org/install.sh | sudo bash
```

This will download the latest version of Rclone and install it on your machine.

### Step 2: Configure Rclone[](https://ucr-research-computing.github.io/Knowledge_Base/how_to_mount_google_drive.html#step-2-configure-rclone)

Before you can mount Google Drive, you will need to configure Rclone to access your Google Drive account. To do this, you can run the following command:

```
rclone config
```

This will launch the Rclone configuration wizard. Follow the prompts to add a new remote for Google Drive.

Select “n” for “New remote” Type a name for the remote (e.g. “gdrive”) Select “drive” from the list of storage providers Follow the prompts to authorize Rclone to access your Google Drive account Finally, select “q” to quit the configuration wizard

### Step 3: Create a Mount Point[](https://ucr-research-computing.github.io/Knowledge_Base/how_to_mount_google_drive.html#step-3-create-a-mount-point)

A mount point is a directory on your machine where you will access the files and directories on your Google Drive. To create a mount point, you can run the following command:

```
mkdir -p ~/gdrive
```

This will create a directory called “gdrive” in your home directory.

### Step 4: Mount Google Drive[](https://ucr-research-computing.github.io/Knowledge_Base/how_to_mount_google_drive.html#step-4-mount-google-drive)

Once you have configured Rclone and created a mount point, you can mount your Google Drive by running the following command:

```
rclone mount gdrive: ~/gdrive
```

This will mount your Google Drive at the mount point you created in step 3. You should now be able to access the files and directories on your Google Drive by navigating to the “gdrive” directory in your home directory.

### Step 5: Automatic Mount on Startup[](https://ucr-research-computing.github.io/Knowledge_Base/how_to_mount_google_drive.html#step-5-automatic-mount-on-startup)

To mount Google Drive automatically when your machine starts up, you can add the mount command to your crontab by running the following command:

```
(crontab -l 2>/dev/null; echo "@reboot rclone mount gdrive: $HOME/gdrive") | crontab -
```

This will add the mount command to your crontab, so that it runs automatically on startup.