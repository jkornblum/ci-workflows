# python-ci-workflow
GitHub Workflows for CI Pipelines.

The workflows are following this diagram

![git-ci drawio](https://user-images.githubusercontent.com/5368903/194207600-0c7acf7a-5131-46d1-8a52-20586b654345.png)


## Usage Python
This repo was last tested with these development requirements
```
black==22.8.0
bandit==1.7.4
pylint==2.15.3
pytest==7.1.3
pytest-cov==4.0.0
radon==5.1.0
```
The workflows require one has `requirements.txt` and `requirements.dev.txt`
in their project root directory.

In your repositories `.github/workflows` folder add a YAML like

`ci.yaml`
```yaml
name: Python CI

on: push

permissions:
  contents: read

jobs:
  code-quality:
    uses: jkornblum/ci-workflows/.github/workflows/python-code-quality.yaml@main
    with:
      pythonVersion: "3.8"
      pylintPath: "*.py myra/ shipment/ users/"
      pylintContinueOnError: true
      radonPath: "*.py myra/ shipment/ users/"
      blackPath: "*.py myra/ shipment/ users/"
      blackContinueOnError: true

  security:
    uses: jkornblum/ci-workflows/.github/workflows/python-security.yaml@main
    with:
      pythonVersion: "3.8"
      banditPath: "*.py myra/ shipment/ users/"
      banditContinueOnError: true

  test:
    uses: jkornblum/ci-workflows/.github/workflows/python-testing.yaml@main
    with:
      pythonVersion: "3.8"
      pytestPath: tests
      coveragePath: "."
      runIntegrationTests: true
```