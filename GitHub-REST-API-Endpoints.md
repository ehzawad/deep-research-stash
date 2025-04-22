# GitHub REST API Endpoints (2022-11-28)

Below is a comprehensive list of GitHub REST API endpoints (version **2022-11-28**), grouped by category. Each endpoint includes the HTTP method, the endpoint path, a sample cURL request (with placeholders like `octocat` for user and `Hello-World` for repo), and a sample JSON response.

## Repositories
- **List User Repositories** (`GET /users/{username}/repos`)  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/users/octocat/repos"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 1296269,
        "name": "Hello-World",
        "full_name": "octocat/Hello-World",
        "owner": {
          "login": "octocat",
          "id": 1,
          ...
        },
        "private": false,
        "html_url": "https://github.com/octocat/Hello-World",
        "description": "This is your first repo!",
        ...
      }
    ]
    ```

- **Get a Repository** (`GET /repos/{owner}/{repo}`)  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 1296269,
      "name": "Hello-World",
      "full_name": "octocat/Hello-World",
      "owner": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "private": false,
      "description": "This is your first repo!",
      "fork": false,
      "created_at": "2011-01-26T19:01:12Z",
      "updated_at": "2023-03-28T14:45:32Z",
      "pushed_at": "2023-03-28T14:45:31Z",
      "size": 108,
      "stargazers_count": 80,
      "watchers_count": 80,
      "language": "Ruby",
      ...
    }
    ```

- **Create a Repository** (`POST /user/repos`)  
    Create a new repository for the authenticated user (use `POST /orgs/{org}/repos` to create in an organization).  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"name": "NewRepo", "description": "My new repository", "private": false}' \
         "https://api.github.com/user/repos"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 123456789,
      "name": "NewRepo",
      "full_name": "octocat/NewRepo",
      "private": false,
      "description": "My new repository",
      "owner": {
        "login": "octocat",
        ...
      },
      "created_at": "2025-04-22T15:00:00Z",
      ...
    }
    ```

- **Update a Repository** (`PATCH /repos/{owner}/{repo}`)  
    Modify repository settings (e.g., change the description or make it private).  
    **Example cURL:**  
    ```shell
    curl -X PATCH -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"description": "Updated description"}' \
         "https://api.github.com/repos/octocat/Hello-World"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 1296269,
      "name": "Hello-World",
      "full_name": "octocat/Hello-World",
      "description": "Updated description",
      "private": false,
      ...
    }
    ```

- **Delete a Repository** (`DELETE /repos/{owner}/{repo}`)  
    Permanently delete a repository. **Note:** This action is irreversible.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/OldRepo"
    ```  
    **Sample Response:** A successful deletion returns `204 No Content` (no JSON response).

## Branches
- **List Branches** (`GET /repos/{owner}/{repo}/branches`)  
    Get all branches in a repository.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/branches"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "name": "main",
        "commit": {
          "sha": "6dcb09b... (SHA)",
          "url": "https://api.github.com/repos/octocat/Hello-World/commits/6dcb09..."
        },
        "protected": true
      },
      {
        "name": "development",
        "commit": {
          "sha": "aa218f... (SHA)",
          "url": "https://api.github.com/repos/octocat/Hello-World/commits/aa218f..."
        },
        "protected": false
      }
    ]
    ```

- **Get a Branch** (`GET /repos/{owner}/{repo}/branches/{branch}`)  
    Retrieve details for a specific branch, including its commit SHA and protection status.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/branches/main"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "name": "main",
      "commit": {
        "sha": "6dcb09b...",
        "url": "https://api.github.com/repos/octocat/Hello-World/commits/6dcb09..."
      },
      "protected": true,
      "protection": {
        "required_status_checks": {
          "enforcement_level": "everyone",
          "contexts": ["ci-test"]
        },
        "enforce_admins": {"enabled": true},
        "required_pull_request_reviews": {
          "required_approving_review_count": 1,
          ...
        },
        ...
      }
    }
    ```

- **Update Branch Protection** (`PUT /repos/{owner}/{repo}/branches/{branch}/protection`)  
    Protect a branch by requiring status checks, reviews, or restrictions before merging. Requires admin access.  
    **Example cURL:**  
    ```shell
    curl -X PUT -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ 
               "required_pull_request_reviews": { "required_approving_review_count": 1 },
               "enforce_admins": true,
               "required_status_checks": null,
               "restrictions": null
             }' \
         "https://api.github.com/repos/octocat/Hello-World/branches/main/protection"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "url": "https://api.github.com/repos/octocat/Hello-World/branches/main/protection",
      "required_pull_request_reviews": { "required_approving_review_count": 1, ... },
      "enforce_admins": { "enabled": true },
      "required_status_checks": null,
      "restrictions": null
    }
    ```

## Collaborators
- **List Collaborators** (`GET /repos/{owner}/{repo}/collaborators`)  
    List all users with access to a repository and their permissions.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/collaborators"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "login": "octocat",
        "id": 1,
        "permissions": {
          "admin": false,
          "push": true,
          "pull": true
        }
      }
    ]
    ```

- **Add a Collaborator** (`PUT /repos/{owner}/{repo}/collaborators/{username}`)  
    Invite a user to collaborate on a repository. (If the user accepts, they become a collaborator.)  
    **Example cURL:**  
    ```shell
    curl -X PUT -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         "https://api.github.com/repos/octocat/Hello-World/collaborators/monalisa"
    ```  
    **Sample JSON Response:** *(If an invitation is created)*  
    ```json
    {
      "id": 987654321,
      "invitee": {
        "login": "monalisa",
        "id": 2,
        ...
      },
      "inviter": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "permissions": "write",
      "created_at": "2025-04-22T15:10:00Z",
      ...
    }
    ```
    *(If the user is already a collaborator, the response is `204 No Content`.)*

- **Remove a Collaborator** (`DELETE /repos/{owner}/{repo}/collaborators/{username}`)  
    Remove a user's access to the repository.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/collaborators/monalisa"
    ```  
    **Sample Response:** A successful removal returns `204 No Content`.

