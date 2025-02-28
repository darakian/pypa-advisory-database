# Python Packaging Advisory Database

This is community owned repository of advisories for packages published on
https://pypi.org.

Advisories live in the [vulns](vulns/) directory and use a YAML encoding of
a [simple format](https://ossf.github.io/osv-schema/).

## Contributing advisories

### Making a pull request
Existing entries can be edited by simply creating a pull request.

To introduce a new entry, create a pull request with a new file that has a name
matching `PYSEC-0000-<anything>.yaml`. This will be later picked up by
automation to allocate a proper ID once merged.

### Triage process
Much of the existing set of vulnerabilities are collected from the
[NVD CVE](https://nvd.nist.gov/vuln/data-feeds) feed.

We use [this tool](https://github.com/google/osv/tree/master/vulnfeeds), which
performs a lot of heuristics to match CVEs with exact Python packages and
versions (which is a difficult problem!) and a small amount of human triage to
generate the `.yaml` entries here.

## Using this data

### Tooling

This data is exposed by [`pip-audit`](https://github.com/pypa/pip-audit),
which provides a CLI for resolving Python dependencies in an environment
or project and identifying known vulnerabilities:

```bash
python -m pip install pip-audit
python -m pip-audit -r requirements.txt
```

You can also use [`pypa/gh-action-pip-audit`](https://github.com/pypa/gh-action-pip-audit)
on GitHub Actions:

```yaml
jobs:
  pip-audit:
    steps:
      - uses: pypa/gh-action-pip-audit@v1.0.8
        with:
          inputs: requirements.txt
```

### APIs

Vulnerabilities are integrated into the
[Open Source Vulnerabilities](https://osv.dev) project, which provides an API to
query for vulnerabilities like so:

```bash
$ curl -X POST -d \
          '{"version": "2.4.1", "package": {"name": "jinja2", "ecosystem": "PyPI"}}' \
          "https://api.osv.dev/v1/query"
```

This data has also been integrated into the
[PyPI JSON API](https://warehouse.pypa.io/api-reference/json.html#known-vulnerabilities).

## Code of Conduct
Everyone interacting with this project is expected to follow the
[PSF Code of Conduct](https://github.com/pypa/.github/blob/main/CODE_OF_CONDUCT.md).
