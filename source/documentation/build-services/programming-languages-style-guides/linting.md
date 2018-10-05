### Linting Python with Flake8

This manual advises the use of the [Flake8][] all in one lint, codestyle and complexity checker.

Flake8 is a command line utility that acts as a drop in replacement for the [pep8/ PyCodeStyle][pycodestyle] command line checker.
It includes pep8/ PyCodeStyle checks as well as adding [complexity checks][McCabe], and [linting][PyFlakes].
If you're already using the [pep8/ PyCodeStyle][pycodestyle] checker you're most of the way there. If not then, never fear. You'll find everything
you need on this page.

#### How to use Flake8

Implementation of Flake8 will depend on whether the repository you want to run the checks on is a module or an application,
and how your dependencies and automated test ci is set up.

You'll want to add the Flake8 module (available from [Python Package Index (PyPI)][]) to your 'dev' or 'test' requirements/ dependencies.
You'll then likely want to run it before you run your unit tests.

#### Example usage

For a quick example run the below code (assuming you have [`virtualenv`][virtualenv] installed)

```bash
mkdir flake8_test; echo 'import os' > flake8_test/flake8_test_file.py
python3 -m venv flake8_test/venv
source flake8_test/venv/bin/activate
pip install flake8
flake8 flake8_test/flake8_test_file.py
deactivate
rm -rf flake8_test
```

The last line of output should read:

`> flake8_test/flake8_test_file.py:1:1: F401 'os' imported but unused`

[PyFlakes][]  catches a linting error:
`F401 '<module>' imported but unused`

This is because our file imports the `os` module but never uses it.
Unused imports are considered a bad thing because they pollute the namespace, increasing the number of names a
developer needs to keep track of.

Unused imports are a fairly simple example but errors like `F811 redefinition of unused <name> from line <N>`
or `F841 local variable <name> is assigned to but never used` can indicate that there are real problems with the code, and that it may not be acting as expected.

For a full list of error codes see:

* PyFlakes - [error and violation codes][pyflakes-error-codes]
* PyCodeStyle - [current list of error and warning codes][pycodestyle-error-codes-list]
* Flake8 - [error and violation codes][flake8-error-codes-list]

#### Plugins
Flake8 has been designed to be extensible and as such has spawned numerous plugins. They're worth a look to see if
any would be particularly beneficial to your
code base.

Examples include checks for requiring copyright/licensing strings, requiring docstrings
or warnings for upcoming deprecations.

A list can be found [by performing a PyPI search.][flake8-plugins]

#### Flake8 Putty

A particularly useful plugin is [Flake8 Putty][flake8-putty]. It allows the user to specify rule exemptions on a per
directory, per file, or per regex match basis.
Commonly it's used for ignoring unused imports in module level `__init__.py` files or imports not being at the top of a
file in settings files or scripts.

A slight drawback is that it is not compatible with the newest [Flake8][] version. and as such requires
Flake8 to be pinned at a v2 release, like [digitalmarketplace-api][DMAPI-requirements-dev].

#### Common Configuration

Digital Marketplace is already running Flake8 on all of it's repos pinned at v2.6.2 with
[Flake8 Putty][flake8-putty]. You can find an example of their configuration in the root of any repo in the
[`.flake8` file][DMAPI-flake8-config].


Commonly a configuration file will live in the root of the package. By default Flake8 will look for a
`.flake8` file in each directory.

```
[flake8]
# Rule definitions: http://flake8.pycqa.org/en/latest/user/error-codes.html
# D203: 1 blank line required before class docstring
# W503: line break before binary operator
exclude = venv*,__pycache__,node_modules,bower_components,migrations
ignore = D203,W503
max-complexity = 9
max-line-length = 120
```

In the above file we exclude directories we want the checker to ignore completely, ignore specific rules we disagree with,
set the maximum line length and set the maximum complexity. We've also included comments detailing what the specific exclusions
are.

Note: you can also ignore rules on particular lines of code or files by adding a `# noqa` comment - see [flake8's noqa syntax][flake8-noqa].

#### Resources

* [Python Slack][slack-python] - if you need help
* [Digital Marketplace Config][DMAPI-flake8-config] - production config to base off
* [Digital Marketplace pinned requirements][DMAPI-requirements-dev] - digitalmarketplace-api
* [Flake8][Flake8] - documentation
* [PyFlakes][pyflakes-error-codes] - error and violation codes
* [PyCodeStyle][pycodestyle-error-codes-list] - error and warning codes
* [Flake8][flake8-error-codes-list] - error and violation codes
* [Flake8][flake8-plugins] - plugins list


[DMAPI-flake8-config]: https://github.com/alphagov/digitalmarketplace-api/blob/master/.flake8
[DMAPI-requirements-dev]: https://github.com/alphagov/digitalmarketplace-api/blob/master/requirements-dev.txt
[slack-python]: https://gds.slack.com/messages/python

[WikiCyclomatic_complexity]: https://en.wikipedia.org/wiki/Cyclomatic_complexity
[PEP8]: https://www.python.org/dev/peps/pep-0008/

[Python Package Index (PyPI)]: https://pypi.python.org/pypi
[Flake8]: http://flake8.pycqa.org/en/latest/
[PyFlakes]: https://github.com/pycqa/pyflakes
[McCabe]: https://pypi.python.org/pypi/mccabe
[pycodestyle]: http://pep8.readthedocs.io
[virtualenv]: https://virtualenv.pypa.io/en/stable/

[flake8-putty]: https://pypi.python.org/pypi/flake8-putty/0.4.0
[flake8-plugins]: https://pypi.python.org/pypi?%3Aaction=search&term=flake8-&submit=search

[pyflakes-error-codes]: http://flake8.pycqa.org/en/latest/user/error-codes.html
[pycodestyle-error-codes-list]: http://pep8.readthedocs.io/en/latest/intro.html#error-codes
[flake8-error-codes-list]: http://flake8.pycqa.org/en/latest/user/error-codes.html
[flake8-noqa]: http://flake8.pycqa.org/en/latest/user/violations.html#in-line-ignoring-errors
