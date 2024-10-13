<p> <img src="/blog/images/hugo-gh-action/gh-logo.png" alt="Image of GitHub fork repository"/> </p>

## How do we build and run it?
To run the project, you'll need to do the following:

- Clone this repository
- Install Hugo with the command
```
brew install hugo
```
- Run the project
```
Hugo server
```

## Image Path Resolution
The images on the blog render well on GitHub Pages because they require the repository name in the URL. 
However, for local development, you need to remove the repository name. For example, update the image src to
---
Local: /images/hugo-gh-action/gh-logo.png
Production: /blog/images/hugo-gh-action/gh-logo.png
---