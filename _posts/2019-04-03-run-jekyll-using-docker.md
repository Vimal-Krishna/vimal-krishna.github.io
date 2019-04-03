---
layout: post
title: "Test your Jekyll Site on localhost using Docker"
date: 2019-04-03
image: '/images/thumbs/thumb_run-jekyll-using-docker.png'
description:
category: 'blog'
tags:
twitter_text:
introduction: No need to install Jekyll
published: true
---

In order to run a Jekyll site, there are at least three steps involved.

+ Create the Jekyll configuration or use a pre-configured template.
+ Build this site
+ Serve the pages

**Step 1:** Download and extract a pre-configured site from [Barry Clark's Github](https://github.com/barryclark/jekyll-now).

**Step 2:** Build the site using **jekyll build**

If the site was extracted to **/c/Users/vimal/Documents/GitHub/vimal-krishna.github.io**, use the following command to use the Jekyll Docker container to do the build for you.

````bash
docker run --rm -v //c/Users/vimal/Documents/GitHub/vimal-krishna.github.io:/srv/jekyll 
	-v //home/docker/vendor/bundle:/usr/local/bundle 
	-it -p 4000:4000 jekyll/jekyll jekyll build

````

We are additionally mapping */home/docker/vendor/bundle* so that any artifacts required by Jekyll need not be downloaded every single time this command is run. 
Remember, Docker containers do not maintain state. This way, the download happens only the first time.

Output:

````bash
ruby 2.6.2p47 (2019-03-13 revision 67232) [x86_64-linux-musl]
Configuration file: /srv/jekyll/_config.yml
            Source: /srv/jekyll
       Destination: /srv/jekyll/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 4.51 seconds.
 Auto-regeneration: disabled. Use --watch to enable.
````

**Step 3:** Serve the pages (which are in _site folder) using **jekyll serve**

````bash
docker run --rm -v //c/Users/vimal/Documents/GitHub/vimal-krishna.github.io:/srv/jekyll 
	-v //home/docker/vendor/bundle:/usr/local/bundle 
	-it -p 4000:4000 jekyll/jekyll jekyll serve --force-polling

````

Notice that we need to use *-p* to publish the internal port *4000* so that the server is accessible from a browser. You can access the Jekyll site at [http://localhost:4000]()
The pages will be served as long as the Docker container is running.

Output:

````bash
ruby 2.6.2p47 (2019-03-13 revision 67232) [x86_64-linux-musl]
Configuration file: /srv/jekyll/_config.yml
            Source: /srv/jekyll
       Destination: /srv/jekyll/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 4.52 seconds.
 Auto-regeneration: enabled for '/srv/jekyll'
    Server address: http://0.0.0.0:4000/
  Server running... press ctrl-c to stop.
````


  

