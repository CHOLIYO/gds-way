### Python

This manual is designed to aid developers in writing Python code that is clear and consistent, within, and across projects at GDS.

We follow [PEP8][], in some cases PEP8 doesn't express a view (e.g. on the usage of language
features such as metaclasses) in these cases we defer to the [Google Python style guide][GPSG].

We suggest using PEP8 and the Google Python style guide (in order of preference) as references unless
something is explicitly mentioned in the GDS Python Style Guide.

#### Additional rules

The [GDS Python Style Guide][GDSPSG] is the place for detailing GDS specific rules/ exemptions to [PEP8][] or the Google Python style guide. An example of which is allowing a line length of 120 instead of 79.

As always the rules in the GDS Python Style Guide should be followed only in conjunction with the advice
on consistency on the main [programming languages manual page][GDSPSG].

If you want to add a new rule or exception please create a pull request against this repo.

#### Linting

_For more on linting (including standard settings and config) see the [linting][linting] section of this manual._

[Flake8][] is the preferred linting tool. It incorporates:

* [PEP-0008][PEP8] inspired style checks using PyCodeStyle
* [Complexity][WikiCyclomatic_complexity] checking using the [McCabe project][McCabe]
* Lint checks using [PyFlakes][]

#### Dependencies

<blockquote>The tools mentioned in this topic are changing quickly, so this
section would benefit from review in July 2018</blockquote>

It is important to have a good strategy for specifying your project's Python
libraries. There are two desirable characteristics:

1. Reproducibility

    Pin your full dependencies or you'll get unpredictability between your dev
    environment and other environments. You want a new starter to avoid small
    hard-to-spot problems. And you want parity between what you test locally,
    what is tested by CI, and what you deploy, or you risk new issues appearing
    on a live server. Additionally these things can be hard to diagnose.

    Traditionally a `requirements.txt` lists the app's immediate dependencies,
    but that leaves sub-dependencies unpinned. It makes sense to use a tool to
    generate these, but we've noticed problems with using
    piptools/pip-compile and pipenv/Pipfile, as detailed in [this commit
    message][dm-deps-commit].

    **Recommendation**: Put your top-level requirements in a
    `requirements-app.txt` file and use [this Makefile
    script][dm-deps-commit-makefile] to generate a fully specified
    `requirements.txt`, which is used whenever you need to pip-install the
    dependencies. The script creates a temporary virtualenv, pip-installs the
    `requirements-app.txt` and pip-freezes the result as `requirements.txt`.

    **Recommendation**: Use `requirements-dev.txt` for any dependencies used in
    development/test but not production environments.


2. Kept up-to-date

    Security issues are found in libraries, so it is important to choose
    libraries that are maintained and to ensure your team has a strategy to
    ensure security updates are installed without significant delay.

    There are several web services which check a project's python dependencies
    daily. When new releases are available, the service automatically creates a
    Pull Request to update your requirements file, prompting the team's typical
    processes to review, merge and deploy. A couple of GDS teams have found that
    keeping up with every single update can be difficult - a daily nag that one
    is soon inclined to start ignoring. At least two of these web services (snyk
    and pyup) track which of the updates are for security reasons (as opposed to
    just bugfixes/features) so there is scope to try getting PRs only for these.
    However it relies on their database being correct about which updates are
    security related, and we've spotted at least one case where a security fix
    was missed.

    **Recommendation**: Trial a service, such as [requires.io][], [pyup.io][],
    [dependabot.com][] or [snyk.io][]. Ideally try two and compare them. Try
    different configuration. Report findings back to GDS's Python group. We're
    keen to track this quickly emerging area to maximize our security assurance
    of public services.

#### How to contribute

This manual, and by extension the [GDS Python Style Guide][GDSPSG], is not presumed to be infallible or beyond dispute.
If you think something is missing or if you'd like to see something changed then:

1. _(optional)_ Start with the [#python][slack-python] Slack channel. See what other developers think and you'll get an idea
of how likely your proposal is of being accepted as a pull request even before you put in any work.
2. Check out the making changes section of the [GDS Way repo][github-gds-way-readme-making-changes]
3. Create a pull request against [GDS Way repo][github-gds-way]



[github-gds-way]: https://github.com/alphagov/gds-way
[github-gds-way-readme-making-changes]: https://github.com/alphagov/gds-way/blob/master/README.md#making-changes
[slack-python]: https://gds.slack.com/messages/python
[linting]: #linting
[WikiCyclomatic_complexity]: https://en.wikipedia.org/wiki/Cyclomatic_complexity
[PyCharm]: https://www.jetbrains.com/pycharm/
[GPSG]: build-services.html#programming-language-style-guides
[PEP8]: https://www.python.org/dev/peps/pep-0008/
[PEP373]: https://www.python.org/dev/peps/pep-0373/
[Flake8]: http://flake8.pycqa.org/en/latest/
[PyFlakes]: https://github.com/pycqa/pyflakes
[McCabe]: https://pypi.python.org/pypi/mccabe
[dm-deps-commit]: https://github.com/alphagov/digitalmarketplace-api/commit/95ac12206d26e6b219dd381dd63641c33467afbd
[dm-deps-commit-makefile]: https://github.com/alphagov/digitalmarketplace-api/commit/95ac12206d26e6b219dd381dd63641c33467afbd#diff-b67911656ef5d18c4ae36cb6741b7965
[requires.io]: https://requires.io/
[pyup.io]: https://pyup.io/
[dependabot.com]: https://dependabot.com/
[snyk.io]: https://snyk.io/
[GDSPSG]: #python
