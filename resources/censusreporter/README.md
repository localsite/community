
# CensusReporter

Project 1: Document steps for deploying static portion of Wazimap to a website. Host on Github or another server.   


Since the Wazimap fork provides a Python 3 version of Census Reporter with international usage, we'll use it as a starting point for updating the US version of Census Reporter.  

Work in our [Wazimap Fork of Census Reporter]( https://github.com/modelearth/wazimap) - a Python 3.0 version of [Census Reporter](https://censusreporter.org/profiles/86000US30313-30313/).  
Wazimap is maintained by [OpenUp](https://openup.org.za/) and in used in Africa and India.  

**Future Projects:**  
1. Add ability to select country to toggle to the [US API](https://github.com/censusreporter/census-api) from [Census Report repo](https://github.com/censusreporter/censusreporter).  
2. Add [Environmentally-Enabled IO Charts](../../../io/charts/)  
3. Create a unified Census Reporter repo that moves US version to Python 3.*   


## Virtual Environment

Venv and Django with Postgres


### Mac Users


<!--
[You may need to make Python3 the default for Mac](virtualenv-troubleshooting.html) - Install a user copy of Python3 using bash, then change your default from Python2 to Python3.   


You may want to use [virtualenv](virtualenv.html) - option for use with Python 2 virtual environment.     

-->

To see the full range of options, run the following command:  

	python -m venv -h

More here: [Venv command (pythonise.com)](https://pythonise.com/categories/python/python-virtual-environments-with-the-venv-command)



Source for following: [Definitive guide to python on Mac OSX](https://medium.com/@briantorresgil/definitive-guide-to-python-on-mac-osx-65acd8d969d0)  


 ### Let's add:  

 **pyenv** for python version management and  
 **poetry** for python package/venv management  


<!-- I'm using xcode, but included this to note the need to change .bash_profile to .zshrc -->

If you chose not to install Xcode, you’ll need to add the SDKROOT environment variable to your shell:

	echo "export SDKROOT=/Library/Developer/CommandLineTools/SDKs/MacOSX10.14.sdk" >> ~/.bash_profile

If using zsh, change the end of that last command from ~/.bash_profile to ~/.zshrc .

	echo "export SDKROOT=/Library/Developer/CommandLineTools/SDKs/MacOSX10.14.sdk" >> ~/.zshrc


Install pyenv:

	brew install pyenv

Add pyenv to your shell:

	echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bash_profile

If using zsh, change the end of that last command from ~/.bash_profile to ~/.zshrc

	echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc

(Optional) You can also <code>brew install pyenv-virtualenv</code> to add virtualenv support to pyenv, but it’s not required since most of the virtualenv work you’ll do with poetry after we install it later. Some people like the pyenv-virtualenv support anyway.  

For the next step, see the [definitive guide](https://medium.com/@briantorresgil/definitive-guide-to-python-on-mac-osx-65acd8d969d0) script for installing Python as a safety net. <!-- skipped because I'd already done this -->  Include python 2.7 <!-- might need this -->

	python -V

Pick a version, then set it as the global python version:

	pyenv global 3.7.6

### Install poetry

Why you should use poetry:  
Obsoletes virtualenv, virtualenvwrapper, pipenv, setup.py, requirements.txt, and more.


\~/ translates to your user’s home directory



My .bash_profile contains:

	# aliases
	alias cd..="cd .."
	alias l="ls -al"
	alias lp="ls -p"
	alias h=history

## Wazimap Census Reporter (Setup)


Based on the [Wazimap Setup](https://wazimap.readthedocs.io/en/latest/started.html)  

Install postgres before you enter a virtual environment

	brew install postgres

Optional, if you need postgresql to be launched on login:

	To have launchd start postgresql now and restart at login:
	  brew services start postgresql
	Or, if you don't want/need a background service you can just run:
	  pg_ctl -D /usr/local/var/postgres start

## Using venv

This will install Python 3.7.3 (or latest) and create a subfolder called "env"   
--prompt is optional for showing a name before your terminal prompt.  
If your default is still python 2, then start commands with python3.  

	python -m venv ~/Documents/env1 --prompt MYTEST

Or if your are already in the folder where you're creating your environment...  
(Default directory on a mac is Users/[username].)  

	python -m venv env1

Exclude your virtual environment directory from your version control system by adding "env1" to .gitignore

Then activate. Your commands will then start with (env1):

	source ~/Documents/env1/bin/activate

Command above differs for bash/zsh. Note that \~/Documents/ is not included here:  

See: https://docs.python.org/3/library/venv.html  

Upgrade pip to 20.0.2+ since 19.0.3 is system default:

	pip install --upgrade pip

## Django

Installs Django 3.0.3

	pip install django
	django-admin startproject wazimap_ex
	cd wazimap_ex
	rm wazimap_ex/urls.py wazimap_ex/wsgi.py

Install GDAL

	brew install gdal --HEAD

Had one error out of many successful. Appears to have built from mpc-1.1.0.tar.gz source:

	==> Pouring mpfr-4.0.2.catalina.bottle.tar.gz
	🍺  /usr/local/Cellar/mpfr/4.0.2: 28 files, 4.7MB
	==> Installing gdal dependency: libmpc
	==> Downloading https://homebrew.bintray.com/bottles/libmpc-1.1.0.catalina.bottle.tar.gz
	-=O=-    #    #    #    #                                                     
	curl: (22) The requested URL returned error: 504 Gateway Time-out
	Error: Failed to download resource "libmpc"
	Download failed: https://homebrew.bintray.com/bottles/libmpc-1.1.0.catalina.bottle.tar.gz
	Warning: Bottle installation failed: building from source.
	==> Downloading https://ftp.gnu.org/gnu/mpc/mpc-1.1.0.tar.gz

You'll need to have PostgreSQL installed first above.  

**Question:** Can we modify so it's not necessary to install PostgreSQL, and only use the API?  

	pip install wazimap

Results with successfully installed Django-2.2.6

See [Step 6](https://wazimap.readthedocs.io/en/latest/started.html) about how to change your settings.py file.

Step 7 - If "createuser -P wazimap" results in error, restart postgresql in another terminal:  

	brew services restart postgresql


<!-- superuser helix p: helix1 -->

You may want to [install DBeaver](https://dbeaver.io/download/) SQL Database tools.  

On Mac, first  install the latest version of Java:  

	brew cask install adoptopenjdk --no-quarantine
	brew cask install dbeaver-community

<!--
	If unable to open DBeaver app, try
	brew cask install adoptopenjdk --no-quarantine

	Source:
	https://github.com/AdoptOpenJDK/homebrew-openjdk/issues/267
-->

DBeaver shortcuts

	Run SQL script: Alt+X
	Run only part of a SQL script: Ctrl+Enter
	You can turn on/off the right panels: F7

	You can generate SQL statements (SELECT/INSERT/UPDATE/DELETE) based on selected rows. To generate SQL, right-click the selected rows, then click Generate SQL and select one of the SQL commands you see.


## django

Trying to get site to appear at: http://localhost:8000/admin
https://docs.djangoproject.com/en/3.0/intro/tutorial01/

	django-admin startproject mysite
	cd mysite
	python manage.py runserver

Stop dejango site

	Ctrl-C

When you're done with the virtual environment

	deactivate