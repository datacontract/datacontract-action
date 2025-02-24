# datacontract-action

GitHub Action to run data contract tests using the [Data Contract CLI](https://github.com/datacontract/datacontract-cli) Docker image.

The action supports testing data contracts in [Data Contract Specification](https://datacontract.com/) and [Open Data Contract Standard
](https://bitol-io.github.io/open-data-contract-standard/latest/) format.



TODO Add Screenshot


## Usage

Add this step to your Github action workflow:

```yaml
      - name: Data Contract Tests
        uses: datacontract/datacontract-action@main
        with:
          location: datacontract.yaml                 # local data contract file in workspace or remote URL
          server: all                                 # The name of server to test or all
          junit-test-report: TEST-datacontract.xml    # This test report can be used with a subsequent action to create a GitHub test summary.
```



## Example

This action can be used in combination with a [test reporter action](https://github.com/dorny/test-reporter) to create and publish a test summary.


```yaml
# .github/workflows/main.yml
on:
  push:
  workflow_dispatch:

permissions:
  contents: read
  actions: read
  checks: write
  
jobs:
  datacontract-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Data Contract Tests
        uses: datacontract/datacontract-action@main
        with:
          location: datacontract.yaml
          server: all
          junit-test-report: TEST-datacontract.xml

      - name: Data Contract Test Results
        uses: dorny/test-reporter@v1
        if: always()
        with:
          name: Data Contract Test Results
          path: ./TEST-datacontract.xml
          reporter: java-junit
          fail-on-error: 'false'
```


## Credentials

TODO Server credentials

## License

TODO


