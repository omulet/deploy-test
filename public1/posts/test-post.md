---
title: Test post
testimonials:
  - author:
      avatar: ''
      name: Anonymous
    quote: 'OOh, that''s so nice'
---
[![Build status](https://badge.buildbox.io/1a9ff722ce0871feeaa718426d59c9e86b03c26704b2b346d7.svg)](https://buildbox.io/zuppler/sites)

# Sites

<div style="background:#ddd; padding: 20px">html markup</div>

**IMPORTANT!!!** Do not modify the original template files (sites/templates/*) unless you know what you are doing. [Ask for permission](mailto:bogdan.silivestru@zuppler.com) before doing that anyway.

![](/public1/media_folder/blt.png)

## Initial Setup

##### Dependinces

Gulp and Bower must be installed globally. If `which gulp` or `which bower` gives _not found_, then install [Gulp](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md) and [Bower](http://bower.io/#install-bower)

##### Install development modules

```
cd path/to/sites
npm install
bower install
```

## Creating a new site

### A. Channel config

1. Create a new channel on Zuppler CP if necessary. Remember the channel permalink as you will need it to define the site folder. For the sake of this documentation let's call it **MY_CHANNEL**
2. Add a nice looking logo to the channel, this will be automatically used by our new site.
3. Edit Site Config and fill in the information, at least the Site Title.
4. Make sure you have at least 1 integration defined on the channel.

### B. Site config & customization

1. Create a new folder under sites and name it **MY_CHANNEL**. Remember, this is our channel permalink.
2. Create the `options.coffe` file and define the site channel, template, etc. _You can copy an options file from other site which uses the same template._
3. Create new nginx config file under config folder (e.g. config/MY_CHANNEL.conf). Edit this file accordingly.

Head to [File structure](#file-structure) section for more info about the template-site relation and how to organize your files.

### C. Preview & making changes

1. Open the terminal and CD into the **sites** project root folder. Make sure you are on the project root and not on your recently created folder.
2. Start the development server with `gulp --site MY_CHANNEL --stage development`. If anything goes wrong at this step, head back to the [Initial Setup](#initial-setup) section.
3. Preview the website at <http://localhost:1337>
4. Add new pages, change content, customize, etc. Keep an eye on your terminal and watch for possible compile errors. You may need to restart the gulp server if you've changed the options.coffee or added new pages and the browser does not reflect the changes: `ctrl+c` and run step #2 again.

### D. Deploy in production

1. Commit your changes. Please [take 15 minutes and get familiar with GIT](https://try.github.io). E.g..


```
git status
git add .
git commit -am "Creating my_channel website"
git pull && git push
```

2. Mark the channel as being updated. In CP, visit one of your channel's restaurants and hit **Publish Changes** from the menu tab. If you skip this step, your site will not be generated in production.
3. To deploy changes in production run `script/deploy` from the project root folder

NOTE: `script/deploy production` also push changes for you. If you just want to save and share your work without deploy, then simply `git push`

### File structure

The configuration file (`options.coffee`) is the only file required for a new site to work. Other files defined on a site folder will override files that has the same path/name in the template folder. This is how you make custom changes.
The following images shows the difference between the original template and site folder structure. Notice how the site only has 2 pages defined under src/views: `index.jade` and `services.jade`.

## Release new site

* Set Nginx server_name directive:
  `server_name juliuscatering juliuscatering.zupplermenus.com *.juliuscatering.com;`
* Change DNS to point to lb.zupplermenus.com, e.g. www.juliuscatering.com CNAME lb.zupplermenus.com

## Development environment

Generate and start container locally:

```shell
gulp generate --site juliuscatering
docker build -t sites .
docker run -it --rm --name sites sites
open http://juliuscatering
```

Get latest available container:

```shell
docker pull tutum.co/zuppler/sites
```

## Staging environment

All sites will be deployed in subdomains of zupplermenus.com, e.g. http://juliuscatering.zupplermenus.com

## Production environment

Release and deploy to production

```shell
script/deploy
```

Check latest deployed version

```shell
curl www.zupplermenus.com/ping
```
