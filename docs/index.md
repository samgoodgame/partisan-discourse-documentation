# Partisan Discourse

**A web application that identifies bias in political discourse and serves as a template for operationalized machine learning.**

[![Build Status][travis_img]][travis_href]
[![Coverage Status][coveralls_img]][coveralls_href]
[![Stories in Ready][waffle_img]][waffle_href]
[![Political Parties](img/partisan.jpg)][partisan.jpg]

For an in-depth description of the project's purpose, head to the [About](about.md) page.

# TODO: Go through Bit.ly and Google OAuth processes in more detail; see if .gitignore steps are actually necessary (does PD ship with a .gitignore?); ((((User Guide))))

## Quick Start Guide for Developers
This quick start is intended to get you setup with Baleen in development mode.

This project uses Django. If you're unfamiliar with Django, it may be helpful to go through their [tutorial](https://docs.djangoproject.com/en/1.10/intro/tutorial01/).

(1) Clone the repository:
```
$ git clone git@github.com:DistrictDataLabs/partisan-discourse.git
$ cd partisan-discourse
```

(2) Create a virtualenv and install the dependencies:

```
$ virtualenv venv
$ source venv/bin/activate
$ pip install -r requirements.txt
```

Download NLTK data, which the app uses for NLP purposes. Open python in your REPL:
```
>>> import NLTK
>>> nltk.download()
```
You should have a directory within your home directory called "nltk_data". You'll use the path to that directory in your .env file, which we'll establish soon.

(3) Add a .gitignore file:

```
$ touch .gitignore  
$ open .gitignore  
```
Paste in the following:
```
# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class

# C extensions
*.so

# Distribution / packaging
.Python
env/
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
*.egg-info/
.installed.cfg
*.egg

# PyInstaller
#  Usually these files are written by a python script from a template
#  before PyInstaller builds the exe, so as to inject date/other infos into it.
*.manifest
*.spec

# Installer logs
pip-log.txt
pip-delete-this-directory.txt

# Unit test / coverage reports
htmlcov/
.tox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*,cover
.hypothesis/

# Translations
*.mo
*.pot

# Django stuff:
*.log
local_settings.py

# Flask stuff:
instance/
.webassets-cache

# Scrapy stuff:
.scrapy

# Sphinx documentation
docs/_build/

# PyBuilder
target/

# IPython Notebook
.ipynb_checkpoints

# pyenv
.python-version

# celery beat schedule file
celerybeat-schedule

# dotenv
.env

# virtualenv
venv/
ENV/

# Spyder project settings
.spyderproject

# Rope project settings
.ropeproject

# Corpora and Fixtures
corpus/fixtures/debates
corpus/fixtures/*.pickle

# Mac Stuff
.DS_Store
```

(4) Create a .env file for your environment variables:

```
$ touch .env  
$ open .env
```

Paste in the following:

```
WEB_CONCURRENCY=2  
DATABASE_URL=postgres://django@localhost/partisan  
DJANGO_SETTINGS_MODULE=partisan.settings.development  
SECRET_KEY=""  
EMAIL_HOST_USER=""  
EMAIL_HOST_PASSWORD=""  
GOOGLE_OAUTH2_CLIENT_ID=""  
GOOGLE_OAUTH2_CLIENT_SECRET=""  
BITLY_ACCESS_TOKEN=""  
NLTK_DATA=/path/to/your/NLTK/data  
```

Here's an explanation of each of the .env lines and their purposes:

- Web concurrency: Heroku hosting setting.
- Database URL: describes how to connect to Postgres locally. Here, you're specifying to use the Postgres protocol, which is necessary for Partisan Discourse. The "django@localhost" specifies that the user should be "django" and the database should be "partisan".
- Django settings module: specifies where to retrieve the settings documents. Instead of just using a settings.py, we use different settings documents for production, development, and testing. Changing the DJANGO_SETTINGS_MODULE in your .env file will change the settings that you use accordingly.
- Secret key: additional security. Using the secret key in your .env file (and including your .env file in your .gitignore file) will help prevent unauthorized access to your app.
- Email host user and password: this allows Django to send email on your behalf.
- Google OAuth Client ID and secret: enable API access from Google+.
    - If you don't have a Google OAuth Client ID or secret, see the [Google Developers page referenceing OAuth 2.0]( https://developers.google.com/identity/protocols/OAuth2).
