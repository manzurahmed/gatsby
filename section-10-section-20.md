# 10. Sourcing Content from the File System (1:51:32)

In this tutorial, I created 1 folder, "posts" and 2 files in it, "gatsby.md" and "react.md". Now I shall tell Gatsby that I am going to use these two files and Gatsby is going to read these files from Filesystem. In order to enable Gatsby to read source file, I install "gatsby-source-filesystem" for Gatsby to read markdown file.

```
npm install gatsby-source-filesystem
```

After installation, I shall open gatsby-config.js and add the following entry to "plugins" section,
```
plugins: [
		'gatsby-plugin-sass',
		{
	      resolve: `gatsby-source-filesystem`,
	      options: {
	        name: `src`,
	        path: `${__dirname}/src/`,
	      },
	    },
	]
```

Save the file, run Gatsby develop script,
```
npm run develop
```

In my browser, I open GraphQL Playground page. Now, I can see 2 new entries in "DOCS", file and allFile. I shall use features of **allFile** in the query panel. The query looks like the following for fetching content of "pages" folder.
```
query {
	allFile {
		edges {
			name
			extension
			dir
		}
	}
}
```

In the next section, I shall read the file and fetch the post details from .md file.

# 11. Working with Markdown Posts (2:03:37)

In this section, I shall install another Gatsby plugin to pull out title, date, body from the mark down file convert them to usable HTML form.

```
npm install --save gatsby-transformer-remark
```
This Gatsby Plugin is a standalone markdown file parser.

I now head over to GraphQL Playground and refresh the page. I should find two new entries in the DOCS section, "markdownRemark" and "allMarkdownRemark".

Let's write a new query in GraphQL Playground,

```
query {
	allMarkdownRemark {
		edges {
			node {
				frontmatter {
					title
					date
				}
				html
				excerpt
			}
		}
	}
}
```

Next, in the blog.js file, I import graphql and useStaticQuery to run a query.
```
import { graphql, useStaticQuery } from 'gatsby';
```

```
const data = useStaticQuery(graphql`
	query {
		allMarkdownRemark {
			edges {
				node {
					frontmatter {
						title
						date
					}
				}
			}
		}
	}
`);
```

# 12. Generating Slugs for Posts (2:19:00)

In this section, I am going to create dynamic URL for each blog post and a template for showing blog content.

I created a new file, "gatsby-node.js" in the root of my project. The content of the file:

```
module.exports.onCreateNode = ( {node, actions} ) => {
	const { creatNodeField } = actions;

	if(node.internal.type === 'MarkdownRemark') {
		console.log(JSON.stringify(node, undefined, 4));
	}
}
```

Now, I stopped the dev server from Terminal and rerun. At the time of traspiling my project, I see something like the picture below.


I am going to use **fileAbsolutePath** to extract "slug" for each of my blog posts.

