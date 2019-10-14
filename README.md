# Pure heroes 
[![CircleCI](https://circleci.com/gh/crappylime/pure-heroes.svg?style=svg)](https://circleci.com/gh/crappylime/pure-heroes)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=crappylime_pure-heroes&metric=alert_status)](https://sonarcloud.io/dashboard?id=crappylime_pure-heroes)
[![Coverage](https://sonarcloud.io/api/project_badges/measure?project=crappylime_pure-heroes&metric=coverage)](https://sonarcloud.io/dashboard?id=crappylime_pure-heroes)
[![Maintainability Rating](https://sonarcloud.io/api/project_badges/measure?project=crappylime_pure-heroes&metric=sqale_rating)](https://sonarcloud.io/dashboard?id=crappylime_pure-heroes)
<a href="https://github.com/crappylime/pure-heroes/commits/master"><img src="https://img.shields.io/github/last-commit/crappylime/pure-heroes.svg?style=plasticr"/></a>
[![Angular Style Guide](https://mgechev.github.io/angular2-style-guide/images/badge.svg)](https://angular.io/styleguide)
[![license](https://img.shields.io/github/license/crappylime/pure-heroes.svg)](https://github.com/crappylime/pure-heroes/blob/master/LICENSE)

Automate code review with static code analysis.  
TSLint & SonarQube configuration and rules for *Tour of Heroes* original Angular tutorial. Overview of ways to speed up the code review process.  
Checkout this project on [stackblitz](https://stackblitz.com/github/crappylime/pure-heroes).

## Table of contents
  - [Motivation](#motivation)
  - [Getting Started](#getting-started)
    - [Prerequisites](#prerequisites)
    - [Installing](#installing)
  - [TSLint](#tslint)
  - [SonarSource](#sonarsource)
  - [License](#license)
  - [Credits](#credits)

## Motivation
Our team had some time off before the first release for production. I asked myself, is the code good enough? At that time, we had the default tslint configuration and, as you can guess, there is always room for improvement. But it would be nice to get priorities, hints in the IDE and there tslint with sonar proved to be very useful. Their configuration took me some time, that's why I will describe step by step how to do it.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites
* [VS Code](https://code.visualstudio.com) (you will get there extensions I recommend) or any other IDE
* [Node.js v. 10.16.3](https://nodejs.org) or higher

### Installing
1. Clone repo

    ```sh
    $ git clone https://github.com/crappylime/pure-heroes.git
    ```

2. Go into the project root directory

    ```sh
    $ cd pure-heroes 
    ```

3. Install dependencies

    ```sh
    $ npm i
    ```

4. Run tests with code coverage

    ```sh
    $ npm run test-ci
    ```

5. Log in to [SonarCloud](https://sonarcloud.io). You can log in with your Github account.
   
6. Click analyze new project.

    ![add project](https://raw.githubusercontent.com/crappylime/pure-heroes/master/docs/images/add-project.png)

7. Click set up manually and fill in the form.

    ![set up manually](https://raw.githubusercontent.com/crappylime/pure-heroes/master/docs/images/setup-manually.png)

8. Click analyze your project manually.

    ![analyze manually](https://raw.githubusercontent.com/crappylime/pure-heroes/master/docs/images/analyze-manually.png)

9. Click other, Linux and copy values of `Dsonar.login`, `Dsonar.projectKey` and `Dsonar.organization`.

    ![sonar credentials](https://raw.githubusercontent.com/crappylime/pure-heroes/master/docs/images/sonar-credentials.png)

10. Paste copied values to `sonar-project.properties`:

    ```diff
    - sonar.projectKey=crappylime_pure-heroes
    + sonar.projectKey=YOUR_KEY
    - sonar.organization=crappylime
    + sonar.organization=YOUR_ORGANIZATION
    + sonar.login=YOUR_LOGIN
    ```

11. Run sonar

    ```sh
    $ npm run sonar
    ```

12. Checkout the produced SonarQube analysis on [SonarCloud](https://sonarcloud.io).

## [TSLint](https://palantir.github.io/tslint/)

the standard linter for `TypeScript`. The default linting tool for `Angular`.  

1. Why?
    Having lint rules in place means that you will get a nice error when you are doing
    something that you should not be. This will enforce consistency in your application and
    readability. Some lint rules even come with fixes to resolve the lint error. If you want to configure
    your own custom lint rule, you can do that too.

2. Rules
    
    The default configuration for `Angular` is specified in the project's `tslint.json` file.

    `tslint.json` extends `"tslint:recommended"` and these rules you can find [here](https://github.com/palantir/tslint/blob/master/src/configs/recommended.ts).

    All rules are described [here](https://palantir.github.io/tslint/rules/) with given **schema** and example.

    Rules are split into 5 categories:
    - TS-specific,
    - Functionality,
    - Maintainability,
    - Style,
    - Format.

    Each rule can have one of the flags: TS Only, Has Fixer, Requires type info.

3. Aren't the default rules enough?

    It depends on your team workflow. However if you tend to put the same comment over and over again during the code review, it probably can be automated.  

    If you follow the [Angular Style Guide](https://angular.io/guide/styleguide), there are some npm packages available that implemented these rules:
    - [tslint-angular](https://www.npmjs.com/package/tslint-angular) recommended by codelyzer,
    - [angular-tslint-rules](https://www.npmjs.com/package/angular-tslint-rules) - when I added this to one project it showed over 2 thousand errors and I'd say that they poorly handle tests, but you can exclude them from linting.

4. Excluding tests from linting in the `angular.json`:

    ```diff
        "lint": {
          "builder": "@angular-devkit/build-angular:tslint",
          "options": {
            "tsConfig": [
              "tsconfig.app.json",
              "tsconfig.spec.json",
              "e2e/tsconfig.json"
            ],
            "exclude": [
    -         "**/node_modules/**"
    +         "**/node_modules/**",
    +         "**/*.spec.ts"
            ]
          }
    ```

5. [Codelyzer](https://github.com/mgechev/codelyzer)

    another tool for static code analysis, included at the end of the `tslint.json`:
    ```
      "rulesDirectory": [
        "codelyzer"
    ]
    ```
    If you wanted to override some rule and you didn't find the description on `tslint` website, that's a second place you should check.

6. Running tslint

    `lint` script is defined in the `package.json`:

    ```
    "lint": "ng lint"
    ```

    [ng lint](https://angular.io/cli/lint) has some flags available. Among them `--fix`.

    you can run it with `npm`:

    ```sh
    $ npm run lint
    ```

    or

    ```sh
    $ ng lint
    ```

    or using `tslint` command:

    ```sh
    $ tslint ./src/**/*.ts -t verbose
    ```

7. Additional rules I found more interesting:




## [SonarSource](https://www.sonarsource.com/)

provides solutions for continuous code quality:
- on-premise **[SonarQube](https://www.sonarqube.org/)** - it is installed and runs on computers on the premises of the person or organization using the software. Community version available.


- **[SonarCloud](https://sonarcloud.io/about)** runs on cloud, free for open-source projects - used for this repo.

<details><summary><b>Show instructions for integrating existing Angular project with Sonar</b></summary>

1. Install [sonar-scanner](https://www.npmjs.com/package/sonar-scanner):

    ```sh
    $ npm i -D sonar-scanner
    ```

2. Install [tslint-sonarts](https://www.npmjs.com/package/tslint-sonarts):

    ```sh
    $ npm i -D tslint-sonarts
    ```

3. Add the `sonar` script to your `package.json`:

    ```diff
      "scripts": {
        "build": "ng build --prod",
    +   "sonar": "sonar-scanner",
        "e2e": "e2e"
      }
    ```

4. Create `sonar-project.properties` file in the project root, content example:

    ```
    sonar.host.url=https://sonarcloud.io
    sonar.projectKey=crappylime_pure-heroes
    sonar.organization=crappylime
    sonar.projectName=pure-heroes
    sonar.projectVersion=1.0
    sonar.sources=src
    sonar.sourceEncoding=UTF-8
    sonar.ts.tslintconfigpath=tslint.json
    sonar.exclusions=**/*.spec.ts,**/src/assets/**/*,**/src/favicon.ico,**/src/karma.conf.js
    sonar.typescript.exclusions=**/main.ts,**/environments/environment*.ts,**/*routing.module.ts
    sonar.tests.inclusions=**/*.spec.ts
    sonar.javascript.lcov.reportPaths=coverage/angular.io-example/lcov.info
    ```

5. Do not forget to change in `sonar-project.properties` the following:
   * `sonar.host.url` to your host if you have SonarQube deployed
   * `sonar.projectKey`
   * `sonar.organization`
   * `sonar.projectName`
   * add `sonar.login` if needed

6. Extend `reports` types with `lcov` in the `karma.conf.js` file:

    ```diff
      coverageInstanbulReporter: {}
    -   reports: ['html', 'lcovonly', 'text-summary'],
    +   reports: ['html', 'lcov', 'lcovonly', 'text-summary'],
    ```

7. Extend `tslint.json` with sonar rules for typescript that are documented [here](www.github.com/SonarSource/SonarTS/tree/master/sonarts-core/docs/rules):

    ```diff
      {
    -   "extends": "tslint:recommended",
    +   "extends": ["tslint:recommended", "tslint-sonarts"],
    ```

8.  Run tests with code coverage:

    ```sh
    $ ng test --code-coverage --no-watch --browsers=ChromeHeadless
    ```

9. Trigger the sonar analysis for project:

    ```sh
    $ npm run sonar
    ```

</details>

## Built With

* [Angular v. 8.0.0](https://angular.io) - The web framework used
* [NPM v. 6.9.0](https://www.npmjs.com) - Dependency Management
* [Node.js v. 10.16.3](https://nodejs.org) - JavaScript runtime built
* [CircleCI](https://circleci.com) - Continuous Integration

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Credits

* analyzed code from [Tour of Heroes App and Tutorial](https://angular.io/tutorial)
* inspiration for `sonar-project.properties` content from [Isaac Martinez](https://isaacmartinezblog.wordpress.com/2018/04/02/angular-code-coverage-in-sonar-qube-and-vsts/)
* why customize tslint from [11. Make use of lint rules](https://www.freecodecamp.org/news/best-practices-for-a-clean-and-performant-angular-application-288e7b39eb6f/)
