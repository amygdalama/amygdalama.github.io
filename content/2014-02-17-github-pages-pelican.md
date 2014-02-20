Title: Porting WordPress Blog to GitHub Pages using Pelican
Date: 2014-02-19
Category: Projects
Tags: pelican, blogging, wordpress, github-pages, markdown
Slug: porting-from-wordpress-to-github-pages-with-pelican
Author: Amy Hanlon
Status: Draft

Over the past five (YES, FIVE) days I have been dog-paddling through the ocean of misery that is migrating a blog from one host (WordPress) to another ([GitHub Pages])(http://pages.github.com/) and attempting to learn enough CSS and [Jinja](http://jinja.pocoo.org/) to handle setting up my site using [Pelican](http://docs.getpelican.com/en/3.3.0/). I have no experience with CSS! And my HTML experience is limited to inserting angst into my MySpace profile! And I became aware of Jinja and Pelican's existence about a week ago! So obviously I've drowned myself in 1.5 bottles of my neighborhood liquor store's 2-bottles-of-wine-for-$10 special.

The great part about this whole process is that with Pelican, I can write my blog posts and pages in [Markdown](http://daringfireball.net/projects/markdown/) (about which I also knew nothing until last week, but it's *wonderfully easy to learn*.) I am so tired of wrangling with WordPress's built-in editor trying to get my code blocks and in-line code to format correctly. Markdown is a blissful alternative.

There's a plethora of material online on Pelican and GitHub pages, but it all presumes a certain level of front-end development experience, of which I have none. Hopefully this post can help others, particularly those without a convenient 2-bottles-for-$10-special make this transition with less misery.

##The Process:

###Required Packages

###GitHub Pages Setup
* Create GitHub repo following [these instructions](http://pages.github.com/)

###Exporting Existing Content
**You can skip this section if you don't have existing content living elsewhere that you want to port to your site on GitHub Pages**  

* Exporting my WordPress content to XML
* Imperfectly converting the XML to Markdown using Pelican
* Manually editing the Markdown output

###Locally Hosting Site

###Posting Site to GitHub

##Customization

* Posting Pelican output HTML to GitHub
7. Choosing a [Pelican theme]
8. Customizing the Pelican theme
9. 

1. Initial GitHub setup: http://pages.github.com/
  * Only create the repository. Don't set up .git on your local computer yet because you'll need to set it up in the output folder that Pelican generates that houses your .html
2. Initial Pelican setup in a virtualenv: http://docs.getpelican.com/en/3.3.0/getting_started.html
  * run quickstart  

	:::bash
	(blog)amygdalama.github.io $ pelican-quickstart  
	Welcome to pelican-quickstart v3.3.0.  

	This script will help you create a new Pelican-based website.  

	Please answer the following questions so this script can generate the files needed by Pelican.  

	> Where do you want to create your new web site? [.] .  
	> What will be the title of this web site? Amy Hanlon  
	> Who will be the author of this web site? Amy Hanlon  
	> What will be the default language of this web site? [en] en  
	> Do you want to specify a URL prefix? e.g., http://example.com   (Y/n) y  
	> What is your URL prefix? (see above example; no trailing slash) http://amygdalama.github.io  
	> Do you want to enable article pagination? (Y/n) y  
	> How many articles per page do you want? [10]  
	> Do you want to generate a Fabfile/Makefile to automate generation and publishing? (Y/n) y  
	> Do you want an auto-reload & simpleHTTP script to assist with theme and site development? (Y/n) y  
	> Do you want to upload your website using FTP? (y/N) n  
	> Do you want to upload your website using SSH? (y/N) n  
	> Do you want to upload your website using Dropbox? (y/N) n  
	> Do you want to upload your website using S3? (y/N) n  
	> Do you want to upload your website using Rackspace Cloud Files? (y/N) n  
	Done. Your new project is available at /Users/amyhanlon/Projects/amygdalama.github.io  
	(blog)amygdalama.github.io $ ls  
	Makefile		develop_server.sh	how-to.md		pelicanconf.py  
	content			fabfile.py		output			publishconf.py

3. Create a folders for static pages (i.e. About Me, Contact, etc) and images
	
	:::bash
	(blog)amygdalama.github.io [master *%] $ cd content/
	(blog)content [master *] $ ls
	(blog)content [master *] $ mkdir pages
	(blog)content [master *] $ ls
	pages
	(blog)content [master *] $ mkdir images
	(blog)content [master *] $ ls
	images	pages

4. Export your WordPress blog to a .xml file by going to Dashboard > Tools > Export. I'm anal and saved this file and an archive in my amygdalama.github.io folder.

	:::bash
	(blog)amygdalama.github.io [master *%] $ cd ~/Projects/amygdalama.github.io/
	(blog)amygdalama.github.io [master *%] $ mkdir wordpress-archive
	(blog)amygdalama.github.io [master *%] $ mv ~/Downloads/amyhanlon.wordpress.2014-02-15.xml wordpress-archive/archive.xml
	(blog)amygdalama.github.io [master *%] $ cd wordpress-archive/
	(blog)wordpress-archive [master *%] $ ls
	archive.xml

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



