## Dev Containers

## My Typical Config

### VS Code

```json
{
"customizations": {
    "vscode": {
        "extensions": [
            // Docs
            "bierner.github-markdown-preview",
            "yahyabatulu.vscode-markdown-alert"
            "davidanson.vscode-markdownlint",
            "yzhang.markdown-all-in-one",

            // Javascript
            "rvest.vs-code-prettier-eslint",
            "dbaeumer.vscode-eslint",

            // Syntax / Linting
            "esbenp.prettier-vscode",
            "redhat.vscode-yaml",

            // Testing
            "sonarsource.sonarlint-vscode",
            "ryanluker.vscode-coverage-gutters",

            // GitHub
            "github.copilot",
            "github.vscode-pull-request-github",
            "github.vscode-github-actions",
            "me-dutour-mathieu.vscode-github-actions",

        ],
        "settings": {
            // Styling
            "editor.defaultFormatter": "esbenp.prettier-vscode",
            "editor.formatOnSave": true,

            // Copilot
            "github.copilot.editor.enableAutoCompletions": true,
            "github.copilot.chat.codeGeneration.instructions": [
                { "file": ".github/copilot/code-style.md" }
            ],
            "github.copilot.chat.commitMessageGeneration.instructions": [
                { "file": ".github/copilot/commit-message-style.md" }
            ],
            "github.copilot.chat.reviewSelection.instructions": [
                { "file": ".github/copilot/review-style.md" }
            ],
            "github.copilot.chat.testGeneration.instructions": [
                { "file": ".github/copilot/test-style.md" }
            ],
        },
        "features": {
            "ghcr.io/devcontainers/features/github-cli:1": {},
            "ghcr.io/devcontainers-contrib/features/prettier:1": {},
            "ghcr.io/devcontainers/features/docker-outside-of-docker:1": {},
            "ghcr.io/dhoeric/features/act:1": {}
        }
    }
}
}
```

## Commands

```json
{
  "postCreateCommand": "npm install",

  "remoteEnv": {
    "GITHUB_TOKEN": "${localEnv:GITHUB_TOKEN}"
  }
}
```
