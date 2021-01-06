[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/danielgtaylor/bibviz)

BibViz Project
==============
A project to visualize negative aspects of the Bible in a way that is not only visually appealing but that makes exploring those aspects of the book quick and intuitive. The main focus of the project is [Bible contradictions](http://bibviz.com/), but it also highlights biblical scientific and historical inaccuracies, [cruelty & violence in the Bible](http://bibviz.com/), [misogyny in the Bible](http://bibviz.com/), and [homophobia in the Bible](http://bibviz.com/).

Data for the website is generated from various sources, including the Skeptic's Annotated Bible, Infidels.org, EvilBible.com, etc. See the `scripts` directory for how each piece was generated. Note that some information is managed by hand as not everything can be generated from the source pages.

The `web` directory contains everything required for the website, which is generated by a static site generator called [Wintersmith](http://wintersmith.io/). Several modifications/plugins are used, which can be found in the `web/plugins` directory.

This project makes use of [Node.js](http://nodejs.org/), Javascript, [Coffeescript](http://coffeescript.org/), [Cheerio](https://github.com/MatthewMueller/cheerio) (a jQuery-like library), [Needle](https://github.com/tomas/needle), [Async.js](https://github.com/caolan/async), [D3.js](http://d3js.org/), [Wintersmith](http://wintersmith.io/), HTML, [Stylus](http://learnboost.github.io/stylus/), and [Nunjucks](http://nunjucks.jlongster.com/) templates.

Contribution Ideas
------------------
The following is a non-exhaustive list of possible contribution ideas. Many require little or even zero programming experience. Feel free to fork and choose one:

 * [Translations](http://bibviz.com/translate.html)
 * New data sources for `web/contents/data/contra.json`
 * Additional visualizations using D3.js
 * Blog entries
 * Meta-analysis of all data sets
 * Tagging contradictions by severity
 * Tagging contradictions by Christian sect beliefs
 * Support for newer Bible versions (e.g. NIV, ESV)
   * Part of this work would be tagging contradictions by version
 * Tagging contradictions with Christian responses
 * Style, script, page, and search optimizations

Running Locally
---------------
First make sure you have [Node.js](http://nodejs.org/) installed, then run some setup commands:

```bash
cd web
npm install
npm install -g wintersmith
cp ../cache/kjv-full.json.gz contents/data/
gunzip contents/data/kjv-full.json.gz
```

For a live preview at http://localhost:8080:

```bash
wintersmith preview
```

To build the site into the `build` directory:

```bash
wintersmith build --clean
```

Wintersmith Modifications
-------------------------
The following extra features are implemented as plugins to Wintersmith:

 * Blog support with Atom.xml feed generation
 * Pass-through JSON data files in the `contents/data` directory
 * Exposing JSON data files to the page renderer
 * i18n support via a preprocessor template tag `{% trans %}some string{% endtrans %}`
 * Generation of translated HTML files based on available languages and `translate: true` metadata
 * Nunjucks templates with Markdown content that can contain Nunjucks tags
 * A generator to create individual contradiction pages
 * Various utilities (book links, getting Bible verses, etc)

Data Format
-----------
Various data files are used to generate the BibViz website. The files fall into three broad categories: Bible information, contradiction information, and specific issue information (used to generate issue bar charts). All of the files use JSON as their format.

### Bible Information
The format of the Bible information data files (`kjv.json` and `kjv-full.json`) is something like the following:

```javascript
{
    "version": "kjv",
    "wordCount": 123,
    "charCount": 123,
    "sections": [
        {
            "name": "Old Testament",
            "wordCount": 123,
            "charCount": 123,
            "relativeLength": 0.7,
            "books": [
                {
                    "name": "The first book of Moses, called Genesis",
                    "shortName": "Genesis",
                    "wordCount": 123,
                    "charCount": 123,
                    "relativeLength": 0.1,
                    "chapters": [
                        {
                            "name": "Chapter 1",
                            "wordCount": 123,
                            "charCount": 123,
                            "relativeLength": 0.3,
                            "verseCount": 123
                        }
                    ]
                }
            ]
        }
    ]
}
```

The `kjv-full.json` additionally adds each verse to the file in a `verses` array alongside `verseCount`, and is used to generate individual contradiction pages where the verses are displayed on the page. The full version is not loaded dynamically when visitors view a page due to its large size. 

### Contradiction Information
The format of the contradiction information data file (`contra.json`) is something like the following:

```javascript
{
    "sab": {
        "name": "sab",
        "desc": "Skeptic's Annotated Bible",
        "url": "http://skepticsannotatedbible.com/contra/",
        "contradictions": [
            {
                "desc": "How many men did the chief of David's captains kill?",
                "refs": {
                    "300": [
                        "1 Chronicles 11:11"
                    ],
                    "800": [
                        "2 Samuel 23:8"
                    ]
                },
                "url": "300or800.html",
                ... other metadata ...
            },
            ... more contradictions ...
        ]
    },
    ... other sources ...
}
```

### Issue Information
The format of the issue bar chart information data files (`science.json`, `violence.json`, `misogyny.json`, and `homosexual.json`) is really just a list of books which each contain a list of references:

```javascript
[
    {
        "name": "Genesis",
        "refs": [
            "1:1",
            "1:3-5",
            "2:3",
            ... more refs ...
        ]
    },
    ... more books ...
]
```

License
-------
This creative work is licensed under a Creative Commons Share-Alike Attribution Unported 3.0 license, and relevant source code in the `scripts` and `web` directories are licensed under an MIT-style license. All contributions will be licensed in a similar fashion.
