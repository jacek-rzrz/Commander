![](https://raw.githubusercontent.com/Keeper-Security/Commander/master/keepercommander/images/commander_logo_250x100.png)

----

Jump to:
* [Overview](#password-management-sdk-for-it-admins--developers)
* [Use Cases](#use-cases)
* [Installation - Linux and Mac](#installation---linux-and-mac)
* [Installation - Windows](#installation---windows)
* [Installation - Developer Mode](#installation---developer-mode)

### Password Management SDK for IT Admins & Developers

Keeper Security develops the world's most downloaded password manager and encrypted digital vault with millions of individual customers and thousands of enterprise customers worldwide.  Keeper offers a zero-knowledge, cloud-based solution to consumers, families and businesses to protect and secure their most sensitive and private information. Keeper Password Manager is available on every mobile and desktop device. <a href="#about-keeper">Read more</a> about Keeper or visit the [Keeper Security](https://keepersecurity.com) website.

Keeper Commander is a command-line, interactive shell and SDK interface to [Keeper&reg; Password Manager](https://keepersecurity.com). Commander can be used to access and control your Keeper vault, rotate passwords and perform Keeper Enterprise administrative functions related to user onboarding and provisioning of vault records.

In addition to vault and administrative functionality, Commander can be used to perform targeted password rotations, integrate password management into your backend systems and eliminate the use of hardcoded passwords. Using our connector [plugins](https://github.com/Keeper-Security/Commander/tree/master/keepercommander/plugins), Commander can execute a password rotation directly to any common system or service account such as Unix systems, SQL Databases, Active Directory, Amazon AWS, local Administator accounts, network devices, etc...

Keeper Commander is an open source project written in Python, and it is under continuous development by the Keeper engineering team. As new features and capabilities are added to the Keeper platform, we add new commands and features to Commander.  If you need any assistance or require specific functionality, please contact ops@keepersecurity.com.

### Use Cases

* Access your Keeper vault through a command-line interface 
* Perform bulk import and export of vault records 
* Manage records, folders and shared folders
* Customize integration into your backend systems 
* Provision new Enterprise user accounts and shared folders
* Rotate passwords on service accounts or other targets
* Integrate Keeper into your existing backend systems 
* Schedule and automate commands 

### Installation - Linux and Mac

1. Get Python 3 from [python.org](https://www.python.org).
2. Install Keeper Commander with pip3:

```bash
pip3 install keepercommander
```

Important: Restart your terminal session after installation

### Installation - Windows 

1. Download and install [WinPython](https://winpython.github.io/)
2. From the install folder of WinPython, run the "WinPython Command Prompt" 
2. Install Keeper Commander with pip3:

```bash
pip3 install keepercommander
```

To upgrade:

```bash
pip3 install --upgrade keepercommander
```

Please do not upgrade a production system without validation in your test environment as commands and functionality is under rapid development.

### Installation - Developer Mode

This type of installation assumes you want to view/modify the Python source code (Compatible with Python 3.4+).

1. Install Python3 from python.org
2. Install virtualenv:
```
sudo pip3 install virtualenv
```
3. Create and activate the virtual environment for your keeper project:

```
cd /path/to/Commander
virtualenv -p python3 venv
source venv/bin/activate
pip install -r requirements.txt
pip install -e .
```

Keeper supports plugins for various 3rd party systems for password reset integration. Depending on the plugin, you will need to also install the modules required by that plugin. For example, our MySQL plugin requires the PyMySQL module.

See the [custom](https://github.com/Keeper-Security/Commander/tree/master/keepercommander/custom) folder for examples on creating your own custom scripts.

### Command-line Usage

Commander's command-line interface and interactive shell is a powerful and convenient way to access and control your Keeper vault and perform many administrative operations. To see all available commands, just type:

```
$ keeper
```

### Interactive shell
To run a series of commands and stay logged in, you can use Commander's linux-style interactive shell.

```bash
keeper shell
```

### Optional command-line arguments

* server

Leave this blank unless you are using a different endpoint.

* user

Keeper email address (username)

* password

Keeper master password

* config

Filename of the JSON config file to use

* debug

Turning debug "on" or "off" will generate logs and debug info for developers

### Command / Feature Reference 

usage: keeper [--server SERVER] [--user USER] [--password PASSWORD]
              [--version] [--config CONFIG] [--debug]
              [command] [options [options ...]]
```  

Running most commands will require you to authenticate to Keeper and decrypt your vault.  Authentication requires your email address ("user"), master password ("password") and your two factor code (if this is enabled on your account).  You can type the "user" and "password" parameters interactively in the terminal or you can store these parameters into a config file called config.json.  Your 2FA code is not stored by you directly in the config, however a 2FA token value will be stored automatically after successful authentication.

Below is a basic "config.json" file which will save you the step of typing your email every time:

```
{
    "user":"youremail@company.com",
    "password":"",
    "mfa_token":"",
    "mfa_type":"",
    "debug":false,
    "plugins":[],
    "commands":[]
}
```

Any parameter that is not filled in will be prompted on the command line interactively. More advanced usage of configuration files for automated commands is in the <a href="#scheduling--automation">Scheduling & Automation</a> section below.

Keeper uses PBKDF2 to derive keys from your master password. By default, Keeper accounts are now set for 100,000 iterations which is very strong. However, this can make logging into your vault take longer depending on the device speed.  If logging in takes too long, consider reducing the iterations to 10,000 from the Keeper Web Vault or Desktop App.

### Command reference

* list|l               ... Display all record UID/titles

* get|g                ... Display specified Keeper record/folder/team

* add|a                ... Add record

* rm                   ... Remove record

* append-note|an       ... Append notes to existing record

* download-attachment  ... Download record attachments

* list-sf|lsf          ... Display all shared folders

* list-team|lt         ... Display all teams

* cd                   ... Change current folder

* ls                   ... List folder content

* tree                 ... Display folder structure

* mkdir                ... Create folder

* rmdir                ... Remove folder and its content

* mv                   ... Move record or folder

* ln                   ... Create a link between record or folder

* rotate|r             ... Rotate Keeper record

* import               ... Import data from local file to Keeper

* export               ... Export data from Keeper to local file

* whoami               ... Information about logged in user

* login                ... Login to Keeper

* logout               ... Logout from Keeper

* sync-down|d          ... Download & decrypt data

* create_user          ... Create Keeper User

* shell                ... Use Keeper interactive shell

* c                    ... Clear the screen

* h                    ... Show command history


### Deep linking to records (Web Vault Hyperlink)

The Record UID that is displayed on password record output can be used for deep linking directly into the Keeper Web Vault only for privileged users. This Vault link can be stored and sent over unsecure channels because it only provides a reference to the record within your vault -- it does not provide access to the actual record content.  To access the content, you must still authenticate into the vault and decrypt the data.  The link is in the format `https://keepersecurity.com/vault#detail/XXXXXX` and you simply replace XXXXXX with the Record UID. Providing this link to another user does NOT initiate sharing.  To share a vault record, you must authenticate to your vault, open the record and click the "Share" feature.

### Automating Commander 

To automate the use of Commander, create a JSON file (let's call it config.json) and place the file in the working directory where you are invoking the shell commands.  If you don't provide a config file, Commander will just prompt you for the information interactively.

Here's an example config.json file:

```
{
    "user":"your_email_here",
    "password":"your_password_here",
    "debug":false,
    "commands":[]
}
```

All fields are optional.  You can also tell Commander which config file to use.  By default, we look at the config.json file.  

Example 1: Simply access your vault interactively (if config.json is in the current folder, it will take precedence)

```bash
keeper shell
```

Example 2: Load up parameters from the specified JSON file

```bash
keeper --config=foo.json shell
```

In this case, Commander will start up using foo.json as the configuration.

### JSON Config file format

```
server: This can be set to the endpoint within Keeper's US or EU  the United States (AWS US-EAST) or AWS EU (EU-WEST-1) environments:
   US: 
user: the Keeper email address
password: the Keeper master password
debug: turn on verbose debugging output
commands: comma-separated list of commands to run
timedelay: number of seconds to wait before running all commands again
mfa_type: if multi-factor auth is used, this will be set to "device_token"
mfa_token: two-factor token used to authenticate this Commander instance
challenge: challenge phrase if you are using a Yubikey device 
```

If you have turned on two-factor authentication on your Keeper account, you will be prompted the first time you run Commander to enter the two-factor code.  Once authenticated, Commander will update the mfa_type and mfa_token parameters in the config file for you. This way, subsequent calls are authenticated without needing additional two-factor tokens.

### Importing Password Records into Keeper

To import records into your vault, you can provide either JSON or tab-delimited file.
If using a JSON file, make sure it's an a valid JSON array.  For example, here's a JSON import file with 2 records. The first record is added to a folder called "My Servers".  The second record is added to "My Servers" and also added to a shared folder called "Shared Servers".

```
[{
    "title":"Dev Server",
    "folders": [
      {
        "folder": "My Servers"
      }
    ],
    "login": "root",
    "password": "lk4j139sk4j",
    "login_url": "https://myserver.com",
    "notes": "These are some notes.",
    "custom_fields": {"Security Group":"Private"}
},
{
    "title":"Prod Server",
    "folders": [
      {
        "folder": "My Servers"
      },
      {
       "shared_folder": "Shared Servers",
       "can_edit": true,
       "can_share": true
      }
    ],
    "login": "root",
    "password": "kj424094fsdjhfs4jf7h",
    "login_url": "https://myprodserver.com",
    "notes": "These are some notes.",
    "custom_fields": {"Security Group":"Public","IP Address":"12.45.67.8"}
}]
```

The format must be perfect JSON or it will fail.  The keys in each JSON hash must be present.  Use a JSON validator if you get errors running this.
Here's the command to run:

```
keeper import --format=json import.json
```

### Scheduling & Automation

If you want to fully automate Commander operations, such as rotating a password on a regular schedule, there are a few different ways to accomplish this.

Using config.json file and **timedelay** setting, you tell Commander the time delay in seconds to wait and then reissue all commands.  This is the easiest way to schedule automated password resets.

Below is an example:

config.json:

```
{
    "debug":false,
    "user":"admin@company.com",
    "password":"somereallystrongpassword",
    "timedelay":600,
    "commands":["d", "r 3PMqasi9hohmyLWJkgxCWg", "r tlCK0x1chKH8keW8-NOraA"]
}
```

Terminal command:

```
keeper --config config.json shell
```

In this example, Commander would download and decrypt records, rotate 2 passwords (with Record UIDs specified), and then wait for 600 seconds (10 minutes) before issuing the commands again.  Also in this example, the master password is stored in the JSON file.  If you don't want to store a credential or Yubikey challenge phrase in the JSON config file, you can leave that out and you'll be prompted for the password on the interactive shell.  But in this scenario, you'll need to leave Commander running in a persistent terminal session.

If you prefer not to keep a persistent terminal session active, you can also add Commander to a cron script (for Unix/Linux systems) or the launchctl daemon on Mac systems.  Below is an example of executing Commander from a Mac launchctl scheduler:

### Two-Factor Authentication and Device Token

If you have Two-Factor Authentication enabled on your Keeper account (highly recommended), Keeper Commander will prompt 
you for the one-time passcode the first time you login.  After successfully logging in, you will be provided a device token. 
This device token is automatically saved to the config file when you login interactively. 
If you have multiple config files, you can just copy-paste this device token into your config.json file.  For example:

```
{
    "debug":false,
    "user":"email@company.com",
    "password":"123456",
    "mfa_token":"vFcl44TdjQcgTVfCMlUw0O9DIw8mOg8fJypGOlS_Rw0WfXbCD9iw",
    "mfa_type":"device_token",
    "device_token_expiration":true,
    "commands":["d", "r 3PMqasi9hohmyLWJkgxCWg", "r tlCK0x1chKH8keW8-NOraA"]
}
```
Note: If you want your device tokens to expire, set "device_token_expiration" to "true". If set, your device token will expire in 30 days.

To activate Two-Factor Authentication on your Keeper account, login to the [Web App](https://keepersecurity.com/vault) 
and visit the Settings screen.  Keeper supports Text Message, Google Authenticator, RSA SecurID and Duo Security methods.

### Yubikey Support 

Commander supports the ability to authenticate a session with a connected Yubikey device instead of using a Master Password.  To configure Yubikey authentication, follow the [setup instructions](https://github.com/Keeper-Security/Commander/tree/master/keepercommander/yubikey).  You will end up using a challenge phrase to authenticate instead of the master password.

### Targeted Password Rotations & Plugins 

Keeper Commander can communicate to internal and external systems for the purpose of rotating a password and synchronizing the change to your Keeper Vault.  For example, you might want to rotate your MySQL password and Active Directory password automatically.  To support a plugin, simply add a set of **custom field** values to the Keeper record that you will be rotating.  To do this, simply login to Keeper on the [Web Vault](https://keepersecurity.com/vault) and edit the record you will be rotating.  Add custom fields to the record and save it. The custom field value tells Commander which plugin to use when rotating the password.

For example:

```
Name: cmdr:plugin
Value: mysql
```
```
Name: cmdr:plugin
Value: adpasswd
```

When a plugin is specified in a record, Commander will search in the plugins/ folder to load the module based on the name provided (e.g. mysql.py and active_directory.py).

Check out the [plugins folder](https://github.com/Keeper-Security/Commander/tree/master/keepercommander/plugins) for all of the available plugins.  Keeper's team is expanding the number of plugins on an ongoing basis. If you need a particular plugin created, just let us know.

### Support 
We're here to help.  If you need help integrating Keeper into your environment, contact us at ops@keepersecurity.com.

### About Our Security

Keeper is a zero-knowledge platform.  This means that the server does not have access to your Keeper Master Password or the crypto keys used to encrypt and decrypt your data.  The cryptography is performed on the *client device* (e.g. iPhone, Android, Desktop, Commander).

When you create a Keeper account from our [web app](https://keepersecurity.com/vault) or [mobile/desktop app](https://keepersecurity.com/download), you are asked to create a Master Password and a security question.  The Keeper app creates your crypto keys, RSA keys and encryption parameters (iv, salt, iterations).  Your RSA private key is encrypted with your data key, and your data key is encrypted with your Master Password.  The encrypted version of your data key is stored in Keeper's Cloud Security Vault and provided to you after successful device authentication.

When you login to Keeper on any device (or on Commander), your Master Password is used to derive a 256-bit PBKDF2 key.  This key is used to decrypt your data key.  The data key is used to decrypt individual record keys.  Finally, your record keys are then used to decrypt your stored vault information (e.g. your MySQL password).

When storing information to your vault, Keeper stores and synchronizes the encrypted data.

For added security, you can enable Two-Factor Authentication on your Keeper account via the [web app](https://keepersecurity.com/vault) settings screen.  When logging into Commander with Two-Factor Authentication turned on, you will be asked for a one time passcode.  After successful authentication, you will be provided with a device token that can be used for subsequent requests without having to re-authenticate.

All of this cryptography is packaged and wrapped into a simple and easy-to-use interface.  Commander gives you the power to access, store and synchronize encrypted vault records with ease.

To learn about Keeper's security, certifications and implementation details, visit the [Security Disclosure](https://keepersecurity.com/security.html) page on our website.

### About Keeper

Keeper is the world's most downloaded password keeper and secure digital vault for protecting and managing your passwords and other secret information.  Millions of people and companies use Keeper to protect their most sensitive and private information.

Keeper's Features &amp; Benefits

* Manages all your passwords and secret info
* Protects you against hackers
* Encrypts everything in your vault 
* High-strength password generator
* Login to websites with one click
* Store private files, photos and videos
* Take private photos inside vault 
* Share records with other Keeper users
* Access on all your devices and computers
* Keeper DNA&trade; multi-factor authentication
* Login with Fingerprint or Touch ID
* Auto logout timer for theft prevention
* Unlimited backups
* Self-destruct protection
* Customizable fields
* Background themes
* Integrated Apple Watch App
* Instant syncing between devices
* AES-256 encryption
* Zero-Knowledge security architecture
* TRUSTe and SOC-2 Certified
* GDPR Compliant 

### Keeper Website
[https://keepersecurity.com](https://keepersecurity.com)

### Pricing
Keeper is free for local password management on your device.  Premium subscription provides cloud-based features and premium device-specific features including Sync, Backup & Restore, Secure Sharing, File Storage and multi-device usage.  More info about our consumer and enterprise pricing plans can be found [here](https://keepersecurity.com/pricing.html). 

### Mobile Apps

[iOS - iPhone, iPad, iPod](https://itunes.apple.com/us/app/keeper-password-manager-digital/id287170072?mt=8)

[Android - Google Play](https://play.google.com/store/apps/details?id=com.callpod.android_apps.keeper&hl=en)

[Kindle and Amazon App Store](http://amzn.com/B00NUK3F6S)

[Windows Phone](https://www.microsoft.com/en-us/store/p/keeper-password-manager/9wzdncrdmpt6)


### Cross-Platform Desktop App

[Windows PC, 32-bit](https://keepersecurity.com/desktop_electron/Win32/KeeperSetup32.zip)

[Windows PC, 64-bit](https://keepersecurity.com/desktop_electron/Win64/KeeperSetup64.zip)

[Windows PC, 32-bit MSI Installer](https://keepersecurity.com/desktop_electron/Win32/KeeperSetup32.msi)

[Mac](https://keepersecurity.com/desktop_electron/Darwin/KeeperSetup.dmg)

[Linux](https://keepersecurity.com/download.html)

### Microsoft Store (Windows 10, Surface) Platform

[Microsoft Store Version - Windows 10](https://www.microsoft.com/en-us/store/p/keeper-password-manager/9wzdncrdmpt6)


### Web Vault and Browser Extensions

[Web App - Online Vault](https://keepersecurity.com/vault)

[KeeperFill for Chrome](https://chrome.google.com/webstore/detail/keeper-browser-extension/bfogiafebfohielmmehodmfbbebbbpei)

[KeeperFill for Firefox](https://addons.mozilla.org/en-US/firefox/addon/keeper-password-manager-digita/)

[KeeperFill for Safari](https://safari-extensions.apple.com/details/?id=com.keepersecurity.safari.KeeperExtension-234QNB7GCA)

[KeeperFill for Edge](https://www.microsoft.com/en-us/store/p/keeper-password-manager-digital-vault/9n0mnnslfz1t)

[Enterprise Admin Console](https://keepersecurity.com/console)

