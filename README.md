# AI PR Summarizer

A GitHub Action that automatically summarizes Pull Request changes using AI, translating technical code changes into clear, business-focused summaries for non-technical stakeholders (Product Managers, QA Engineers, Business Analysts).

## 🚀 Features

- **Smart File Filtering**: Automatically ignores lock files, SVGs, and auto-generated files to save tokens
- **Diff Truncation**: Limits diff length to prevent excessive token usage
- **Comment Management**: Updates existing comments instead of creating duplicates
- **Business-Focused Summaries**: AI generates summaries focused on business impact and QA focus areas
- **Multiple Model Support**: Works with any OpenAI-compatible model (GPT-4, GPT-3.5, etc.)

## 📋 Prerequisites

- GitHub repository
- OpenAI API key
- GitHub token (automatically provided by GitHub Actions)

## 🔧 Setup

### 1. Add Repository Secrets

Go to your repository settings: `Settings` → `Secrets and variables` → `Actions` → `New repository secret`

Add the following secret:
- `OPENAI_API_KEY`: Your OpenAI API key

### 2. Create Workflow File

Create `.github/workflows/pr-summarizer.yml` in your repository:

```yaml
name: PR Summarizer

on:
  pull_request:
    types: [opened, synchronize, reopened]
    branches:
      - main
      - develop
      - 'release/**'

permissions:
  pull-requests: write
  contents: read

jobs:
  summarize:
    name: Summarize PR Changes
    runs-on: ubuntu-latest
    steps:
      - name: AI PR Summarizer
        uses: pnnnhan99/action-pr-summarizer@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          openai-api-key: ${{ secrets.OPENAI_API_KEY }}
          model: gpt-4o-mini
          max-diff-length: '50000'
          ignored-files: '*.lock,*.svg,*.csv,*.min.js,*.min.css,package-lock.json,yarn.lock,pnpm-lock.yaml'
```

## ⚙️ Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `github-token` | Yes | `${{ github.token }}` | GitHub token for API access |
| `openai-api-key` | Yes | - | OpenAI API key for summarization |
| `model` | No | `gpt-4o-mini` | OpenAI model to use |
| `max-diff-length` | No | `50000` | Maximum character length for diff before truncation |
| `ignored-files` | No | `*.lock,*.svg,*.csv,*.min.js,*.min.css,package-lock.json,yarn.lock,pnpm-lock.yaml` | Comma-separated file patterns to ignore |

## 📤 Outputs

| Output | Description |
|--------|-------------|
| `summary` | The generated PR summary |

## 🎯 Example Output

The AI generates a structured summary:

```markdown
## 📋 Summary
This PR adds user authentication functionality with JWT tokens and implements password reset flow.

## ✨ Key Changes
- Added JWT authentication middleware for protected routes
- Implemented password reset email functionality
- Updated user model to include reset token fields

## 🎯 Business Impact
Users can now securely authenticate and reset their passwords without admin intervention, improving user experience and reducing support tickets.

## ⚠️ Areas for QA Focus
- Test login with valid and invalid credentials
- Verify password reset email delivery
- Check JWT token expiration handling
- Test concurrent login sessions

## 📝 Notes
- Requires SMTP configuration for email delivery
- JWT secret needs to be configured in environment variables
```

## 🛠️ Development

### Install Dependencies

```bash
npm install
```

### Build

```bash
npm run build
```

### Watch Mode

```bash
npm run watch
```

## 📝 How It Works

1. **Trigger**: Action runs when a PR is opened, updated, or reopened
2. **Fetch Diff**: Retrieves PR changes using GitHub API
3. **Filter Files**: Ignores irrelevant files (lock files, SVGs, etc.)
4. **Truncate Diff**: Limits diff length if too long
5. **Generate Summary**: Sends filtered diff to OpenAI API with a specialized system prompt
6. **Upsert Comment**: Updates existing comment or creates new one with the summary

## 🔐 Security

- GitHub token is automatically provided by GitHub Actions
- OpenAI API key should be stored as a repository secret
- No sensitive data is logged or exposed

## 📄 License

MIT

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📧 Support

For issues and questions, please open an issue in the repository.
