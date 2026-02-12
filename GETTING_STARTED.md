# Getting Started with Meshery - A Guide for New Contributors

Welcome to Meshery! 🎉 We're excited to have you here as you begin your open source journey. This guide is specifically designed for new contributors, including CS students looking to make their first contribution to a cloud native project.

## Table of Contents

- [About Meshery](#about-meshery)
- [Understanding the Codebase](#understanding-the-codebase)
- [Prerequisites](#prerequisites)
- [Setting Up Your Development Environment](#setting-up-your-development-environment)
- [Building Meshery Locally](#building-meshery-locally)
- [Making Your First Contribution](#making-your-first-contribution)
- [Testing Your Changes](#testing-your-changes)
- [Creating Your First Pull Request](#creating-your-first-pull-request)
- [Getting Help](#getting-help)

## About Meshery

Meshery is a self-service engineering platform and the open source, cloud native manager that enables the design and management of all Kubernetes-based infrastructure and applications. As a CNCF (Cloud Native Computing Foundation) project, Meshery supports 300+ integrations and provides visual and collaborative GitOps capabilities.

**What does this mean for you as a contributor?**
- You'll learn about cloud native technologies, Kubernetes, and service meshes
- You'll work with modern tech stacks including Go, React, Next.js, and GraphQL
- You'll collaborate with a welcoming global community

## Understanding the Codebase

Meshery is organized into several main components:

### Repository Structure

```
meshery/
├── server/           # Backend API server (Go)
│   ├── cmd/         # Main server entry point
│   ├── models/      # Data models and business logic
│   └── handlers/    # HTTP request handlers
├── ui/              # Frontend web application (React/Next.js)
│   ├── components/  # Reusable UI components
│   ├── pages/       # Next.js pages and routes
│   └── public/      # Static assets
├── mesheryctl/      # Command-line tool (Go)
│   └── internal/cli # CLI commands and logic
├── docs/            # Documentation website (Jekyll)
├── install/         # Installation scripts and configs
└── Makefile         # Build automation commands
```

### Key Technologies

**Backend (Server & CLI)**
- **Language**: Go 1.25.5
- **Framework**: Standard Go net/http, Cobra (for CLI)
- **Database**: PostgreSQL
- **GraphQL**: gqlgen
- **Key Libraries**: MeshKit (Meshery's utility library)

**Frontend (UI)**
- **Framework**: Next.js (React framework)
- **Node Version**: 20 LTS
- **Styling**: Material UI (MUI) + Sistent (Meshery's design system)
- **State Management**: Redux Toolkit
- **API Communication**: GraphQL (via Relay) + REST

**Documentation**
- **Engine**: Jekyll (Ruby-based static site generator)

### How Components Interact

1. **User Interface (UI)** - Users interact with Meshery through the web UI
2. **API Server** - UI sends requests to the Go backend server (REST/GraphQL)
3. **Database** - Server stores configuration and state in PostgreSQL
4. **Kubernetes** - Server communicates with Kubernetes clusters
5. **CLI** - Users can also interact via `mesheryctl` command-line tool

## Prerequisites

Before you start, make sure you have these tools installed:

### Required Tools

1. **Git** - Version control
   ```bash
   # Check if installed
   git --version
   # Install on Ubuntu/Debian
   sudo apt-get install git
   # Install on macOS
   brew install git
   ```

2. **Go** - Version 1.25.5 or later
   ```bash
   # Check if installed
   go version
   # Download from https://golang.org/dl/
   ```

3. **Node.js** - Version 20 LTS
   ```bash
   # Check if installed
   node --version
   npm --version
   # Install via nvm (recommended)
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
   nvm install 20
   nvm use 20
   ```

4. **Make** - Build automation (usually pre-installed on Linux/Mac)
   ```bash
   # Check if installed
   make --version
   ```

### Optional but Recommended

- **Docker** - For running Meshery in containers
- **Kubernetes** - For testing with a local cluster (minikube, kind, or Docker Desktop)
- **VS Code** or your preferred code editor

## Setting Up Your Development Environment

### Step 1: Fork and Clone the Repository

1. **Fork the repository**
   - Visit https://github.com/meshery/meshery
   - Click the "Fork" button in the top-right corner
   - This creates your own copy of the repository

2. **Clone your fork**
   ```bash
   # Replace YOUR_USERNAME with your GitHub username
   git clone https://github.com/YOUR_USERNAME/meshery.git
   cd meshery
   ```

3. **Add upstream remote**
   ```bash
   # This allows you to sync with the main repository
   git remote add upstream https://github.com/meshery/meshery.git
   git remote -v  # Verify remotes are set up correctly
   ```

### Step 2: Install Dependencies

#### Server Dependencies

```bash
# Go dependencies are managed via go.mod
# They'll be downloaded automatically when you build
cd server/cmd
go mod download
cd ../..
```

#### UI Dependencies

```bash
cd ui
npm install
cd ..
```

#### CLI Dependencies

```bash
cd mesheryctl
go mod download
cd ..
```

### Step 3: Set Up Your Git Identity

```bash
# Configure your name and email
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Meshery requires DCO (Developer Certificate of Origin)
# This means you need to sign your commits with -s flag
```

## Building Meshery Locally

Meshery provides convenient Makefile targets for building and running components.

### Build and Run the Server

```bash
# Build and run the Meshery server
make server

# The server will start on http://localhost:9081
```

This command will:
- Download Go dependencies
- Build the server binary
- Start the server
- Generate component models (if needed)

**Note**: The first build may take several minutes as it downloads dependencies and generates files.

### Build and Run the UI

In a **separate terminal**:

```bash
# Install UI dependencies (first time only)
make ui-setup

# Run the UI development server
make ui

# The UI will start on http://localhost:3000
```

The UI automatically proxies API requests to the server on port 9081.

### Build the CLI (mesheryctl)

```bash
cd mesheryctl
make

# The binary will be in the mesheryctl directory
./mesheryctl version
```

### Build Everything with Docker

If you prefer using Docker (doesn't require local Go/Node.js):

```bash
# Build Meshery container
make docker-build

# Run Meshery
make docker-cloud
```

## Making Your First Contribution

### Step 1: Find an Issue to Work On

Look for beginner-friendly issues:

1. Visit [Meshery Issues with "help wanted" label](https://github.com/issues?q=is%3Aopen+is%3Aissue+archived%3Afalse+(org%3Ameshery+OR+org%3Aservice-mesh-performance+OR+org%3Aservice-mesh-patterns+OR+org%3Ameshery-extensions)+label%3A%22help+wanted%22)
2. Filter by labels:
   - `good first issue` - Perfect for first-time contributors
   - `help wanted` - Community needs help with these
   - `area/ui` - Frontend/UI work
   - `area/server` - Backend work
   - `area/cli` - CLI tool work
   - `area/docs` - Documentation improvements

3. Comment on the issue saying you'd like to work on it
4. Wait for a maintainer to assign it to you

### Step 2: Create a Branch

```bash
# Make sure you're on master and it's up to date
git checkout master
git pull upstream master

# Create a new branch for your work
# Use a descriptive name like: fix/issue-1234 or feature/add-xyz
git checkout -b fix/issue-1234
```

### Step 3: Make Your Changes

#### For Code Changes

1. **Understand the existing code**
   - Read through related files
   - Look at similar implementations
   - Check existing tests

2. **Write your code**
   - Follow the existing code style
   - Keep changes focused and minimal
   - Add comments where necessary

3. **Format your code**
   ```bash
   # For Go code
   cd server
   go fmt ./...
   
   # For UI code
   cd ui
   npm run lint:fix
   ```

#### For Documentation Changes

1. Documentation is in the `docs/` directory
2. Use Markdown format
3. Test locally if possible:
   ```bash
   make docs  # Runs Jekyll server on http://localhost:4000
   ```

### Step 4: Commit Your Changes

```bash
# Stage your changes
git add .

# Commit with DCO sign-off (required!)
git commit -s -m "[component] Brief description of your changes"

# Examples:
# git commit -s -m "[UI] Fix button alignment on dashboard"
# git commit -s -m "[Server] Add validation for user input"
# git commit -s -m "[Docs] Update installation instructions"
```

**Important**: Always use the `-s` flag to add DCO sign-off!

### Step 5: Push Your Changes

```bash
# Push to your fork
git push origin fix/issue-1234
```

## Testing Your Changes

### Testing Server Changes

```bash
cd server/cmd

# Run tests
go test ./...

# Run specific test
go test -run TestFunctionName

# Run with verbose output
go test -v ./...
```

### Testing UI Changes

```bash
cd ui

# Run linter
npm run lint

# Fix linting issues automatically
npm run lint:fix

# Format code
npm run format

# Run E2E tests (requires Meshery server running)
npm run test:e2e
```

### Testing CLI Changes

```bash
cd mesheryctl

# Run unit tests
go test --short ./...

# Run integration tests
go test -run Integration ./...
```

### Manual Testing

1. **Run the server and UI locally**
   ```bash
   # Terminal 1
   make server
   
   # Terminal 2
   make ui
   ```

2. **Open browser** to http://localhost:3000

3. **Test your changes** manually to ensure they work as expected

4. **Check for errors** in browser console and server logs

## Creating Your First Pull Request

### Step 1: Ensure Your Branch is Up to Date

```bash
# Fetch latest changes from upstream
git fetch upstream

# Rebase your branch on the latest master
git rebase upstream/master

# If there are conflicts, resolve them and continue:
# git add <resolved-files>
# git rebase --continue

# Force push to your fork (if you rebased)
git push -f origin fix/issue-1234
```

### Step 2: Create the Pull Request

1. Go to your fork on GitHub: `https://github.com/YOUR_USERNAME/meshery`
2. Click "Compare & pull request" button
3. Fill in the PR template:
   - **Title**: `[Component] Brief description` (e.g., `[UI] Fix button alignment`)
   - **Description**: Explain what you changed and why
   - **Link the issue**: Add "Fixes #1234" to auto-close the issue
   - **Screenshots**: Add before/after screenshots for UI changes
   - **Testing**: Describe how you tested your changes

4. Click "Create pull request"

### Step 3: Respond to Review Feedback

- Maintainers will review your PR
- They may request changes
- Make the requested changes in your branch
- Commit and push - the PR will update automatically
- Be patient and polite in all communications

### Example PR Description

```markdown
## Description
This PR fixes the button alignment issue on the dashboard page.

## Related Issue
Fixes #1234

## Changes Made
- Updated CSS styles for dashboard buttons
- Ensured responsive layout works on mobile

## Screenshots
Before:
[screenshot]

After:
[screenshot]

## Testing
- [x] Tested on Chrome, Firefox, Safari
- [x] Tested on mobile viewport
- [x] Ran `npm run lint` with no errors
- [x] Manually verified button behavior
```

## Getting Help

### Community Resources

- **Slack**: Join [Meshery Slack](https://slack.meshery.io) - The fastest way to get help
- **Discussion Forum**: [Community Discussions](https://meshery.io/community#discussion-forums)
- **Documentation**: [docs.meshery.io](https://docs.meshery.io)
- **Contributing Guide**: [Full Contributing Guide](https://docs.meshery.io/project/contributing)

### Finding a Mentor

- **MeshMates Program**: Experienced contributors who help newcomers
- **Find a MeshMate**: https://meshery.io/community#meshmates
- Request a MeshMate to guide you through your first contributions

### Weekly Community Meetings

- **Community Calendar**: https://meshery.io/calendar
- Attend weekly meetings to meet the community
- Ask questions and get real-time help
- See recordings: https://www.youtube.com/@mesheryio

## Tips for Success

1. **Start small** - Don't try to fix everything at once
2. **Ask questions** - The community is here to help
3. **Read existing code** - Learn from what's already there
4. **Be patient** - Reviews may take time
5. **Stay positive** - Everyone was a beginner once
6. **Keep learning** - Each contribution teaches you something new

## Common Issues and Solutions

### "go: cannot find main module"
```bash
# Make sure you're in the right directory
cd server/cmd  # for server
cd mesheryctl  # for CLI
```

### "Port 9081 already in use"
```bash
# Find and kill the process
lsof -ti:9081 | xargs kill -9
```

### "npm ERR! code EINTEGRITY"
```bash
# Clear npm cache
npm cache clean --force
rm -rf node_modules package-lock.json
npm install
```

### Docker build fails
```bash
# Clean up Docker cache
docker system prune -a
```

### "Permission denied" errors
```bash
# Make sure build script is executable
chmod +x install/docker/*.sh
```

## Next Steps

After your first PR is merged:

1. **Celebrate!** 🎉 You're now a Meshery contributor!
2. **Update your profile** - Add Meshery to your GitHub profile
3. **Find another issue** - Keep contributing
4. **Help others** - Answer questions from other newcomers
5. **Join community meetings** - Get more involved

## Additional Resources

- **Meshery Architecture**: https://docs.meshery.io/concepts/architecture
- **API Documentation**: https://docs.meshery.io/extensibility/api
- **UI Development Guide**: https://docs.meshery.io/project/contributing/contributing-ui
- **Server Development Guide**: https://docs.meshery.io/project/contributing/contributing-server
- **CLI Development Guide**: https://docs.meshery.io/project/contributing/contributing-cli-guide
- **Community Handbook**: https://meshery.io/community#handbook

---

**Remember**: Everyone in the Meshery community was once where you are now. Don't hesitate to ask questions, make mistakes, and learn. We're here to support you on your open source journey! 🚀

**Welcome to the Meshery community!** 💚
