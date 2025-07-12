# Github Linux sync

Instructions on how to setup github automatic synching and backup for programs with files used as databases.

## Choose local git folder

The folder where the database files are or will be.

## Create repository

Create repository on github and get the ssh url for cloning

Note: You will need an ssh key with github having the public version:

`ssh-keygen -t ed25519 -C "your_email@example.com"`

[Add ssh key to gibhub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

You may also need to rename your branch to `main` after you do `git init`, git will tell you about it

```
git init
```
```
git remote add origin <remote>
```
```
git pull origin main
```

replace \<remote\> with the ssh url

## Create bash file that will replace your app binary launcher

Whatever folder you add the file in, make sure the folder is in the PATH

you can check your path with `echo $PATH`

if it's not in PATH open `~/.bashrc` and add the following line at the bottom:

```
export PATH=$PATH:<path to folder>
```

now add this to your bash file:

```
#!/usr/bin/bash
cd <path to folder with git>
git pull origin main
<call original binary launcher>
git add .
git commit -m "update"
git push origin main
```

At this point you can launch your app from the terminal by calling your bash file and it'll sync with github. You can stop here or make an app launcher so you can launch the app from the system tray.

## Create app launcher (optional)

Create a \<unique\_name>.desktop file in ~/.local/share/applications and put the following in it:
```
[Desktop Entry]
Name=<app name>
Comment=<app description>
Exec=<call your bash file>
Terminal=false
Type=Application;
```

If everything was done correctly you should now be able to start your app from the system tray and it'll sync up with github.

## Notes

There is an edge case bug with this that if you poweroff your system without closing the app, any modifications you made will not be uploaded to github. I know there is a way to add a script that'll do it on system poweroff. I might add it later.