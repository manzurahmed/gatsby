# Learning Gatsby

Gatsby is available as NPM module. Install it first.

```
npm install -g gatsby-cli
```

## Create Gatsby project

```
gatsby new gatsby-bootcamp https://github.com/gatsbyjs/gatsby-starter-hello-world
```

While working on files, start GatsbyJS's realtime script-transpiller from NPM.

```
npm run develop
```

I typed "**localhost:8000**" in my browser to view my first Gatsby site.

There is an important folder, "pages" under "src" folder. If I need "About Us" and "Contact Us" page, I shall create two files, like "about.js" and "contact.js" files. These two files work as "router". That means, if I type "localhost:8000/contact", my browser will navigate to content of my "contact.js" file.

