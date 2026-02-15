# Homework 1: Git Fundamentals - Local Repository Mastery

**Section**: 1 - Introduction to Git
**Estimated Time**: 90-120 minutes
**Difficulty**: Beginner
**Prerequisites**: None

---

## 📋 Assignment Overview

In this homework, you'll master the fundamental concepts of Git version control. You'll install Git, create your first local repository, practice the basic Git workflow, and understand how Git tracks changes to your files. This hands-on approach will give you a solid foundation in version control before moving to collaborative features.

**Learning Objectives:**
- Install and configure Git on your system
- Understand Git's core concepts (repository, commits, working directory, staging area)
- Practice the basic Git workflow (init, add, commit, status, log)
- Experience Git's snapshot-based approach to version control
- Learn to navigate and understand repository history

---

## 🎯 Requirements

### Part 1: Git Installation & Configuration (25 points)

#### Task 1.1: Install Git
**Objective**: Install Git version control system on your computer

**Requirements:**
- Download Git from https://git-scm.com/downloads
- Install with default settings (recommended for beginners)
- Verify installation by checking the version

**Installation Verification:**
```bash
git --version
# Should show: git version 2.x.x
```

**Deliverables:**
- Screenshot showing successful Git installation
- Output of `git --version` command
- Confirmation that Git is properly installed

#### Task 1.2: Configure Git
**Objective**: Set up your Git identity and basic configuration

**Requirements:**
- Configure your name and email address
- Set up line ending preferences for your operating system
- Verify your configuration settings

**Git Configuration Commands:**
```bash
# Set your identity
git config --global user.name "Your Full Name"
git config --global user.email "your.email@example.com"

# Configure line endings (choose based on your OS)
# For Windows:
git config --global core.autocrlf true

# For macOS/Linux:
git config --global core.autocrlf input

# Verify configuration
git config --global --list
```

**Deliverables:**
- Screenshot of Git configuration commands and their output
- Confirmation that user.name and user.email are set correctly

---

### Part 2: Create Your First Git Repository (30 points)

#### Task 2.1: Initialize Local Repository
**Objective**: Create your first Git repository and understand its structure

**Requirements:**
- Create a new directory for your Git practice project
- Initialize it as a Git repository
- Explore the `.git` folder structure
- Understand what happens when you initialize a repository

**Repository Creation Steps:**
```bash
# Create a new directory for your project
mkdir git-fundamentals-practice
cd git-fundamentals-practice

# Initialize Git repository
git init

# Check what was created
ls -la
# You should see a .git folder

# Check repository status
git status
```

**Deliverables:**
- Screenshot showing the directory creation and `git init` command
- Screenshot of `ls -la` showing the `.git` folder
- Output of `git status` for an empty repository

#### Task 2.2: Add Your First Files
**Objective**: Practice adding files to Git and understanding the working directory

**Requirements:**
- Create several text files with different content
- Check repository status after each file creation
- Understand the difference between tracked and untracked files
- Add files to Git tracking

**File Creation and Tracking:**
```bash
# Create some files (run these commands one by one)
echo "This is my first Git repository!" > README.md
echo "print('Hello, Git World!')" > hello.py
mkdir notes
echo "Learning Git fundamentals" > notes/learning-notes.txt

# Check status after each file creation
git status

# Add files to Git tracking
git add README.md
git status

# Add all files
git add .
git status
```

**Deliverables:**
- Screenshots showing file creation commands
- Multiple `git status` outputs showing the progression from untracked to staged files

---

### Part 3: Understanding Commits & History (30 points)

#### Task 3.1: Make Your First Commits
**Objective**: Experience Git's snapshot-based version control

**Requirements:**
- Make your first commit with staged files
- Understand what a commit contains
- Modify files and create additional commits
- Practice writing meaningful commit messages

**Creating Commits:**
```bash
# Make your first commit
git commit -m "Initial commit: Add README and basic project structure"

# Check the commit history
git log
git log --oneline

# Modify a file and create another commit
echo "Added more content to README" >> README.md
git add README.md
git commit -m "Update README with additional information"

# Check history again
git log --oneline
```

**Deliverables:**
- Screenshot of your first `git commit` command and its output
- Screenshot of `git log` showing your commit history
- At least 3 commits with different meaningful messages

#### Task 3.2: Explore Git Concepts Hands-On
**Objective**: Demonstrate understanding of Git's core concepts

