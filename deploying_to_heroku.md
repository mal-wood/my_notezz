**Create HEROKU remote**<br>
run `heroku git:remote -a <PROJECTNAME>`<br>
confirm with `git remote -v`<br>
if this does not work, do `gem install heroku`<br><br>

**PUSH to Heroku**<br>
git push heroku <BRANCHNAME> (probably master)<br><br>

**Terminal commands w/ heroku**<br>
`heroku run <TERMINAL COMMAND>`
i.e. check Ruby version with `heroku run ruby -v`

