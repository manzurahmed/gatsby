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

# 3. Linking between pages with Gatsby (27:02 minute)

In order to link a page, I need to import a "gatsby" module and then link to the address as shown below:

```
import { Link} from 'gatsby';
...
...
<p>Need a developer? <Link to="/contact">Contact me.</Link></p>
```

# 4. Creating Shared Page Components (35:58 minute)

In this episode, a new folder, named, "components" is created. I created a new file, "Footer.js" in it. This React component will be used at the bottom of every Gatsby pages. That is why is "Shared Components".

# 5. Creating Gatsby Page Layouts

# 6. Styling Gatsby Projects

# 7. Styling Gatsby with CSS Modules

# 8. Gatsby Data with GraphQL

# 9. GraphQL Playground

# 10. Sourcing Content from the File System

# 11. Working with Markdown Posts

# 12. Generating Slugs for Posts

# 13. Dynamically Generating Pages

# 14. Rendering Post Data in Blog Template

# 15. Adding Images to Posts

# 16. Getting Started with Contentful

# 17. Rendering Contentful Posts

# 18. Dynamic Pages from Contentful

# 19. 404 Pages and React Helmet

# 20. Deploying Your Gatsby Site
