# PressCanvas

**Where words find their canvas** — A beautiful static blog platform designed for GitHub Pages.

PressCanvas is a lightweight, elegant blogging platform that combines the simplicity of static sites with the power of modern web design. Perfect for writers, creators, and anyone who values beautiful typography and distraction-free content creation.

## ✨ Features

- **Beautiful Design**: Editorial-inspired layout with elegant typography (Playfair Display + Source Sans Pro)
- **Static & Fast**: Pure HTML/CSS/JS - no server required, perfect for GitHub Pages
- **Client-Side Editor**: Built-in markdown editor with live preview
- **Responsive**: Looks great on all devices
- **Search & Filter**: Find content by tags or search terms
- **Social Sharing**: Built-in sharing buttons for Twitter, LinkedIn, and copy-to-clipboard
- **Reading Progress**: Visual progress bar for long-form content
- **Accessible**: Semantic HTML with proper ARIA labels and screen reader support

## 🚀 Quick Start

### 1. Deploy to GitHub Pages

1. **Fork or download** this repository
2. **Enable GitHub Pages** in your repository settings:
   - Go to Settings → Pages
   - Source: Deploy from a branch
   - Branch: `main` (or `master`)
   - Folder: `/ (root)`
3. **Visit your site** at `https://yourusername.github.io/your-repo-name`

### 2. Customize Your Site

Edit the following files to make it yours:

- **`index.html`**: Update site title, description, and branding
- **`assets/logo.png`**: Replace with your own logo
- **`assets/favicon.ico`**: Add your custom favicon

## 📝 Adding New Posts

PressCanvas uses a simple workflow for adding new posts:

### Method 1: Using the Built-in Editor

1. **Visit the editor** at `/editor.html` on your live site
2. **Fill in your post details**:
   - Title (auto-generates slug)
   - Excerpt (for post previews)
   - Author name
   - Content in Markdown format
3. **Click "Generate Markdown File"**
4. **Copy the generated content**
5. **Create a new file** in your GitHub repository:
   - Go to your repo on GitHub
   - Navigate to `_posts/` folder
   - Click "Add file" → "Create new file"
   - Name it `your-post-slug.md`
   - Paste the generated content
   - Commit the file

### Method 2: Manual Creation

Create a new `.md` file in the `_posts/` directory with this frontmatter format:

