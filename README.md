# my-private-repo version 2.0

This is a private repository for testing orphan branch.

## Create private repo for development


## Add public remote for release
```bash
git remote add public git@github.com:user_name/my-public-repo.git    
```

## Create orphan branch for release without history
```bash
git checkout --orphan release-branch
```

## List remote after adding public repo
```bash
~ git remote -v                                                  
origin  git@github.com:user_name/my-private-repo.git (fetch)
origin  git@github.com:user_name/my-private-repo.git (push)
public  git@github.com:user_name/my-public-repo.git (fetch)
public  git@github.com:user_name/my-public-repo.git (push)
```

## Release Process

To push the release code to the public remote using an orphan branch without private repo's commit history, follow these steps:

## Option 1. Update Public Repo (keeping public repo's commit history)

To update the public repository with the latest changes from the private repository, maintaining the separate history:

1. **Switch to release branch**:
   ```bash
   git checkout release-branch
   ```

2. **Sync content from main**:
   Bring in the latest files from the main branch without merging the commit history.
   ```bash
   git checkout main -- .
   git add .
   git commit -m "Update public release"
   ```

3. **Push updates**:
   ```bash
   git push public release-branch:main
   ```

## Option 2. Reset Public Repo (reset public repo's commit history)

1. **Delete release-branch**:
   ```bash
   git branch -D release-branch
   ```

2. **Create release-branch with --orphan**:
   ```bash
   git checkout --orphan release-branch
   ```

3. **Add files and commit**:
   ```bash
   git add .
   git commit -m "Clean history release v1.2"
   ```

4. **Push updates**:
   ```bash
   git push public release-branch:main --force
   ```

## Diagram
```mermaid
graph TD
    %% Node Definitions with manual line breaks
    Start("<b>Work Completed</b><br/>on Private main branch") --> Choice{Choose Update<br/>Method}

    subgraph Option_A [Option A: Re-create]
        Choice -->|Fresh Start| A1["<b>1. Delete Old Local Branch</b><br/>git branch -D release-branch"]
        A1 --> A2["<b>2. Create New Orphan</b><br/>git checkout --orphan release-branch"]
        A2 --> A3["<b>Result:</b><br/>Exactly 1 Clean Commit"]
    end

    subgraph Option_B [Option B: Keep & Update]
        Choice -->|Incremental| B1["<b>1. Switch to Release</b><br/>git checkout release-branch"]
        B1 --> B2["<b>2. Sync Files Only</b><br/>git checkout main -- ."]
        B2 --> B3["<b>Result:</b><br/>Clean Release History<br/>(v1, v2, v3...)"]
    end

    A3 --> Push["<b>Final Step: Force Push</b><br/>git push public <br/>release-branch:main <br/>--force"]
    B3 --> Push
    Push --> Final((<b>Public Repo<br/>Updated!</b>))

    %% Styling for better visibility
    style Option_A fill:#fff4dd,stroke:#d4a017,stroke-width:2px
    style Option_B fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    style Final fill:#dcedc8,stroke:#33691e,stroke-width:3px
    style Start fill:#f5f5f5,stroke:#333
```