**Requirements:**
- Modify files in different ways (content changes, file deletions, renames)
- Use `git diff` to see changes before staging
- Practice unstaging files
- Understand the relationship between working directory, staging area, and repository

**Exploring Git States:**
```bash
# Create and modify files
echo "New feature implementation" > feature.txt
git status

# See changes before staging
git diff

# Stage the file
git add feature.txt
git status

# See staged changes
git diff --staged

# Modify the staged file
echo "Updated feature implementation" >> feature.txt

# Check status (file is both staged and modified)
git status
git diff    # Shows unstaged changes
git diff --staged  # Shows staged changes

# Commit current staged version
git commit -m "Add initial feature implementation"

# Stage the additional changes
git add feature.txt
git commit -m "Update feature with additional functionality"
```

**Deliverables:**
- Screenshots demonstrating different file states (untracked, modified, staged)
- Examples of `git diff` and `git diff --staged` output
- Commit history showing multiple commits with clear progression

#### Task 3.3: Repository History Analysis
**Objective**: Understand how Git tracks project history

**Requirements:**
- Create a comprehensive commit history
- Use different `git log` formats to view history
- Understand commit metadata (author, date, message)
- Analyze how your project evolved through commits

**Analyzing History:**
```bash
# Create more commits to build history
mkdir src
echo "def main():" > src/main.py
echo "    print('Git fundamentals project')" >> src/main.py
git add src/
git commit -m "Add main source code directory and entry point"

# Create documentation
echo "# Git Fundamentals Practice" > docs/project-overview.md
echo "This project demonstrates basic Git concepts." >> docs/project-overview.md
git add docs/
git commit -m "Add project documentation"

# View history in different formats
git log
git log --oneline
git log --pretty=format:"%h %ad | %s%d [%an]" --date=short
```

**Deliverables:**
- Repository with at least 5 commits showing project progression
- Screenshots of different `git log` formats
- Analysis of how your repository history tells the story of your project development

---

### Part 4: Git Concepts Demonstration & Analysis (15 points)

#### Task 4.1: Demonstrate Git Understanding
**Objective**: Show mastery of Git concepts through practical examples

**Requirements:**
- Create a scenario that demonstrates Git's advantages over manual file management
- Show how Git tracks changes and enables experimentation
- Explain the difference between Git and simple file backups

**Practical Demonstration:**
```bash
# Create a "backup" scenario manually (the old way)
cp -r . ../git-practice-backup-manual
echo "Manual backup created at $(date)" > ../git-practice-backup-manual/backup-info.txt

# Now show how Git handles the same scenario
echo "Experimenting with new features" > experimental-feature.txt
git add experimental-feature.txt
git commit -m "Start experimental feature branch"

# Modify the experimental feature
echo "Updated experimental implementation" >> experimental-feature.txt
git add experimental-feature.txt
git commit -m "Improve experimental feature"

# Realize the experiment isn't working - easy rollback with Git
git log --oneline
git reset --hard HEAD~2  # Go back to before the experiment

# The experimental files are gone, but we can get them back if needed
git reflog
git checkout <commit-hash>  # Could restore if we wanted
```

**Deliverables:**
- Demonstration of Git's snapshot-based approach vs manual backups
- Examples showing how Git enables safe experimentation
- Explanation of Git's advantages over traditional file management

#### Task 4.2: Repository Analysis & Summary
**Objective**: Analyze your Git repository and summarize learning

**Requirements:**
- Provide a complete analysis of your repository structure
- Explain what each Git command you used accomplished
- Reflect on how Git changes your approach to file management
- Document any challenges faced and solutions found

**Repository Analysis:**
- Repository structure and file organization
- Commit history and progression
- Git commands used and their purposes
- Key concepts demonstrated
- Personal insights about version control

**Deliverables:**
- Complete repository analysis document
- Summary of Git concepts learned
- Personal reflection on version control benefits

---

## 📋 Repository Structure Template

For your Git practice project, consider organizing your files like this:

```
git-fundamentals-practice/
├── README.md                    # Project description and instructions
├── hello.py                     # Simple Python script
├── notes/
│   └── learning-notes.txt       # Your Git learning notes
├── src/
│   └── main.py                  # Main application code
├── docs/
│   └── project-overview.md      # Project documentation
└── experimental-feature.txt     # Feature development file
```

