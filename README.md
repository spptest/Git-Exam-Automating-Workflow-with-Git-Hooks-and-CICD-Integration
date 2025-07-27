# ✅ Project Development Workflow

This project follows a structured CI/CD pipeline and Git workflow to ensure code quality and consistency. A minimal Node.js project demonstrating a basic `sum` function, unit testing with Jest, and Git workflow tracking.

---

## 🧪 Pre-Commit Hook

The pre-commit hook enforces the following:

1. ✅ **Commit Message Format**  
   All commit messages must follow this pattern: TASK-123: Short description

   ### 📄 Hook Script (`.git/hooks/commit-msg`)

```sh
#!/bin/bash

COMMIT_MSG_FILE=$1
COMMIT_MSG=$(head -n1 "$COMMIT_MSG_FILE")

# Regex: starts with TASK-123: Some message
if [[ ! "$COMMIT_MSG" =~ ^TASK-[0-9]+:\  ]]; then
  echo "❌ Invalid commit message format."
  echo "✅ Required format: TASK-123: Fix something"
  exit 1
fi

echo "✅ Commit message format is valid."

```

2. ✅ **Code Formatting Check**
   Runs `prettier --check` to ensure code is properly formatted.

### 📄 Hook Script (`.git/hooks/pre-commit`)

```sh
#!/bin/bash

echo "🔍 Checking code format with Prettier..."
npx prettier --check .

if [ $? -ne 0 ]; then
  echo "❌ Code is not formatted correctly. Commit aborted."
  exit 1
fi

echo "✅ Code is formatted. Proceeding with commit."
```

---

## 🚫 Pre-Push Hook

The pre-push hook runs **before pushing code to any remote branch**. It performs:

1. 🧪 **Unit Test Execution**

- Runs the full test suite using `npm test`.
- Push is **blocked if any test fails**.

### 📄 Script Example (`scripts/push-with-tests.sh`)

```sh
#!/bin/sh

echo "🧪 Running tests before push..."
npm test

if [ $? -ne 0 ]; then
  echo "❌ Tests failed. Push aborted."
  exit 1
fi

echo "✅ Tests passed. Proceeding with push."
```

---

## 🔁 CI/CD Pipeline (GitHub Actions)

- **Workflow File**: `.github/workflows/ci.yml`
- **Trigger**: Automatically runs on every pull request to the `main` branch.
- **Steps:**
  - Installs dependencies.
  - Runs all tests using `npm test` (Jest).
  - Ensures CI must pass before merging.

> ✅ Pull requests **require approvals** and **passing CI** before they can be merged into `main`.

---
