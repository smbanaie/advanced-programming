# Homework 3: Git States Mastery - Understanding File Lifecycle

**Section**: 3 - Understanding Git States
**Estimated Time**: 100-130 minutes
**Difficulty**: Intermediate
**Prerequisites**: Homework 1 & 2 (Git Fundamentals & Repository Operations)

---

## 📋 Assignment Overview

In this homework, you'll become a master of Git's state management system. You'll explore Git's three main areas (working directory, staging area, repository), practice all possible file state transitions, and learn to troubleshoot common state-related issues. This deep understanding of Git's internal workings will make you a confident and effective version control user.

**Learning Objectives:**
- Master Git's three areas: working directory, staging area, and repository
- Understand and control all file states (untracked, modified, staged, committed)
- Practice state transitions using appropriate Git commands
- Use diff commands to compare between states
- Troubleshoot and resolve common state management issues
- Develop intuition for Git's snapshot-based approach

---

## 🎯 Requirements

### Part 1: State Exploration Repository Setup (20 points)

#### Task 1.1: Create Practice Repository
**Objective**: Set up a dedicated repository for exploring Git states

**Requirements:**
- Create a new local Git repository for state practice
- Initialize with a clear project structure
- Add initial files to establish baseline state

**Repository Setup:**
```bash
# Create new directory for state practice
mkdir git-states-practice
cd git-states-practice

# Initialize repository
git init

# Create initial project structure
mkdir src docs notes
echo "# Git States Practice Project" > README.md
echo "print('Exploring Git states')" > src/main.py
echo "# Project notes" > notes/learning.md

# Check initial status
git status
```

**Deliverables:**
- Local repository initialized and configured
- Initial project structure created
- Screenshot of `git status` showing untracked files

#### Task 1.2: Establish Repository History
**Objective**: Create a commit history to work with state transitions

**Requirements:**
- Add and commit initial files
- Create multiple commits to establish history
- Verify repository state after each operation

**Initial Commits:**
```bash
# Add and commit initial files
git add README.md
git commit -m "Initial commit: Add project README"

git add src/
git commit -m "Add source code directory and main script"

git add notes/
git commit -m "Add notes directory for learning documentation"

# Verify repository state
git status
git log --oneline
```

**Deliverables:**
- Repository with 3+ commits establishing history
- Clean repository state (no untracked/modified files)
- Commit history log

---

### Part 2: File State Exploration (35 points)

#### Task 2.1: Demonstrate All File States
**Objective**: Experience every possible file state in Git

**Requirements:**
- Create files in each state: untracked, modified, staged, committed
- Use `git status` to observe state changes
- Document the characteristics of each state

**State Demonstration:**
```bash
# Start with clean repository (from Part 1)

# 1. Create untracked file
echo "This is an untracked file" > untracked.txt
git status  # Should show: Untracked files: untracked.txt

# 2. Modify existing tracked file
echo "Modified content" >> src/main.py
git status  # Should show: Changes not staged for commit: modified: src/main.py

# 3. Stage the modified file
git add src/main.py
git status  # Should show: Changes to be committed: modified: src/main.py

# 4. Create and stage a new file
echo "New staged file" > staged-new.txt
git add staged-new.txt
git status  # Should show both files staged

# Document each state observation
```

**Deliverables:**
- Screenshots showing each file state
- Clear documentation of state characteristics
- Understanding of how files move between states

#### Task 2.2: State Transition Practice
**Objective**: Master moving files between all possible states

**Requirements:**
- Practice all state transitions using appropriate commands
- Use `git diff` to compare between states
- Understand when and why to use each transition

**Transition Exercises:**
```bash
# From staged back to modified (unstaging)
git reset HEAD src/main.py
git status  # File should be modified but not staged

# From modified to staged (staging)
git add src/main.py
git status  # File should be staged again

# From staged to committed (committing)
git commit -m "Update main script with new functionality"
git status  # Should be clean

# Discard working directory changes (modified → committed)
echo "Temporary change" >> src/main.py
git status  # Shows modified
git checkout -- src/main.py  # Discard changes
git status  # Should be clean again
```

