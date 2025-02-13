# Methodology

### Enumeration

```powershell
# Download a .git directory from a website
git-dumper http://example.com/.git/ example.com

# Check repo status
git status

# Compare current with previous commits
git diff
git diff --staged

# Display branches
git branch

# Change branches
git switch $BRANCH

# Show commits within the current branch
git log

# Show changes related to a specified commit
git show $COMMIT_ID
```
