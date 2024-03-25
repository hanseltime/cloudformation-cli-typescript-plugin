# AWS CloudFormation Resource Provider Typescript Plugin

We're excited to share our progress with adding new languages to the CloudFormation CLI!

## AWS CloudFormation Resource Provider TypeScript Plugin

[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/aws-cloudformation/cloudformation-cli-typescript-plugin/ci/master)](https://github.com/aws-cloudformation/cloudformation-cli-typescript-plugin/actions?query=branch%3Amaster+workflow%3Aci) [![Codecov](https://img.shields.io/codecov/c/gh/aws-cloudformation/cloudformation-cli-typescript-plugin)](https://codecov.io/gh/aws-cloudformation/cloudformation-cli-typescript-plugin) [![PyPI version](https://img.shields.io/pypi/v/cloudformation-cli-typescript-plugin)](https://pypi.org/project/cloudformation-cli-typescript-plugin) [![NPM version](https://img.shields.io/npm/v/@amazon-web-services-cloudformation/cloudformation-cli-typescript-lib)](https://www.npmjs.com/package/@amazon-web-services-cloudformation/cloudformation-cli-typescript-lib) [![Node.js version](https://img.shields.io/badge/dynamic/json?color=brightgreen&url=https://raw.githubusercontent.com/aws-cloudformation/cloudformation-cli-typescript-plugin/master/package.json&query=$.engines.node&label=nodejs)](https://nodejs.org/)

The CloudFormation CLI (cfn) allows you to author your own resource providers that can be used by CloudFormation.

This plugin library helps to provide TypeScript runtime bindings for the execution of your providers by CloudFormation.

Usage
-----

If you are using this package to build resource providers for CloudFormation, install the [CloudFormation CLI TypeScript Plugin](https://github.com/aws-cloudformation/cloudformation-cli-typescript-plugin) - this will automatically install the [CloudFormation CLI](https://github.com/aws-cloudformation/cloudformation-cli)! A Python virtual environment is recommended.

**Prerequisites**

- Python version 3.6 or above
- [SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
- Your choice of TypeScript IDE

**Installation**


```shell
pip3 install cloudformation-cli-typescript-plugin
```

Refer to the [CloudFormation CLI User Guide](https://docs.aws.amazon.com/cloudformation-cli/latest/userguide/resource-types.html) for the [CloudFormation CLI](https://github.com/aws-cloudformation/cloudformation-cli) for usage instructions.

**Howto**

Example run:

```
$ cfn init
Initializing new project
What's the name of your resource type?
(Organization::Service::Resource)
>> Foo::Bar::Baz
Select a language for code generation:
[1] java
[2] typescript
(enter an integer):
>> 2
Use docker for platform-independent packaging (Y/n)?
This is highly recommended unless you are experienced
with cross-platform Typescript packaging.
>> y
Initialized a new project in <>
$ cfn submit --dry-run
$ sam local invoke --event sam-tests/create.json TestEntrypoint
```

## Development
-----------

### Recommended Setup:

Local development tooling is included in the root level [Pipfile](./Pipfile)

To use the correct tooling install a pipenv:

```shell
# Do once if you don't have pipenv
pip install --user pipenv

# Install dev and runtime tooling
pipenv install --dev
```

If you would like to keep environment as the activated environment:

```shell
pipenv shell
```

If you would like to just run one of the tools installed by pipenv:

```shell
pipenv run <command>
```

### Local Developing

For changes to the plugin, a Python virtual environment is recommended. Check out and install the plugin in editable mode:

```shell
python3 -m venv env
source env/bin/activate
pip3 install -e /path/to/cloudformation-cli-typescript-plugin
```

You may also want to check out the [CloudFormation CLI](https://github.com/aws-cloudformation/cloudformation-cli) if you wish to make edits to that. In this case, installing them in one operation works well:

```shell
pip3 install \
  -e /path/to/cloudformation-cli \
  -e /path/to/cloudformation-cli-typescript-plugin
```

That ensures neither is accidentally installed from PyPI.

For changes to the typescript library "@amazon-web-services-cloudformation/cloudformation-cli-typescript-lib" pack up the compiled javascript:

```shell
npm run build
npm pack
```

You can then install this in a cfn resource project using:

```shell
npm install ../path/to/cloudformation-cli-typescript-plugin/amazon-web-services-cloudformation-cloudformation-cli-typescript-lib-1.0.1.tgz
```

Linting and running unit tests is done via [pre-commit](https://pre-commit.com/), and so is performed automatically on commit after being installed (`pre-commit install`). The continuous integration also runs these checks. Manual options are available so you don't have to commit:

```shell
# run all hooks on all files, mirrors what the CI runs
pre-commit run --all-files
# run unit tests only. can also be used for other hooks, e.g. black, flake8, pylint-local
pre-commit run pytest-local
```

## Troubleshooting Tests

When we run plugin tests, the pytest suite will create temporary folders and then initialize a project via the current plugin there.
Additionally, it will make sure to build the typescript project in this repo and link it for testing.  This can lead to a few unexpected
surprises in tests if you are not careful.

### 1 Project is not buildable in typescript

Always make sure that you can build the library via `npm run build` before running tests.  If not, build errors before any codegen_test.py
tests actually run, are a good indicator that this is the case.

### 2 Plugin generation fails during codegen_test.py

Due to the nature of nested tooling when calling build methods (i.e. sam cli calls that call internal commands), you will want to run just
the one test, open a new terminal in the temporary directory, and then try to run the logged commands that failed within the folder.

If you are failing with non-descript SAM messages during the build phase.  It is most advisable to make sure to run the command that failed
during SAM build but just within the folder (unless you are failing a docker build, in which case you will need to determine the correct triage).

## License
-------

This library is licensed under the Apache 2.0 License.
