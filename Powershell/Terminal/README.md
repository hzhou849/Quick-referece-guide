# Powershell Terminal Notes:

## Table of Contents:
* [Profile / Bashrc Default Settings](#profile-bashrc-default-settings)
  - set alias
  - disable bell sound in terminal
* [ENV path for session](#env-path-for-session)
  - Set temporary path for session


## Profile / Bashrc Default Settings
* The ```$PROFILE``` env variable is linked to the default config file.
* user ```<text_editor> ie code $PROFILE``` to edit and make changes to the profile file.

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
