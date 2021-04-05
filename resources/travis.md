---
layout: page
title: Travis CI Guide
published: true
---

## Guide to setting up TravisCI, linters, and githooks for your Github Repo

### Motivations

#### TravisCI

TravisCI is a continuous integration system that allows you to easily test and deploy code on the fly. We will be using TravisCI to integrate automatic linting and unit testing within our GitHub repository. More information about TravisCI can be found [here](https://travis-ci.com/).

#### Linters

Linters are code parsers that check your code for syntax errors, common style mistakes, and makes sure that your code falls under best practices. Linters help save time, detect bugs, and improve code quality. Linters exist for all types of languages and even markdown such as HTML, CSS, and JSON. Linters are especially helpful for detecting syntax errors while using lightweight code editors such as Atom or VSCode that don't have that built in functionality of IDEs such as IntelliJ or Pycharm.

### Setup

1. As an admin for your GitHub repo, sign up for a [Travis-CI account](https://travis-ci.com)

2. Once you’re signed in, you'll be prompted to synchronized your repositories from GitHub. If you are in CS52 then this is automatically enabled on all repos and you can skip to step 3.

      *Note: since we use private repositories you need to go to [http://travis-ci.com](http://travis-ci.com) or [http://travis-ci.org](http://travis-ci.org) for personal public repositories.*

3. Travis configuration

    Create a `.travis.yml` file in your home directory. This is the configuration file that Travis uses. You can read more about detailed configurations [here](https://docs.travis-ci.com/user/customizing-the-build/). By default, `node_js` configurations will run `npm test` or `yarn test` after the build completes. That is a script you have defined in your `package.json` file named 'test'.

```yml
language: node_js
node_js:
  - "13"
```

4. Push to GitHub

    After you are finished setting up your `.travis.yml`, simply push your changes to GitHub

        git add .travis.yml
        git commit -m 'Travis CI Config'
        git push origin main

5. Turn on GitHub Protected Branch.

    You want to set up protected branches for your GitHub repository to ensure that all required CI tests are passing before collaborators can make changes to a protected branch. This will prevent potentially buggy code from being merged into master. To do so simply visit `https://github.com/<team>/<repo>/settings/branches` and turn on protected branch for `main`.

    ![protected-branch](/assets/imgs/travis/protected-branch.jpg)

    After setting this up, click the `edit` button for more configuration options and make sure to check the `Require status checks to pass before merging` button.

    ![status](/assets/imgs/travis/check-status.png)

6. Setting up a linter for your project

    It is recommended that you set up a linter plugin for your text editor of choice. You can install specific linters for your selected language and the linter will display inline errors.

    ![linter error](/assets/imgs/travis/linter-error.png)

    The recommended linter plugin for javascript is [Eslint](eslint.org). You can install `eslint` using `yarn add --dev eslint`.

    The `--dev` flag adds `eslint` as a dev dependency in your `package.json` file. devDependencies are installed on `npm install` on a directory that contains `package.json` but not installed with the `--production` flag. This is used for local development.

    `eslint` comes with a series of plugins for various javascript packages. It is also recommended that you install the [react plugin](https://github.com/yannickcr/eslint-plugin-react) and if you would like, you can also follow a style guide, we are a fan of [Airbnb's](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb).

    Below is an example of a potential `eslintrc` file. You probably already have one set up!  If you do, great - no need to modify it if it works as you like it.

        ```
        module.exports = {
        "extends": "airbnb",
        "plugins": [
            "react",
            "filenames",
            "meteor",
        ],
        "rules": {
            "import/no-unresolved": [0, { "commonjs": true }],
            "no-underscore-dangle": "off",
            "react/jsx-no-bind": [2, {
                "ignoreRefs": false,
                "allowArrowFunctions": false,
                "allowBind": true,
            }],
            "camelcase": [2, { "properties": "always" }],
            "filenames/filenames": [2, "^[A-Z][a-z]+(?:[A-Z][a-z]+)*$"],
            "react/prefer-stateless-function": 0,
            "no-undef": 1,
            "prefer-arrow-callback": ["error", { "allowNamedFunctions": true }],
            "no-else-return": 0,
            "arrow-body-style": 0,
              }
          };
        ```

7. Connecting Eslint and TravisCI

    By default all Node.js configurations of Travis run `npm test` or `yarn test` automatically after the build completes.

    `yarn test` can be configured to run a script of your choice, we will be running `eslint`.
    Later we can replace this with tests once your project has those.

    Below is a sample `package.json` file that runs eslint on all javascript files in your src directory

        {
          "scripts": {
            "test": "./node_modules/.bin/eslint src"
          }
        }

8. Turn on Auto-Cancel

    Now that you are all set up with Travis, there is a setting that can help keep it running more smoothly for everyone.

    🚀 Go to your specific repository settings page: 
    https://travis-ci.com/github/(github-organization-name)/(repository-name)

    ![auto-cancel](img/auto-cancel.jpg)

    This will clear up the queue by not having older commits also tested.
