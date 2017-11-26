# 七星龙井

### [Lanin Blog &rarr;]( )

## Document

* Get Started
	* [Environment](#environment)
	* [Get Started](#get-started)
	* [Write Posts](#write-posts)
* Components
	* [SideBar](#sidebar)
	* [Mini About Me](#mini-about-me)
	* [Featured Tags](#featured-tags)
	* [Friends](#friends)
	* [Keynote Layout](#keynote-layout)
* Comment & Analysis
	* [Comment](#comment)
	* [Analytics](#analytics)
* Advanced
	* [Customization](#customization)
	* [Header Image](#header-image)
	* [SEO Title](#seo-title)
	* [Page Build Warning](#page-build-warning)

#### Environment

If you have jekyll installed, simply run `jekyll serve` in Command Line
and preview the themes in your browser. You can use `jekyll serve --watch` to watch for changes in the source files as well.


#### Write Posts

Feel free to checkout Markdown files in the `_posts/`, you will quickly realized how to post your articles with magical markdown plus this nice theme.

The **front-matter** of a post looks like that:

```
---
layout:     post
title:      "未来 已来"
subtitle:   "Hello World, 未来"
date:       2017-19-29 12:00:00
author:     "Lanin"
header-img: "img/post-bg-2015.jpg"
tags:
    - Life
---

```

#### SideBar


Seeing more information may be necessary for you to display, from V1.1, a clean, gorgeous **SideBar** is added for you, which provide more area for displaying possible modules. You can enable *(it is default enable)* this feature by simply config:

```
# Sidebar settings
sidebar: true
sidebar-about-description: "your description here"
sidebar-avatar: /img/avatar-hux.jpg     # use absolute URL.
```

We default support *[Featured Tags](#featured-tags)*, *[Mini About Me](#mini-about-me)* and *[Friends](#friends)* these three modules and you can add your own. The sidebar is naturally responsive and would be push to bottom in a small screen size (`<= 992px`, according to [Bootstarp Grid System](http://getbootstrap.com/css/#grid))  
More details of these three separate modules are talking below.

#### Mini About Me

Mini-About-Me module display all your SNS buttons also your avatar and the description if you set `sidebar-avatar` and `sidebar-about-description` which is very useful and common for a sidebar so it is default with your sidebar.

It is really nice-looking and well-designed. It would be hidden in a small screen seeing the sidebar would be push to bottom and there is already a footer including SNS feature which is similar.

#### Featured Tags

Considering the Featured-Tags feature in [Medium](http://medium.com) is pretty cool, so I add it in my blog theme also.   
This module is independent of sidebar from V1.4, so it can definitely live without enable sidebar, which would be displayed in the bottom when `sidebar` set to false, and it is not only displayed in home page but also every post page bottom.


```
# Featured Tags
featured-tags: true  
featured-condition-size: 1     # A tag will be featured if the size of it is more than this condition value
```

The only one thing need to be paid attention to is the `featured-condition-size`: A tag will be featured if the size of it is more than this condition value.  
Internally, a condition template `{% if tag[1].size > {{site.featured-condition-size}} %}` is used to do the filter.



#### Analytics

From V1.5, we support Google Analytics and Baidu Tongji officially with a deathly simple config:

```
# Baidu Analytics
ba_track_id: 百度分析id

# Google Analytics
ga_track_id: 'google跟踪id'            # Format: UA-xxxxxx-xx
ga_domain: forestlp.github.io
```

Just checkout the code offered by Google/Baidu, and copy paste here, all the rest is already done for you.

(Google might ask for meta tag "google-site-verification")


#### Customization

If you wanna do more customization and change code yourself, a [Grunt](gruntjs.com) environment is also included. (Thanks to Clean Blog.)

There are a number of tasks it performs like minification of the JavaScript, compiling of the LESS files, adding banners to keep the Apache 2.0 license intact, and watching for changes. Run the grunt default task by entering `grunt ` into your command line which will build the files. You can use `grunt watch` if you are working on the JavaScript or the LESS.

**Try to understand code in `_include/` and `_layouts/`, then you can modify Jekyll [Liquid](https://github.com/Shopify/liquid/wiki) template directly to do more creative customization.**


#### Header Image

Change header images of any pages or any posts is pretty easy as mentioned above. But, thanks to [issue #6 (in Chinese)](https://github.com/Huxpro/huxpro.github.io/issues/6) asked, **how to make it looks great?**

**Well...it is actually a design issue**, not a coding stuff. It is better that you have basic design knowledge, but not is ok, let me told you how to make it well-designed:

Seeing the title text above image is **white**, the image should be **dark** to emphasize the contract. so we can easily add a **black overlay with fews of opacity**, which is depends on the brightness of the original images you used. you can process it in Photoshop, Sketch etc.

In technical views, it can be done with CSS. However, the opacity of the black overlay is really hard to assigned, **every image has different brightness so the  degree it should be adjusted is different so it is impossible to hard code it.**


## License

Apache License 2.0.
Copyright (c) 2015-2017 Lanin


