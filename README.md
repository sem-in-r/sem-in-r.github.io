# SEMinR Website Repository

This is the repository for the SEMinR wesbite, intended for SEMinR authors, maintainters, and collaborators to design and maintain the SEMinR Wesbite.

## Setup

**Clone this repo:** Get the website repo from Github.

```shell
git clone git@github.com:sem-in-r/sem-in-r.github.io.git
```

**Create an empty `_build/` folder:** This folder is a `gitignored` folder in which the site will be rendered locally for you to preview.

```shell
cd sem-in-r.github.io
mkdir _build/
```

**Install the relevant packages:** Launch the project by opening the `.Rproj` file. Install the following package(s) are required to maintain and render the website.

- distill - The site management and publication package. Full documentation available on distill website.

```r
install.packages('distill')
```

## Working with Git

**Important branches:**

- `main` - major code branch where all changes to the site source code are made
- `build` - where all rendered content is (*gitignored* -- do not edit in this branch and do not try to push this to Github); note that you will not find the `build` branch in your repo - only the website manager maintains it.

**Working with branches:** Please branch out of `main` branch and work in your own branch only! Create your articles/posts and submit a Pull Request (PR) back to main. Do not include the `_build/` folder in your PRs. The website maintainer will re-render your posts or other changes.

## Rendering Website

To render the whole website locally for preview, use the *Build Website* button in the *Build* pane of RStudio, or use the following command in R console:

```r
library(rmarkdown)

render_site()
```

## Creating New Blog Posts

*See the example post in `_posts/2021-05-26-seminr-20-released` as you read the following instructions.*

**Create a new post:** Use `distill::create_post()` function to create a skeleton post in the `_posts/` directory.

```r
library(distill)

create_post("Title of your post")
```

**Edit your post:** Find the named folder for your new post in the root `_posts/` folder. 

- Edit the `.Rmd` file including its relevant frontmatter. 
- Create an engaging thumbnail picture in an `images/` folder within the post's folder and refer to it in the post frontmatter. 
- Please use the `images/` folder for all other relevant images too.
- Knit the `.Rmd` when you wish to see a preview and one last time when you are finished. Note that `distill` will cache the rendered post within its folder and will not rerender it again with the entire website - rendering posts can be quite time consuming.
