# Powershell Terminal Notes:

## Table of Contents:
* [Terminal split views](#terminal-split-views)
* [Profile / Bashrc Default Settings](#profile--bashrc-default-settings)
  - set alias
  - disable bell sound in terminal
* [ENV path for session](#env-path-for-session)
  - Set temporary path for session
* [Install packages with Winget](#install-packages-with-winget)


## Profile / Bashrc Default Settings
* The ```$PROFILE``` env variable is linked to the default config file.
* user ```<text_editor> ie code $PROFILE``` to edit and make changes to the profile file.

#### Refresh the terminal window
Simply run ```.$PROFILE``` in termianl to re-source it in the current session

## How to edit file
* with vscode ```code $PROFILE``` in terminal 
```powershell
Set-PSReadlineOption -BellStyle None  # Disables the backspace bell sound 
function goto-python { Set-Location "C:\Development\python" }    # sets alias
function goto-proj { Set-Location "C:\Development\python\tb_server_project\mdt_csr_generator" }
```

## ENV Path for session
TO set temporary ENV path for the session only
```
$env:Path += ";C:\Program Files\OpenSSL-Win64\bin"
```

## Terminal split views
In the windows terminal app you can split the views 

### Controls

| Feature   | Commmand |
|-----------| ---------|
| Horizontal | Alt + Shift + ``` - ``` |
| Vertical   | Alt + Shift + ```+```   |
| Adjust size | Alt + shit + ```up,down,left, right``` |
| Move btwn windows panes | Alt + ```up,down,left,right``` |


## Install packages with winget

#### Install package
```winget install -e --id <package_Idname>
#### Search for package name
```winget search openssl```

this will return
```
winget search openssl
Name               Id                             Version Match            Source
---------------------------------------------------------------------------------
OpenSSL            ShiningLight.OpenSSL.Dev       4.0.1                    winget
FireDaemon OpenSSL FireDaemon.OpenSSL             4.0.1   Command: openssl winget
OpenSSL Light LTS  ShiningLight.OpenSSL.LTS.Light 3.5.6   Command: openssl winget
OpenSSL Light      ShiningLight.OpenSSL.Light     4.0.1   Command: openssl winget
FireDaemon Lozenge FireDaemon.FireDaemonLozenge   3.1.3   Tag: openssl     winget
Stunnel            MichalTrojnara.Stunnel         5.78    Tag: openssl     winget
```
