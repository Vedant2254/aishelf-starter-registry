---
name: dependabot-security-fixes
description: Systematically fix Dependabot vulnerabilities with smart dependency management
---

# Dependabot Security Fixes Workflow

This workflow provides a systematic approach to resolving Dependabot security alerts while maintaining code quality and avoiding technical debt.

## Overview

**Strategy:**
1. **Fetch Alerts**: Get all open Dependabot security alerts
2. **Analyze Dependencies**: Identify direct vs transitive vulnerabilities
3. **Smart Updates**: Prioritize direct dependency updates
4. **Minimal Overrides**: Use overrides only as last resort

## Prerequisites

**Required Tools:**
- GitHub CLI (`gh`) - installed and authenticated
- Node.js and npm - for package management
- Repository with Dependabot alerts enabled

**Setup Commands:**
```bash
# Install GitHub CLI
brew install gh  # macOS
# Or: sudo apt install gh  # Ubuntu

# Authenticate with GitHub
gh auth login

# Verify installation
gh --version
```

## Steps

### 1. Fetch Dependabot Alerts

Fetch all open Dependabot alerts for the current repository:

```bash
# Get current repository info
REPO=$(gh repo view --json nameWithOwner -q .nameWithOwner)

# Fetch open alerts and save to JSON file
gh api repos/$REPO/dependabot/alerts \
  --jq '.[] | select(.state == "open") | {
    number, 
    severity: .security_advisory.severity, 
    package: .dependency.package.name, 
    ecosystem: .dependency.package.ecosystem,
    vulnerable_range: .security_vulnerability.vulnerable_version_range,
    patched_version: .security_vulnerability.first_patched_version.identifier,
    manifest: .dependency.manifest_path,
    ghsa_id: .security_advisory.ghsa_id,
    summary: .security_advisory.summary
  }' > dependabot-alerts.json
```

### 2. Analyze Alert Data

Review the fetched alerts to understand the scope:

```bash
# Count total alerts
cat dependabot-alerts.json | jq length

# Group by severity
cat dependabot-alerts.json | jq -r '.severity' | sort | uniq -c

# List affected packages
cat dependabot-alerts.json | jq -r '.package' | sort | uniq
```

### 3. Identify Package Manifests

Determine which manifest files contain vulnerabilities:

```bash
# List all affected manifest files
cat dependabot-alerts.json | jq -r '.manifest' | sort | uniq

# Filter alerts for specific manifest
MANIFEST_PATH="path/to/package.json"
cat dependabot-alerts.json | jq --arg manifest "$MANIFEST_PATH" \
  'select(.manifest == $manifest)'
```

### 4. Analyze Dependency Types

For each vulnerable package, determine if it's a direct or transitive dependency:

```bash
# Check if package is a direct dependency
PACKAGE_NAME="vulnerable-package"
grep -q "\"$PACKAGE_NAME\":" package.json && echo "Direct" || echo "Transitive"

# Get full dependency tree
npm ls $PACKAGE_NAME
```

### 5. Update Direct Dependencies

**Strategy**: Always update direct dependencies first.

```bash
# Get latest safe version
LATEST_VERSION=$(npm view $PACKAGE_NAME version)

# Update package.json
# Change: "package": "^old-version"
# To: "package": "^$LATEST_VERSION"

# Install updated version
npm install
```

### 6. Handle Transitive Dependencies

**Critical Principle**: Prefer parent updates over overrides.

**Why this matters:**
- **Override breaks** → You debug THEIR code (parent package)
- **Parent update breaks** → You fix YOUR code

**Resolution Process:**

1. **Find Parent Dependency:**
```bash
npm ls $PACKAGE_NAME | grep -B1 "$PACKAGE_NAME@" | head -1
```

2. **Check Parent Update Options:**
```bash
PARENT_PACKAGE="parent-package"
PARENT_LATEST=$(npm view $PARENT_PACKAGE version)

# Check if latest parent fixes the vulnerability
npm view $PARENT_PACKAGE@$PARENT_LATEST dependencies.$PACKAGE_NAME
```

3. **Choose Resolution Path:**
- **Parent update available** → Update parent (PREFERRED)
- **No parent update** → Use override (LAST RESORT)

### 7. Implement Overrides (Last Resort)

Only use overrides when no parent update resolves the issue:

```json
{
  "overrides": {
    "vulnerable-package": "^patched-version"
  }
}
```

**Documentation Requirements:**
- Add comment explaining why override was necessary
- Note which parent packages were checked
- Record when to re-evaluate the override

### 8. Install and Verify

Apply changes and verify fixes:

```bash
# Install updated dependencies
npm install --legacy-peer-deps

# Verify package version
npm ls $PACKAGE_NAME

# Check vulnerability status
npm audit | grep $PACKAGE_NAME

# Run full audit
npm audit
```

### 9. Test Application

Ensure updates don't break functionality:

```bash
# Run test suite
npm test

# Run linting
npm run lint

# Start application locally
npm run dev  # or appropriate start command

# Run smoke tests
npm run test:smoke  # if available
```

### 10. Document Changes

Create a record of security fixes:

```bash
# Create fix summary
cat > SECURITY_FIXES.md << EOF
# Security Vulnerability Fixes

## Fixed Vulnerabilities
- [Package]: [CVE/GHSA] - [Brief description]
- [Package]: [CVE/GHSA] - [Brief description]

## Resolution Method
- Direct dependencies updated: [list]
- Overrides added: [list with rationale]

## Verification
- All tests pass
- Application starts successfully
- Security audit clean
EOF
```