## Commits
- **List Commits** (`GET /repos/{owner}/{repo}/commits`)  
    List recent commits on a repository. You can filter by branch, path, author, etc.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/commits"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "sha": "6dcb09b5b57875f334f61aebed695e2e4193db5e",
        "commit": {
          "author": {
            "name": "Monalisa Octocat",
            "email": "mona@github.com",
            "date": "2011-04-14T16:00:49Z"
          },
          "message": "Fix all the bugs",
          ...
        },
        "author": {
          "login": "octocat",
          "id": 1,
          ...
        },
        "html_url": "https://github.com/octocat/Hello-World/commit/6dcb09b5b57875f334f61aebed695e2e4193db5e",
        ...
      }
    ]
    ```

- **Get a Commit** (`GET /repos/{owner}/{repo}/commits/{ref}`)  
    Retrieve a specific commit by SHA (or branch/tag name). Shows commit message, author, diff stats, etc.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/commits/6dcb09b5b57875f334f61aebed695e2e4193db5e"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "sha": "6dcb09b5b57875f334f61aebed695e2e4193db5e",
      "commit": {
        "author": {
          "name": "Monalisa Octocat",
          "email": "mona@github.com",
          "date": "2011-04-14T16:00:49Z"
        },
        "committer": {
          "name": "Monalisa Octocat",
          "email": "mona@github.com",
          "date": "2011-04-14T16:00:49Z"
        },
        "message": "Fix all the bugs",
        "tree": {
          "sha": "827efc6d56897b048c772eb4087f854f46256132",
          "url": "https://api.github.com/repos/octocat/Hello-World/git/trees/827efc6..."
        },
        ...
      },
      "author": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "stats": {
        "additions": 104,
        "deletions": 4,
        "total": 108
      },
      "files": [
        {
          "filename": "bugfix.txt",
          "additions": 10,
          "deletions": 2,
          "changes": 12,
          "status": "modified",
          "raw_url": "https://github.com/octocat/Hello-World/raw/...",
          ...
        }
      ]
    }
    ```

- **Compare Two Commits** (`GET /repos/{owner}/{repo}/compare/{base}...{head}`)  
    Compare differences between two commits (for example, between two branches or tags).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/compare/main...feature-branch"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "status": "ahead",
      "ahead_by": 5,
      "behind_by": 0,
      "total_commits": 5,
      "commits": [ ... ],
      "files": [ ... ]
    }
    ```

- **List Commit Comments** (`GET /repos/{owner}/{repo}/comments`)  
    List all commit comments in the repository (comments on any commit).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/comments"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 1,
        "body": "Great fix!",
        "user": {
          "login": "octocat",
          "id": 1,
          ...
        },
        "commit_id": "6dcb09b5b57875f334f61aebed695e2e4193db5e",
        "created_at": "2011-04-14T16:00:49Z",
        ...
      }
    ]
    ```

- **Create a Commit Comment** (`POST /repos/{owner}/{repo}/commits/{sha}/comments`)  
    Add a new comment to a specific commit (by SHA).  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"body": "This change looks good"}' \
         "https://api.github.com/repos/octocat/Hello-World/commits/6dcb09b5b57875f334f61aebed695e2e4193db5e/comments"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 2,
      "body": "This change looks good",
      "user": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "commit_id": "6dcb09b5b57875f334f61aebed695e2e4193db5e",
      "created_at": "2025-04-22T15:20:00Z",
      ...
    }
    ```

- **Create a Commit Status** (`POST /repos/{owner}/{repo}/statuses/{sha}`)  
    Set the status of a commit (e.g., success, failure) for CI or other checks.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "state": "success", "context": "ci/test", "description": "Tests passed" }' \
         "https://api.github.com/repos/octocat/Hello-World/statuses/6dcb09b5b57875f334f61aebed695e2e4193db5e"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 123456,
      "state": "success",
      "description": "Tests passed",
      "context": "ci/test",
      "creator": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "created_at": "2025-04-22T15:25:00Z",
      ...
    }
    ```

## Releases
- **List Releases** (`GET /repos/{owner}/{repo}/releases`)  
    List all releases (published or draft) in the repository.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/releases"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 1,
        "tag_name": "v1.0.0",
        "name": "v1.0.0",
        "draft": false,
        "prerelease": false,
        "created_at": "2013-02-27T19:35:32Z",
        "published_at": "2013-02-27T19:35:32Z",
        "author": {
          "login": "octocat",
          "id": 1,
          ...
        },
        "body": "Description of the release",
        ...
      }
    ]
    ```

- **Get a Release** (`GET /repos/{owner}/{repo}/releases/{release_id}`)  
    Get information for a single release by ID. (Alternatively, use `GET /repos/{owner}/{repo}/releases/latest` for the latest release.)  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/releases/1"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 1,
      "tag_name": "v1.0.0",
      "name": "v1.0.0",
      "draft": false,
      "prerelease": false,
      "created_at": "2013-02-27T19:35:32Z",
      "published_at": "2013-02-27T19:35:32Z",
      "author": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "assets": [
        {
          "id": 1,
          "name": "example.zip",
          "content_type": "application/zip",
          "download_count": 42,
          "browser_download_url": "https://github.com/octocat/Hello-World/releases/download/v1.0.0/example.zip",
          ...
        }
      ],
      "body": "Description of the release",
      ...
    }
    ```

- **Create a Release** (`POST /repos/{owner}/{repo}/releases`)  
    Publish a new release. Provide a tag, and optionally a title and description.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"tag_name": "v1.0.0", "name": "v1.0.0", "body": "Initial release", "draft": false, "prerelease": false}' \
         "https://api.github.com/repos/octocat/Hello-World/releases"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 2,
      "tag_name": "v1.0.0",
      "name": "v1.0.0",
      "draft": false,
      "prerelease": false,
      "created_at": "2025-04-22T15:30:00Z",
      "published_at": "2025-04-22T15:30:00Z",
      "author": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "upload_url": "https://uploads.github.com/repos/octocat/Hello-World/releases/2/assets{?name,label}",
      "body": "Initial release",
      ...
    }
    ```

- **Delete a Release** (`DELETE /repos/{owner}/{repo}/releases/{release_id}`)  
    Delete a release. (This does not delete the associated Git tag.)  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/releases/2"
    ```  
    **Sample Response:** Returns `204 No Content` on successful deletion.

## Issues
- **List Repository Issues** (`GET /repos/{owner}/{repo}/issues`)  
    List all issues (open and closed) in a repository. Can be filtered by state, labels, etc via query params.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/issues"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 1,
        "number": 1347,
        "state": "open",
        "title": "Found a bug",
        "body": "I'm having a problem with this.",
        "user": {
          "login": "octocat",
          "id": 1,
          ...
        },
        "labels": [
          {
            "id": 208045946,
            "name": "bug",
            "description": "Something isn't working",
            ...
          }
        ],
        "comments": 0,
        "created_at": "2011-04-22T13:33:48Z",
        ...
      }
    ]
    ```

- **Create an Issue** (`POST /repos/{owner}/{repo}/issues`)  
    Open a new issue in a repository. You can specify title, body, assignees, labels, etc.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"title": "Bug report", "body": "I found an issue.", "assignees": ["octocat"]}' \
         "https://api.github.com/repos/octocat/Hello-World/issues"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 2,
      "number": 1348,
      "state": "open",
      "title": "Bug report",
      "body": "I found an issue.",
      "user": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "labels": [],
      "comments": 0,
      "created_at": "2025-04-22T15:40:00Z",
      ...
    }
    ```

