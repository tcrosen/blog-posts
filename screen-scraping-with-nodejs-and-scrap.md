#Screen Scraping with Node.js & Scrap

Date: 2013-05-01

Screen scraping is the practice of scanning (*scraping*) a public URL's markup (*screen*) for the purpose of extracting or manipulating the data contained within.

Basically you get to work with an HTML page like it's your own and use the straightforward jQuery (or other preferred DOM traverser) methods you know and love.

The most common use case for this method is when you wish to use public data that isn't supplied via API.

**Disclaimer**:  I am not well versed on the legalities of various types of screen scraping but suffice it to say you should do a bit of reading if you plan on making money on what you're about to learn.

###Stuff we're going to use



+ [NodeJS](http://nodejs.org/)
+ [jQuery](http://www.jquery.org/)
+ [Scrap](https://github.com/jprichardson/node-scrap) by [JP Richardson](http://about.me/jprichardson)
+ [Sour Patch Kids](http://www.sourpatch.com/) *(Optional)*

If you don't know what the first two are you should probably stop reading now.

###Quick Start (and Finish)


Create a new directory for your app.

Open your terminal to the directory you created and type
`npm install scrap`.  This installs the **scrap** module.

Create a new file, call it something like `scraper.js`.

Open the file you created and add the following:

    var scrap = require('scrap');

    // $ now represents the jQuery(document) of the scraped page
    scrap("http://www.google.com", function(err, $) {
      console.log($('title').text().trim());
      // ==> Google
    });

Go watch Game of Thrones again.  Amazing, I know.










