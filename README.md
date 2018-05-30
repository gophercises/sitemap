# Exercise #5: Sitemap Builder

[![exercise status: released](https://img.shields.io/badge/exercise%20status-released-green.svg?style=for-the-badge)](https://gophercises.com/exercises/sitemap)

## Exercise details

A sitemap is basically a map of all of the pages within a specific domain. They are used by search engines and other tools to inform them of all of the pages on your domain.

One way these can be built is by first visiting the root page of the website and making a list of every link on that page that goes to a page on the same domain. For instance, on `calhoun.io` you might find a link to `calhoun.io/hire-me/` along with several other links.

Once you have created the list of links, you could then visit each and add any new links to your list. By repeating this step over and over you would eventually visit every page that on the domain that can be reached by following links from the root page.

In this exercise your goal is to build a sitemap builder like the one described above. The end user will run the program and provide you with a URL (*hint - use a flag or a command line arg for this!*) that you will use to start the process.

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

*Note: This should be the same as the [standard sitemap protocol](https://www.sitemaps.org/index.html)*

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


**2. The following packages will be helpful...**

- [net/http](https://golang.org/pkg/net/http/) - to initiate GET requests to each page in your sitemap and get the HTML on that page
- the `solution` branch of [github.com/gophercises/link](https://github.com/gophercises/link) - you won't be able to `go get` this package because it isn't committed to master, but if you complete the exercise locally you can use the code from it in this exercise. If this causes confusion or issues reach out and I'll help you figure out how to do all of this! <jon@calhoun.io>
- [encoding/xml](https://golang.org/pkg/encoding/xml/) - to print out the XML output at the end
- [flag](https://golang.org/pkg/flag/) - to parse user provided flags like the website domain

I'm probably missing a few packages here so don't worry if you are using others. This is just a rough list of packages I expect to use myself  when I code this for the screencasts 😁

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
