# Release Python Action

This Github Action provides a consistent action for building and releasing
Python command line tools on the St. Jude Cloud project. It may also be used
generally, provided your package conforms the following set of prerequisites:

- [Poetry] is used for package management.
- [Conventional commits] are used to describe changes to the project.
- You'd like to use [python-semantic-release] to automate semantic releases
  to PyPI.
- You'd like to use [conventional-github-releaser] for release notes on Github
  (we've found this to be better than using [python-semantic-release] for this
  purpose).

## Setup

The most basic implementation using this Github action can be achieved by
creating a `release.yml` file within the `.github/workflows` directory with the
following contents:

```yaml
name: Release
on:
  push:
    branches:
      - master

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: stjudecloud/release-python-action@v1
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          PYPI_TOKEN: ${{ secrets.PYPI_TOKEN }}
```

You'll need to complete a few more steps to ensure things work correctly:

- [ ] Ensure the branch above matches your default branch. If your default 
      branch is `main`, be sure to change it! 
- [ ] Make sure you add a `PYPI_TOKEN` secret to your repository so that the
      package can be published successfully.
- [ ] Ensure you have a complete `pyproject.toml` file. You'll need to set the
      appropriate settings for `python-semantic-release`, which you can find
      documented
      [here](https://python-semantic-release.readthedocs.io/en/latest/#getting-started).
      We recommend using the following code snippet as a starting place.

```toml
[tool.semantic_release]
build_command = "poetry build"
commit_author = "St. Jude Cloud <support@stjude.cloud>"
commit_message = ""
commit_subject = "chore: bumping version to v{version}"
upload_to_release = "false"
version_source = "commit"
version_variable = "pyproject.toml:version"
```

## Support

Please file an issue on this repository if you encounter any issues.

[Poetry]: https://python-poetry.org/
[Conventional commits]: https://www.conventionalcommits.org/en/v1.0.0-beta.2/
[python-semantic-release]: https://python-semantic-release.readthedocs.io/en/latest/
[conventional-github-releaser]: https://www.npmjs.com/package/conventional-github-releaser