![File Absolute Path](https://github.com/manzurahmed/gatsby/blob/master/fileAbsolutePath.jpg)

Restart development server. If everything goes right, I should see the **fields** field in GraphQL Playground. I used the following query:

```
query {
	allMarkdownRemark {
		edges {
			node {
				frontmatter {
					title
					date
				}
				html
				excerpt
        fields {
          slug
        }
			}
		}
	}
}
```

# 13. Dynamically Generating Pages (2:35:14)

In this section, I created a folder, called **templates** in the "src" folder and created a file **blog.js"".

To create a new page for each Blog post, I require another Gatsby API call, **createPages" in "gatsby-node.js" file.

```

module.exports.createPages = async ({ graphql, actions }) => {

	// 1. Get path to template
	// 2. Get markdown data
	// 3. Create new pages

	const { createPage } = actions;
	const blogTemplate = path.resolve('./src/templates/blog.js');

	// graphql returs Promise
	const res = await graphql(`
		query {
			allMarkdownRemark {
				edges {
					node {
						fields {
				          slug
				        }
					}
				}
			}
		}
	`);

	res.data.allMarkdownRemark.edges.forEach(
		(edge) => {
			createPage({
				component: blogTemplate,
				path: `/blog/${edge.node.fields.slug}`,
				context:  {
					slug: edge.node.fields.slug
				}
			})
		}
	);
}
```

And in my /src/pages/blog.js file I modified the GraphQL query and used the data field to make link to each blog pages.
```
const data = useStaticQuery(graphql`
		query {
			allMarkdownRemark {
				edges {
					node {
						frontmatter {
							title
							date
						}
						fields {
							slug
						}
					}
				}
			}
		}
	`);
```
```
<ol>
{
	data.allMarkdownRemark.edges.map((edge, index) => {
		return (
			<li key={index}>
				<Link to={`/blog/${edge.node.fields.slug}`}>
					<h2>{edge.node.frontmatter.title}</h2>
					<p>{edge.node.frontmatter.date}</p>
				</Link>
			</li>
		)
	})
}
</ol>
```

# 14. Rendering Post Data in Blog Template (2:52:08)

In the "templates/blogjs" file in placed the following in it,
```
import React from 'react'
import { graphql } from 'gatsby'
import Layout from '../components/Layout'

export const query = graphql`
	query($slug: String!) {
		markdownRemark(fields: { slug: { eq: $slug } }) {
			frontmatter {
				title
				date
			}
			html
		}
	}
`;

const Blog = (props) => {

	console.log(props);

	return (
		<Layout>
			<h1>{props.data.markdownRemark.frontmatter.title}</h1>
			<p>{props.data.markdownRemark.frontmatter.date}</p>
			<div dangerouslySetInnerHTML={{ __html: props.data.markdownRemark.html }}></div>
		</Layout>
	)
}

export default Blog;
```

# 15. Adding Images to Posts (3:03:28)

In this section, I shall put an Image in a blog detail page. I downloaded a free photo from the Internet and paste it in the "src/posts" folder. So, I opened the "src/posts/gatsby.md" file and wrote the markdown for the image file like below,

```
![Grass](./grass.jpg)
```

Gatsby is not aware of the image although I have written the image markdown in the file. Now, let me instruct Gatsby about the image in the blog post file. To show image, I need to install 3 new Gatsby Plugin.

```
npm install gatsby-plugin-sharp gatsby-remark-images gatsby-remark-relative-images
```

I have only post in "src/posts" folder. What if I have 500 posts. It will look like a mess. To organize posts and assets I created a folder for each folder.

To stylize my post detailed page, I created a Footer styling module, named, **Footer.module.scss" 

# 16. Getting Started with Contentful (3:21:19)

In this section of Gatsby bootcamp, I shall connect Gatsby with a Content Management System (CMS) named, "ContentFull CMS". I visited https://www.contentful.com website, created my free account and published 2 blog posts.

In order to access the blog posts in Contentful CMS via the GraphQL API, I shall now install a Gatsby plugin again. Its name is **gatsby-source-contentful**. I install it via NPM,

```
npm install --save gatsby-source-contentful
```

After plugin installation, I open **.env.development** and **gatsby-config.js** files.

I .env.development files I put "Space ID" and "Access Token" in it. I got it from Contentful CMS settings. The codes are like below,

```
CONTENTFUL_SPACE_ID=njxtbp61v219
CONTENTFUL_ACCESS_TOKEN=bnVtfz59rhwo8do-xTg-AEJdugFFUSv__Zavdri41qU
```

In the "gatsby-config.js" file, the configuration of the new plugin was,

```
{
	resolve: 'gatsby-source-contentful',
	options: {
		spaceId: '',
		accessToken: '',
	}
},
```

Now, I opened GraphQL and clicked the DOCS link on the right side of the UI. Here I found some new entries for Contentful CMS, like, contentfulContentType, allContentfulContentType, contentfulBlogPost, allContentfulBlogPost, contentfulBlogPostBodyRichTextNode and allContentfulBlogBodyRichTextNode. I shall use **allContentfulBlogPost** entry to fetch remote data from Contentful CMS.

Here is my GraphQL query looks like,

```
query {
  allContentfulBlogPost {
    edges {
      node {
        title
        slug
        publishedDate
      }
    }
  }
}
```


# 17. Rendering Contentful Posts (3:38:29)

In this section of the video, I learnt how to sort posts and format the published date of the post. In the GraphQL Playground, my query looks like below,

```
query {
  allContentfulBlogPost (
    sort: {
      fields: publishedDate,
      order: DESC
    }
  ) {
    edges {
      node {
        title
        slug
        publishedDate(formatString: "MMMM Do, YYYY")
      }
    }
  }
}
```

Now, I opened my "src/pages/blog.js" file and replaced my query I used before.

```
const data = useStaticQuery(graphql`
		query {
		  allContentfulBlogPost ( sort: { fields: publishedDate, order: DESC } ) {
		    edges {
		      node {
		        title
		        slug
		        publishedDate(formatString: "MMMM Do, YYYY")
		      }
		    }
		  }
		}
	`);
```

In the render section of my Component I modified the fields in it,

```
return (
		<div>
			<Layout>
				<h1>Blog</h1>
				<ol className={blogStyles.posts}>
					{
						data.allContentfulBlogPost.edges.map((edge, index) => {
							return (
								<li key={index} className={blogStyles.post}>
									<Link to={`/blog/${edge.node.slug}`}>
										<h2>{edge.node.title}</h2>
										<p>{edge.node.publishedDate}</p>
									</Link>
								</li>
							)
						})
					}
				</ol>
			</Layout>
		</div>
	)
```

In the next section, I shall fetch content data from the Contentful CMS for each post.

# 18. Dynamic Pages from Contentful (3:49:24)

In this section of the Gatsby Bootcamp, I opened the **gatsby-node.js** file and modified the **module.exports.createPages** section to take data from **allContentfulBlogPost** data. The **createPages** module now takes the following codes:

```
module.exports.createPages = async ({ graphql, actions }) => {

	// 1. Get path to template
	// 2. Get markdown data
	// 3. Create new pages

	const { createPage } = actions;
	const blogTemplate = path.resolve('./src/templates/blog.js');

	// graphql returs Promise
	const res = await graphql(`
		query {
			allContentfulBlogPost {
				edges {
					node {
						slug
					}
				}
			}
		}
	`);

	res.data.allContentfulBlogPost.edges.forEach(
		(edge) => {
			createPage({
				component: blogTemplate,
				path: `/blog/${edge.node.slug}`,
				context: {
					slug: edge.node.slug
				}
			})
		}
	);
}
```

Now, I next move to **blog.js** file in "templates" folder. Here I wrote a new query to fetch data from GraphQL and show them using <Layout> component. GraphQL returns the content of the body in complex JSON folder.

To parse the content I am going to install a new **Gatsby Plugin** again.

```
npm install @contentful/rich-text-react-renderer
```

After installation completion, I import **documentToReactComponent** in blog.js and make some changes in the <Layout> section. The code of section is given at the end of this section.

Later, I opened up Contentful CMS and inserted a heading paragraph, a new image and another paragraph with bold-styled text.

To fetch that content I changed the GraphQL query like below,

```
query {
  allContentfulBlogPost (
    sort: {
      fields: publishedDate,
      order: DESC
    }
  ) {
    edges {
      node {
        title
        slug
        publishedDate(formatString: "MMMM Do, YYYY")
        body {
          json
        }
      }
    }
  }
}
```

Code of blog.js,

```
import React from 'react'
import { graphql } from 'gatsby'
import { documentToReactComponents } from '@contentful/rich-text-react-renderer';
import Layout from '../components/Layout'

export const query = graphql`
	query($slug: String) {
		contentfulBlogPost(slug: {eq: $slug}) {
			title
			publishedDate(formatString: "MMMM Do, YYYY")
			body {
				json
			}
		}
	}
`;

const Blog = (props) => {

	const options = {
		renderNode: {
			"embedded-asset-block": (node) => {
				const alt = node.data.target.fields.title['en-US'];
				const url = node.data.target.fields.file['en-US'].url;
				return <img alt={alt} src={url} />
			}
		}
	}

	console.log(props);

	return (
		<Layout>
			<h1>{props.data.contentfulBlogPost.title}</h1>
			<p>{props.data.contentfulBlogPost.publishedDate}</p>
			{documentToReactComponents(props.data.contentfulBlogPost.body.json, options)}
		</Layout>
	)
}

export default Blog;
```

# 19. 404 Pages and React Helmet (4:10:18)

In this sectio, mentor shows us how to setup a 404 page and to customize the head of a document to setup page title. 

In **pages** folder, I created a file **404.js** file. I am also going to install Gatsby plugin **gatsby-plugin-react-helmet** and React library called **React Helmet** to setup head of our document.

Let us install the two packages,

```
npm i gatsby-plugin-react-helmet react-helmet
```

After installation, I change the gatsby-config.js file to include the libraries into plugins section.

```
plugins: [
	'gatsby-plugin-react-helmet',
	...
	...
	...
]
```

The content of 404.js files is given below.

```
import React from 'react';
import { Link } from 'gatsby';

import Layout from '../components/layout';

const NotFound = () => {
	return (
		<Layout>
			<h1>Page not found</h1>
			<p><Link to="/">Head home</Link></p>
		</Layout>
	);
}

export default NotFound;
```

To take advantage of React Helmet, I shall create new component file, called, **Head.js". I am going to load it in **templates/blog.js" file so that each of your single blog page get "Title" tag in the page head.

Content of "Head.js",

```
import React from 'react';
import { Helmet } from 'react-helmet';

import { useStaticQuery, graphql } from 'gatsby'

const Head = ({ title }) => {

	const data = useStaticQuery(graphql`
		query {
			site {
				siteMetadata {
					title
				}
			}
		}
	`);

	return (
		<Helmet title={`${title} | ${data.site.siteMetadata.title}`} />
	)
}

export default Head;
```

In each of my files in "pages" and "templates" file I set up a reference to Head.js by importing it and called <Head /> components along with necessary data in **title"" props.

Importing Component

```
import Head from '../components/Head'
```

Setting up Head,

```
<Head title="Contact" />
```


# 20. Deploying Your Gatsby Site (4:25:38)
