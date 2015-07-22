NewsLynx Docs
=============

> Documentation for the NewsLynx project. View at <http://newslynx.readthedocs.org>

## How to generate

Make sure you have [Sphinx](http://sphinx-doc.org) installed. This project was built with Sphinx 1.3.1. Run the following

````shell
make html
````

That will output the website to the [`_build/`](_build/) folder. This repo is set up with a webhook so whenever you push to master, the readthedocs.org site will update automatically.
