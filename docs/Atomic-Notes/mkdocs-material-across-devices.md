# Mkdocs Material Multiple Devices Setup

## Debian


1. Clone your git repo locally
	- `git clone https://github.com/reponame`
	***Note: make sure that the `.gitignore` file micmics the one in the official repo. To avoid issues just copy the `.gitignore` file into the newly cloned repo***

2. Create a `python3` virtual enviroment
	- `python3 -m venv 'repo-name-folder'`
	- cd into the newly created 'repo-name-folder'
    - now, all changes you do to the `mkdocs.yml` will be tracked
    - notes addition should always be added into the `docs` folder

3. If you wish to test changes locally, install `mkdocs-material`
	- activate the `python3` venv, `source ./bin/activate'
	- install `mkdocs-material` by running in a already activated enviroment `pip install mkdocs-material'
	
4. Lastly, just run, `mkdocs serve -a 0.0.0.0:8000 --liverealod`
