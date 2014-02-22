Title: Porting WordPress Blog to GitHub Pages using Pelican
Date: 2014-02-19
Category: Projects
Tags: pelican, blogging, wordpress, github-pages, markdown
Slug: porting-from-wordpress-to-github-pages-with-pelican
Author: Amy Hanlon
Status: draft

Over the past five (YES, FIVE) days I have been dog-paddling through the ocean of misery that is migrating a blog from one host (WordPress) to another ([GitHub Pages](http://pages.github.com/)) and attempting to learn enough CSS and [Jinja](http://jinja.pocoo.org/) to handle setting up my site using [Pelican](http://docs.getpelican.com/en/3.3.0/). I have no experience with CSS! And my HTML experience is limited to inserting angst into my MySpace profile! And I became aware of Jinja and Pelican's existence about a week ago! So obviously I've drowned myself in 1.5 bottles of my neighborhood liquor store's 2-bottles-of-wine-for-$10 special.

The great part about this whole process is that with Pelican, I can write my blog posts and pages in [Markdown](http://daringfireball.net/projects/markdown/) (about which I also knew little until last week, but it's *wonderfully easy to learn*.) I am so tired of wrangling with WordPress's built-in editor trying to get my code blocks and in-line code to format correctly. Markdown is a blissful alternative.

There's a plethora of material online on Pelican and GitHub pages, but it is fairly disconnected and presumes a certain level of front-end development experience, of which I have none. Hopefully this post can help others, particularly those without a convenient 2-bottles-for-$10-special, make this transition with less misery.

##GitHub Pages Setup

1. Create GitHub repo following the [GitHub Pages instructions](http://pages.github.com/) (the first step only!)

*A note on GitHub Pages:* I believe your HTML files (particularly your index.html file) must be in the *main directory* of your git repo for this to work. This will be important later, specifically when you're organizing your site content on your local computer and when you're pushing your content to GitHub.

##Pelican Setup

1. Install necessary [packages](http://docs.getpelican.com/en/3.1.1/getting_started.html#installing-pelican)

2. Run Pelican [quickstart](http://docs.getpelican.com/en/3.1.1/getting_started.html#kickstart-a-blog)

	This will ask you lots of questions that probably seem foreign. These questions will set up some configuration files that you can later edit with your preferred [settings](http://docs.getpelican.com/en/3.1.1/settings.html). As an example, here's how I answered (I put my responses on separate lines starting with `>` to make this easier to read):

	  	:::console
	  	$ pelican-quickstart
		Where do you want to create your new web site? [.]  
		>  
		What will be the title of this web site?  
		> Amy Hanlon  
		Who will be the author of this web site?  
		> Amy Hanlon  
		What will be the default language of this web site? [en]  
		> 
		Do you want to specify a URL prefix? e.g., http://example.com   (Y/n) 
		> y  
		What is your URL prefix? (see above example; no trailing slash) 
		> http://amygdalama.github.io  
		Do you want to enable article pagination? (Y/n) 
		> y  
		How many articles per page do you want? [10] 
		>  
		Do you want to generate a Fabfile/Makefile to automate generation and publishing? (Y/n) 
		> y  
		Do you want an auto-reload & simpleHTTP script to assist with theme and site development? (Y/n) 
		> y  
		Do you want to upload your website using FTP? (y/N) 
		> n  
		Do you want to upload your website using SSH? (y/N) 
		> n  
		Do you want to upload your website using Dropbox? (y/N) 
		> n  
		Do you want to upload your website using S3? (y/N) 
		> n  
		Do you want to upload your website using Rackspace Cloud Files? (y/N) 
		> n  

	Now if you type the `tree` command within your blog's main directory, you should see:

	  	:::bash
	  	$ tree
		.
		├── Makefile
		├── content
		├── develop_server.sh
		├── fabfile.py
		├── output
		├── pelicanconf.py
		└── publishconf.py

	If you don't have `tree`, you should! It's neat. `brew install tree`. If you're on OSX and don't have [Homebrew](http://brew.sh/), you should! It's neat.

	I'll briefly explain each of these files/directories:

	* `Makefile` tells the command `make` what to do. This file defines commands like `make devserver`. More information on `make` can be found [here](http://www.gnu.org/software/make/manual/make.html). I'll cover more on how to use this command for developing your site in the **Generating Your Site** section.

	* `content` is the directory that should house all of your Markdown files. Pelican assumes that your articles/blog posts will be inside this directory. Additionally, there are some special directories you should create within `content`:  

		 	:::bash
		 	$ mkdir content/pages
		 	$ mkdir content/images

		Pelican by default is configured to know that your pages (i.e. non-temporal pages like About Me, Contact, etc) are found within this `pages` directory and that images are found within the `images` directory.  

	* `develop_server.sh` is a bash script that I believe handles serving your site locally during development (i.e. it serves your site to http://localhost:8000).

	* `fabfile.py` is a configuration file for [Fabric](http://docs.fabfile.org/en/1.8/) which allows you to generate your site using the `fab` command. You'll need to `pip install fabric` if you want to use it. Alternatively you can just use `make`.

	* `output` is, by default, where Pelican will store your HTML files when you run `pelican content`. This can cause issues which I describe in the section **Integrating with GitHub**.

	* `pelicanconf.py` houses your Pelican configuration [settings](http://docs.getpelican.com/en/3.3.0/settings.html).

	* `publishconf.py` is like `pelicanconf.py` in that it houses Pelican configuration settings, but is not intended to be used for local development. The reasoning behind having two separate files is described in [this Stack Overflow answer](http://stackoverflow.com/a/20845195).

##Exporting Existing Content

This section assumes you have existing content on a WordPress blog. Pelican also has an importer for Dotclear and RSS/Atom feeds. You can skip this section if you don't have existing content living elsewhere that you want to port to your site on GitHub Pages.  

1. [Export WordPress content to XML](http://en.blog.wordpress.com/2006/06/12/xml-import-export/)
2. [Imperfectly convert the XML to Markdown using Pelican](http://docs.getpelican.com/en/3.1.1/importer.html)
3. Manually export your images from your WordPress Media Library (I know. This sucks.) Move these images to `content/images`.
4. Manually edit the Markdown output (your code blocks, links, embedded images will likely need editing).
5. Move your Markdown files to the `content` directory within your website's main directory. Content intended to be non-temporal pages (i.e. About Me, Contact, etc) should go in the `content/pages` directory. Articles/blog posts should go in the `content` directory.

##Pelican Themes
1. Clone the available [Pelican Themes](https://github.com/getpelican/pelican-themes) into your blog's main directory.
	
		:::bash
		$ git clone https://github.com/getpelican/pelican-themes

2. Choose a theme you'd like to use. Pelican by default comes with the notmyidea and simple themes. Most other themes have a sample image in the pelican-themes repo to help you decide.

3. After you've chosen a theme, set the THEME variable in your `pelicanconf.py` file to the absolute or relative path to the theme. For example, I'm using the subtle theme and added this line to my `pelicanconf.py` file:
	
		:::python
		THEME = "pelican-themes/subtle"

	I recommend using an absolute or relative path to the theme, rather than using `pelican-themes --install subtle`. This ensures that Pelican HTML output will reflect any changes you make to the theme.

##Customization
All elements of your theme are customizable! You can change attributes of text like font, size, color, and more in the `main.css` file found in your theme's directory. For example, I've made many edits to the file `pelican-themes/subtle/static/css/main.css`.

Similarly, you can change layouts of your pages (like what shows up in your site nav menu) by exploring the HTML files in the `templates` folder within your theme. There will usually be a `base.html` file (or something similar) that provides the foundation for things like your header and site nav menu that will apply to every page. 

There should also be HTML files that serve as templates for specific types of pages. For example, `article.html` defines the basic structure for your articles/blog posts. If you want to change whether the author, date, tags, etc, display above article content, you should look there.

If you see something on your website that you want to change, and you're not sure where to look in your theme's CSS/HTML files, right click on the element in the browser and go to "Inspect Element". This will show you where in the HTML the element is (on the left) and what parts of the CSS file define its style (on the right). You can adjust things here in the browser to test out different fonts, colors, etc, but note that changes you make to the code in your browser will not be reflected in your source files.

##Generating Your Site
Once you have markdown files in your `content` folder, you can run `pelican` on those files to generate HTML for your site using the theme you specify in your `pelicanconf.py` file:

	:::bash
	$ pelican content

##Posting to GitHub
Recall that you need a repository on GitHub named *username.github.io* (this will be the remote repository for your blog), and that your HTML files need to be in this repository's main directory (not within a subdirectory).

It's intuitive to initialize a local repository for your blog within your blog's main directory, because in addition to posting the HTML, you'd also like to backup your content Markdown files, configuration files, and customized theme. This is a reasonable desire!

However, if you do this, GitHub won't generate your site! It isn't smart enough to know that the HTML files it needs to serve are actually contained within the `output` folder (recall that Pelican by default saves the HTML it generates in this folder).

There seem to be two solutions for this problem:

1. Create two separate repositories - one inside the `output` directory where Pelican generates your HTML (this repo should have *username.github.io* on GitHub as a remote), and another in your blog's main directory with your source Markdown files (in `content`), theme, and configuration files (this repo should have a different remote on GitHub).
2. Create a single repository in your blog's main directory that houses everything (this repo should have *username.github.io* on GitHub as a remote), and edit Pelican's settings to create output in your blog's main directory rather than within the `output` subdirectory.

	To change Pelican's output settings, you'll need to edit these files:  
	
	* `pelicanconf.py` - change your `OUTPUT` settings variable

			:::python
			OUTPUT_PATH = '.'
	
	* `publishconf.py` - change your `DELETE_OUTPUT_DIRECTORY` variable

			:::python
			DELETE_OUTPUT_DIRECTORY = False

	* `Makefile` and `develop_server.sh`- change your `OUTPUTDIR`

			:::python
			OUTPUTDIR=$(BASEDIR)



I decided to go with #2, but if there's a more elegant solution to this problem, please let me know!

Usually I'll start working on a blog post by writing it up in a Markdown file locally on my computer, setting its status as draft by including `Status: draft` in the metadata and then previewing it with the following command (within the main directory of my blog):

	:::bash
	$ make devserver

This command runs `pelican content` locally at the address [http://localhost:8000](http://localhost:8000). Your draft pages will be available at addresses like http://localhost:8000/drafts/posts-slug-here.


5. Convert this .xml file to a collection of .md files using the `pelican-import` command.

	:::bash
	(blog)wordpress-archive [master *%] $ pelican-import --wpfile -o output -m markdown archive.xml
	output/bio.md
	output/endorsements.md
	output/visualizing-social-circles-using-facebook-data.md
	output/homebrew-path-pythonpath.md
	output/adventures-in-miami-bullish-conference-2013.md
	output/home.md
	WARNING: Post "No title [92]" is lacking a proper title
	output/92.md
	WARNING: Post "No title [94]" is lacking a proper title
	output/94.md
	WARNING: Post "No title [95]" is lacking a proper title
	output/95.md
	output/using-pymc-to-analyze-ab-testing-data.md
	output/wget-sanity-part-2-im-an-idiot-and-decided-to-switch-to-a-brewed-python-and-reinstall-the-scientific-stack-at-10pm.md
	output/i-moved-cross-country-for-hacker-school-and-im-only-slightly-convinced-this-is-real-life.md
	output/hacker-school-day-2-goals.md
	output/ipython-and-pandas-are-whoa.md

Move these .md files to the content folder that was created when you ran pelican-quickstart.

	:::bash
	(blog)amygdalama.github.io [master *%] $ cp wordpress-archive/output/* content

Move the .md files intended to be pages (e.g. Bio, About Me, Contact, Resume pages) to the content/pages directory.

You'll need to *manually* export all of the images you've been hosting on WordPress. Make a folder for images within your content directory and save the images there.

(blog)$ mkdir content/images

Generate your site!

	:::bash
	(blog)amygdalama.github.io [master *%] $ pelican content
	WARNING: Empty alt attribute for image screen-shot-2013-11-26-at-9-04-50-pm.png?w=300" title="facebook-graph in /Users/amyhanlon/Projects/amygdalama.github.io/content/visualizing-social-circles-using-facebook-data.md
	WARNING: Empty alt attribute for image screen-shot-2013-11-26-at-9-04-50-pm.png?w=300" title="facebook-graph in /Users/amyhanlon/Projects/amygdalama.github.io/content/visualizing-social-circles-using-facebook-data.md
	Done: Processed 11 articles and 3 pages in 0.56 seconds.
	(blog)amygdalama.github.io [master *%] $ ls output
	92.html
	94.html
	95.html
	adventures-in-miami-bullish-conference-2013.html
	archives.html
	author
	authors.html
	bio.html
	categories.html
	category
	endorsements.html
	hacker-school-day-2-goals.html
	home.html
	homebrew-path-pythonpath.html
	i-moved-cross-country-for-hacker-school-and-im-only-slightly-convinced-this-is-real-life.html
	index.html
	index2.html
	ipython-and-pandas-are-whoa.html
	pages
	tag
	tags.html
	theme
	using-pymc-to-analyze-ab-testing-data.html
	visualizing-social-circles-using-facebook-data.html
	wget-sanity-part-2-im-an-idiot-and-decided-to-switch-to-a-brewed-python-and-reinstall-the-scientific-stack-at-10pm.html
	(blog)amygdalama.github.io [master *%] $ cd output && python -m SimpleHTTPServer
	Serving HTTP on 0.0.0.0 port 8000 ...
	127.0.0.1 - - [14/Feb/2014 19:34:52] "GET / HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:34:52] "GET /theme/css/main.css HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:34:53] "GET /theme/css/reset.css HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:34:53] "GET /theme/css/pygment.css HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:34:53] "GET /theme/css/typogrify.css HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:34:53] code 404, message File not found
	127.0.0.1 - - [14/Feb/2014 19:34:53] "GET /favicon.ico HTTP/1.1" 404 -
	127.0.0.1 - - [14/Feb/2014 19:35:29] "GET /hacker-school-day-2-goals.html HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:35:29] "GET /theme/css/main.css HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:35:29] "GET /theme/css/reset.css HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:35:29] "GET /theme/css/pygment.css HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:35:29] "GET /theme/css/typogrify.css HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:35:35] "GET /category/little-eureka-moments.html HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:35:43] "GET /pages/bio.html HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:35:58] "GET /pages/home.html HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:35:58] "GET /theme/css/main.css HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:35:58] "GET /theme/css/reset.css HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:35:58] "GET /theme/css/pygment.css HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:35:58] "GET /theme/css/typogrify.css HTTP/1.1" 200 -
	127.0.0.1 - - [14/Feb/2014 19:36:02] "GET / HTTP/1.1" 200 -

This will locally host your site at http://localhost:8000/. Navigate there in your browser and you can preview your site!

Initialize a .git in the output folder that Pelican created. Then add the GitHub repo you created as a remote:

	:::bash
	(blog)$ cd output
	(blog)$ git init
	(blog)$ git remote add origin https://github.com/amygdalama/amygdalama.github.io

Commit all the files in the output folder, and then push your commits to GitHub

	:::bash
	(blog)$ git add --all
	(blog)$ git commit -m "initial commit"
	(blog)$ git push origin master

Within about 10 minutes, your site should be up and working!

Clone the available themes and pick one! The themes are listed here https://github.com/getpelican/pelican-themes and most of them have screenshots for easy viewing.

	:::bash
	(blog) blog$ git clone https://github.com/getpelican/pelican-themes.git

I'm interested in looking at the following themes:
* gum - good! bolder titles? header image? no tag cloud
* pelican-bootstrap3 - close but not quite, no tag cloud, would have to change header format
* subtle - ooh! nice! might want to switch title headers to a font easier to read



To install themes, I created a themes folder within my blog folder

	:::bash
	(blog) blog$ mkdir themes
	(blog) blog$ cd themes
	(blog) blog$ git clone git://github.com/alexex/bootlex.git
	(blog) blog$ ls
	bootlex

Do this will all the themes you want to test out, and then run pelican-themes:

	:::bash
	(blog) blog$ pelican-themes --install themes/*


Things you'll probably have to fix:

* Image links - Since your posts previously linked to images you were hosting on WordPress, you'll need to update the URLs for your images to *raw* GitHub links.

* Your source code probably needs re-formatting.

Questions!
* Header image?
* Links to articles turn white after hovering
* Main Nav - get rid of a bunch of the links
  * Home
  * Biography
  * Projects
  * Hacker School
  * Endorsements
  * Add tag cloud to right of all pages