- **Get an Issue** (`GET /repos/{owner}/{repo}/issues/{issue_number}`)  
    Retrieve a single issue by number, including its current state and details.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/issues/1347"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 1,
      "number": 1347,
      "state": "open",
      "title": "Found a bug",
      "body": "I'm having a problem with this.",
      "user": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "labels": [
        {
          "name": "bug",
          ...
        }
      ],
      "comments": 0,
      "created_at": "2011-04-22T13:33:48Z",
      ...
    }
    ```

- **Update an Issue** (`PATCH /repos/{owner}/{repo}/issues/{issue_number}`)  
    Modify an issue's title, body, state, or other fields (e.g., close an issue by setting `state` to `closed`).  
    **Example cURL:**  
    ```shell
    curl -X PATCH -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"state": "closed", "title": "Found a bug (resolved)"}' \
         "https://api.github.com/repos/octocat/Hello-World/issues/1347"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 1,
      "number": 1347,
      "state": "closed",
      "title": "Found a bug (resolved)",
      "body": "I'm having a problem with this.",
      ...
    }
    ```

- **Lock an Issue** (`PUT /repos/{owner}/{repo}/issues/{issue_number}/lock`)  
    Lock an issue conversation (for example, to prevent further comments).  
    **Example cURL:**  
    ```shell
    curl -X PUT -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"lock_reason": "resolved"}' \
         "https://api.github.com/repos/octocat/Hello-World/issues/1347/lock"
    ```  
    **Sample Response:** Returns `204 No Content` if locking is successful.

- **Unlock an Issue** (`DELETE /repos/{owner}/{repo}/issues/{issue_number}/lock`)  
    Unlock a previously locked issue.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/issues/1347/lock"
    ```  
    **Sample Response:** Returns `204 No Content` if unlocking is successful.

- **List Issue Comments** (`GET /repos/{owner}/{repo}/issues/{issue_number}/comments`)  
    Get all comments on a specific issue.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/issues/1347/comments"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 1000,
        "body": "Any update on this issue?",
        "user": {
          "login": "monalisa",
          "id": 2,
          ...
        },
        "created_at": "2025-04-22T15:45:00Z",
        ...
      }
    ]
    ```

- **Create an Issue Comment** (`POST /repos/{owner}/{repo}/issues/{issue_number}/comments`)  
    Add a new comment to an issue.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"body": "We are looking into this."}' \
         "https://api.github.com/repos/octocat/Hello-World/issues/1347/comments"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 1001,
      "body": "We are looking into this.",
      "user": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "created_at": "2025-04-22T15:50:00Z",
      ...
    }
    ```

- **List Labels** (`GET /repos/{owner}/{repo}/labels`)  
    List all labels in a repository.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/labels"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 208045946,
        "name": "bug",
        "color": "f29513",
        "description": "Something isn't working",
        ...
      }
    ]
    ```

- **Create a Label** (`POST /repos/{owner}/{repo}/labels`)  
    Create a new label in the repository.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"name": "feature", "color": "0e8a16", "description": "New feature"}' \
         "https://api.github.com/repos/octocat/Hello-World/labels"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 987654321,
      "name": "feature",
      "color": "0e8a16",
      "description": "New feature",
      ...
    }
    ```

- **Add Labels to an Issue** (`POST /repos/{owner}/{repo}/issues/{issue_number}/labels`)  
    Add one or more labels to an issue.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '["bug", "feature"]' \
         "https://api.github.com/repos/octocat/Hello-World/issues/1347/labels"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {"id": 208045946, "name": "bug", ...},
      {"id": 987654321, "name": "feature", ...}
    ]
    ```

- **Remove a Label from an Issue** (`DELETE /repos/{owner}/{repo}/issues/{issue_number}/labels/{name}`)  
    Remove a specific label from an issue.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/issues/1347/labels/bug"
    ```  
    **Sample Response:** Returns `204 No Content` on success (issue no longer has that label).

- **List Milestones** (`GET /repos/{owner}/{repo}/milestones`)  
    List all milestones in a repository.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/milestones"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 1002604,
        "number": 1,
        "title": "v1.0",
        "state": "open",
        "open_issues": 2,
        "closed_issues": 5,
        "due_on": "2025-05-01T00:00:00Z",
        ...
      }
    ]
    ```

- **Create a Milestone** (`POST /repos/{owner}/{repo}/milestones`)  
    Create a new milestone (with title, due date, etc.).  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"title": "v2.0", "description": "Next version", "due_on": "2025-12-31T23:59:59Z"}' \
         "https://api.github.com/repos/octocat/Hello-World/milestones"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 11223344,
      "number": 2,
      "title": "v2.0",
      "state": "open",
      "open_issues": 0,
      "closed_issues": 0,
      "due_on": "2025-12-31T23:59:59Z",
      ...
    }
    ```

- **Add Assignees to an Issue** (`POST /repos/{owner}/{repo}/issues/{issue_number}/assignees`)  
    Assign one or more users to an issue.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"assignees": ["monalisa"]}' \
         "https://api.github.com/repos/octocat/Hello-World/issues/1347/assignees"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 1,
      "number": 1347,
      "assignees": [
        {
          "login": "monalisa",
          "id": 2,
          ...
        }
      ],
      ...
    }
    ```

- **Remove Assignees from an Issue** (`DELETE /repos/{owner}/{repo}/issues/{issue_number}/assignees`)  
    Remove one or more assignees from an issue.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"assignees": ["monalisa"]}' \
         "https://api.github.com/repos/octocat/Hello-World/issues/1347/assignees"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 1,
      "number": 1347,
      "assignees": [],
      ...
    }
    ```
## Pull Requests
- **List Pull Requests** (`GET /repos/{owner}/{repo}/pulls`)  
    List pull requests in a repository (open by default, can filter state).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/pulls"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 42,
        "number": 42,
        "state": "open",
        "title": "Improve documentation",
        "user": {
          "login": "monalisa",
          "id": 2,
          ...
        },
        "head": {
          "ref": "feature-branch",
          "sha": "d1e9f... (SHA)",
          ...
        },
        "base": {
          "ref": "main",
          "sha": "6dcb09... (SHA)",
          ...
        },
        ...
      }
    ]
    ```

- **Get a Pull Request** (`GET /repos/{owner}/{repo}/pulls/{pull_number}`)  
    Retrieve details of a specific PR, including branch info and merge status.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/pulls/42"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 42,
      "number": 42,
      "state": "open",
      "title": "Improve documentation",
      "body": "Updates to README and docs.",
      "user": {
        "login": "monalisa",
        "id": 2,
        ...
      },
      "head": {
        "label": "monalisa:feature-branch",
        "ref": "feature-branch",
        "sha": "d1e9f...",
        ...
      },
      "base": {
        "label": "octocat:main",
        "ref": "main",
        "sha": "6dcb09...",
        ...
      },
      "merged": false,
      "mergeable": true,
      "changed_files": 3,
      ...
    }
    ```

