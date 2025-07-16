git add .
git commit -m "Your commit message"

git checkout main

git pull origin main

git checkout your-feature-branch
git rebase main

git checkout main
git merge your-feature-branch

git push origin main

# Delete local branch
git branch -d your-feature-branch

# Delete remote branch
git push origin --delete your-feature-branch