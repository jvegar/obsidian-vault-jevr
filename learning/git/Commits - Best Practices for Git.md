## Good commit vs Bad commit

### What is a commit?
In Git, a commit is referring to the ==state of your code at one specific point in time==. Commits with the metadata (author, timestamp, commit message etc). Commits are used for saving progression, stating changes and merging developed pieces with others work.

## Characteristics of a Good Commit
#### Atomic and focused
A commit should be atomic - it has to represent ***one and only one logical change***. Do not mix several independent changes in one commit.  
Example:
```sh
# Good commit
git commit -m "Add user authentication"
# Bad commit
git commit -m "Add user authentication and update UI styles"
```

#### Descriptive Commit Message
A descriptive commit message clearly explains what the commit does and why the change was made. It should provide enough context for others (and your future self) to understand the change without reading the code.  
Example:
```sh
# Good commit message
git commit -m "Fix Correct null pointer exception in user login"
# Bad commit message
git commit -m "Fix bug"
```

#### Follow Conventional Commit Guidelines
You can use the standard commit guidelines to keep your git history clean, consistent and easy to read. Usually these guidelines interpret in the form of type (***feat, fix, chore, refactor, docs***), and ***short summary*** plus occasionally a long explanation or REF to other relative issues.  
Example:
```sh
# Good commit message following conventional guidelines
git commit -m "feat(auth): add JWT-based authentication"
git commit -m "fix(login): resolve race condition in login flow"
```

#### Tested and verified
Make sure that the changes in your commit have been tested, and properly run. Broken/untested code can disturb the flow and other members.  
**Properly Scoped:** Scope your commits appropriately. For instance, if you’re working on a specific feature or fixing a bug, ensure that all changes related to that task are included in a single commit. Avoid partial changes that might leave the codebase in an inconsistent state.  
Example:
```sh
# Good commit with proper scope
git commit -m "refactor(auth): split auth logic into separate module"
# Bad commit with mixed scope
git commit -m "refactor and minor fixes"
```


## Characteristics of a Bad Commit
#### Large and Unfocused
A commit with too many changes is a bad commit. It makes difficult to understand what the commit does. Large, unfocused commits are challenging to review and debug.  
Example:
```sh
# Bad commit
git commit -m "Update project"
```

#### Vague or Misleading Messages
Commit messages that are vague or misleading do not provide useful information about the changes. This lack of detail can cause confusion and make it hard to track the history of changes.  
Example:
```sh
# Bad commit message
git commit -m "Stuff"
```

#### Unrelated Changes
Combining unrelated changes into a single commit makes it difficult to isolate specific changes, potentially introducing bugs and complicating the review process.  
Example:
```sh
# Bad commit
git commit -m "Update readme and fix login issue"
```

#### Incomplete or Untested Code
Committing incomplete or untested code can disrupt the workflow, causing issues for other team members and potentially breaking the build.

#### Lack of Context
A bad commit often lacks context, making it hard to understand why a change was made. This can lead to confusion and difficulties when revisiting the code in the future.

## Best practices for Good Commits

1. **Commit Often, but Not Too Often:** Strive for a ==balance== between committing ==too frequently== and ==not enough==. Each commit should represent a meaningful change. Never push unrelated changes in a single commit.
2. **Write Clear and Descriptive Messages:** Your commit messages should be explaining what the commit does and why you made the change.
3. **Use Branches Effectively:** Use feature branches for new features, bug fixes, and experiments. Raise Pull Requests for those branches and project manager or admin will review you code and merge them into the main.
4. **Review and Squash Commits:** If you are project owner, leader, admin or someone reviewing the code, before merging a branch, review and squash small or fixup commits into logical units. This practice keeps the commit history clean and easy to follow.
5. **Automate Testing:** Use continuous integration tools to automatically test your code with each commit. This ensures that your changes are verified and reduces the risk of introducing bugs.
6. **Use Husky:** Using a library like husky can improve you git skills. It does not allow the commit if you are breaking the rules configured in husky.