- Bitly Access Token: URL shortening. Partisan Discourse shortens URLs using the Bitly service. Go to Bit.ly, create an account, and register your application to acquire a "Generic Access Token."
- NLTK data: paste the path to your nltk_data directory. It should have installed to your home directory.


(6) Create the proper role ("django") and database ("partisan"). In your terminal, open psql and type the following:

```
=# CREATE ROLE django WITH LOGIN;
=# CREATE DATABASE partisan WITH OWNER django;
```

When you view your databases, you should now see one with the name "partisan" and the owner "django."

(7) Migrate the app. Django uses migrations to propogate changes you've made in your models (like adding a field) into your database schema. Enter the following into your terminal:
```
$ python manage.py migrate
```

(8) Run the server. By default, Django uses port 8000.
```
python manage.py runserver
```

If you see a login page, then congratulations! You have successfully set up Partisan Discourse locally.

## Contributing to Partisan Discourse

1. Head to the [Github repository](https://github.com/DistrictDataLabs/partisan-discourse) and fork the project.

2. Check out the appropriate branch:
```
$ git fetch origin develop
$ git checkout develop
```

3. After making changes that you feel would benefit the project, push them to Github:

```
$ git add .
$ git commit -m "bug fix #(insert fix number here)"
$ git push origin develop
```

4. Submit a pull request. Head to the Partisan Discourse [Github repository](https://github.com/DistrictDataLabs/partisan-discourse), click on "pull requests," and then click on "new pull request." Choose the proper base fork and branch (Partisan Discourse; develop) and the proper head fork and branch (your fork; develop).

If you find an issue but can't fix it, be sure to submit an issue [here](https://github.com/DistrictDataLabs/partisan-discourse/issues).

#### Changelog

The release versions that are deployed to the web servers are also tagged in GitHub. You can see the tags through the GitHub web application and download the tarball of the version you'd like.

The versioning uses a three part version system, "a.b.c" - "a" represents a major release that may not be backwards compatible. "b" is incremented on minor releases that may contain extra features, but are backwards compatible. "c" releases are bug fixes or other micro changes that developers should feel free to immediately update to.

#### Version 0.1 Beta 1

* **tag**: [v0.1b1](https://github.com/DistrictDataLabs/partisan-discourse/releases/tag/v0.1b1)
* **deployment**: Monday, July 18, 2016
* **commit**: [see tag](#)

This is the first beta release of the Partisan Discourse application. Right now this simple web application allows users to sign in, then add links to go fetch web content to the global corpus. These links are then preprocessed using NLP. Users can tag the documents as Republican or Democrat, allowing us to build a political classifier.

#### Attribution

The image used in this README, [Partisan Fail][partisan.jpg] by [David Colarusso](https://www.flickr.com/photos/dcolarusso/) is licensed under [CC BY-NC 2.0](https://creativecommons.org/licenses/by-nc/2.0/)

<!-- References -->
[travis_img]: https://travis-ci.org/DistrictDataLabs/partisan-discourse.svg
[travis_href]: https://travis-ci.org/DistrictDataLabs/partisan-discourse
[waffle_img]: https://badge.waffle.io/DistrictDataLabs/partisan-discourse.png?label=ready&title=Ready
[waffle_href]: https://waffle.io/DistrictDataLabs/partisan-discourse
[coveralls_img]: https://coveralls.io/repos/github/DistrictDataLabs/partisan-discourse/badge.svg?branch=master
[coveralls_href]:https://coveralls.io/github/DistrictDataLabs/partisan-discourse?branch=master
[partisan.jpg]: https://flic.kr/p/a3bXVU
