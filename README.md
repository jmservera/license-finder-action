# License Finder Action

A GitHub Action for running [Pivotal License Finder](https://github.com/pivotal/LicenseFinder)

## Inputs

### `permitted-licenses`

A comma separated list of licenses that are permitted. For example:

```yaml
permitted-licenses: MIT,Apache-2.0
```

### `approved-dependencies`

A comma separated list of dependencies that are approved

```yaml
approved-dependencies: jquery,bootstrap
```

### `report-name`

**Required** The name of the report to be generated.

Default: license_finder_report.xml

### `base-path`

**Required** If it is not in the root workspace parth, the base path with the code to review.

Default: ${{ github.workspace }}

## Usage

```yaml
uses: jmservera/license-finder-action@v0.1.4-alpha
with:
  permitted-licenses: MIT,Apache-2.0
  approved-dependencies: jquery,bootstrap
```

This action becomes useful when combined with some other actions like the
[upload artifact action](https://github.com/actions/upload-artifact)
and the [Publish unit test result action](https://github.com/EnricoMi/publish-unit-test-result-action)

```yaml
- name: 'License Scan'
  uses: jmservera/license-finder-action@v0.1.4-alpha
  with:
    permitted-licenses: MIT,Apache-2.0
    approved-dependencies: jquery,bootstrap
- name: Publish Test Results
  uses: EnricoMi/publish-unit-test-result-action@v2.0.0
  if: always()
  with:
    junit_files: "license_finder_report.xml"
- name: 'Upload Dependency Review Report'
  if: always()
  uses: actions/upload-artifact@v2
  with:
    name: license-finder-report
    path: license_finder_report.xml
```
