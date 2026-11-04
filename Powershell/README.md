# Powershell windows notes

## Can't run windows script due to permissions
Run this and you should be able to execute ```.ps1``` scripts
```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
