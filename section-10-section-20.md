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

# 12. Generating Slugs for Posts (2:19:00)

# 13. Dynamically Generating Pages (2:35:14)

# 14. Rendering Post Data in Blog Template (2:52:08)

# 15. Adding Images to Posts (3:03:28)

# 16. Getting Started with Contentful (3:21:19)

# 17. Rendering Contentful Posts (3:38:29)

# 18. Dynamic Pages from Contentful (3:49:24)

# 19. 404 Pages and React Helmet (4:10:18)

# 20. Deploying Your Gatsby Site (4:25:38)
