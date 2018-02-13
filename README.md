Add a new post in `_posts/` directory with proper name format (as shown in placeholder posts)
Commit and push to master 

## Configuration variables

Edit the `_config.yml` file and set the following variables:

```yml
title: [The title of your blog]
description: [A short description of your blog's purpose]
author:
  name: [Your name]
  email: [Your email address]
  url: [URL of your website]

baseurl: [The base url for this blog.]

paginate: [Number of posts in one paginated section (default: 3)]
owner: [Your name]
year: [Current Year]
```

*Note: All links in the site are prepended with `baseurl`. Default `baseurl` is `/`. Any other baseurl can be setup like `baseurl: /hacker-blog`, which makes the site available at `http://domain.name/hacker-blog`.*

Additionally, you may choose to set the following optional variables:

```yml
google_analytics: [Your Google Analytics tracking ID]
```

## About Page

Edit `about.md`

## Layout

If you would like to modify the site style:

**HTML**

Footer: Edit `_includes/footer.html`

Header: Edit `_includes/header.html`

Links in the header: Edit `_includes/links.html`

Meta tags, blog title display, and additional CSS: Edit `_includes/head.html`

Index page layout: Edit `_layouts/default.html`

Post layout: Edit `_layouts/post.html`

**CSS**

Site wide CSS: Edit `_sass/base.scss`

Custom CSS: Make `_sass/custom.scss` and use it. Then add `@import "custom";` to `css/main.scss`

**404 page**

Edit `404.md`

## License

CC0 1.0 Universal
