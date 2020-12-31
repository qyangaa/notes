# Configuration

```
git config --global core.eitor "code --wait"
git config --global -e //edit
git config --global core.autocrlf input //for mac, linux; 'true' for windows

```

# Manipulating files

```
git add .
git status
git commit -am //add+commit
git rm //rm from directory & git
git mv original_file destination_file
git rm --cached // only remove from the index(staging area)
git ls-files
```

+ github search gitigore: find template gitignore files for different languages

+ short status:

  ```bash
  git status -s
  >>> _ _ file1.js // {staing area}{not yet instaging}
  ```


## Git Objects

+ Commit
+ Blobs (files)
+ Trees (Directories)
+ 