- **Create a Pull Request** (`POST /repos/{owner}/{repo}/pulls`)  
    Create a new pull request. Requires a head branch (from a fork or same repo) and a base branch.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"title": "New Feature", "head": "feature-branch", "base": "main", "body": "Please merge my feature"}' \
         "https://api.github.com/repos/octocat/Hello-World/pulls"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 100,
      "number": 100,
      "state": "open",
      "title": "New Feature",
      "body": "Please merge my feature",
      "head": {
        "ref": "feature-branch",
        "sha": "abcd1234...",
        ...
      },
      "base": {
        "ref": "main",
        "sha": "6dcb09...",
        ...
      },
      "merged": false,
      "mergeable": true,
      ...
    }
    ```

- **Update a Pull Request** (`PATCH /repos/{owner}/{repo}/pulls/{pull_number}`)  
    Modify PR information (e.g., title, body, or reopen a closed PR by setting `state`).  
    **Example cURL:**  
    ```shell
    curl -X PATCH -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"title": "Improve documentation (updated)"}' \
         "https://api.github.com/repos/octocat/Hello-World/pulls/42"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 42,
      "number": 42,
      "state": "open",
      "title": "Improve documentation (updated)",
      ...
    }
    ```

- **Merge a Pull Request** (`PUT /repos/{owner}/{repo}/pulls/{pull_number}/merge`)  
    Merge an open pull request. You can specify the commit message and merge method (`merge`, `squash`, or `rebase`).  
    **Example cURL:**  
    ```shell
    curl -X PUT -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "commit_title": "Merge PR #42", "merge_method": "merge" }' \
         "https://api.github.com/repos/octocat/Hello-World/pulls/42/merge"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "sha": "ec26c3...",
      "merged": true,
      "message": "Pull Request successfully merged"
    }
    ```

- **List Pull Request Commits** (`GET /repos/{owner}/{repo}/pulls/{pull_number}/commits`)  
    List all commits included in a pull request.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/pulls/42/commits"
    ```  
    **Sample JSON Response:** (array of commit objects similar to repository commits)

- **List Pull Request Files** (`GET /repos/{owner}/{repo}/pulls/{pull_number}/files`)  
    List all changed files in a pull request (filename, additions/deletions).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/pulls/42/files"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "filename": "README.md",
        "additions": 10,
        "deletions": 2,
        "changes": 12,
        "status": "modified",
        "patch": "@@ ... @@",
        ...
      }
    ]
    ```

- **List PR Reviews** (`GET /repos/{owner}/{repo}/pulls/{pull_number}/reviews`)  
    List all review events on a pull request (each review may include comments and a state like approved or changes_requested).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/pulls/42/reviews"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 80,
        "user": {
          "login": "octocat",
          "id": 1,
          ...
        },
        "body": "Looks good to me",
        "state": "APPROVED",
        "submitted_at": "2025-04-22T16:00:00Z",
        ...
      }
    ]
    ```

- **Create/Submit a Review** (`POST /repos/{owner}/{repo}/pulls/{pull_number}/reviews`)  
    Submit a review for a pull request (approve, request changes, or comment).  
    **Example cURL (Approve PR):**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "event": "APPROVE", "body": "LGTM!" }' \
         "https://api.github.com/repos/octocat/Hello-World/pulls/42/reviews"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 81,
      "user": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "body": "LGTM!",
      "state": "APPROVED",
      "submitted_at": "2025-04-22T16:05:00Z",
      ...
    }
    ```

- **List Review Comments** (`GET /repos/{owner}/{repo}/pulls/{pull_number}/comments`)  
    List comments on the pull request diff (code review comments).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/pulls/42/comments"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 100,
        "user": {
          "login": "monalisa",
          "id": 2,
          ...
        },
        "body": "Please update this line.",
        "path": "README.md",
        "position": 5,
        "commit_id": "d1e9f...",
        ...
      }
    ]
    ```

- **Create a Review Comment** (`POST /repos/{owner}/{repo}/pulls/{pull_number}/comments`)  
    Create a comment on a specific line in the diff of a pull request. Requires the file path and position or a commit SHA and line.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "body": "Needs clarification.", "commit_id": "d1e9f...", "path": "README.md", "position": 5 }' \
         "https://api.github.com/repos/octocat/Hello-World/pulls/42/comments"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 101,
      "user": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "body": "Needs clarification.",
      "path": "README.md",
      "position": 5,
      "commit_id": "d1e9f...",
      ...
    }
    ```

- **Request Reviewers** (`POST /repos/{owner}/{repo}/pulls/{pull_number}/requested_reviewers`)  
    Ask specific users or teams to review a PR.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "reviewers": ["monalisa"] }' \
         "https://api.github.com/repos/octocat/Hello-World/pulls/42/requested_reviewers"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 42,
      "number": 42,
      "requested_reviewers": [
        {
          "login": "monalisa",
          "id": 2,
          ...
        }
      ],
      ...
    }
    ```

- **Remove Requested Reviewers** (`DELETE /repos/{owner}/{repo}/pulls/{pull_number}/requested_reviewers`)  
    Remove one or more requested reviewers from a PR.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "reviewers": ["monalisa"] }' \
         "https://api.github.com/repos/octocat/Hello-World/pulls/42/requested_reviewers"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 42,
      "number": 42,
      "requested_reviewers": [],
      ...
    }
    ```
## Actions (CI/CD Workflows)
- **List Workflows** (`GET /repos/{owner}/{repo}/actions/workflows`)  
    List all GitHub Actions workflows in the repository.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/workflows"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "total_count": 2,
      "workflows": [
        {
          "id": 161234,
          "name": "CI",
          "state": "active",
          "path": ".github/workflows/ci.yml",
          "created_at": "2021-01-01T00:00:00Z",
          "updated_at": "2021-05-01T00:00:00Z",
          ...
        },
        ...
      ]
    }
    ```

- **Get a Workflow** (`GET /repos/{owner}/{repo}/actions/workflows/{workflow_id}`)  
    Fetch metadata about a specific workflow by ID or file name.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/workflows/161234"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 161234,
      "node_id": "MDg6V29ya2Zsb3cxNjEyMzQ=",
      "name": "CI",
      "path": ".github/workflows/ci.yml",
      "state": "active",
      "created_at": "2021-01-01T00:00:00Z",
      "updated_at": "2021-05-01T00:00:00Z",
      "url": "https://api.github.com/repos/octocat/Hello-World/actions/workflows/161234",
      ...
    }
    ```

- **Trigger a Workflow (Dispatch)** (`POST /repos/{owner}/{repo}/actions/workflows/{workflow_id}/dispatches`)  
    Manually trigger a workflow run using a workflow dispatch event. Requires specifying a ref (branch or tag) and optionally input parameters.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "ref": "main", "inputs": { "param1": "value" } }' \
         "https://api.github.com/repos/octocat/Hello-World/actions/workflows/161234/dispatches"
    ```  
    **Sample Response:** Returns `204 No Content` if the dispatch was accepted.

