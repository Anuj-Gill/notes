#git #gitFlag

This flag helps update the latest commit made. This technically creates a new commit and overwrites the previous one, updating the commit hash as well. 

```cmd
git commit --amend
```

To directly update just the commit message:

```
git commit --amend -m "New commit message"
```

Another use case is when new changes to be added to previous commit only instead of creating new commit:
- Stage the new changes
- use the --amend flag and update the previous commit
- In case you directly want to amend the commit, use the following command
```
  git commit --amend --no-edit
  ```
  