**Deliverables:**
- Successful execution of all state transitions
- Screenshots of `git status` after each transition
- Understanding of appropriate commands for each transition
cp /path/to/course/Topic-01-Development-Environment/resources/github-pages-template/* .

# Verify files are present
ls -la
```

**Deliverables:**
- Screenshot of downloaded template files
- Local directory structure verification
- Template README review confirmation

#### Task 2.2: Local Development Environment
**Objective**: Set up local web development environment

**Requirements:**
- Open HTML files in a web browser for testing
- Use a code editor (VS Code recommended) for editing
- Set up live preview if possible (optional but recommended)

**Development Setup:**
1. **Browser Testing**: Open `index.html` directly in Chrome/Firefox
2. **Code Editor**: Use VS Code with HTML/CSS/JS extensions
3. **Live Preview**: Install "Live Server" extension in VS Code (optional)

**Deliverables:**
- Screenshot of template opening in web browser
- Code editor setup confirmation
- Basic template functionality verification

---

### Part 3: Advanced State Management & Troubleshooting (30 points)

#### Task 3.1: Complex State Scenarios
**Objective**: Handle complex file state situations

**Requirements:**
- Create files that exist in multiple states simultaneously
- Practice selective staging with `git add -p`
- Understand partial commits and staging

**Advanced State Management:**
```bash
# Create a file with multiple changes
echo "Line 1" > complex-file.txt
echo "Line 2" >> complex-file.txt
echo "Line 3" >> complex-file.txt
echo "Line 4" >> complex-file.txt

# Stage the file
git add complex-file.txt
git status  # Shows staged

# Modify the staged file (creates both staged and unstaged changes)
echo "Line 5 - unstaged" >> complex-file.txt
git status  # Shows: staged changes and unstaged changes

# Use diff to see the differences
git diff    # Shows unstaged changes
git diff --staged  # Shows staged changes

# Commit only staged changes
git commit -m "Add initial content to complex file"

# Check what remains
git status
git diff
```

**Deliverables:**
- Demonstration of files with multiple simultaneous states
- Understanding of staged vs unstaged changes
- Selective commit capabilities

#### Task 3.2: State Troubleshooting Scenarios
**Objective**: Learn to troubleshoot common state-related issues

**Requirements:**
- Create and resolve common state problems
- Practice recovery techniques
- Understand when to use different commands

**Troubleshooting Exercises:**
```bash
# Scenario 1: Accidentally staged wrong file
echo "Secret data" > sensitive.txt
git add sensitive.txt  # Oops, staged sensitive file
git status

# Unstage the file without losing changes
git reset HEAD sensitive.txt
git status  # File is modified but not staged

# Remove the file completely
rm sensitive.txt
git status  # Shows deleted file

# Scenario 2: Discard unwanted changes
echo "Unwanted changes" >> src/main.py
git status  # Shows modified

# Discard changes and revert to last commit
git checkout -- src/main.py
git status  # Should be clean

# Scenario 3: Use git diff effectively
echo "Change 1" >> src/main.py
echo "Change 2" >> src/main.py
git diff  # See working directory changes

git add src/main.py
echo "Change 3" >> src/main.py
git diff     # Shows unstaged changes
git diff --staged  # Shows staged changes
```

**Deliverables:**
- Successful resolution of state issues
- Understanding of appropriate commands for different scenarios
- Recovery techniques for common mistakes

### Part 4: State Analysis & Documentation (15 points)

#### Task 4.1: Repository State Analysis
**Objective**: Document comprehensive understanding of Git states

**Requirements:**
- Analyze the final state of your repository
- Document all state transitions performed
- Explain the purpose and effect of each Git command used

**State Analysis:**
- Complete repository state documentation
- File state transitions performed
- Command purposes and effects
- Troubleshooting scenarios encountered and resolved

**Deliverables:**
- Comprehensive state analysis document
- Repository state verification
- Understanding of Git's state management system

#### Task 4.2: Git States Mastery Reflection
**Objective**: Reflect on learning about Git's state management

**Requirements:**
- Explain how understanding Git states improves development workflow
- Describe the advantages of Git's three-area system
- Document personal insights about version control state management

**Reflection Topics:**
- How Git states enable safe development practices
- Benefits of staging area for selective commits
- Importance of understanding file states for troubleshooting
- Real-world applications of state management knowledge

**Deliverables:**
- Personal reflection on Git states learning
- Understanding of state management benefits
- Future applications of learned concepts
**Objective**: Customize skills section to reflect your expertise

**Requirements:**
- Update skill cards with your actual technologies
- Adjust proficiency levels and descriptions
- Add or remove skills based on your experience
- Include both technical and soft skills

**Skills Section Structure:**
```html
<div class="skill-card">
    <i class="fab fa-python"></i>
    <h3>Python</h3>
    <p>Advanced programming, data analysis, web development</p>
</div>
```

**Skills to Include:**
- Programming languages you've mastered
- Frameworks and libraries you use
- Development tools and technologies
- Soft skills relevant to development

**Deliverables:**
- Customized skills section
- Screenshot of updated skills grid
- Skills relevance explanation

#### Task 3.3: Projects Portfolio Integration
**Objective**: Showcase your best projects on the website

**Requirements:**
- Add 3-6 of your best projects
- Include project descriptions and technologies used
- Add links to GitHub repositories or live demos
- Use appropriate icons or images for each project

**Project Information to Include:**
- Project title and brief description
- Technologies/languages used
- GitHub repository link
- Live demo link (if available)
- Key features or achievements

**Deliverables:**
- Projects section with your actual work
- Screenshots of project cards
- Links verification (working GitHub/demo links)

#### Task 3.4: Visual Design Customization
**Objective**: Personalize the website's visual appearance

**Requirements:**
- Modify color scheme to match your preferences
- Update fonts if desired (stick to web-safe fonts)
- Adjust layout spacing and typography
- Ensure design remains professional and readable

**CSS Customization Areas:**
```css
/* Primary colors */
.btn-primary {
    background: #3498db; /* Change to your preferred color */
}