### 11. Commit Changes

Commit security fixes with proper documentation:

```bash
git add package.json package-lock.json SECURITY_FIXES.md
git commit -m "fix: resolve Dependabot security vulnerabilities

- Updated direct dependencies: [packages]
- Added minimal overrides: [packages]
- Fixes security alerts: #[numbers]
- Verified: tests pass, application functional"
```

## Decision Tree

```
For each vulnerable package:
├─ Is it a direct dependency?
│  ├─ YES → Update to latest version in package.json
│  └─ NO → Is it transitive?
│     ├─ Find parent dependency
│     ├─ Is there a newer version of parent?
│     │  ├─ YES → Does new parent version fix the vulnerability?
│     │  │  ├─ YES → Update parent in package.json
│     │  │  └─ NO → Add override for vulnerable package
│     │  └─ NO → Add override for vulnerable package
```

## Best Practices

1. **ALWAYS Update Parents First**: Check if updating the direct dependency fixes transitive vulnerabilities
2. **Minimize Overrides**: Only use overrides when NO parent update is available
3. **Rationale for Overrides**: Breaking parent packages = YOUR CODE breaks (you can fix). Breaking via overrides = THEIR CODE breaks (you can't fix)
4. **Test Thoroughly**: Run full test suite after each batch of updates
5. **Document Everything**: Keep track of why each override was added and what parent versions were checked
6. **Review Regularly**: Periodically check if overrides can be removed when parent packages update
7. **Batch Similar Updates**: Group related dependency updates together
8. **Check Breaking Changes**: Review changelogs for major version updates, but prefer breaking YOUR code over THEIRS

## Common Patterns

### Pattern 1: Direct Dependency Update
```json
// Before
"axios": "^1.7.9"

// After (if latest is 1.8.0)
"axios": "^1.8.0"
```

### Pattern 2: Transitive Dependency via Parent Update
```json
// Before
"parent-package": "^2.0.0"  // depends on vulnerable-dep@1.0.0

// After
"parent-package": "^3.0.0"  // depends on vulnerable-dep@2.0.0 (fixed)
```

### Pattern 3: Override as Last Resort
```json
// Only when no parent update available
"overrides": {
  "vulnerable-package": "^2.0.0"  // fixed version
}
```

## Troubleshooting

### Issue: npm install fails with peer dependency conflicts
**Solution**: Use `npm install --legacy-peer-deps`

### Issue: Override conflicts with direct dependency
**Solution**: Remove the override and update the direct dependency instead

### Issue: Can't find which package depends on vulnerable package
**Solution**: Use `npm ls vulnerable-package` to see full dependency tree

### Issue: Latest version has breaking changes
**Solution**: Update to the minimum version that fixes the vulnerability, not necessarily the latest

## Verification Checklist

- [ ] All Dependabot alerts fetched and analyzed
- [ ] Direct dependencies updated to latest (or minimum fixed) versions
- [ ] Transitive dependencies resolved via parent updates where possible
- [ ] Overrides added only as last resort
- [ ] `npm install` completes successfully
- [ ] `npm audit` shows reduced vulnerability count
- [ ] Application starts without errors
- [ ] Tests pass
- [ ] Changes documented
- [ ] Committed to feature branch

## Example Execution

```bash
# 1. Fetch alerts
REPO=$(gh repo view --json nameWithOwner -q .nameWithOwner)
gh api repos/$REPO/dependabot/alerts --jq '.[] | select(.state == "open")' > alerts.json

# 2. For each alert, check if direct dependency
for pkg in $(cat alerts.json | jq -r '.dependency.package.name' | sort -u); do
  if grep -q "\"$pkg\":" package.json; then
    echo "$pkg is DIRECT - update in package.json"
    npm view $pkg version
  else
    echo "$pkg is TRANSITIVE - check parent"
    npm ls $pkg | head -10
    
    # Find parent package from npm ls output
    parent=$(npm ls $pkg 2>&1 | grep -B1 "$pkg@" | grep -v "$pkg@" | tail -1 | sed 's/.*─ //' | cut -d'@' -f1)
    
    if [ -n "$parent" ]; then
      echo "  Parent: $parent"
      echo "  Current: $(npm ls $parent --depth=0 2>&1 | grep $parent | cut -d'@' -f2)"
      echo "  Latest: $(npm view $parent version)"
      
      # Check if latest parent fixes the vulnerability
      latest_version=$(npm view $parent version)
      echo "  Checking if $parent@$latest_version fixes $pkg..."
      npm view $parent@$latest_version dependencies.$pkg
      
      echo "  → If empty or shows patched version, UPDATE PARENT"
      echo "  → If still vulnerable or no update available, use override"
    fi
  fi
done

# 3. Make updates to package.json (prioritize parent updates)

# 4. Install and verify
npm install --legacy-peer-deps
npm audit

# 5. Test
npm test
```

## Notes

- This workflow prioritizes maintainability over quick fixes
- Overrides should be temporary and reviewed regularly
- Always test after dependency updates
- Document the reasoning for each override
- Consider creating a `DEPENDENCY_DECISIONS.md` file to track override rationale