**Recommended Project Structure:**
- `README.md`: Describe your project and Git learning journey
- `src/`: Source code files
- `docs/`: Documentation and notes
- `notes/`: Personal learning notes and observations

---

## 📋 Submission Requirements

### Required Files
- **Local Git Repository**: Your `git-fundamentals-practice` directory with complete history
- **Repository Screenshots**: Screenshots of `git status`, `git log`, and repository structure
- **Command Outputs**: Screenshots showing Git commands and their results
- **Learning Documentation**: Your notes and observations about Git concepts

### Written Documentation
- **Git Command Journal**: Step-by-step record of commands used and their purposes
- **Concept Explanations**: Your understanding of Git concepts (repository, commits, staging, etc.)
- **Challenges & Solutions**: Any difficulties encountered and how you resolved them
- **Git vs Manual Comparison**: Analysis of Git's advantages over traditional file management

### Submission Format
- Create folder: `homework-01-git-fundamentals`
- Include repository folder and all documentation
- Submit repository analysis and command journal

---

## 🎯 Evaluation Criteria

### Git Installation & Configuration (25%)
- Successful Git installation and version verification
- Proper Git user configuration (name and email)
- Correct system-specific settings (line endings)
- Configuration verification and documentation

### Repository Creation & Management (30%)
- Proper repository initialization with `git init`
- Understanding of `.git` folder structure
- Correct file staging and committing process
- Meaningful commit messages and history

### Git Concepts Understanding (25%)
- Demonstration of Git's three areas (working directory, staging, repository)
- Proper use of `git status`, `git diff`, and `git log`
- Understanding of file states and transitions
- Safe experimentation and rollback capabilities

### Analysis & Documentation (20%)
- Clear explanation of Git concepts and commands
- Repository structure analysis and justification
- Personal reflection on version control benefits
- Professional documentation and presentation

---

## 🆘 Troubleshooting

### GitHub Pages Issues
**Site not loading:**
- Wait 1-5 minutes after enabling Pages
- Check repository name format: `username.github.io`
- Ensure repository is public

**404 errors:**
- Check that `index.html` is in repository root
- Verify GitHub Pages source settings

### Git Issues
**Permission denied:**
- Ensure you're using correct GitHub credentials
- Check if repository exists and is accessible

**Changes not appearing:**
- Ensure files are committed and pushed
- Clear browser cache when testing

### Code Issues
**JavaScript not working:**
- Check browser console for errors
- Ensure script tag has correct path

**CSS not applying:**
- Verify file paths in HTML
- Check for syntax errors in CSS

---

## 📞 Getting Help

### Resources
- **GitHub Pages Guide**: https://pages.github.com/
- **HTML/CSS/JS Tutorials**: MDN Web Docs, freeCodeCamp
- **GitHub Learning Lab**: Hands-on GitHub tutorials
- **W3Schools**: Interactive web development examples

### Community Support
- **Course Discord**: #web-development channel
- **Stack Overflow**: Tag questions with `github-pages`
- **GitHub Community**: https://github.community

### Debug Tools
- **Browser DevTools**: Inspect elements and debug JavaScript
- **GitHub Repository Settings**: Check Pages configuration
- **W3C Validators**: HTML and CSS validation tools

---

## ✅ Success Criteria

Your Git fundamentals homework is complete when:

- ✅ Git successfully installed and configured on your system
- ✅ Local repository created with proper `git init`
- ✅ Files added, staged, and committed with meaningful messages
- ✅ Repository history demonstrates understanding of Git workflow
- ✅ Different file states experienced (untracked, modified, staged, committed)
- ✅ Git commands properly used: `status`, `add`, `commit`, `log`, `diff`
- ✅ Safe experimentation demonstrated with rollback capabilities
- ✅ Repository structure and history properly analyzed
- ✅ Personal understanding of Git concepts documented

---

**Estimated Completion Time**: 90-120 minutes
**Difficulty Level**: Beginner
**Points**: 100

Congratulations on mastering Git fundamentals! You now understand the core concepts that power modern software development. This foundation will serve you well as you progress to collaborative development and advanced Git features.

---

**Pro Tips:**
- Practice Git commands regularly to build muscle memory
- Use `git status` frequently to understand your repository state
- Write clear, descriptive commit messages
- Experiment safely - Git makes it easy to undo mistakes
- Keep learning advanced Git features as you progress