# WP-CLI on Windows 11

WP-CLI (WordPress Command Line Interface) is a command-line tool for managing WordPress installations. This guide provides step-by-step instructions for installing WP-CLI on Windows 11.

## Installation Methods

### Method 1: Using Windows Subsystem for Linux (WSL) (Recommended)

1. **Enable WSL** (Run in PowerShell as Administrator):
   ```powershell
   wsl --install
   ```
   Restart your system if required.

2. **Install Ubuntu**:
   - Open **Microsoft Store**, search for **Ubuntu**, and install it.
   - Launch Ubuntu and set up a username/password.

3. **Install WP-CLI in WSL**:
   ```bash
   curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
   chmod +x wp-cli.phar
   sudo mv wp-cli.phar /usr/local/bin/wp
   ```

4. **Verify Installation**:
   ```bash
   wp --info
   ```

---

### Method 2: Native Windows Installation (Without WSL)

#### Step 1: Install PHP
1. Download PHP from [windows.php.net/download](https://windows.php.net/download/).
2. Extract it to `C:\php`.
3. Add `C:\php` to the system **PATH**:
   - Search **"Environment Variables"** → Edit **Path** → Add `C:\php`
4. Verify PHP installation:
   ```powershell
   php -v
   ```

#### Step 2: Install WP-CLI
1. Download WP-CLI:
   ```powershell
   curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
   ```
2. Move `wp-cli.phar` to `C:\wp-cli`.
3. Create a `wp.bat` file in `C:\wp-cli` with the following content:
   ```bat
   @ECHO OFF
   php "C:\wp-cli\wp-cli.phar" %*
   ```
4. Add `C:\wp-cli` to the system **PATH**.
5. Verify Installation:
   ```powershell
   wp --info
   ```

---

6. create `C:\xamp\htdocs\wordpress`.
7. download wordpress :
   ```powershell
   wp core download
   ```

---

8. clone wordpress theme and plugin.
   ```powershell
   git clone -b feature-ec2WPSetup https://gitlab.com/Avidia/learnqwikxr.git
   ```

---


9. activate all plugin :
   ```powershell
   wp plugin activate --all
   ```

---

10. activate theme :
   ```powershell
   wp theme activate geeks
   ```

---


### Method 3: Using Git Bash

1. Install **Git for Windows**: [Download Git](https://git-scm.com/download/win)
2. Follow **Method 2** steps but use Git Bash to run WP-CLI commands.

---

## Running WP-CLI Commands
Once installed, navigate to your WordPress directory and use WP-CLI commands like:
```powershell
wp core version
wp plugin list
wp theme list
```

---

## Troubleshooting
- **Command Not Recognized**: Restart your system and verify **PATH** settings.
- **PHP Not Found**: Ensure PHP is installed and accessible via the command line.
- **WSL Issues**: Ensure your WordPress installation is accessible from WSL.

For more information, visit the [WP-CLI Official Documentation](https://wp-cli.org/).

