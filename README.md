# APIVerve Data Validation Action

> Validate phone numbers, VAT IDs, IBANs, and other data formats

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Data_Validation-blue?logo=github)](https://github.com/apiverve/action-data-validation)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**[Browse All APIs](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=data-validation)** | **[Get Free API Key](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=data-validation)** | **[Documentation](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=data-validation)**

---

## What does this action do?

This action provides access to APIVerve's Data Validation APIs directly in your GitHub workflows:

- Validate phone numbers
- Verify VAT identification numbers
- Validate IBAN bank account numbers
- Lookup BIN/IIN card information

### Available APIs

| API | Description |
|-----|-------------|
| `phonenumbervalidator` | Phone Number Validator checks a phone number and country code to determine validity, line type, and country of origin. It flags disposable or VoIP numbers and outputs standardized international, national, RFC3966, and E.164 formats. |
| `binlookup` | BIN Lookup checks the first six digits of a payment card to return card brand, type, issuing bank, and country of issuance. Pass a card BIN to inspect whether incoming cards are debit, credit, or prepaid. |
| `routinglookup` | Routing Number Lookup verifies US ABA routing numbers to confirm bank names, states, and checksum validity. It identifies the routing type and Federal Reserve district, while paid plans add full street addresses, branch details, and replacement numbers. |

---

## Quick Start

```yaml
- name: Data Validation
  uses: apiverve/action-data-validation@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: phonenumbervalidator
    params: '{"number": "+14155551234", "country": "us"}'
```

---

## Setup

### 1. Get Your API Key

Sign up for a free account at [dashboard.apiverve.com/signup](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=data-validation) and create an API key.

### 2. Add Secret to Repository

Go to your repository **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

- Name: `APIVERVE_KEY`
- Value: Your API key from the dashboard

### 3. Use in Workflow

```yaml
- name: Data Validation
  uses: apiverve/action-data-validation@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: phonenumbervalidator
    params: '{"your": "parameters"}'
```

---

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `api_key` | Your APIVerve API key (or set `APIVERVE_API_KEY` env var) | Yes* | - |
| `api` | API to use: `phonenumbervalidator`, `binlookup`, `routinglookup` | No | `phonenumbervalidator` |
| `params` | JSON parameters for the API | No | `{}` |
| `output_file` | Path to save binary output (images, PDFs) | No | - |
| `format` | Response format: `json`, `yaml`, or `xml` | No | `json` |
| `fail_on_error` | Fail workflow if API returns error | No | `true` |
*\*API key is required but can be provided via input OR `APIVERVE_API_KEY` / `APIVERVE_KEY` environment variable.*

## Outputs

| Output | Description |
|--------|-------------|
| `result` | Full API response as JSON |
| `data` | The `data` field from response as JSON |
| `status` | API status (`ok` or `error`) |
| `file` | Path to downloaded file (if `output_file` was used) |
---

## Examples

### Phone Validation

Validate a phone number

```yaml
- name: Phone Validation
  id: data-validation-0
  uses: apiverve/action-data-validation@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: phonenumbervalidator
    params: '{"number": "+14155551234", "country": "us"}'

- name: Use result
  run: echo "Result: ${{ steps.data-validation-0.outputs.data }}"
```


---

## Full Workflow Example

```yaml
name: Data Validation Workflow

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  data-validation:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Data Validation
        id: result
        uses: apiverve/action-data-validation@v1
        with:
          api_key: ${{ secrets.APIVERVE_KEY }}
          api: phonenumbervalidator
          params: '{"number": "+14155551234", "country": "us"}'

      - name: Show result
        run: |
          echo "Status: ${{ steps.result.outputs.status }}"
          echo "Data: ${{ steps.result.outputs.data }}"
```

---

## Related Actions

Looking for more APIVerve actions?

- [apiverve/action](https://github.com/apiverve/action) - Generic action for all 350+ APIs
- [apiverve/action-release-assets](https://github.com/apiverve/action-release-assets) - Generate QR codes, barcodes, and badges for your GitHub releases
- [apiverve/action-visual-testing](https://github.com/apiverve/action-visual-testing) - Capture screenshots and generate PDFs for visual regression testing and documentation
- [apiverve/action-dns-monitor](https://github.com/apiverve/action-dns-monitor) - Verify DNS configuration, check propagation, and validate DNSSEC after deployments

**[Browse all APIVerve Actions →](https://github.com/marketplace?query=apiverve)**

---

## Pricing

- **Free tier** - Get started with generous free limits
- **Pro plans** - Higher rate limits and priority support for production use

Check out [pricing details](https://apiverve.com/pricing?utm_source=github&utm_medium=action&utm_campaign=data-validation).

---

## Resources

- **API Documentation**: [docs.apiverve.com](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=data-validation)
- **API Marketplace**: [apiverve.com/marketplace](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=data-validation)
- **Issues & Support**: [GitHub Issues](https://github.com/apiverve/action-data-validation/issues)
- **Email**: support@apiverve.com

---

## License

MIT - see [LICENSE](LICENSE)

---

Built by [APIVerve](https://apiverve.com?utm_source=github&utm_medium=action&utm_campaign=data-validation) - 350+ APIs for developers