\`\`\`markdown
---
title: "Your Post Title"
slug: "your-post-slug"
date: "2024-01-15"
excerpt: "A compelling summary of your post..."
author: "Your Name"
cover: "/placeholder.svg?height=400&width=600"
tags: ["writing", "technology"]
---

# Your Post Title

Your content here in Markdown format...
\`\`\`

### Method 3: GitHub CLI (Advanced)

If you have GitHub CLI installed:

\`\`\`bash
# Create new post file
gh repo clone yourusername/your-repo-name
cd your-repo-name
echo "---
title: \"Your Post Title\"
slug: \"your-post-slug\"
date: \"$(date +%Y-%m-%d)\"
excerpt: \"Your post excerpt...\"
author: \"Your Name\"
cover: \"\"
tags: []
---

# Your Post Title

Your content here..." > _posts/your-post-slug.md

# Commit and push
git add _posts/your-post-slug.md
git commit -m "Add new post: Your Post Title"
git push
\`\`\`

## 📊 Updating the Posts Index

After adding a new post, you need to update `data/posts.json`:

1. **Open `data/posts.json`** in your GitHub repository
2. **Add your new post** to the beginning of the array:

\`\`\`json
{
  "title": "Your Post Title",
  "slug": "your-post-slug",
  "date": "2024-01-15",
  "excerpt": "A compelling summary...",
  "author": "Your Name",
  "cover": "/placeholder.svg?height=400&width=600",
  "tags": ["writing", "technology"]
}
\`\`\`

3. **Commit the changes**

> **Note**: This manual step is required because GitHub Pages doesn't support server-side processing. For automatic index generation, see the "Advanced Setup" section below.

## 🎨 Customization

### Colors & Branding

The site uses CSS custom properties for easy theming. Edit the `<script>` section in each HTML file:

\`\`\`javascript
tailwind.config = {
  theme: {
    extend: {
      colors: {
        'primary': '#7c3aed',    // Your brand color
        'accent': '#8b5cf6',     // Accent color
        'neutral-dark': '#374151' // Text color
      }
    }
  }
}
\`\`\`

### Typography

Fonts are loaded from Google Fonts. To change them, update the `<link>` tags and font family definitions:

\`\`\`html
<link href="https://fonts.googleapis.com/css2?family=Your+Heading+Font&family=Your+Body+Font&display=swap" rel="stylesheet">
\`\`\`

### Layout & Styling

- **Homepage**: Edit `index.html`
- **Post template**: Edit `post.html`
- **Editor**: Edit `editor.html`
- **Global styles**: Modify the `<style>` sections in each file

## 🔧 Advanced Setup

### Automated Post Index Generation

For automatic `posts.json` generation, you can use GitHub Actions:

1. **Create `.github/workflows/update-posts.yml`**:

\`\`\`yaml
name: Update Posts Index
on:
  push:
    paths: ['_posts/**']
jobs:
  update-index:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Generate posts.json
        run: |
          echo '[' > data/posts.json
          for file in _posts/*.md; do
            # Extract frontmatter and generate JSON
            # (Add your parsing logic here)
          done
          echo ']' >> data/posts.json
      - name: Commit changes
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add data/posts.json
          git commit -m "Auto-update posts index" || exit 0
          git push
\`\`\`

### Content Management System Integration

Replace the basic editor with a full CMS:

#### Option 1: Netlify CMS
- **Pros**: User-friendly interface, media management
- **Setup**: Add `admin/` folder with `config.yml`
- **Guide**: [Netlify CMS with GitHub](https://www.netlifycms.org/docs/github-backend/)

#### Option 2: Forestry (now TinaCMS)
- **Pros**: Live preview, advanced editing features
- **Setup**: Connect your GitHub repo
- **Guide**: [TinaCMS Documentation](https://tina.io/docs/)

#### Option 3: Decap CMS (Netlify CMS fork)
- **Pros**: Open source, self-hosted option
- **Setup**: Similar to Netlify CMS
- **Guide**: [Decap CMS Documentation](https://decapcms.org/docs/)

### Custom Domain

1. **Add a CNAME file** to your repository root:
   \`\`\`
   yourdomain.com
   \`\`\`
2. **Configure DNS** with your domain provider:
   - Add a CNAME record pointing to `yourusername.github.io`
3. **Enable HTTPS** in GitHub Pages settings

## 📁 Project Structure

\`\`\`
presscanvas-blog/
├── index.html              # Homepage with post listing
├── post.html               # Individual post renderer
├── editor.html             # Content creation interface
├── _posts/                 # Markdown post files
│   ├── example-post-1.md
│   ├── example-post-2.md
│   └── example-post-3.md
├── data/
│   └── posts.json          # Post metadata index
├── assets/
│   ├── logo.svg            # Site logo
│   ├── favicon.ico         # Site favicon
│   └── images/             # Post images
└── README.md               # This file
\`\`\`

## 🚨 Limitations & Considerations

### Current Limitations

1. **Manual Index Updates**: Adding posts requires manually updating `posts.json`
2. **No Server-Side Features**: No comments, user accounts, or dynamic features
3. **Basic Search**: Client-side search only, limited functionality
4. **Image Management**: No built-in image upload or optimization
5. **No Analytics**: Requires external analytics integration

### Performance Considerations

- **Large Sites**: With 100+ posts, consider pagination improvements
- **Images**: Optimize images before adding to reduce load times
- **Caching**: GitHub Pages provides good caching, but consider CDN for heavy traffic

### Security Notes

- **Static Only**: No server-side vulnerabilities, but ensure external links are safe
- **Content Validation**: Validate markdown content to prevent XSS in comments/shares

## 🛠️ Development

### Local Development

1. **Clone the repository**
2. **Serve locally**:
   \`\`\`bash
   # Using Python
   python -m http.server 8000
   
   # Using Node.js
   npx serve .
   
   # Using PHP
   php -S localhost:8000
   \`\`\`
3. **Visit** `http://localhost:8000`

### Making Changes

- **HTML/CSS/JS**: Edit files directly
- **Posts**: Add to `_posts/` and update `data/posts.json`
- **Styling**: Modify Tailwind classes or add custom CSS

## 🤝 Contributing

Contributions are welcome! Please:

1. **Fork the repository**
2. **Create a feature branch**
3. **Make your changes**
4. **Test thoroughly**
5. **Submit a pull request**

### Ideas for Contributions

- **Themes**: Additional color schemes and layouts
- **Features**: Enhanced search, categories, author pages
- **Integrations**: Analytics, comments, newsletter signup
- **Performance**: Lazy loading, image optimization
- **Accessibility**: Enhanced screen reader support

## 📄 License

MIT License - feel free to use PressCanvas for personal or commercial projects.

## 🙏 Acknowledgments

- **Design Inspiration**: The New Yorker, Medium, and other editorial publications
- **Typography**: Google Fonts (Playfair Display, Source Sans Pro)
- **Icons**: Heroicons
- **Framework**: Tailwind CSS
- **Markdown**: Marked.js

## 📞 Support

- **Issues**: Report bugs via GitHub Issues
- **Questions**: Start a GitHub Discussion
- **Documentation**: Check this README and inline comments

---

**Happy writing!** 🎨✍️

*PressCanvas - Where words find their canvas*