- **List Workflow Runs** (`GET /repos/{owner}/{repo}/actions/runs`)  
    List recent workflow runs in the repository (across all workflows or filter by workflow_id).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/runs"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "total_count": 5,
      "workflow_runs": [
        {
          "id": 30012345,
          "name": "CI",
          "head_branch": "main",
          "head_sha": "6dcb09b...",
          "event": "push",
          "status": "completed",
          "conclusion": "success",
          "workflow_id": 161234,
          "run_number": 15,
          "created_at": "2025-04-22T15:00:00Z",
          "updated_at": "2025-04-22T15:10:00Z",
          ...
        },
        ...
      ]
    }
    ```

- **Get a Workflow Run** (`GET /repos/{owner}/{repo}/actions/runs/{run_id}`)  
    Retrieve details of a specific workflow run, including status and conclusion.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/runs/30012345"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 30012345,
      "name": "CI",
      "head_branch": "main",
      "head_sha": "6dcb09b...",
      "event": "push",
      "status": "completed",
      "conclusion": "success",
      "workflow_id": 161234,
      "check_suite_id": 123456789,
      "run_number": 15,
      "created_at": "2025-04-22T15:00:00Z",
      "updated_at": "2025-04-22T15:10:00Z",
      "actor": {
        "login": "octocat",
        "id": 1,
        ...
      },
      ...
    }
    ```

- **Cancel a Workflow Run** (`POST /repos/{owner}/{repo}/actions/runs/{run_id}/cancel`)  
    Cancel a running workflow run.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/runs/30012345/cancel"
    ```  
    **Sample Response:** Returns `202 Accepted` if cancellation is initiated.

- **Re-run a Workflow** (`POST /repos/{owner}/{repo}/actions/runs/{run_id}/rerun`)  
    Re-run a completed workflow run.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/runs/30012345/rerun"
    ```  
    **Sample Response:** Returns `201 Created` if re-run is successfully triggered.

- **List Jobs for a Workflow Run** (`GET /repos/{owner}/{repo}/actions/runs/{run_id}/jobs`)  
    List the jobs that were executed in a given workflow run.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/runs/30012345/jobs"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "total_count": 2,
      "jobs": [
        {
          "id": 7001,
          "run_id": 30012345,
          "name": "build",
          "status": "completed",
          "conclusion": "success",
          "started_at": "2025-04-22T15:00:00Z",
          "completed_at": "2025-04-22T15:05:00Z",
          ...
        },
        ...
      ]
    }
    ```

- **List Artifacts** (`GET /repos/{owner}/{repo}/actions/artifacts`)  
    List all artifacts generated by workflow runs in the repository.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/artifacts"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "total_count": 1,
      "artifacts": [
        {
          "id": 12345,
          "name": "build-output",
          "size_in_bytes": 1048576,
          "url": "https://api.github.com/repos/octocat/Hello-World/actions/artifacts/12345",
          "archive_download_url": "https://api.github.com/repos/octocat/Hello-World/actions/artifacts/12345/zip",
          "expired": false,
          "created_at": "2025-04-22T15:05:00Z",
          "expires_at": "2025-05-22T15:05:00Z"
        }
      ]
    }
    ```

- **Get an Artifact** (`GET /repos/{owner}/{repo}/actions/artifacts/{artifact_id}`)  
    Retrieve information about a specific artifact (use the `archive_download_url` to download it).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/artifacts/12345"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 12345,
      "node_id": "MDEwOkFydGlmYWN0MTIzNDU=",
      "name": "build-output",
      "size_in_bytes": 1048576,
      "url": "https://api.github.com/repos/octocat/Hello-World/actions/artifacts/12345",
      "archive_download_url": "https://api.github.com/repos/octocat/Hello-World/actions/artifacts/12345/zip",
      "expired": false,
      "created_at": "2025-04-22T15:05:00Z",
      "expires_at": "2025-05-22T15:05:00Z"
    }
    ```

- **Delete an Artifact** (`DELETE /repos/{owner}/{repo}/actions/artifacts/{artifact_id}`)  
    Permanently delete an artifact.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/artifacts/12345"
    ```  
    **Sample Response:** Returns `204 No Content` if deletion is successful.

- **List Repository Secrets** (`GET /repos/{owner}/{repo}/actions/secrets`)  
    List all Actions secrets names in the repository (only metadata, not values).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/secrets"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "total_count": 1,
      "secrets": [
        {
          "name": "API_KEY",
          "created_at": "2025-01-01T12:00:00Z",
          "updated_at": "2025-01-01T12:00:00Z"
        }
      ]
    }
    ```

- **Get a Repository Public Key** (`GET /repos/{owner}/{repo}/actions/secrets/public-key`)  
    Get the public key used for encrypting secrets. The response includes a `key` (Base64) and `key_id`.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/secrets/public-key"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "key_id": "0123456789abcdef",
      "key": "LS0tLS1CRUdJTiBPUk0gUFVCTElDIEtFWS0tLS0t..."
    }
    ```

- **Create or Update a Secret** (`PUT /repos/{owner}/{repo}/actions/secrets/{secret_name}`)  
    Add a new secret or update an existing one. The secret value must be encrypted with the repository's public key.  
    **Example cURL:**  
    ```shell
    curl -X PUT -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "encrypted_value": "BASE64-ENCODED-ENCRYPTED-VALUE", "key_id": "0123456789abcdef" }' \
         "https://api.github.com/repos/octocat/Hello-World/actions/secrets/API_KEY"
    ```  
    **Sample Response:** Returns `204 No Content` if the secret was stored successfully.

- **Delete a Secret** (`DELETE /repos/{owner}/{repo}/actions/secrets/{secret_name}`)  
    Remove a secret from the repository.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/secrets/API_KEY"
    ```  
    **Sample Response:** Returns `204 No Content` on success.

- **List Repository Variables** (`GET /repos/{owner}/{repo}/actions/variables`)  
    List all Actions variables in the repository (unencrypted key-value pairs).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/variables"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "total_count": 1,
      "variables": [
        {
          "name": "ENVIRONMENT",
          "value": "production",
          "created_at": "2025-02-01T10:00:00Z",
          "updated_at": "2025-02-01T10:00:00Z"
        }
      ]
    }
    ```

- **Create or Update a Variable** (`PUT /repos/{owner}/{repo}/actions/variables/{name}`)  
    Set the value of a repository variable (creates if it doesn't exist).  
    **Example cURL:**  
    ```shell
    curl -X PUT -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "name": "ENVIRONMENT", "value": "staging" }' \
         "https://api.github.com/repos/octocat/Hello-World/actions/variables/ENVIRONMENT"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "name": "ENVIRONMENT",
      "value": "staging",
      "created_at": "2025-04-22T16:15:00Z",
      "updated_at": "2025-04-22T16:15:00Z"
    }
    ```

- **List Self-Hosted Runners** (`GET /repos/{owner}/{repo}/actions/runners`)  
    List all self-hosted runners registered to a repository.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/runners"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "total_count": 1,
      "runners": [
        {
          "id": 101,
          "name": "my-runner",
          "os": "linux",
          "status": "online"
        }
      ]
    }
    ```

- **Delete a Self-Hosted Runner** (`DELETE /repos/{owner}/{repo}/actions/runners/{runner_id}`)  
    Remove a self-hosted runner from the repository (forces it to unregister).  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/actions/runners/101"
    ```  
    **Sample Response:** Returns `204 No Content` if the runner was removed.

