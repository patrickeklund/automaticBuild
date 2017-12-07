# automaticBuild

Automatic Build with travis.ci

## Setting up Push access from Travis to GitHub
First I go to https://github.com/settings/applications and generate a token that I call outreach-resources with the permissions public_repo. This secret token needs to be stored somewhere, I don’t want it to be revealed inside my .travis.yml or on the travis build server. Travis supports encrypted environment variables, so I run the command echo GITHUB_PERSONAL_ACCESS_TOKEN=my_github_token | travis encrypt --add where you’d replace my_github_token with the access token generated earlier; this command stores the encrypted github token inside the .travis.yml file.

Now that I’ve got an access token available on travis we can write the script that will push things back to github (checkout push.sh). I set up the username and email address of the git user on travis, checkout the branch I wish to push to, add the files I want and commit using the environment variable $TRAVIS_BUILD_NUMBER which helps me identify which commits correspond to which builds (totally optional). I finally push this commit back to the repository which takes the form: https://${GITHUB_PERSONAL_ACCESS_TOKEN}@github.com/<user_name>/<repo_name>.git, here GH_TOKEN is substituted inside the build server which acts as a username to the repository with full commit rights!

Travis’s build process is instructed by a file inside your repository named .travis.yml which contains information on the language of the repository, build comamands, dependencies, post build hooks etc. In my YAML file you can see I’m using the hooks before_install, script and after_success, all of which take a command, or a list of commands and execute them. You’ll want to keep the push.sh commands outside of the YAML file (i.e. don’t get rid of push.sh and put them all in after_success as ${GH_TOKEN} won’t be substituted).