/* Hero section gradient */
.hero {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    /* Modify gradient colors */
}
```

**Design Principles:**
- Maintain readability (good contrast)
- Keep professional appearance
- Ensure mobile responsiveness
- Use consistent color scheme

**Deliverables:**
- Customized CSS with personal styling
- Before/after design comparison screenshots
- Design rationale explanation

---

### Part 4: Repository Setup & Deployment (20 points)

#### Task 4.1: Git Repository Initialization
**Objective**: Set up local Git repository for your portfolio

**Requirements:**
- Initialize Git repository in your portfolio directory
- Add all template and customized files
- Create meaningful commit messages
- Set up proper .gitignore file

**Git Setup Process:**
```bash
# Initialize repository
git init

# Add all files
git add .

# Create initial commit
git commit -m "Initial commit: Add portfolio template"

# Add GitHub remote
git remote add origin https://github.com/yourusername/yourusername.github.io.git

# Push to GitHub
git push -u origin main
```

**Deliverables:**
- Git repository initialization confirmation
- Successful push to GitHub Pages repository
- Repository file structure on GitHub

#### Task 4.2: GitHub Pages Deployment Verification
**Objective**: Ensure website is live and accessible

**Requirements:**
- Wait for GitHub Pages deployment to complete
- Verify website loads correctly at your GitHub Pages URL
- Test all links and functionality
- Check mobile responsiveness

**Deployment Verification:**
1. **URL Access**: Visit `https://yourusername.github.io`
2. **Content Verification**: Check all sections load properly
3. **Link Testing**: Verify all internal and external links work
4. **Mobile Testing**: Test on different screen sizes

**Common Issues to Check:**
- Repository name matches username.github.io format
- Files are in the root directory (not in a subfolder)
- No build process required (static HTML/CSS/JS only)
- GitHub Pages is enabled in repository settings

**Deliverables:**
- Live website URL confirmation
- Screenshots of deployed website
- Functionality testing results

#### Task 4.3: Repository Maintenance Setup
**Objective**: Configure repository for ongoing maintenance

**Requirements:**
- Add repository description and topics
- Set up repository social media preview
- Configure repository settings for portfolio presentation

**Repository Configuration:**
- **Description**: "Personal portfolio and project showcase"
- **Topics**: `portfolio`, `web-development`, `personal-website`
- **Website**: Your GitHub Pages URL
- **Social Preview**: Add repository image (optional)

**Deliverables:**
- Configured repository settings screenshot
- Repository topics and description
- Professional repository presentation

---

### Part 5: Advanced Features & Testing (15 points)

#### Task 5.1: JavaScript Enhancement
**Objective**: Add interactive features to your portfolio

**Requirements:**
- Enhance existing JavaScript functionality
- Add new interactive features
- Ensure all JavaScript works correctly

**Enhancement Ideas:**
- Add project filtering functionality
- Implement smooth scroll to sections
- Create a contact form with validation
- Add dark/light mode toggle

**Deliverables:**
- Enhanced JavaScript functionality
- New features demonstration
- JavaScript error-free operation

#### Task 5.2: Cross-Browser & Mobile Testing
**Objective**: Ensure website works across different platforms

**Requirements:**
- Test on multiple web browsers (Chrome, Firefox, Safari, Edge)
- Verify mobile responsiveness
- Check tablet and desktop layouts
- Validate accessibility features

**Testing Checklist:**
- [ ] Chrome browser compatibility
- [ ] Firefox browser compatibility
- [ ] Mobile device testing
- [ ] Tablet responsiveness
- [ ] Desktop layout verification
- [ ] Touch interaction testing

**Deliverables:**
- Cross-browser testing results
- Mobile responsiveness screenshots
- Accessibility validation confirmation

#### Task 5.3: Performance Optimization
**Objective**: Optimize website loading and performance