## Codespaces
- **List Codespaces in a Repository** (`GET /repos/{owner}/{repo}/codespaces`)  
    List all codespaces belonging to the authenticated user for a given repository.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/codespaces"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "total_count": 1,
      "codespaces": [
        {
          "name": "octocat-hello-world-abcd1234", 
          "id": 1234,
          "owner": { "login": "octocat", ... },
          "repository": { "full_name": "octocat/Hello-World", ... },
          "machine": "standardLinux",
          "state": "Running",
          "created_at": "2025-04-01T12:00:00Z",
          "last_used_at": "2025-04-22T16:20:00Z",
          ...
        }
      ]
    }
    ```

- **Create a Codespace** (`POST /repos/{owner}/{repo}/codespaces`)  
    Start a new codespace on a repository. You can specify parameters like branch, machine type, devcontainer, etc.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "ref": "main", "location": "WestUS", "machine": "basicLinux" }' \
         "https://api.github.com/repos/octocat/Hello-World/codespaces"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "name": "octocat-hello-world-efgh5678",
      "id": 5678,
      "state": "Pending",
      "repository": { "full_name": "octocat/Hello-World", ... },
      "branch": "main",
      "machine": "basicLinux",
      "git_status": { "ahead": 0, "behind": 0, "has_unpushed_changes": false, ... },
      ...
    }
    ```

- **List Available Machine Types** (`GET /repos/{owner}/{repo}/codespaces/machines`)  
    List the types of virtual machines available for creating a codespace in this repo (varies by region and repository).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/codespaces/machines"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "total_count": 2,
      "machines": [
        { "name": "basicLinux", "display_name": "Basic Linux", "cpus": 2, "memory_in_gb": 4, ... },
        { "name": "standardLinux", "display_name": "Standard Linux", "cpus": 4, "memory_in_gb": 8, ... }
      ]
    }
    ```

- **Delete a Codespace** (`DELETE /user/codespaces/{codespace_name}`)  
    Delete a codespace owned by the authenticated user. This will remove the remote environment.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/user/codespaces/octocat-hello-world-abcd1234"
    ```  
    **Sample Response:** Returns `204 No Content` if the codespace was successfully deleted.

- **Stop a Codespace** (`POST /user/codespaces/{codespace_name}/stop`)  
    Stop a running codespace (without deleting it).  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" "https://api.github.com/user/codespaces/octocat-hello-world-abcd1234/stop"
    ```  
    **Sample Response:** Returns `202 Accepted` if the stop request was initiated.

## Security Alerts
- **List Code Scanning Alerts** (`GET /repos/{owner}/{repo}/code-scanning/alerts`)  
    List code scanning alerts (from CodeQL or other scanners) in a repository.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/code-scanning/alerts"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "number": 42,
        "state": "open",
        "rule_id": "JS1001",
        "rule_severity": "moderate",
        "rule_description": "Potential XSS vulnerability",
        "tool": { "name": "CodeQL", ... },
        "created_at": "2025-01-10T10:00:00Z",
        ...
      }
    ]
    ```

- **Get a Code Scanning Alert** (`GET /repos/{owner}/{repo}/code-scanning/alerts/{alert_number}`)  
    Get details of a specific code scanning alert.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/code-scanning/alerts/42"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "number": 42,
      "state": "open",
      "rule": {
        "id": "JS1001",
        "severity": "moderate",
        "description": "Potential XSS vulnerability",
        ...
      },
      "tool": { "name": "CodeQL", ... },
      "most_recent_instance": {
        "ref": "refs/heads/main",
        "commit_sha": "d1e9f...",
        ...
      },
      ...
    }
    ```

- **List Dependabot Alerts** (`GET /repos/{owner}/{repo}/dependabot/alerts`)  
    List dependency vulnerability alerts (Dependabot alerts) for a repository.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/dependabot/alerts"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "number": 2,
        "state": "dismissed",
        "dependency": {
          "package": { "ecosystem": "pip", "name": "django" },
          "manifest_path": "path/to/requirements.txt"
        },
        "security_advisory": {
          "ghsa_id": "GHSA-rf4j-j272-fj86",
          "severity": "high",
          ...
        },
        "security_vulnerability": {
          "first_patched_version": { "identifier": "2.0.2" },
          "vulnerable_version_range": ">= 2.0.0, < 2.0.2",
          ...
        },
        ...
      }
    ]
    ```

- **Get a Dependabot Alert** (`GET /repos/{owner}/{repo}/dependabot/alerts/{alert_number}`)  
    Retrieve details for a specific Dependabot alert (includes information about the dependency and the advisory).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/dependabot/alerts/2"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "number": 2,
      "state": "dismissed",
      "dependency": {
        "package": { "ecosystem": "pip", "name": "django" },
        "manifest_path": "path/to/requirements.txt"
      },
      "security_advisory": {
        "ghsa_id": "GHSA-rf4j-j272-fj86",
        "severity": "high",
        "cvss": { "score": 7.5, ... },
        ...
      },
      "security_vulnerability": {
        "first_patched_version": { "identifier": "2.0.2" },
        "vulnerable_version_range": ">= 2.0.0, < 2.0.2",
        ...
      },
      ...
    }
    ```

- **List Secret Scanning Alerts** (`GET /repos/{owner}/{repo}/secret-scanning/alerts`)  
    List any exposed secret alerts in a repository.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/secret-scanning/alerts"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "number": 2,
        "state": "resolved",
        "resolution": "false_positive",
        "secret_type": "adafruit_io_key",
        "secret_type_display_name": "Adafruit IO Key",
        "secret": "aio_XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        "resolved_by": { "login": "monalisa", ... },
        ...
      }
    ]
    ```

- **Get a Secret Scanning Alert** (`GET /repos/{owner}/{repo}/secret-scanning/alerts/{alert_number}`)  
    Get details of a specific secret scanning alert, including the type of secret and where it was found.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/repos/octocat/Hello-World/secret-scanning/alerts/2"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "number": 2,
      "state": "resolved",
      "resolution": "false_positive",
      "secret_type": "adafruit_io_key",
      "secret": "aio_XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "resolved_by": {
        "login": "monalisa",
        "id": 2,
        ...
      },
      ...
    }
    ```
## Organizations
- **Get an Organization** (`GET /orgs/{org}`)  
    Retrieve an organization's profile information by name.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/orgs/octo-org"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "login": "octo-org",
      "id": 1001,
      "description": "Example organization",
      "url": "https://api.github.com/orgs/octo-org",
      "repos_url": "https://api.github.com/orgs/octo-org/repos",
      "members_url": "https://api.github.com/orgs/octo-org/members{/member}",
      "public_members_url": "https://api.github.com/orgs/octo-org/public_members{/member}",
      "html_url": "https://github.com/octo-org",
      "type": "Organization",
      ...
    }
    ```

