# Quick Start Commands - Meshery Development

This is a quick reference card for common development commands. For detailed explanations, see [GETTING_STARTED.md](GETTING_STARTED.md).

## Initial Setup

```bash
# Fork and clone
git clone https://github.com/YOUR_USERNAME/meshery.git
cd meshery
git remote add upstream https://github.com/meshery/meshery.git

# Install dependencies
cd ui && npm install && cd ..
cd server/cmd && go mod download && cd ../..
cd mesheryctl && go mod download && cd ..
```

## Running Meshery Locally

```bash
# Terminal 1: Run the server (port 9081)
make server

# Terminal 2: Run the UI (port 3000)
make ui

# Access at http://localhost:3000
```

## Building Components

```bash
# Build server binary
make build-server

# Build UI for production
make ui-build

# Build CLI
cd mesheryctl && make
```

## Testing

```bash
# Test server
cd server && go test ./...

# Lint and test UI
cd ui
npm run lint
npm run lint:fix
npm run test:e2e

# Test CLI
cd mesheryctl
go test --short ./...
```

## Git Workflow

```bash
# Create a new branch
git checkout -b fix/issue-number

# Make changes and commit with DCO sign-off (required!)
git add .
git commit -s -m "[Component] Description"

# Push to your fork
git push origin fix/issue-number

# Keep your branch updated
git fetch upstream
git rebase upstream/master
git push -f origin fix/issue-number
```

## Code Formatting

```bash
# Format Go code
go fmt ./...

# Format and lint UI code
cd ui
npm run lint:fix
npm run format
```

## Docker

```bash
# Build Docker image
make docker-build

# Run in Docker
make docker-cloud

# Run with local provider
make docker-local-cloud
```

## Documentation

```bash
# Run docs site locally (http://localhost:4000)
make docs

# Build docs
make docs-build
```

## Common Issues

```bash
# Port 9081 already in use
lsof -ti:9081 | xargs kill -9

# Clear npm cache
npm cache clean --force
rm -rf node_modules package-lock.json
npm install

# Clean Docker
docker system prune -a
```

## Make Targets Summary

| Command | Description |
|---------|-------------|
| `make server` | Run Meshery server locally |
| `make ui` | Run UI development server |
| `make ui-setup` | Install UI dependencies |
| `make ui-build` | Build UI for production |
| `make build-server` | Build server binary |
| `make docker-build` | Build Docker container |
| `make docs` | Run documentation site |
| `make golangci` | Lint Go code |
| `make ui-lint` | Lint UI code |

## Prerequisites

- **Go**: 1.25.5+
- **Node.js**: 20 LTS
- **Make**: Build automation
- **Git**: Version control
- **Docker** (optional): Container runtime

## Getting Help

- **Slack**: https://slack.meshery.io
- **Docs**: https://docs.meshery.io
- **Issues**: https://github.com/meshery/meshery/issues
- **Getting Started**: [GETTING_STARTED.md](GETTING_STARTED.md)

---

**Tip**: Always use `git commit -s` to sign your commits with DCO!
