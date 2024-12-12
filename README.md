# Socrabytes.github.io

My personal website/blog built with Hugo, featuring technical articles, projects, and more. Visit the live site at [socrabytes.github.io](https://socrabytes.github.io).

## 🚀 Features

- Built with [Hugo](https://gohugo.io/) static site generator
- Theme: [Congo](https://github.com/jpanther/congo) with customizations
- Comments powered by [Giscus](https://giscus.app/) (GitHub Discussions)
- Full-text search functionality
- Dark/Light mode support
- Responsive design
- Tags for content organization
- Automatic image optimization
- Code syntax highlighting

## 💻 Tech Stack

- **Static Site Generator**: Hugo
- **Theme**: Congo
- **Hosting**: GitHub Pages
- **CI/CD**: GitHub Actions
- **Comments**: Giscus (GitHub Discussions integration)
- **Version Control**: Git

## 🛠️ Local Development

1. Clone the repository:
   ```bash
   git clone https://github.com/socrabytes/socrabytes.github.io.git
   cd socrabytes.github.io
   ```

2. Install Hugo (extended version required):
   ```bash
   # For Ubuntu/Debian
   sudo apt install hugo
   # For other systems, see: https://gohugo.io/installation/
   ```

3. Start the local development server:
   ```bash
   hugo server -D
   ```

4. View the site at: `http://localhost:1313`

## 🚀 Deployment

The site is automatically built and deployed using GitHub Actions whenever changes are pushed to the main branch. The workflow:

1. Triggers on push to main branch
2. Sets up Hugo environment
3. Builds the site
4. Deploys to GitHub Pages

The GitHub Actions workflow configuration can be found in `.github/workflows/hugo.yaml`.

## 📝 Content Management

Content is written in Markdown and organized in the following sections:
- `/content/tech-journal/` - Technical articles and blog posts
- `/content/projects/` - Project showcases
- `/content/about/` - About page
- `/content/legal/` - Legal documents

## 💬 Comments

Comments are powered by Giscus, which integrates GitHub Discussions with the website. Comments are automatically synchronized between the website and GitHub Discussions, providing:
- GitHub OAuth authentication for commenters
- Full GitHub-based moderation tools
- Automatic discussion creation for new pages
- Reactions support

## 📄 License

- Code: [MIT License](LICENSE)
- Content: See [CONTENT_LICENSE](CONTENT_LICENSE)

## 🤝 Contributing

While this is a personal website, if you find any issues or have suggestions, feel free to:
1. Open an issue
2. Submit a pull request
3. Start a discussion using Giscus

---
Built with ❤️ using Hugo
