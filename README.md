# Pure heroes 
[![CircleCI](https://circleci.com/gh/crappylime/pure-heroes.svg?style=svg)](https://circleci.com/gh/crappylime/pure-heroes)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=crappylime_pure-heroes&metric=alert_status)](https://sonarcloud.io/dashboard?id=crappylime_pure-heroes)
[![Coverage](https://sonarcloud.io/api/project_badges/measure?project=crappylime_pure-heroes&metric=coverage)](https://sonarcloud.io/dashboard?id=crappylime_pure-heroes)
[![Maintainability Rating](https://sonarcloud.io/api/project_badges/measure?project=crappylime_pure-heroes&metric=sqale_rating)](https://sonarcloud.io/dashboard?id=crappylime_pure-heroes)
<a href="https://github.com/crappylime/pure-heroes/commits/master"><img src="https://img.shields.io/github/last-commit/crappylime/pure-heroes.svg?style=plasticr"/></a>
[![Angular Style Guide](https://mgechev.github.io/angular2-style-guide/images/badge.svg)](https://angular.io/styleguide)
[![license](https://img.shields.io/github/license/crappylime/pure-heroes.svg)](https://github.com/crappylime/pure-heroes/blob/master/LICENSE)

Automate code review with static code analysis.  
TSLint & SonarQube configuration and rules for the *Tour of Heroes* original Angular tutorial. Overview of ways to speed up the code review process.  
Check out this project on [stackblitz](https://stackblitz.com/github/crappylime/pure-heroes).

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
It is always good to ask yourself about the quality of the code, whether it will be easy to maintain, and maybe management also wants to know about it. When the SonarQube report can help, TSLint can check and enforce the high quality code of your peers.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites
* [VS Code](https://code.visualstudio.com) (you will get extensions that I recommend) or other IDE
* [Node.js v. 10.16.3](https://nodejs.org) or higher

### Installing
1. Clone repo

    ```sh
    $ git clone https://github.com/crappylime/pure-heroes.git
    ```

2. Go to the project root

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

7. Click set up manually and fill out the form.

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

12. Check out the produced SonarQube analysis on [SonarCloud](https://sonarcloud.io).

## [TSLint](https://palantir.github.io/tslint/)

the standard linter for `TypeScript`. The default linting tool for `Angular`.  

1. Why?

    Having lint rules in place means that you will get a nice error when you are doing
    something that you should not be. This will enforce consistency in your application and
    readability. Some lint rules even come with fixes to resolve the lint error. If you want to configure
    your own custom lint rule, you can do that too.

2. [Configuration](https://palantir.github.io/tslint/usage/configuration/)
    
    The default configuration for `Angular` is specified in the project's `tslint.json` file.

    `tslint.json` extends `"tslint:recommended"` to include rules recommended for TypeScript projects, they can be found [here](https://github.com/palantir/tslint/blob/master/src/configs/recommended.ts).

    All these rules are described [here](https://palantir.github.io/tslint/rules/) along with the given **scheme** and example.

    The TS rules are divided into 5 categories:
    - TS-specific,
    - Functionality,
    - Maintainability,
    - Style,
    - Format.

    Each rule can have one of the flags: TS Only, Has Fixer, Requires type info.

    There is a [playground](https://palantir.github.io/tslint-playground/) available to test each of them.

    The `Angular` `tslint.json` file also contains [Codelyzer](https://github.com/mgechev/codelyzer) rules that are specific to `Angular`:
    ```
      "rulesDirectory": [
        "codelyzer"
    ]
    ```

3. [Custom rule sets from the community](https://github.com/palantir/tslint/blob/master/README.md#custom-rules--plugins)

    If you follow the [Angular Style Guide](https://angular.io/guide/styleguide), there are several npm packages that implement these rules:
    - [tslint-angular](https://www.npmjs.com/package/tslint-angular),
    - [angular-tslint-rules](https://www.npmjs.com/package/angular-tslint-rules)

4. You can exclude files from linting in the `angular.json`, for example tests:

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

    You can also [exclude some files](https://palantir.github.io/tslint/usage/configuration/) in the `tslint.json` using `linterOptions?: { exclude?: string[] }`.

5. Running tslint

    `lint` script is defined in the `package.json`:

    ```
    "lint": "ng lint"
    ```

    [ng lint](https://angular.io/cli/lint) has several flags. Among them `--fix`.

    You can run it with `npm`:

    ```sh
    $ npm run lint
    ```

    or

    ```sh
    $ ng lint
    ```

    or with [tslint cli](https://palantir.github.io/tslint/usage/cli/), for example:

    ```sh
    $ tslint ./src/**/*.ts -t verbose
    ```
    will lint only `ts` files within `src` directory and display the errors in verbose format.

6. Are default rules not enough?

    It depends on your team's workflow. However, if you tend to put the same comment over and over when reviewing the code, you can probably automate it.
    
    Some tslint rules that I found interesting in the [angular-tslint-rules](https://github.com/fulls1z3/angular-tslint-rules/blob/master/tslint.json): no-implicit-dependencies, prefer-template, typedef, strict-boolean-expressions, no-boolean-literal-compare, arrow-return-shorthand, max-file-line-count, restrict-plus-operands, prefer-conditional-expression, template-cyclomatic-complexity, prefer-readonly.

7. Deprecation

    > __TSLint will be deprecated some time in 2019__. See this issue for more details: [Roadmap: TSLint &rarr; ESLint](https://github.com/palantir/tslint/issues/4534). If you're interested in helping with the TSLint/ESLint migration, please check out our [OSS Fellowship](https://medium.com/palantir/fellowships-for-open-source-developers-3892e6b75ee1) program.
    >
    > --[tslint readme](https://github.com/palantir/tslint/blob/master/README.md) as of 15th October 2019.


## [SonarSource](https://www.sonarsource.com/)

provides solutions for continuous code quality:
- on-premise **[SonarQube](https://www.sonarqube.org/)** - it is installed and runs on computers on the premises of the person or organization using the software. Community version available.
- **[SonarCloud](https://sonarcloud.io/about)** works in the cloud, free for open-source projects - used in this repository.

<details><summary><b>Show instructions on how to integrate an existing Angular project with Sonar</b></summary>

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
   * add `sonar.login` if necessary

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
