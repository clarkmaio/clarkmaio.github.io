# clarkmaio.github.io
This is a repo for future Andrea to not forget how to deploy new pages


## What you are using to edit this github page
You are using [**MKDocs**](https://squidfunk.github.io/mkdocs-material/publishing-your-site/) in particular theme **material**.


## Pages
You have to edit only **docs** folder in principle.

### Create new page
1. Create following structure:
```
docs
└── pages
    └── newpage
        ├── index.md
        └── subpages
```

2. Edit `index.md`
**HINT**
You can add properties of the pages with the following syntax to use on top of the page 
```
# Example: hide page navigation and table of content
---
hide:
    - navigation
    - toc
---
```


### Link to page
* To add the page to navigation bar edit `mkdocs.yml/nav` adding new item with path to the new page.

* To create a link to the page use the syntax:
```
https://clarkmaio.github.io/???
```


## Deploy

From terminal:
```
cd /PATH/TO/clarkmaio_page
mkdocs gh-deploy
```

Everything will be pushed on `gh-pages` branch and the github page will be updated. 
