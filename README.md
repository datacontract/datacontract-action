# datacontract-action

GitHub Action to run data contract tests using the [Data Contract CLI](https://github.com/datacontract/datacontract-cli).

The action supports testing data contracts in [Data Contract Specification](https://datacontract.com/) and [Open Data Contract Standard
](https://bitol-io.github.io/open-data-contract-standard/latest/) format.

![datacontract-action in combination with test reporter action](https://github.com/user-attachments/assets/50e7614b-aedd-4e90-9249-6fbfba718dfd)

You can use this GitHub action to enforce data contracts whenever your data contract specification changes and for periodic checks. The action generates a test report in JUnit XML format that can be used in a subsequent step to create a GitHub test summary, as shown above.


## Usage

Add this step to your Github action workflow:

```yaml
      - name: Data Contract Tests
        uses: datacontract/datacontract-action@main
        with:
          location: datacontract.yaml                 # local data contract file in workspace or remote URL
          server: all                                 # The name of server to test or all
          junit-test-report: TEST-datacontract.xml    # This test report can be used with a subsequent action to create a GitHub test summary.
        env: # Define server credentials as environment variables. Use Github Secrets for secure configuration.
          DATACONTRACT_SNOWFLAKE_USERNAME: ${{ secrets.DATACONTRACT_SNOWFLAKE_USERNAME }}
          DATACONTRACT_SNOWFLAKE_PASSWORD: ${{ secrets.DATACONTRACT_SNOWFLAKE_PASSWORD }}
          DATACONTRACT_SNOWFLAKE_WAREHOUSE: ${{ secrets.DATACONTRACT_SNOWFLAKE_WAREHOUSE }}
          DATACONTRACT_SNOWFLAKE_ROLE: ${{ secrets.DATACONTRACT_SNOWFLAKE_ROLE }}
```



## Full Example

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
        env:
          DATACONTRACT_SNOWFLAKE_USERNAME: ${{ secrets.DATACONTRACT_SNOWFLAKE_USERNAME }}
          DATACONTRACT_SNOWFLAKE_PASSWORD: ${{ secrets.DATACONTRACT_SNOWFLAKE_PASSWORD }}
          DATACONTRACT_SNOWFLAKE_WAREHOUSE: ${{ secrets.DATACONTRACT_SNOWFLAKE_WAREHOUSE }}
          DATACONTRACT_SNOWFLAKE_ROLE: ${{ secrets.DATACONTRACT_SNOWFLAKE_ROLE }}

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

Server credentials (such as username and password) can be defined as environment variables.
See [documentation](https://cli.datacontract.com/#test) for supported environment variables depending on the server type.

Use [Github secrets](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions) to store sensitive information for your repository or environment.


## License

MIT


