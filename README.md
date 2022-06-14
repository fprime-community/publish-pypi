# publish-pypi: Github action to publish a PyPI action

This will publish PyPI packages for you using the following steps:

1. `python setup.py <build steps>` with default steps `sdist bdist_wheel`
2. `twine check` the package
3. `twine upload` the package
4. `pip install` the package, with 3 retries spaced at 30 intervals

## Inputs

Required where no default available.

| input    | default             | description               |
|----------|---------------------|---------------------------|
| package  |                     | Name of PyPI package      |
| location | `$GITHUB_WORKSPACE` | Directory with `setup.py` |
| steps    | `sdist bdist_wheel` | Build steps for package   |
| repo     | `testpypi`          | Repo to publish to        |


## Environment

`TWINE_PASSWORD`: credential based password to supply to twine. Must supply from GH secret.

## Workflow Example

```
name: Build Package
on:
  release:
    types: [published]
jobs:
  Build-PyPI-Package:
    runs-on: ubuntu-latest
    steps:
    - name: Test PyPI
      uses: fprime-community/publish-pypi@main
      env:
        TWINE_PASSWORD: ${{ secrets.TESTPYPI_CREDENTIAL }}
      with:
        package: "fprime-fpp"
        steps: "sdist"
    - name: PyPI
      uses: fprime-community/publish-pypi@main
      env:
        TWINE_PASSWORD: ${{ secrets.PYPI_CREDENTIAL }}
      with:
        repo: "pypi"
        package: "fprime-fpp"
        steps: "sdist"
```

**Note:** this assumes that the secrets have been setup for the repo.
