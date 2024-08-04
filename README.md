# github-action-react-deployment

This is based on the following tutorial:

> https://www.linkedin.com/pulse/automate-react-app-deployment-github-actions-elar-saks-/

The tutorial provides a simple GitHub Actions example for deploying a React App to `github.io`. The repository you are viewing now successfully delivers on that.

View the demo result here:

> https://codingkriggs.github.io/github-action-react-deployment/

You should see "`Edit src/App.js and save to reload.`", the recognizable boilerplate html that is found in that very file, `src/App.js`. This is how we know the code is hosted and React is working.

The tutorial is clear and self-explanatory. However I will draw your attention to differences I've added to this version.

---

## Differences

### Creation of a React project

The tutorial left this as an exercise and prerequisite to the tutorial.

First I created a git repo called `github-action-react-deployment` and hosted it on GitHub.

Within that repo's root directory, I issued the following terminal command to create a react project *as a subdirectory*:

* `npx create-react-app react-deployment-example --template minimal`

### Added support for monorepo design

You'll note that, unlike the tutorial, the react project itself is not at the root, but a subdirectory. I've done this to illustrate how to support multiple projects in a single repo, the so-called monorepo design.

In order to achieve this, the following changes were made compared to the tutorial:

* In `.github/workflows/deploy-react-app.yml`, under yaml's `jobs/deploy-react-to-gh-pages`, I added the following lines:
  
  ```yaml
  defaults:
    run:
      working-directory: ./react-deployment-example
  ```

  This does most actions (except `Deploy`) under that working directory.
  <br/>

* In the `Deploy` step, rather than
  
  ```yaml
  publish_dir: ./build
  ```

  I included the child directory:

  ```yaml
  publish_dir: ./react-deployment-example/build
  ```

* The tutorials shows a URL that has the pattern `https://<githubuser>.github.io/react-deployment-example`, but the pattern for this monorepo should instead use the repo name, not the node package name, so `https://<githubuser>.github.io/git-action-react-deployment`.

---

## To summarize

With the changes mentioned above, the tutorial is now transformed to support the monorepo design, supporting multiple child directory packages and projects, even those not part of the NodeJS ecosystem.
