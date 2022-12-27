## How do I...

### Navigate to the project?

```bash
# From the git bash command prompt...

cd ~/Documents/VSCode/erik-website  # Navigate to the root of the website
git checkout master                 # Navigate to the master branch
```

### Make minor changes to the site?
Edit the site using VSCode. Once you're satisfied with your changes:

```bash
git status              # See what you changed
git add <FILE>          # Add each file you changed
git commit -m <MESSAGE> # Commit your changes
git status              # Make sure your working branch is clean
git push                # Optionally, push your changes to github
```

### Make major changes to the site?
You'll probably want a separate branch for this

```bash
git checkout -b <BRANCH NAME>                 # Create a new branch
...                                           # Make some changes, add files, commit
git push --set-upstream origin <BRANCH NAME>  # Optionally, push your changes to github
git checkout master                           # Checkout the master branch
git merge <BRANCH NAME>                       # Merge your changes
git branch -D <BRANCH NAME>                   # Optionally, delete the branch
```

### Deploy the site locally?

```bash
docker-compose up  # Navigate to localhost:8080 in your browser once this comes up. Press <ctrl+c> to close
```

### Deploy the site to github pages?

Before deploying to github pages, always check that the site loads locally. If it does:

```bash
git checkout master
git status           # Make sure your working branch is clean. If not, commit or abort your changes
./bin/deploy         # Once this is done, navigate to the github actions pane and check that the deployment worked
```


## Important files
You can change almost every aspect of the site if you know where to look.
Here are some quick highlights:

```text
.
├── assets                             # Storage place for images/PDFs/GIFs/etc
│   ├── img
│   │   ├── favicon.ico                # Your site icon
│   │   ├── prof_pic.jpg               # Your profile picture
│   ├── pdf
│   │   └── wendt_erik_cv_2022.pdf     # Your CV
├── _bibliography
│   └── papers.bib                     # Citations for your papers
├── bin
│   ├── deploy                         # Deployment script. I changed this to use docker-compose (instead of building the site locally)
├── _config.yml                        # Site config file. You probably won't have to touch this much
├── docker-compose.yml                 # File for deploying docker container. You probably won't have to touch this
├── Dockerfile                         # File for building docker container. You probably won't have to touch this
├── Gemfile                            # Site dependencies. You probably won't have to touch this
├── _pages                             # Pages for your site. Editing this should update the site
    ├── about.md
    ├── cv.md
    ├── dropdown.md
    ├── publications.md
    └── teaching.md
```


## Help! It looks like...

### I'm not on the master branch
```bash
git checkout master    # This should get you back most of the time
```

### The deployment failed
I can think of at least 3 causes for this:

1. You're on the wrong branch. Run `git branch` and make sure it is pointing to master
2. The docker compose portion of the script failed. Run `docker container prune` and try deploying again
3. You made a bad change. Run `git status` to make sure all your changes are committed. If they are, you might
   want to navigate around commits to try and find the last commit that worked. You can view recent commits
   by running `git log`, and can navigate beween commits using `git checkout <COMMIT HASH>`.

### The docker engine isn't running
(Re)start the docker application in your desktop. Check that Docker is up by running `docker container ls`.


## Technical details
This site uses [Jekyll](jekyllrb.com) to generate a static webpage from markdown files.

If you run `docker-compose up -d` and then `ls`, you'll see a `_site` folder. If you navigate to that `_site`
folder you'll see the static files that make up your site.

When the local docker deployment runs, it generates the static files and serves them using a pre-built
image. By default it serves the content on localhost:8080.

When the github deployment script runs, it does the following:

- Deploys your site locally using docker-compose to generate the static files
- Copies those static files over to the  `gh-pages` branch, commits the files, and pushes to github
- Runs a github action to deploy the site
