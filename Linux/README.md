# LInux quick notes

## Table of contents
* [Tar / Untar](#tar--untar)
* [Tmux terminal splitting](#tmux-terminal-splitting)


## Tar / Untar
#### Tar folder
  * ```-J``` is for tar xz format
```bash
tar -cvJf <archive_name>.tar.xz <target_folder1> <target_folder2>
```

#### Untar archive
```bash
tar -xvf <archive_name>tar.xz
```

## Tmux Terminal Splitting
* Install tmux in linux ```sudo apt install tmux```
* All commands are shift + b first then shortcut
  
| Function | Command |
| -------- | ------- |
| Vertical split   | Ctrl + b, ```%``` (shift + 5) |
| Horizontal split | Ctrl + b, ```"``` (shift + ') |
| Navigate panes   | Ctrl + b, ```up,down,left,right``` |
| Resize window    | Ctrl + b, hold Ctrl and press ```up, down,left,right``` |
