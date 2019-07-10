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


# 16. Getting Started with Contentful (3:21:19)

# 17. Rendering Contentful Posts (3:38:29)

# 18. Dynamic Pages from Contentful (3:49:24)

# 19. 404 Pages and React Helmet (4:10:18)

# 20. Deploying Your Gatsby Site (4:25:38)
