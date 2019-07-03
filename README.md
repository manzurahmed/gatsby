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

# 5. Creating Gatsby Page Layouts (48:14)

In the previous lessons, I created Header.js and Footer.js files and I put them in my About.js, Blog.js, and Contact.js files. What if I had 20 files? I needed to open every files and write codes for header and footer.

To ease this embarassment, I created a ReactJS Page Layout.
```
import React from 'react';

import Header from './Header';
import Footer from './Footer';

const Layout = (props) => {
	return (
		<div>
			<Header />
			{props.children}
			<Footer />
		</div>
	)
}

export default Layout;
```

So, I open my index, about, blog and contact files (and, my subsequent files),  and modified them to use my Layout files.
Now, my index.js files takes the following lines of code:

```
import React from "react";
import { Link} from 'gatsby';

import Layout from '../components/Layout';

const IndexPage = () => {

	return (
		<Layout>
			<h1>Hello</h1>
			<h2>I'm Manzur, a full-stack developer living in polluted Dhaka.</h2>
			<p>Need a developer? <Link to="/contact">Contact me.</Link></p>
		</Layout>
	)
};

export default IndexPage;
```

# 6. Styling Gatsby Projects (56:13)

In this episode, a new Gatsby plugin is installed "gatsby-plugin-sass".

```
npm install --save node-sass gatsby-plugin-sass
```

Create a file in the root folder of my project called "gatsby-config.js" (if not present) and put the following contest in it.

```
module.exports = {
  plugins: [
  	'gatsby-plugin-sass'
  ]
}
```

Now, run the Gatsby development server again from cli:

```
npm run develop
```


# 7. Styling Gatsby with CSS Modules (1:06:49)

"CSS Module" is a ReactJS thing.

This project supports CSS Modules alongside regular stylesheets using the [name].module.css file naming convention. CSS Modules allows the scoping of CSS by automatically creating a unique classname of the format [filename]\_[classname]\_\_[hash].

CSS Modules let you use the same CSS class name in different files without worrying about naming clashes. Learn more about CSS Modules here.

For more information, visit https://facebook.github.io/create-react-app/docs/adding-a-css-modules-stylesheet.

If there is CSS class like,
```
.nav-list {
}
```
in **Header.module.scss** CSS Module, in the calling **Header.js** file, it is written as,
```
<ul className={HeaderStyles.navList}>
```

# 8. Gatsby Data with GraphQL (1:28:23)

# 9. GraphQL Playground (1:47:12)

# 10. Sourcing Content from the File System (1:51:32)

# 11. Working with Markdown Posts (2:03:37)

# 12. Generating Slugs for Posts (2:19:00)

# 13. Dynamically Generating Pages (2:35:14)

# 14. Rendering Post Data in Blog Template (2:52:08)

# 15. Adding Images to Posts (3:03:28)

# 16. Getting Started with Contentful (3:21:19)

# 17. Rendering Contentful Posts (3:38:29)

# 18. Dynamic Pages from Contentful (3:49:24)

# 19. 404 Pages and React Helmet (4:10:18)

# 20. Deploying Your Gatsby Site (4:25:38)
