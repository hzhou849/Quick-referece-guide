# Putty ssh

## setup ssh with no password using ED25519 keys
* In order to ssh on to an ubuntu node without password, you must add the rsa public/private pair keys
1) Using PuTTygen generate a Ed25519 type key
2) save both public, private keys files
3) using Pageant (from taskbar) this is how you tell putty which private key you're using, add your private key file
4) ssh onto ubuntu and goto `.ssh/authrized_keys' add the public key
5) The format of you key should look like this for ubuntu:
```
ssh-ed25519 <the key_AAAAC3NzaC1lZDI1NTE5AA....adfef@$#> <comment:putty-key>
```

### Sample plink ssh command to execute binary on target
```ps
plink -batch -ssh -v -l username 195.168.0.9 '/home/user/./test_code -s1'
```
