# sphinx-transifex
testing a workflow for multi-language documentation using sphinx and transifex  

`pip install transifex-client`
`pip install sphinx-intl`

create project repository on Github  
```
git clone https://github.com/{{account}}/{{repository}}.git
cd {{repository}}
echo $'.DS_Store\npage/*' >.gitignore
git clone https://github.com/{{account}}/{{repository}}.git page
cd page
git branch gh-pages
git symbolic-ref HEAD refs/heads/gh-pages
rm .git/index
git clean -fdx
cd ../
mkdir docs
cd docs
sphinx-quickstart
# fill in project name and author name(s)
# change 'epub output' to [y]
# accept other defaults
```
create your .rst files for your documentation
create project on [Transifex](https://www.transifex.com/)     
```
make gettext  
sphinx-intl update -p _build/locale -l {{language_code}}
tx init
# press enter to accept default for Transifex instance
sphinx-intl update-txconfig-resources --pot-dir _build/locale --transifex-project-name {{project_name}}
tx push -s
```
translate on Transifex website  
```
tx pull -l {{language_code}}
sphinx-build -b dirhtml -d _build/doctrees -D language="en" . ../page/en/
sphinx-build -b dirhtml -d _build/doctrees -D language="{{language_code}}" . ../page/{{language_code}}/
cd ../page
touch .nojekyll
git add .
git commit -m "built docs"
git push origin gh-pages
```


### To do:
- check and document the update process for the translation files
- implement a pretty theme
  - figure out homepage thing and links between languages
- figure out generation/availability of PDF and epub downloads
- automate the transifex and build process (write a bash script or figure out how to edit the makefile to do it)
