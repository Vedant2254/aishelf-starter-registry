---
name: dependency-troubleshooting
description: Systematically diagnose and fix library dependency compatibility issues
---

# Dependency Troubleshooting Workflow

This workflow helps developers systematically diagnose and resolve library dependency compatibility issues after major framework upgrades, dependency updates, or architecture changes.

## When to Use This Workflow

- After framework upgrades (React, Next.js, Expo, etc.)
- When dependency updates cause build failures
- During major version bumps of key libraries
- When encountering "module not found" or "incompatible version" errors
- For peer dependency conflicts

## Workflow Steps

### 1. Identify the Breaking Library

**Actions:**
- Read error logs carefully to identify the problematic library
- Note the specific error message patterns:
  - "module not found" → Missing dependency
  - "incompatible version" → Version conflict
  - "deprecated API" → API changes
  - "peer dependency" → Peer version mismatch

**Commands:**
```bash
# Check error details in build logs
npm run build 2>&1 | grep -i error

# Identify which library is failing
npm list | grep [error-pattern]
```

### 2. Analyze Current Setup

**Actions:**
- Check currently installed versions
- Review version constraints in package files
- Identify dependency type (direct vs transitive)

**Commands:**
```bash
# Check current version
npm list [library-name]

# Check version constraints
cat package.json | grep -A 5 -B 5 "[library-name]"

# Check dependency tree
npm ls [library-name]
```

### 3. Research Compatible Versions

**Research Strategy:**
- Always search for "latest" documentation, not year-specific
- Prioritize official documentation over community solutions
- Check for official SDKs before using raw APIs

**Search Patterns:**
```
"[library-name] latest upgrade guide"
"[library-name] migration guide"
"[library-name] [old-version] to [new-version] breaking changes"
"[library-name] [framework-name] latest compatibility"
```

**Research Sources:**
- Official documentation (always latest version)
- GitHub CHANGELOG.md files
- GitHub issues related to the error
- Community discussions on compatibility

**SDK Preference:**
When integrating with external services, always:
1. Check for official SDKs (GitHub, Stripe, AWS, etc.)
2. Verify SDK is official/trusted (downloads, stars, maintainer)
3. Compare SDK benefits vs raw API calls
4. Prefer SDKs for better developer experience

### 4. Identify Root Cause

**Common Root Causes:**
- **API Deprecation**: Old methods removed or changed
- **Architecture Changes**: New architecture incompatibility
- **Version Conflicts**: Peer dependency mismatches
- **Breaking Changes**: Major version updates

**Documentation Sections to Review:**
- "Breaking Changes"
- "Migration Guide" 
- "Upgrading from X.x"
- "New Architecture Support"

### 5. Evaluate Solution Confidence

**Confidence Levels:**
- **High (80%+)**: Official docs provide clear migration steps
- **Medium (50-80%)**: Solution from GitHub issues or community
- **Low (<50%)**: No clear solution, requires experimentation

### 6. Implement Solution

**Solution Types:**
- **Version Upgrade**: Update to compatible version
- **Code Changes**: Update deprecated API calls
- **Workarounds**: Temporary fixes with documentation
- **Dependency Overrides**: Last resort for transitive issues

**Implementation Steps:**
1. Update package.json with new versions
2. Run `npm install` to apply changes
3. Update deprecated code patterns
4. Test affected functionality

### 7. Verify and Test

**Verification Steps:**
- Run build command to check error resolution
- Test affected functionality
- Check for new errors introduced
- Run test suite if available

### 8. Document Changes

**Documentation Requirements:**
- Add code comments explaining changes
- Note any temporary workarounds
- Update relevant documentation
- Record decision rationale

## Key Principles

- **Research Before Implementing**: Never guess at solutions
- **Official Documentation First**: Prefer official upgrade guides
- **Latest Documentation Only**: Always search for "latest" versions
- **SDK Over Raw APIs**: Use official SDKs when available
- **Transparent Confidence**: Be honest about solution certainty
- **One Issue at a Time**: Fix problems individually
- **Incremental Testing**: Test each change separately

## Common Troubleshooting Patterns

### Pattern 1: Deprecated API
- **Symptoms**: "X is not a function" or "X is deprecated"
- **Solution**: Find replacement API in upgrade guide

### Pattern 2: Architecture Incompatibility  
- **Symptoms**: Framework-specific architecture errors
- **Solution**: Check library architecture support

### Pattern 3: Peer Dependency Mismatch
- **Symptoms**: "peer dependency" warnings during install
- **Solution**: Align versions of related packages

### Pattern 4: Breaking Changes
- **Symptoms**: Props not working, TypeScript errors
- **Solution**: Check changelog for API changes

## Example: React Navigation Upgrade

**Problem**: "useLegacyImplementation prop not recognized"

**Resolution Process:**
1. **Identify**: `react-navigation/drawer` issue
2. **Check Version**: Currently on v6.x
3. **Research**: Search "react-navigation drawer v7 upgrade"
4. **Find Docs**: React Navigation v7 upgrade guide
5. **Root Cause**: Prop removed in v7, new default behavior
6. **Solution**: Remove prop or use new API
7. **Confidence**: 95% (official docs)
8. **Implement**: Update component code
9. **Verify**: Build succeeds

## Best Practices

- **Document Decisions**: Record why specific solutions were chosen
- **Test Thoroughly**: Verify fixes don't break other functionality
- **Plan Rollbacks**: Know how to revert if needed
- **Communicate Clearly**: Explain changes to team members
- **Monitor Dependencies**: Keep track of upcoming major releases