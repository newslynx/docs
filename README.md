NewsLynx Docs
=============

> Documentation for the NewsLynx project. View at <http://newslynx.readthedocs.org>

## How to generate

This project requires Sphinx to build. Run `pip install -r requirements.txt`, preferably in a virtual environment, to install.

Run the following

````shell
make html
````

That will output the website to the [`_build/`](_build/) folder. This repo is set up with a webhook so whenever you push to master, the readthedocs.org site will update automatically.
