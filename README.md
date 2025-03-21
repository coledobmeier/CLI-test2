# Creating a local repo and cloning to Github
### Heres how to create a repo on a local machine and then push it to github.
1.) Make a directory that will be your repo title and cd into it:
```
mkdir <repo name> && cd <repo name>
```
2.) Create a git folder so you can use git functions:
```
git init .
```
3.) Adding a text file containing initial commits or text:
```
echo "text" > test.txt
```
4.) If you would like to add a directory/folder within the repo: 
```
mkdir test && echo "text content" > test/testfile.txt
```
5.) Now we stage the files we want to commit:
```
git add .
git commit -m "message"
```
6.) Finally pushing to your github:
```
gh repo create <repo name> --public --source=. --push.
```

---

# Possible Errors and fixes
If you get an error that says: ```fatal: failed to push some refs to <repo.url>"``` This usually happens because the remote repo has changes that your local repo doesn't. (Someone else made a commit,of you intialized the repo with a README on github).
### Heres how to fix it ->
```
git pull origin main --rebase 
```
This syncs your local repo with the latest changes from GitHub. If there are no conflicts, it will automatically apply your local commits on top of the remote ones.
```
git push origin main
```
If this works - awesome. If it doesn't, continue to step 3.
If git pull gives a merge conflict, Git will tell you which files are affected. To fix this:
  - open each file and fix the errors
  - once you fix that we will stage the files
```
git add .
```
Commit the merge resolution:
```
git commit -m "Resolved merge conflicts"
```
Push the changes:
```
git push origin main
```
yay! it should work now

---

If You Just Want to Force Push (⚠️ Warning: This will overwrite any changes on GitHub that you don’t have locally):
  - If you are sure your local version is correct and you want to overwrite the remote repo, use:
```
git push --force origin main
```

---

If ``` git pull origin main ``` is giving you a fatal error, it could be due to one of these issues:
1.) Check If Your Remote Is Set Up Correctly:
```
git remote -v
```
If it doesn’t show your GitHub repo, add it manually:
```
git remote add origin https://github.com/<your-username>/CLI-test.git
```
2. Check If Your Branch Exists on GitHub:
```
git branch -a
```
If you only see master and not main, rename your branch:
```
git branch -m master main
git push -u origin main
```
3. Try Pulling Without Rebasing
If your remote and branch are correct but git pull origin main --rebase failed, try:
```
git pull origin main --allow-unrelated-histories
```
This forces Git to merge different histories (useful if you created the repo manually on GitHub).

4. If Pull Still Fails, Try a Hard Reset (⚠️ This Will Overwrite Local Changes!)
If you're okay losing local changes that aren’t committed yet, reset to match the remote repo:
```
git fetch origin
git reset --hard origin/main
git pull origin main
```
5. Finally, Push Your Changes
Once your repo is synced, try pushing again:
```
git push origin main
```

---

### Why Did This Happen?

Your local repo initially had master as the branch name. You renamed master to main using: ```git branch -m master main``` When you pushed, GitHub didn’t overwrite master — it just created a new main branch. Since both master and main existed, GitHub assumed you wanted to merge them, hence the pull request (PR) suggestion.

How to Fix It?
Option 1: Delete master on GitHub (if it's empty or unused) - (only do this after commiting to main and merging)
If master is empty or unnecessary, delete it from GitHub:
```
git push origin --delete master
```
Then, set main as the default branch on GitHub.
Option 2: Merge master into main via GitHub’s PR
- If master has important commits, merge the pull request suggested by GitHub. After merging, you can delete master from GitHub.
