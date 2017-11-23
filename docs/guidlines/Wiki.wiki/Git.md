##.gitconfig

Ändra i C:\Users\ [username] \ .gitconfig för smidig merge 
```
[diff]
   tool = DiffMerge
[difftool "DiffMerge"]
   cmd = 'C:/Program Files/SourceGear/Common/DiffMerge/sgdm.exe' "$LOCAL" "$REMOTE"
[merge]
   tool = DiffMerge
[mergetool "DiffMerge"]
   cmd = 'C:/Program Files/SourceGear/Common/DiffMerge/sgdm.exe' -merge -result="$PWD/$MERGED" "$PWD/$LOCAL" "$PWD/$BASE" "$PWD/$REMOTE"
   trustExitCode = true
[mergetool]
   keepBackup = false

[alias]
 co = checkout
 ci = commit
 st = status
 br = branch
 hist = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=short
 type = cat-file -t
 dump = cat-file -p
 cam = commit -a -m
 pullr = pull --rebase
 ls = log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate
```