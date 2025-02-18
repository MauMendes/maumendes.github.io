---
layout: post
title: "Bitbucket-GitHub Migration"
date: 2025-02-17 19:18:00 +0200
author: mauricio
categories: [introduction]
tags: [github, bitbucket, bash]
---

A few weeks ago, a friend came to me with a concern that Bitbucket had started to change their policies and even left some of his workspaces public. So, he asked me if I could help with migrating his repositories from Bitbucket to GitHub. 

He had more than 100 repositories, so this had to be automated.

So the script bellow does the following:

- **Clone the repositories** from Bitbucket
- **Create new repositories** on GitHub
- **Push the repositories** to GitHub

Here’s the full Bash script I used to migrate my repositories from Bitbucket to GitHub:

```bash
#!/bin/bash

# GITHUB_ORG="example_org"
# GITHUB_TOKEN="example_token"

# List of repositories with only Bitbucket URLs (example repositories)
REPO_LIST=(
    "https://bitbucket.org/fargo3d/public.git"  # example public repository from bitbucket
    # "https://username@bitbucket.org/bitbucket_user/repo1.git"
    # "https://username@bitbucket.org/bitbucket_user/repo2.git"
)

set -e  # Stop execution on any error
trap 'echo "ERROR at line $LINENO: Command failed with exit code $?"; read -p "Press Enter to continue..."' ERR

for BITBUCKET_URL in "${REPO_LIST[@]}"; do
    # Add "bitbucket_" prefix to the repository name in one line
    REPO_NAME="bitbucket_$(basename "$BITBUCKET_URL" .git)"

    # Check if repository exists on GitHub
    echo "Checking if repository $REPO_NAME exists on GitHub..."
    HTTP_STATUS=$(curl -s -o /dev/null -w "%{http_code}" \
        -H "Authorization: token $GITHUB_TOKEN" \
        "https://api.github.com/repos/$GITHUB_ORG/$REPO_NAME")

    if [ "$HTTP_STATUS" -eq 200 ]; then
        echo "Repository $REPO_NAME already exists on GitHub."
    else
        echo "Creating repository $REPO_NAME on GitHub..."
        CREATE_REPO_RESPONSE=$(curl -s -X POST \
            -H "Accept: application/vnd.github.v3+json" \
            -H "Authorization: token $GITHUB_TOKEN" \
            "https://api.github.com/orgs/$GITHUB_ORG/repos" \
            -d "{\"name\":\"$REPO_NAME\", \"private\":true}")

        if echo "$CREATE_REPO_RESPONSE" | grep -q "created_at"; then
            echo "Repository $REPO_NAME successfully created!"
        else
            echo "ERROR: Failed to create repository $REPO_NAME on GitHub."
            echo "GitHub Response: $CREATE_REPO_RESPONSE"
            read -p "Press Enter to exit..."
            exit 1
        fi
    fi

    # Clone the Bitbucket repository as a bare repository
    echo "Cloning repository from Bitbucket..."
    git clone --bare "$BITBUCKET_URL"
    
    # Extract the directory name after cloning
    CLONED_DIR=$(basename "$BITBUCKET_URL" .git).git
    if [ ! -d "$CLONED_DIR" ]; then
        echo "ERROR: Directory $CLONED_DIR does not exist after cloning."
        exit 1
    fi

    # Move into the cloned directory
    cd "$CLONED_DIR"

    # Setup remote and push to GitHub
    GITHUB_URL="https://github.com/$GITHUB_ORG/$REPO_NAME.git"    
    git remote set-url origin "$GITHUB_URL"
    git push --mirror

    # Remove the temp cloned folder
    cd ..
    rm -rf "$CLONED_DIR"

    echo "Sync for $REPO_NAME completed!"
done

echo "All repositories have been synchronized!"
read -p "Press Enter to exit..."
```


Happy Migrating.
