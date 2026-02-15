# Personal Website Template

This is a professional, modern template for your first personal website hosted on GitHub Pages.

## 📁 Files Included

- `index.html` - Main HTML structure with semantic markup
- `styles.css` - Modern CSS with responsive design and animations
- `script.js` - Interactive JavaScript features
- `README.md` - This documentation file

## 🎨 Template Features

### Design
- **Clean & Modern**: Professional appearance suitable for developers
- **Responsive**: Works perfectly on desktop, tablet, and mobile
- **Dark Mode Ready**: Infrastructure for dark mode toggle (optional enhancement)

### Functionality
- **Smooth Scrolling**: Navigation links scroll smoothly to sections
- **Form Validation**: Contact form with real-time validation
- **Animations**: CSS transitions and JavaScript-powered animations
- **Interactive Elements**: Hover effects and dynamic content

### Sections
- **Hero Section**: Introduction with call-to-action
- **Skills Section**: Showcase your technical skills
- **Contact Section**: Contact form with validation
- **Footer**: Copyright and branding

## 🚀 Quick Start

1. **Copy Template Files**:
   ```bash
   # From your repository root
   cp template/* .
   ```

2. **Customize Content**:
   - Edit `index.html` to add your personal information
   - Modify `styles.css` to change colors and styling
   - Enhance `script.js` with additional features

3. **Deploy**:
   ```bash
   git add .
   git commit -m "Add personal website"
   git push origin main
   ```

4. **Enable GitHub Pages**:
   - Go to repository Settings → Pages
   - Select "main" branch and "/(root)" folder
   - Your site will be live at `https://yourusername.github.io`

## 🎯 Customization Guide

### Personal Information (index.html)

**Change the title and name:**
```html
<title>Your Name - Personal Website</title>
<!-- And in the nav and hero sections -->
<h1 class="nav-title">Your Name</h1>
<h2>Hello, I'm <span class="highlight">Your Name</span></h2>
```

**Update the hero description:**
```html
<p class="hero-description">Your personal description here.</p>
```

**Customize skills:**
```html
<div class="skill-card">
    <h3>Your Skill</h3>
    <p>Description of your expertise</p>
</div>
```

### Styling (styles.css)

**Change color scheme:**
```css
:root {
    --primary-color: #your-color;
    --secondary-color: #your-color;
    --accent-color: #your-color;
}
```

**Modify fonts:**
```css
body {
    font-family: 'Your Font', sans-serif;
}
```

**Adjust layout:**
```css
.hero-content h2 {
    font-size: your-size;
}
```

### JavaScript Enhancements (script.js)

**Add new interactive features:**
```javascript
// Example: Add a skill level indicator
function addSkillProgress() {
    const skillCards = document.querySelectorAll('.skill-card');
    skillCards.forEach(card => {
        const progress = document.createElement('div');
        progress.className = 'skill-progress';
        progress.innerHTML = '<div class="progress-bar" style="width: 75%"></div>';
        card.appendChild(progress);
    });
}

// Call it when page loads
document.addEventListener('DOMContentLoaded', function() {
    addSkillProgress();
});
```

## 🛠️ Advanced Customizations

### Adding New Sections

1. **Create HTML structure:**
```html
<section id="projects" class="projects">
    <div class="container">
        <h2>My Projects</h2>
        <div class="projects-grid">
            <!-- Project cards here -->
        </div>
    </div>
</section>
```

2. **Add CSS styling:**
```css
.projects {
    padding: 80px 0;
    background: var(--bg-color);
}

.projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 30px;
}
```

3. **Update navigation:**
```html
<li><a href="#projects">Projects</a></li>
```

### Adding Images

1. **Create an images folder:**
```bash
mkdir images
```

2. **Add images to your repository**
3. **Reference in HTML:**
```html
<img src="images/your-photo.jpg" alt="Your Photo" class="profile-photo">
```

### Form Enhancements

**Add more form fields:**
```html
<div class="form-group">
    <label for="subject">Subject</label>
    <select id="subject" name="subject">
        <option value="general">General Inquiry</option>
        <option value="project">Project Collaboration</option>
        <option value="job">Job Opportunity</option>
    </select>
</div>
```

**Update validation in JavaScript:**
```javascript
const subject = document.getElementById('subject').value;
if (!subject) {
    isValid = false;
    errors.push('Please select a subject');
}
```

## 📱 Responsive Design

The template is already responsive, but you can customize breakpoints:

```css
/* Large screens */
@media (min-width: 1200px) {
    .hero-content h2 {
        font-size: 4rem;
    }
}

/* Tablets */
@media (max-width: 768px) {
    .nav-container {
        flex-direction: column;
    }
}

/* Mobile phones */
@media (max-width: 480px) {
    .hero-content h2 {
        font-size: 2rem;
    }
}
```

## 🚀 Deployment Checklist

- [ ] Files copied from template folder
- [ ] Personal information updated
- [ ] Colors and styling customized
- [ ] Additional features added
- [ ] Tested locally (open index.html in browser)
- [ ] Files committed and pushed to GitHub
- [ ] GitHub Pages enabled in repository settings
- [ ] Website tested at live URL

## 🆘 Troubleshooting

### Common Issues

**Website not loading:**
- Wait 1-5 minutes after enabling GitHub Pages
- Check that files are in repository root
- Ensure repository is public

**Styling not applying:**
- Check file paths in HTML
- Verify CSS syntax in browser dev tools
- Clear browser cache

**JavaScript not working:**
- Check browser console for errors
- Ensure script tag has correct path
- Test JavaScript in browser dev tools

### Browser Testing

Test your site in multiple browsers:
- Chrome/Chromium
- Firefox
- Safari
- Edge

Use responsive design tools to test mobile layouts.

## 🎨 Inspiration & Resources

### Color Schemes
- **Cool Blue**: #2563eb, #64748b, #f59e0b
- **Warm Orange**: #ea580c, #dc2626, #2563eb
- **Forest Green**: #059669, #10b981, #f59e0b

### Font Combinations
- **Modern**: 'Segoe UI', sans-serif
- **Classic**: 'Georgia', serif
- **Tech**: 'Monaco', monospace

### Additional Resources
- [MDN Web Docs](https://developer.mozilla.org/)
- [CSS Tricks](https://css-tricks.com/)
- [JavaScript Info](https://javascript.info/)
- [Coolors](https://coolors.co/) - Color palette generator

---

Happy coding! This template is your starting point - customize it to reflect your unique personality and skills. As you learn more, come back and enhance it with new features and technologies.