- **List Organizations for a User** (`GET /users/{username}/orgs`)  
    List public organizations a user is a member of. (For the authenticated user's orgs, use `GET /user/orgs`.)  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/users/octocat/orgs"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "login": "octo-org",
        "id": 1001,
        "description": "Example organization",
        ...
      }
    ]
    ```

- **List Organization Members** (`GET /orgs/{org}/members`)  
    List members of an organization. By default, this lists regular members (not including outside collaborators).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/orgs/octo-org/members"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "login": "octocat",
        "id": 1,
        "type": "User",
        ...
      },
      {
        "login": "monalisa",
        "id": 2,
        "type": "User",
        ...
      }
    ]
    ```

- **Add or Update Organization Membership** (`PUT /orgs/{org}/memberships/{username}`)  
    Invite a user to an organization or update their membership (if authenticated user is an org owner).  
    **Example cURL:**  
    ```shell
    curl -X PUT -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         "https://api.github.com/orgs/octo-org/memberships/monalisa"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "state": "pending",
      "role": "direct_member",
      "organization_url": "https://api.github.com/orgs/octo-org",
      "user": {
        "login": "monalisa",
        "id": 2,
        ...
      }
    }
    ```

- **Remove an Organization Member** (`DELETE /orgs/{org}/members/{username}`)  
    Remove a member from an organization.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/orgs/octo-org/members/monalisa"
    ```  
    **Sample Response:** Returns `204 No Content` if the user was removed.

- **List Organization Webhooks** (`GET /orgs/{org}/hooks`)  
    List webhooks configured on an organization.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/orgs/octo-org/hooks"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 98765,
        "name": "web",
        "active": true,
        "events": ["organization", "member"],
        "config": {
          "url": "https://example.com/webhook",
          "content_type": "json",
          ...
        },
        ...
      }
    ]
    ```

- **Create an Organization Webhook** (`POST /orgs/{org}/hooks`)  
    Create a new webhook for an organization.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "name": "web", "config": { "url": "https://example.com/webhook", "content_type": "json" }, "events": ["member"], "active": true }' \
         "https://api.github.com/orgs/octo-org/hooks"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 123456,
      "name": "web",
      "active": true,
      "events": ["member"],
      "config": {
        "url": "https://example.com/webhook",
        "content_type": "json",
        "insecure_ssl": "0",
        ...
      },
      "created_at": "2025-04-22T17:00:00Z",
      ...
    }
    ```

## Teams (Within Organizations)
- **List Teams** (`GET /orgs/{org}/teams`)  
    List all teams in an organization.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/orgs/octo-org/teams"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 201,
        "name": "Developers",
        "slug": "developers",
        "description": "Development team",
        "privacy": "closed",
        "permission": "push",
        ...
      }
    ]
    ```

- **Create a Team** (`POST /orgs/{org}/teams`)  
    Create a new team in an organization.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "name": "QA Team", "description": "Quality Assurance team", "privacy": "closed" }' \
         "https://api.github.com/orgs/octo-org/teams"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 202,
      "name": "QA Team",
      "slug": "qa-team",
      "description": "Quality Assurance team",
      "privacy": "closed",
      "permission": "pull",
      ...
    }
    ```

- **Get Team (by slug)** (`GET /orgs/{org}/teams/{team_slug}`)  
    Fetch information about a specific team by its slug.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/orgs/octo-org/teams/developers"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 201,
      "name": "Developers",
      "slug": "developers",
      "description": "Development team",
      "permission": "push",
      "members_count": 5,
      "repos_count": 10,
      ...
    }
    ```

- **Add Team Member** (`PUT /orgs/{org}/teams/{team_slug}/memberships/{username}`)  
    Add a user to a team (or update their membership role). Requires org admin or team maintainer permission.  
    **Example cURL:**  
    ```shell
    curl -X PUT -H "Authorization: token YOUR_TOKEN" "https://api.github.com/orgs/octo-org/teams/developers/memberships/monalisa"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "url": "https://api.github.com/orgs/octo-org/teams/developers/memberships/monalisa",
      "role": "member",
      "state": "active"
    }
    ```

- **Remove Team Member** (`DELETE /orgs/{org}/teams/{team_slug}/memberships/{username}`)  
    Remove a user from a team.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/orgs/octo-org/teams/developers/memberships/monalisa"
    ```  
    **Sample Response:** Returns `204 No Content` if removal was successful.

- **List Team Repositories** (`GET /orgs/{org}/teams/{team_slug}/repos`)  
    List the repositories that a team has access to.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/orgs/octo-org/teams/developers/repos"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 1296269,
        "name": "Hello-World",
        "full_name": "octo-org/Hello-World",
        "permissions": { "pull": true, "push": true, "admin": false },
        ...
      }
    ]
    ```

- **Add Team Repository** (`PUT /orgs/{org}/teams/{team_slug}/repos/{owner}/{repo}`)  
    Give a team access to a repository.  
    **Example cURL:**  
    ```shell
    curl -X PUT -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         "https://api.github.com/orgs/octo-org/teams/developers/repos/octo-org/Hello-World"
    ```  
    **Sample Response:** Returns `204 No Content` if the repo was added to the team.

- **Remove Team Repository** (`DELETE /orgs/{org}/teams/{team_slug}/repos/{owner}/{repo}`)  
    Revoke a team's access to a repository.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/orgs/octo-org/teams/developers/repos/octo-org/Hello-World"
    ```  
    **Sample Response:** Returns `204 No Content` on success.

## Users
- **Get the Authenticated User** (`GET /user`)  
    Get the profile of the currently authenticated user.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/user"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "login": "octocat",
      "id": 1,
      "name": "Monalisa Octocat",
      "email": "octocat@github.com",
      "created_at": "2008-01-14T04:33:35Z",
      ...
    }
    ```

- **Get a User by Username** (`GET /users/{username}`)  
    Fetch public profile information for any GitHub user.  
    **Example cURL:**  
    ```shell
    curl "https://api.github.com/users/octocat"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "login": "octocat",
      "id": 1,
      "name": "Monalisa Octocat",
      "company": "@github",
      "blog": "https://github.blog",
      "public_repos": 2,
      "followers": 20,
      "following": 0,
      ...
    }
    ```

- **Update the Authenticated User** (`PATCH /user`)  
    Update the profile of the authenticated user (e.g., name, bio).  
    **Example cURL:**  
    ```shell
    curl -X PATCH -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"name": "Monalisa Octocat", "blog": "https://mona.example"}' \
         "https://api.github.com/user"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "login": "octocat",
      "id": 1,
      "name": "Monalisa Octocat",
      "blog": "https://mona.example",
      ...
    }
    ```

- **List User's Followers** (`GET /users/{username}/followers`)  
    List the followers of a user.  
    **Example cURL:**  
    ```shell
    curl "https://api.github.com/users/octocat/followers"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "login": "user1",
        "id": 100,
        ...
      },
      {
        "login": "user2",
        "id": 200,
        ...
      }
    ]
    ```

