Creating a website hosted on GitHub pages using Pelican, with content ported from Wordpress.

1. Initial GitHub setup: http://pages.github.com/
2. Initial Pelican setup in a virtualenv: http://docs.getpelican.com/en/3.3.0/getting_started.html
  * run quickstart  

     (blog)amygdalama.github.io [master #%] $ pelican-quickstart  
     Welcome to pelican-quickstart v3.3.0.  

     This script will help you create a new Pelican-based website.  

     Please answer the following questions so this script can generate the files needed by Pelican.  

    ```
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
     (blog)amygdalama.github.io [master #%] $ ls  
     Makefile		develop_server.sh	how-to.md		pelicanconf.py  
     content			fabfile.py		output			publishconf.py  
I don't want this to be in a code block.