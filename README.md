# Git Hooks

To ensure code quality and consistency, we can use **SwiftLint** to lint our code before each commit. This guide will show you how to set up a Git pre-commit (or pre-push) hook that runs SwiftLint, preventing commits if any lint violations are found.

#### Setting Up the Pre-Commit Hook:


1. **List Available Git Hooks**

   In your project root, list the available Git hooks:

   ```bash
   ls .git/hooks
   ```

2. **Navigate to the Git Hooks Directory**

   In your project root, navigate to the `.git/hooks` directory:

   ```bash
   cd .git/hooks
   ```

3. **Create/Edit the Pre-Commit Hook**

   Create a new file named `pre-commit` (or edit the existing one):

   ```bash
   touch pre-commit
   ```

4. **Add the SwiftLint Script**

   Open the `pre-commit` file in your preferred text editor (e.g., `TextEdit`):

   Using TextEdit (macOS):
   
     ```bash
     open -e pre-commit
     ```

   Paste the following script into the file:

   ```bash
     #!/bin/bash

   # Add Homebrew to the PATH to ensure SwiftLint is found
   export PATH="$PATH:/opt/homebrew/bin"

   # Print a section header
   echo "============================"
   echo "🚀 Running SwiftLint in strict mode..."
   echo "============================"

   # Check if SwiftLint is installed
   if ! which swiftlint >/dev/null; then
     echo "⚠️  Error: SwiftLint is not installed or not found in the PATH."
     echo "💡 Tip: Please install it using 'brew install swiftlint'."
     echo "----------------------------"
     exit 1
   fi

   # Run SwiftLint in strict mode to treat warnings as errors
   swiftlint --strict

   # Capture the exit code from SwiftLint
   LINT_EXIT_CODE=$?

   # Check for any violations (warnings or errors)
   if [ $LINT_EXIT_CODE -ne 0 ]; then
     echo "============================"
     echo "⚠️ SwiftLint found violations."
     echo "❌ Aborting commit."
     echo "============================"
     exit 1
   else
     echo "============================"
     echo "✅ SwiftLint passed without violations!"
     echo "✅ Proceeding with commit."
     echo "============================"
   fi

   ```

5. **Make the Pre-Commit Hook Executable**

   After saving the file, make it executable by running:

   ```bash
   chmod +x pre-commit
   ```

6. **Verify the Setup**

   Now, each time you attempt to commit code, SwiftLint will automatically run. If there are any violations, the commit will be blocked until the issues are fixed.

   You can test it by running:

   ```bash
   git commit -m "Test commit"
   ```

   If no violations are found, the commit will proceed. Otherwise, you’ll see a message explaining why the commit was blocked.

---


