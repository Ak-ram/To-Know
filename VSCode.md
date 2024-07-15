# Mac & VSCode Hell

## VS Code: [Insufficient permissions](https://stackoverflow.com/questions/51674627/insufficient-permissions-in-vscode)

**Problem:** 

Each time I try to edit a file in VS Code that I don't own (such as a file in a cloned repository), I encounter an "Insufficient Permission" issue when saving the file.

**Solution:**

```terminal
sudo chown -R username directory_name
```

**Description:**
  - sudo – admin rights must be used since we are dealing with a folder that belongs to another user 
  - chown – the command for changing ownership 
  - -R – the recursive switch to make sure all child objects get the same ownership changes 
  - username – the new owner of the folder 
  - directory_name – the directory to be modified
