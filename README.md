# trivy-vuln-test

Contains intentionally vulnerable repository containing direct and transitive Node.js dependencies for use in [Trivy](https://trivy.dev) scan demonstrations. Do not use in production.

## Install Trivy

macOS (Homebrew):

```sh
brew install trivy
```

Other platforms: see the [Trivy installation docs](https://trivy.dev/latest/getting-started/installation/).

Verify:

```sh
trivy --version
```

## Run a scan

```sh
trivy fs --dependency-tree ./
```

Vulnerabilities only, as a table:

```sh
trivy fs --scanners vuln --dependency-tree ./
```

The scan reads `package.json` and `package-lock.json`, so no `npm install` is needed. The output lists direct and transitive vulnerable packages, with the dependency tree showing which direct dependency pulls in each transitive one.

## Verify a fix

1. Update a vulnerable package (lodash) to the fixed version (4.17.21) shown in the Trivy output, for example:

   ```sh
   npm install lodash@4.17.21
   ```

   For transitive packages, upgrade the parent (for example `express`) or add an `overrides` entry in `package.json`.

2. Refresh the lockfile:

   ```sh
   npm install --package-lock-only
   ```

3. Re-scan and confirm the CVE no longer appears in the table and the dependency tree:

   ```sh
   trivy fs --dependency-tree ./
   ```
