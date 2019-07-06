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

When Gatsby is install via NPM, **GraphiQL** is installed. It is a in-browser IDE for exploring a GraphQL API.
To access it, type "http://localhost:8000/___graphql".

In this session, I wrote the following files in **gatsby-config.js**:

```
module.exports = {
	siteMetadata: {
		title: 'Full-stack Bootcamp',
		author: 'Andrew Mead'
	},
	plugins: [
		'gatsby-plugin-sass'
	]
}
```

To use Grapql in Header.js file, I opened it and import **graphql** and **useStaticQuery** named exports from 'gatsby' library. After that I fetched **title** from siteMetadata object in gatsby-config.js file.

```
import HeaderStyles from './Header.module.scss';
```

Fetched data,
```
const data = useStaticQuery(
	graphql`
		query {
			site {
				siteMetadata {
					title
				}
			}
		}
	`
);
```

and, use the data where required,
```
{ data.site.siteMetadata.title }
```

# 9. GraphQL Playground (1:47:12)

There is alternate way to query data from GraphQL, which is "GraphQL Playground IDE". To use it, I need to prepare proper environment. In order to do that, I created a file, named, **.env.development** for my PC to set environment variable to make it cross-OS compatible. In the file, I wrote the following line

```
GATSBY_GRAPHQL_IDE=playground
```
To take advantage of GraphQL Playground IDE, I also installed a NPM package called, "env-cmd".
```
npm i --save-dev env-cmd
```

Now, I opened package.json file and in the "scripts" section I modified the following line to made GraphQL Playground run properly on http://localhost:8000/___graphql,
```
"develop": "env-cmd -f .env.development gatsby develop",
```

Now, from terminal I stated gatsby server by running the following command:
```
npm run develop
```
I then run GraphQL Playfound and run the following query and be confirmed that it's running properly:
```
query {
  site {
    siteMetadata {
      author
    }
  }
}
```
