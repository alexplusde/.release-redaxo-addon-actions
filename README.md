# Release Webhook

```
name: Release Webhook

on:
  release:
    types: [published]

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Send Release Webhook
        uses: alexplusde/.release-redaxo-addon-actions//release-webhook@v1
        with:
          webhook_url: ${{ secrets.WEBHOOK_URL }}
          package_file: package.yml
```

# Release Workflow

```yml
name: Release Workflow

on:
  release:
    types: [published]

jobs:
  pre-release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Set release datetime and repo URL
        uses: alexplusde/.release-redaxo-addon-actions//pre-release-updater@v1
        with:
          package_file: package.yml

  notify-webhook:
    runs-on: ubuntu-latest
    needs: pre-release
    steps:
      - uses: actions/checkout@v4
      - name: Notify webhook
        uses: my-org/release-metadata-actions/release-webhook@v1
        with:
          webhook_url: ${{ secrets.WEBHOOK_URL }}
```

### Secrets

<https://github.com/organizations/alexplusde/settings/secrets/actions>

`WEBHOOK_URL`
