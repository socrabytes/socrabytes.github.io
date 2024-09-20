---
title: "Building My Personal Site with Hugo and Congo"
slug: "Building-My-Personal-site-with-Hugo-and-Congo"
description: "In this post, I share my journey of building this very website using Hugo and the Congo theme. From setting up the environment to tweaking the configurations, here are the steps I followed and the lessons I learned."
summary: "A practical guide to building a personal website using Hugo and the Congo theme, covering setup, customization, and deployment to GitHub Pages."
date: 2024-08-06
tags: ["Static Site Generators", "Hugo", "Congo Theme", "GitHub Pages", "Continuous Deployment"]
categories: ["Technical Projects"]
cover: "hugo-cover.png"
coverAlt: "Hugo Website logo with slogan from Hugo's Homepage"
thumbnail: "HUGO-site-thumb.webp"
thumbnailAlt: "Hugo Site Thumbnail"
draft: false
---

In this technical deep dive, I document the process of constructing a personal website using Hugo, a high-performance static site generator, paired with the Congo theme—a minimalist, highly customizable theme. This post explores the intricate details of setting up the environment, configuring site parameters, organizing content directories, and implementing live previews with Hugo's server capabilities.

## **1. Environment Setup and Tooling**
The development environment was established with the following tools:
- **Hugo**: Deployed as the core static site generator due to its efficiency and flexibility in managing content.
- **Congo Theme**: Integrated via Git submodule to ensure seamless updates. Command executed:
	```shell
	git submodule add -b stable https://github.com/jpanther/congo.git themes/congo
	```
- **GitHub Pages**: Chosen for hosting, with continuous deployment facilitated by GitHub Actions.
### **1.1. Initializing the Hugo Project**
The project was initialized with Hugo, which generated the necessary directory structure. This structure was then tailored to align with the Congo theme's requirements:
```shell
hugo new site socrabytes.github.io
```
## **2. Directory Structure and Content Organization**
Understanding Hugo's directory structure is crucial, especially when working with themes like Congo that leverage Hugo’s content management features.
### **2.1. Default Directory Layout**
Hugo's default directory structure was expanded as follows to accommodate the Congo theme:
```shell
content/
├── _index.md       # Home page content
├── about.md        # About page
└── insights/       # Blog or article posts
    ├── _index.md   # Section index
    ├── first-post.md
    └── another-post/
        ├── index.md
        └── image.jpg  # Associated media
```

The structure highlights the use of **page bundles** and **section indexes**. Page bundles allow content (like `index.md` files) to be grouped with their associated media, ensuring a clean and organized approach to managing content.
### **2.2. Customizing Content Sections**
To align the content with the Congo theme's design philosophy, the following customizations were made:

- **Insights Section**: Created to house blog posts and articles, providing a centralized location for all written content.
- **Sandbox Section**: Designed as an experimental space for projects and technical demonstrations.

Each section was meticulously crafted to leverage Hugo's powerful content rendering capabilities. For example, the **Sandbox** section was populated with an initial project detailing a Kubernetes Helm Chart for deploying a mass calculator application. This section is designed to showcase technical projects in a manner that's both informative and visually appealing.
## **3. Configuring the Congo Theme**

### **3.1. Configuration Files**
The Congo theme requires a specific set of configuration files to be properly integrated into a Hugo project. These files were added to the `config/_default/` directory, replacing Hugo's default configuration:
```shell
cp themes/congo/config/_default/* config/_default/
```

Key configurations were then adjusted in the `config.toml` and `languages.en.toml` files to reflect the site’s identity and purpose.
### **3.2. Parameter Customization**
Significant attention was given to the following parameters to ensure the site accurately represented its intended branding:
- **Site Title and Description**: Set in `languages.en.toml` to clearly communicate the site's focus on DevOps, automation, and cloud engineering.
- **Author Details**: Customized in the `params.author` section to provide a personal touch while maintaining professionalism.

## **4. Live Preview and Continuous Development**

### **4.1. Using Hugo’s Server for Live Previews**

To streamline the development process, Hugo’s development server was employed with options to include draft content and automatically navigate to the latest changes:
```shell
hugo server -D --navigateToChanged
```

This command was instrumental in enabling a rapid feedback loop, allowing for real-time adjustments to content and configuration settings.
### **4.2. Integration with GitHub Pages**

The final step involved setting up continuous deployment to GitHub Pages. This integration ensures that any commits pushed to the repository trigger an automated build and deployment process, keeping the live site in sync with the latest updates.

## **Conclusion**
Building this site with Hugo and the Congo theme was an exercise in meticulous configuration and thoughtful content organization. The result is a streamlined, professional platform that not only serves as a personal portfolio but also as a demonstration of technical expertise in modern web development practices.

For professionals and enthusiasts alike, this guide provides a comprehensive overview of the steps involved in setting up a Hugo site with a focus on clean architecture, efficient content management, and robust deployment practices.
