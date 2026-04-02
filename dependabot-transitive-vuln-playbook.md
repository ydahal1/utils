# Handling Transitive Dependency Vulnerabilities (pnpm)

This document describes the atomic steps to analyze and resolve a transitive
dependency vulnerability reported by Dependabot when using `pnpm`.

---

## Step 1 — Identify the culprit package

Determine which package introduces the vulnerable dependency.

```bash
pnpm why <vulnerable-package>
```

✅ Result: identifies the full dependency chain and the _root package_
(e.g. `eslint`) you need to evaluate or upgrade.

---

## Step 2 — Confirm current vulnerable version

Verify which version is currently installed.

```bash
pnpm list <vulnerable-package>
```

Compare the version against the advisory:

- Vulnerable range
- Patched range

---

## Step 3 — Explore upgrade options within the same major version

List all versions in the current major to avoid breaking changes.

Example (if using `eslint@9.x`):

```bash
pnpm info eslint@9 version
```

Identify newer patch or minor releases (`9.x.y`) to evaluate.

---

## Step 4 — Inspect dependency tree **and upgrade if fixed**

For a candidate version:

```bash
pnpm info eslint@9.x.y dependencies
```

If needed, inspect intermediate dependencies:

```bash
pnpm info <intermediate-dep>@<version> dependencies
```

### ✅ If a non‑vulnerable version is found

Upgrade the package:

```bash
pnpm add -D eslint@9.x.y
pnpm audit
```

✅ Stop only after:

- `package.json` is updated
- lockfile changes
- the vulnerability is removed

> Note: Inspection alone does **not** resolve the Dependabot alert.

---

## Step 5 — Check newer major versions

If no fix exists in the current major:

```bash
pnpm info eslint@10 version
pnpm info eslint@10.x.y dependencies
```

### ✅ If the newer major uses a patched dependency

Upgrade to that major version.

```bash
pnpm add -D eslint@10.x.y
pnpm audit
```

---

## Step 6 — If no fixed version exists

If **no released version** resolves the vulnerability:

### Option A — Use a pnpm override (recommended)

```json
{
  "pnpm": {
    "overrides": {
      "<vulnerable-package>": ">=patched.version"
    }
  }
}
```

```bash
pnpm install
pnpm audit
```

✅ Effective for transitive and dev‑only dependencies  
✅ Prevents recurring Dependabot alerts

---

### Option B — Mark “fix in progress”

Use when overrides are not acceptable.

- Document that the issue is transitive / dev‑only
- Reference upstream issues or advisories
- Acknowledge the alert will reappear until fixed upstream

---

## Final Rule

- ✅ Upgrade when possible
- ✅ Override when necessary and safe
- ✅ Document when neither is possible