**Requirements:**
- Minimize file sizes where possible
- Optimize images (if added)
- Ensure fast loading times
- Test with browser developer tools

**Optimization Tasks:**
1. **Minimize CSS/JS**: Remove unnecessary code
2. **Optimize Images**: Compress any images added
3. **Reduce HTTP Requests**: Combine files if possible
4. **Enable Compression**: Ensure GitHub Pages compression

**Deliverables:**
- Performance optimization results
- Loading speed test results
- Optimization techniques applied

---

## 📋 Submission Requirements

### Repository Submission
- **Local Repository**: Your `git-states-practice` directory with complete history
- **Command Logs**: Screenshots of all Git operations and state observations
- **State Documentation**: Detailed records of file states and transitions

### Documentation Deliverables
- **State Transition Journal**: Complete log of all state changes performed
- **Command Analysis**: Explanation of each Git command used and its effects
- **Troubleshooting Report**: Issues encountered and how they were resolved
- **State Management Reflection**: Personal insights about Git states

### Submission Format
- Create folder: `homework-03-git-states`
- Include repository with all state transition history
- Document all commands used and their purposes
- Provide clear explanations of state management concepts

---

## 🎯 Evaluation Criteria

### Repository Setup & History (20%)
- Proper repository initialization and structure
- Establishment of commit history for state practice
- Clean baseline state for exercises

### File State Understanding (30%)
- Demonstration of all file states (untracked, modified, staged, committed)
- Proper use of `git status` to observe states
- Understanding of state characteristics and transitions

### State Transition Mastery (30%)
- Successful execution of all state transitions
- Appropriate use of Git commands (add, reset, commit, checkout)
- Effective use of diff commands to compare states

### Troubleshooting & Analysis (20%)
- Resolution of common state management issues
- Documentation of state management understanding
- Reflection on Git's state management benefits
- Visual design cohesive and appealing

### Technical Implementation (25%)
- HTML/CSS/JS properly structured and functional
- Responsive design working across devices
- Interactive features implemented correctly

### Deployment & Maintenance (15%)
- Successful website deployment and accessibility
- Repository properly maintained and documented
- Professional online presence established

### Advanced Features (10%)
- JavaScript enhancements implemented
- Performance optimizations applied
- Cross-browser compatibility achieved

### Documentation (5%)
- Clear customization documentation
- Technical implementation explained
- Future maintenance guidelines provided

---

## 🆘 Troubleshooting

### GitHub Pages Issues

**Repository Name Problems:**
- Must be exactly `username.github.io`
- Check for typos in repository name
- Ensure username matches your GitHub account

**Deployment Delays:**
- GitHub Pages can take 5-10 minutes to deploy
- Check repository settings if deployment fails
- Verify files are in root directory

**404 Errors:**
- Repository must be public
- GitHub Pages must be enabled
- Check repository name matches URL

### Template Customization Issues

**Styling Not Applying:**
- Check CSS file path in HTML
- Verify CSS syntax is correct
- Clear browser cache after changes

**JavaScript Not Working:**
- Check script file path in HTML
- Verify JavaScript console for errors
- Test functions individually

**Mobile Responsiveness:**
- Test with browser developer tools
- Check CSS media queries
- Verify viewport meta tag

---

## 📞 Getting Help

### Resources
- **GitHub Pages Documentation**: https://pages.github.com
- **HTML/CSS/JS Tutorials**: MDN Web Docs, W3Schools
- **Template Documentation**: Check template README.md
- **Course Discord**: #web-development-help

### Common Questions
- "How do I change the color scheme?"
- "My website isn't showing up - what do I do?"
- "How do I add my own projects?"
- "Can I use a custom domain?"

---

## ✅ Success Criteria

Your Git states homework is complete when:

- ✅ Repository created with proper baseline commit history
- ✅ All file states experienced and documented (untracked, modified, staged, committed)
- ✅ State transitions performed using appropriate Git commands
- ✅ Complex state scenarios handled successfully
- ✅ Common state issues troubleshooted and resolved
- ✅ Diff commands used effectively to compare states
- ✅ State management understanding demonstrated
- ✅ Personal insights about Git states documented

---

**Estimated Completion Time**: 100-130 minutes
**Difficulty Level**: Intermediate
**Points**: 100

Congratulations on mastering Git's state management system! You now have deep insight into how Git tracks and manages file changes. This understanding will make you a much more effective and confident Git user.

---

**Pro Tips:**
- Always check `git status` before committing to understand your repository state
- Use `git diff` and `git diff --staged` to review changes before committing
- The staging area gives you control - use it to create focused, logical commits
- Understanding states makes troubleshooting much easier