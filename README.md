# Exercise #5: Sitemap Builder

[![topic: bfs-algorithm](https://img.shields.io/badge/topic-bfs%20algorithm-green.svg?style=flat-square)](https://github.com/search?q=topic%3Abfs-algorithm+org%3Agophercises&type=Repositories)
[![topic: web-requests](https://img.shields.io/badge/topic-web%20requests-green.svg?style=flat-square)](https://github.com/search?q=topic%3Aweb-requests+org%3Agophercises&type=Repositories)

![video status: unreleased](https://img.shields.io/badge/video%20status-unreleased-red.svg?style=flat-square)
![code status: unreleased](https://img.shields.io/badge/code%20status-unreleased-red.svg?style=flat-square)

## Exercise details

A sitemap is basically a map of all of the pages within a specific domain. They are used by search engines and other tools to inform them of all of the pages on your domain.

One way these can be built is by first visiting the root page of the website and making a list of every link on that page that goes to a page on the same domain. For instance, on `calhoun.io` you might find a link to `calhoun.io/hire-me/` along with several other links.

Once you have created the list of links, you could then visit each and add any new links to your list. By repeating this step over and over you would eventually visit every page that on the domain that can be reached by following links from the root page.

In this exercise your goal is to build a sitemap builder like the one described above. The end user will run the program and provide you with a domain (*hint - I suggest using a flag for this!*) that you will use to start the process.

Once you have determined all of the pages of a site, your sitemap builder should then output the data in the following XML format:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>http://www.example.com/</loc>
  </url>
  <url>
    <loc>http://www.example.com/dogs</loc>
  </url>
</urlset>
```

*Note: This is the same format that Google uses.*

Where each page is listed in its own `<url>` tag and includes the `<loc>` tag inside of it.

In order to complete this exercise I highly recommend first doing the [link parser exercise](https://github.com/gophercises/link) and using the package created in it to parse your HTML pages for links.

From there you will likely need to figure out a way to determine if a link goes to the same domain or a different one. If it goes to a different domain we shouldn't include it in our sitemap builder, but if it is the same domain we should. Remember that links to the same domain can be in the format of `/just-the-path` or `https://domain.com/with-domain`, but both go to the same domain.

### Notes

**1. Be aware that links can be cyclical.**

That is, page `abc.com` may link to page `abc.com/about`, and then the about page may link back to the home page (`abc.com`). These cycles can also occur over many pages, for instance you might have:

```
/about -> /contact
/contact -> /pricing
/pricing -> /testimonials
/testimonials -> /about
```

Where the cycle takes 4 links to finally reach it, but there is indeed a cycle.

This is important to remember because you don't want your program to get into an infinite loop where it keeps visiting the same few pages over and over. If you are having issues with this, the bonus exercise might help temporarily alleviate the problem but we will cover how to avoid this entirely in the screencasts for this exercise.


## Bonus

As a bonus exercises you can also add in a `depth` flag that defines the maximum number of links to follow when building a sitemap. For instance, if you had a max depth of 3 and the following links:

```
a->b->c->d
```

Then your sitemap builder would not visit or include `d` because you must follow more than 3 links to to get to the page.

On the other hand, if the links for the page were like this:

```
a->b->c->d
b->d
```

Where there is also a link to page `d` from page `b`, then your sitemap builder should include `d` because it can be reached in 3 links.

*Hint - I find using a BFS ([breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)) is the best way to achieve this bonus exercise without doing extra work, but it isn't required and you could likely come up with a working solution without using a BFS.*
