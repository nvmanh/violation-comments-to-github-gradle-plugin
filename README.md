# Violation Comments to GitHub Gradle Plugin [![Build Status](https://travis-ci.org/tomasbjerre/violation-comments-to-github-gradle-plugin.svg?branch=master)](https://travis-ci.org/tomasbjerre/violation-comments-to-github-gradle-plugin) [![Maven Central](https://maven-badges.herokuapp.com/maven-central/se.bjurr.violations/violation-comments-to-github-gradle-plugin/badge.svg)](https://maven-badges.herokuapp.com/maven-central/se.bjurr.violations/violation-comments-to-github-gradle-plugin)

This is a Gradle plugin for [Violation Comments to GitHub Lib](https://github.com/tomasbjerre/violation-comments-to-github-lib).

It can be used in Travis, or any other build server, to read results from static code analysis and comment pull requests in GitHub with them.

You can have a look at [violations-test](https://github.com/tomasbjerre/violations-test/pull/2) to see what the result may look like.

It supports:
 * [_Checkstyle_](http://checkstyle.sourceforge.net/)
 * [_CPPLint_](https://github.com/theandrewdavis/cpplint)
 * [_CPPCheck_](http://cppcheck.sourceforge.net/)
 * [_CSSLint_](https://github.com/CSSLint/csslint)
 * [_Findbugs_](http://findbugs.sourceforge.net/)
 * [_Flake8_](http://flake8.readthedocs.org/en/latest/) ([_PyLint_](https://www.pylint.org/), [_Pep8_](https://github.com/PyCQA/pycodestyle), [_Mccabe_](https://pypi.python.org/pypi/mccabe), [_PyFlakes_](https://pypi.python.org/pypi/pyflakes))
 * [_JSHint_](http://jshint.com/)
 * _Lint_ A common XML format, used by different linters.
 * [_PerlCritic_](https://github.com/Perl-Critic)
 * [_PMD_](https://pmd.github.io/)
 * [_ReSharper_](https://www.jetbrains.com/resharper/)
 * [_XMLLint_](http://xmlsoft.org/xmllint.html)
 
## Usage ##
There is a running example [here](https://github.com/tomasbjerre/violation-comments-to-github-gradle-plugin/tree/master/violation-comments-to-github-gradle-plugin-example).

Here is and example: 

```
  buildscript {
    repositories {
      maven {
        url "https://plugins.gradle.org/m2/"
      }
    }
    dependencies {
      classpath "se.bjurr.violations:violation-comments-to-github-gradle-plugin:1.6"
    }
  }

  apply plugin: "se.bjurr.violations.violation-comments-to-github-gradle-plugin"

  task violationCommentsToGitHub(type: se.bjurr.violations.comments.github.plugin.gradle.ViolationCommentsToGitHubTask) {
   repositoryOwner = "tomasbjerre";
   repositoryName = "violations-test"
   pullRequestId = System.properties['GITHUB_PULLREQUESTID']
   username = System.properties['GITHUB_USERNAME']
   password = System.properties['GITHUB_PASSWORD']
   oAuth2Token = System.properties['GITHUB_OAUTH2TOKEN']
   gitHubUrl = "https://api.github.com/"
   createCommentWithAllSingleFileComments = false
   createSingleFileComments = true
   commentOnlyChangedContent = true
   violations = [
    ["FINDBUGS",   ".", ".*/findbugs/.*\\.xml\$"],
    ["PMD",        ".", ".*/pmd/.*\\.xml\$"],
    ["CHECKSTYLE", ".", ".*/checkstyle/.*\\.xml\$"],
    ["JSHINT",     ".", ".*/jshint/.*\\.xml\$"],
    ["CSSLINT",    ".", ".*/csslint/.*\\.xml\$"]
   ]
  }
```

To send violations, just run:
```
./gradlew violationCommentsToGitHub -DGITHUB_PULLREQUESTID=$GITHUB_PULL_REQUEST -DGITHUB_USERNAME=... -DGITHUB_PASSWORD=...
```

Or if you want to use OAuth2:
```
./gradlew violationCommentsToGitHub -DGITHUB_PULLREQUESTID=$TRAVIS_PULL_REQUEST -DGITHUB_OAUTH2TOKEN=$GITHUB_OAUTH2TOKEN
```

You may also have a look at [Violations Lib](https://github.com/tomasbjerre/violations-lib).

## Developer instructions

To build the code you need to run `build.sh` in root of repo. You may also have a look at `.travis.yml`.

To do a release you need to do `./gradlew release -Dgradle.publish.key=... -Dgradle.publish.secret=...` and release the artifact from [staging](https://oss.sonatype.org/#stagingRepositories). More information [here](http://central.sonatype.org/pages/releasing-the-deployment.html).
