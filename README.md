### Markdown
Codecov NodeJS / Javascript Example
Guide
Travis Setup
Add the following to your .travis.yml:

language:
  node_js
install:
  - npm install -g codecov
script:
  - istanbul cover ./node_modules/mocha/bin/_mocha --reporter lcovonly -- -R spec
  - codecov
The first script line will change depending on your coverage collecting tool, see below.

Produce Coverage Reports
Mocha + Blanket.js
Install blanket.js
Configure blanket according to docs.
Run your tests with a command like this:
NODE_ENV=test YOURPACKAGE_COVERAGE=1 ./node_modules/.bin/mocha \
  --require blanket \
  --reporter mocha-lcov-reporter
codecov
Mocha + JSCoverage
Instrumenting your app for coverage is probably harder than it needs to be (read here), but that's also a necessary step.

In mocha, if you've got your code instrumented for coverage, the command for a travis build would look something like this:

YOURPACKAGE_COVERAGE=1 ./node_modules/.bin/mocha test -R mocha-lcov-reporter
Istanbul
With Mocha:

istanbul cover ./node_modules/mocha/bin/_mocha --report lcovonly -- -R spec && codecov
With Jasmine:

istanbul cover node_modules\jasmine\bin\jasmine.js
With Karma:

The lcov.info can be used as in other configurations. Some projects experienced better results using json output but it is no longer enabled by default. In karma.config.js both can be enabled:

module.exports = function karmaConfig (config) {
    config.set({
        ...
        reporters: [
            ...
            // Reference: https://github.com/karma-runner/karma-coverage
            // Output code coverage files
            'coverage'
        ],
        // Configure code coverage reporter
        coverageReporter: {
            reporters: [
                // generates ./coverage/lcov.info
                {type:'lcovonly', subdir: '.'},
                // generates ./coverage/coverage-final.json
                {type:'json', subdir: '.'},
            ]
        },
        ...
    });
};
In package.json supply either lcov.info or coverage-final.json to codecov:

{
  "scripts": {
    "report-coverage": "codecov",
    ...
  }
  ...
}
Nodeunit + JSCoverage
Depend on nodeunit and jscoverage:

npm install nodeunit jscoverage codecov --save-dev
Add a codecov script to "scripts" in your package.json:

"scripts": {
  "test": "nodeunit test",
  "codecov": "jscoverage lib && YOURPACKAGE_COVERAGE=1 nodeunit --reporter=lcov test && codecov"
}
Ensure your app requires instrumented code when process.env.YOURPACKAGE_COVERAGE variable is defined.

Run your tests with a command like this:

npm run codecov
Poncho
Client-side JS code coverage using PhantomJS, Mocha and Blanket:

Configure Mocha for browser
Mark target script(s) with data-cover html-attribute
Run your tests with a command like this:
./node_modules/.bin/poncho -R lcov test/test.html && codecov
Lab
istanbul cover ./node_modules/lab/bin/lab --report lcovonly  -- -l  && codecov
nyc
{
  "scripts": {
    "report-coverage": "nyc report --reporter=text-lcov > coverage.lcov && codecov",
    ...
  }
  ...
}
Jest
Add it in your package.json:

"jest": {
  "coverageDirectory": "./coverage/",
  "collectCoverage": true
}
Jest will now generate coverage files into coverage/

Run your tests with a command like this:

jest && codecov
JSX
There have been reports of gotwarlost/istanbul not working properly with JSX files, which provide inaccurate coverage results. Please try using ambitioninc/babel-istanbul.

Caveats
Private Repo
Repository tokens are required for (a) all private repos, (b) public repos not using Travis-CI, CircleCI or AppVeyor. Find your repository token at Codecov and provide via codecov --token=:token or export CODECOV_TOKEN=":token"

Support
FAQ
Q: Is there a TypeScript example?
A: Yes codecov/example-typescript.
More documentation at https://docs.codecov.io
Configure codecov through the codecov.yml https://docs.codecov.io/docs/codecov-yaml
We are happy to help if you have any questions. Please contact email our Support at support@codecov.io