- **List Users Followed by Authenticated User** (`GET /user/following`)  
    List the users the authenticated user is following.  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/user/following"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "login": "someuser",
        "id": 300,
        ...
      }
    ]
    ```

- **Follow a User** (`PUT /user/following/{username}`)  
    Follow a user (for the authenticated user).  
    **Example cURL:**  
    ```shell
    curl -X PUT -H "Authorization: token YOUR_TOKEN" "https://api.github.com/user/following/octocat"
    ```  
    **Sample Response:** Returns `204 No Content` if the follow was successful.

- **Unfollow a User** (`DELETE /user/following/{username}`)  
    Unfollow a user.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/user/following/octocat"
    ```  
    **Sample Response:** Returns `204 No Content` on success.

- **List Public SSH Keys** (`GET /users/{username}/keys`)  
    List the public SSH keys for a user. (For the authenticated user's own keys, use `/user/keys`).  
    **Example cURL:**  
    ```shell
    curl "https://api.github.com/users/octocat/keys"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": 123456,
        "key": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCu...",
        "title": "octocat@GitHub"
      }
    ]
    ```

- **Create an SSH Key** (`POST /user/keys`)  
    Add a new SSH key to the authenticated user's account.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{"title": "Laptop", "key": "ssh-rsa AAAAB3Nza..."}' \
         "https://api.github.com/user/keys"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": 789012,
      "key": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCu...",
      "title": "Laptop"
    }
    ```

- **Delete an SSH Key** (`DELETE /user/keys/{key_id}`)  
    Remove an SSH key from the authenticated user's account.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/user/keys/789012"
    ```  
    **Sample Response:** Returns `204 No Content` if deletion was successful.

## Gists
- **List Authenticated User's Gists** (`GET /gists`)  
    List gists owned by the authenticated user (includes private gists if authenticated).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/gists"
    ```  
    **Sample JSON Response:**  
    ```json
    [
      {
        "id": "aa5a315d61ae9438b18d",
        "description": "Example gist",
        "public": true,
        "files": {
          "hello_world.txt": {
            "filename": "hello_world.txt",
            "type": "text/plain",
            "language": "Text",
            "size": 20
          }
        },
        "owner": {
          "login": "octocat",
          "id": 1,
          ...
        },
        "created_at": "2010-04-14T02:15:15Z",
        ...
      }
    ]
    ```

- **List Public Gists** (`GET /gists/public`)  
    List all public gists.  
    **Example cURL:**  
    ```shell
    curl "https://api.github.com/gists/public"
    ```  

- **Get a Gist** (`GET /gists/{gist_id}`)  
    Retrieve a specific gist by ID.  
    **Example cURL:**  
    ```shell
    curl "https://api.github.com/gists/aa5a315d61ae9438b18d"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": "aa5a315d61ae9438b18d",
      "description": "Example gist",
      "public": true,
      "files": {
        "hello_world.txt": {
          "filename": "hello_world.txt",
          "type": "text/plain",
          "language": "Text",
          "raw_url": "https://gist.githubusercontent.com/...",
          "size": 20
        }
      },
      "owner": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "created_at": "2010-04-14T02:15:15Z",
      ...
    }
    ```

- **Create a Gist** (`POST /gists`)  
    Create a new gist with one or more files.  
    **Example cURL:**  
    ```shell
    curl -X POST -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "description": "Hello World Example", "public": true, "files": { "hello.txt": { "content": "Hello World" } } }' \
         "https://api.github.com/gists"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "id": "bb5a315d61ae9438b190",
      "description": "Hello World Example",
      "public": true,
      "files": {
        "hello.txt": {
          "filename": "hello.txt",
          "language": "Text",
          "raw_url": "https://gist.githubusercontent.com/...",
          "size": 11
        }
      },
      "owner": {
        "login": "octocat",
        "id": 1,
        ...
      },
      "created_at": "2025-04-22T17:30:00Z",
      ...
    }
    ```

- **Update a Gist** (`PATCH /gists/{gist_id}`)  
    Edit an existing gist (change files or description).  
    **Example cURL:**  
    ```shell
    curl -X PATCH -H "Authorization: token YOUR_TOKEN" -H "Accept: application/vnd.github+json" \
         -d '{ "files": { "hello.txt": { "content": "Hello GitHub World" } } }' \
         "https://api.github.com/gists/aa5a315d61ae9438b18d"
    ```  
    **Sample JSON Response:** *(returns the updated gist JSON)*

- **Delete a Gist** (`DELETE /gists/{gist_id}`)  
    Permanently delete a gist owned by the authenticated user.  
    **Example cURL:**  
    ```shell
    curl -X DELETE -H "Authorization: token YOUR_TOKEN" "https://api.github.com/gists/aa5a315d61ae9438b18d"
    ```  
    **Sample Response:** Returns `204 No Content` on successful deletion.

## Miscellaneous
- **Search Repositories** (`GET /search/repositories?q={query}`)  
    Search for repositories via keywords. There are many query parameters for advanced search (stars, forks, etc).  
    **Example cURL:**  
    ```shell
    curl "https://api.github.com/search/repositories?q=topic:github+language:python&sort=stars"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "total_count": 12345,
      "items": [
        {
          "id": 1296269,
          "name": "Hello-World",
          "full_name": "octocat/Hello-World",
          "owner": { "login": "octocat", ... },
          "private": false,
          "description": "This is your first repository",
          "forks_count": 9,
          "stargazers_count": 80,
          "language": "Ruby",
          ...
        },
        ...
      ]
    }
    ```

- **Search Code** (`GET /search/code?q={query}`)  
    Search for code within repositories. For example, search for a string in a specific repo.  
    **Example cURL:**  
    ```shell
    curl "https://api.github.com/search/code?q=TODO+in:file+repo:octocat/Hello-World"
    ```

- **Rate Limit Status** (`GET /rate_limit`)  
    Get the current API rate limit status for the authenticated user (or IP if no auth).  
    **Example cURL:**  
    ```shell
    curl -H "Authorization: token YOUR_TOKEN" "https://api.github.com/rate_limit"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "resources": {
        "core": { "limit": 5000, "remaining": 4999, "reset": 1632920000, ... },
        "search": { "limit": 30, "remaining": 30, "reset": 1632916500, ... },
        ...
      },
      "rate": { "limit": 5000, "remaining": 4999, "reset": 1632920000, ... }
    }
    ```

- **Get GitHub Meta Information** (`GET /meta`)  
    Retrieve metadata about GitHub.com, such as the IP ranges for webhook deliveries and API.  
    **Example cURL:**  
    ```shell
    curl "https://api.github.com/meta"
    ```

- **List Emojis** (`GET /emojis`)  
    Get a JSON mapping of all the GitHub emoji codes to their images.  
    **Example cURL:**  
    ```shell
    curl "https://api.github.com/emojis"
    ```  
    **Sample JSON Response:**  
    ```json
    {
      "smile": "https://github.githubassets.com/images/icons/emoji/unicode/1f604.png?v8",
      "octocat": "https://github.githubassets.com/images/icons/emoji/octocat.png?v8",
      ...
    }
    ```
