---
name: repository-documentation
description: Create comprehensive onboarding documentation for repositories
---

# Repository Documentation Setup Skill

This skill helps create complete onboarding documentation for repositories, supporting both single projects and monorepos.

## Repository Types

**Single Repository**: One project per repository
**Monorepo**: Multiple related projects in one repository

## Documentation Structure

### Core Components

1. **README.md** - Main project documentation
2. **CODEOWNERS** - Review routing configuration  
3. **CHANGELOG.md** - Version history tracking
4. **Code Style Guide** - Development standards

### Monorepo Additional Components

- **Top-level README** - Monorepo overview and navigation
- **Individual READMEs** - Documentation for each sub-project
- **Shared Documentation** - Common standards and workflows

## Setup Process

### 1. Repository Analysis

**Information to Gather:**
- Repository purpose and scope
- Technology stack details
- Team structure and maintainers
- Build and deployment processes
- Environment requirements

**Detection Commands:**
```bash
# Identify repository type
find . -maxdepth 2 -name "package.json" | wc -l

# List sub-projects (monorepo)
find . -maxdepth 2 -name "package.json" -not -path "*/node_modules/*"

# Check for monorepo tools
ls -la lerna.json nx.json pnpm-workspace.yaml turbo.json 2>/dev/null
```

### 2. Documentation Templates

**Single Repository README Structure:**
```markdown
# Project Name

## Overview
[Project description and purpose]

## Tech Stack
[Languages, frameworks, key libraries]

## Setup
[Installation and configuration]

## Development
[Development workflow and guidelines]

## Deployment
[Build and deployment process]
```

**Monorepo README Structure:**
```markdown
# Monorepo Name

## Overview
[Monorepo purpose and structure]

## Projects
[List of sub-projects with descriptions]

## Common Setup
- Avoid premature abstraction - don't create interfaces until you have multiple implementations
- No unnecessary wrapper classes
- Use [language]'s built-in functions before writing custom utilities

**Why:** Simple code is easier to debug, test, and maintain. Complexity should be earned, not assumed.

---

## 3. Think Before Coding

Do not write code immediately. First brainstorm and validate the approach.

**Requirements:**
- Before implementing any feature, explain the approach in comments or pseudocode
- Consider at least 2 alternative approaches before picking one
- If unsure about language features or patterns, search for the idiomatic way to do it
- Ask "Is there a simpler way?" before finalizing
- For algorithms, sketch the logic flow before writing code

**Process:**
1. Understand what needs to be done
2. Think about how to do it
3. Research if unsure about best practices
4. Explain the chosen approach
5. Then write the code

**Why:** Planning prevents rewrites. A few minutes of thinking saves hours of refactoring.

---

## 4. Modular Code Organization

Keep related code together in modules. Only extract to common when truly shared.

**Requirements:**
- Each module should be self-contained
- Types, utils, and constants specific to a module stay in that module
- Only move to `common/` if used by 3+ modules
- No circular dependencies between modules

**Decision rule for `common/`:**
- Is this used by 3+ modules? → Move to common
- Is this generic (not domain-specific)? → Move to common
- Otherwise → Keep in the module

**Why:** Modules with clear boundaries are easier to understand, test, and modify independently.

---

## Summary Checklist

Before writing any code, verify:

- [ ] Did I search the official documentation for the library/API I'm using?
- [ ] Is this the simplest solution that works?
- [ ] Did I think through the approach before coding?
- [ ] Is this code in the right module?
- [ ] If moving to common/, is it used by 3+ modules?
```

---

## Step 14: Clean Up Unnecessary Documentation

**Remove these files if they exist:**
- `SECURITY.md` (consolidate into README if needed)
- `CONTRIBUTING.md` (consolidated into README)
- `docs/` directory (consolidate into README)
- `.github/ISSUE_TEMPLATE/` (not needed, AI generates these)
- `.github/PULL_REQUEST_TEMPLATE.md` (not needed, AI generates these)

**Keep these files:**
- `CHANGELOG.md` (separate file)
- `LICENSE` (separate file)
- `README.md` (main documentation)
- `.github/CODEOWNERS` (for review routing)

**Ask user before deleting:**
```bash
# List files to be deleted
echo "The following files will be removed:"
ls -la SECURITY.md CONTRIBUTING.md docs/ .github/ISSUE_TEMPLATE/ .github/PULL_REQUEST_TEMPLATE.md 2>/dev/null

# Confirm with user before proceeding
```

---

## Step 15: Update .gitignore

Ensure `.gitignore` includes:

```
# Secrets
secrets.properties
.env
.env.local
*.keystore
*.jks
local.properties

# IDE
.idea/
.vscode/
*.swp
*.swo

# Build outputs
[build-output-dirs]

# OS files
.DS_Store
Thumbs.db
```

---

## Step 16: Create Branch and Commit

**For MONOREPO:**

```bash
# Create documentation branch
git checkout -b docs/monorepo-onboarding-documentation

# Stage all changes (top-level and sub-project READMEs)
git add README.md */README.md CHANGELOG.md .github/CODEOWNERS .windsurf/rules/code-style-guide.md .gitignore

# Commit
git commit -m "docs: add comprehensive monorepo onboarding documentation

- Create top-level README with monorepo overview and project links
- Create individual READMEs for each sub-project
- Add CODEOWNERS for project-specific review routing
- Create CHANGELOG.md for version tracking
- Add code style guide in .windsurf/rules/
- Update .gitignore for secrets management"

# Push
git push --set-upstream origin docs/monorepo-onboarding-documentation
```

**For SINGLE REPO:**

```bash
# Create documentation branch
git checkout -b docs/onboarding-documentation

# Stage changes
git add README.md CHANGELOG.md .github/CODEOWNERS .windsurf/rules/code-style-guide.md .gitignore

# Commit
git commit -m "docs: add comprehensive onboarding documentation

- Create detailed README with setup, workflow, and code style guidelines
- Add CODEOWNERS for review routing
- Create CHANGELOG.md for version tracking
- Add code style guide in .windsurf/rules/
- Update .gitignore for secrets management"

# Push
git push --set-upstream origin docs/onboarding-documentation
```

---

## Step 17: Create Pull Request

**For MONOREPO:**

**PR Title:**
```
docs: Add comprehensive monorepo onboarding documentation
```

**PR Description:**
```
## Summary
This PR adds comprehensive onboarding documentation for new MapUp engineers working with the [Monorepo Name].

## Changes
- ✅ Created top-level README.md with:
  - Monorepo overview and structure
  - Links to all sub-project READMEs
  - Common development workflow
  - Shared code style rules
  - Quick start guide
  
- ✅ Created individual READMEs for each sub-project:
  - [Project 1] - Complete setup and development guide
  - [Project 2] - Complete setup and development guide
  - [Project N] - Complete setup and development guide
  
- ✅ Created CODEOWNERS for project-specific review routing
- ✅ Created CHANGELOG.md for monorepo-wide version tracking
- ✅ Added code style guide in .windsurf/rules/
- ✅ Updated .gitignore for secrets management
- ✅ Removed redundant documentation files

## Goal
Reduce onboarding time for new engineers by providing:
- Clear monorepo structure overview
- Individual project documentation
- Consistent documentation format across all projects
- Easy navigation between projects

## Review Notes
- Documentation follows MapUp's GitHub best practices
- All sensitive information uses placeholder values
- Instructions are action-oriented and concise
- Each sub-project has complete standalone documentation
```

**For SINGLE REPO:**

**PR Title:**
```
docs: Add comprehensive onboarding documentation
```

**PR Description:**
```
## Summary
This PR adds comprehensive onboarding documentation for new MapUp engineers joining the [Project Name] team.

## Changes
- ✅ Created detailed README.md with:
  - Project overview and purpose
  - Complete tech stack documentation
  - Environment setup instructions
  - Local development setup guide
  - Development workflow and PR process
  - Code style rules and best practices
  - Testing and deployment guidelines
  - Safety and security guidelines
  
- ✅ Created CODEOWNERS for automatic review routing
- ✅ Created CHANGELOG.md for version tracking
- ✅ Added code style guide in .windsurf/rules/
- ✅ Updated .gitignore for secrets management
- ✅ Removed redundant documentation files (SECURITY.md, CONTRIBUTING.md, etc.)

## Goal
Reduce onboarding time for new engineers by providing a single, comprehensive, scannable documentation source that answers:
- What is this project?
- How do I set it up?
- How do I contribute?
- What are the rules?
- Who do I ask for help?

## Review Notes
- Documentation follows MapUp's GitHub best practices
- All sensitive information uses placeholder values
- Instructions are action-oriented and concise
```

---

## Step 18: Final Verification

**For MONOREPO:**

**Verify the following:**

1. ✅ Top-level README.md provides clear monorepo overview
2. ✅ All sub-projects have individual READMEs
3. ✅ All READMEs link correctly to each other
4. ✅ All maintainer names and emails are correct
5. ✅ All GitHub usernames in CODEOWNERS are correct
6. ✅ CODEOWNERS routes reviews to correct project maintainers
7. ✅ All secrets are documented but not committed
8. ✅ Build/run instructions are accurate for each project
9. ✅ Tech stack information is complete for each project
10. ✅ Code style rules are clear and actionable
11. ✅ No redundant documentation files remain
12. ✅ .gitignore includes all sensitive files
13. ✅ CHANGELOG.md is initialized
14. ✅ Navigation between project READMEs is intuitive

**Test the setup instructions:**
- Clone the repo fresh
- Follow the top-level README to understand the monorepo
- Pick a sub-project and follow its README step-by-step
- Verify everything works

**For SINGLE REPO:**

**Verify the following:**

1. ✅ README.md is comprehensive but scannable
2. ✅ All maintainer names and emails are correct
3. ✅ All GitHub usernames in CODEOWNERS are correct
4. ✅ All secrets are documented but not committed
5. ✅ Build/run instructions are accurate
6. ✅ Tech stack information is complete
7. ✅ Code style rules are clear and actionable
8. ✅ No redundant documentation files remain
9. ✅ .gitignore includes all sensitive files
10. ✅ CHANGELOG.md is initialized

**Test the setup instructions:**
- Clone the repo fresh
- Follow the README step-by-step
- Verify everything works

---

## Completion

**For MONOREPO:**

Once the PR is merged, the monorepo will have:
- ✅ Top-level README with monorepo overview and navigation
- ✅ Individual READMEs for each sub-project
- ✅ Automatic project-specific review routing via CODEOWNERS
- ✅ Version tracking via CHANGELOG
- ✅ Code style enforcement via rules
- ✅ Clean, consistent documentation structure across all projects

**New engineers can now:**
- Understand the monorepo structure in 5 minutes
- Navigate to the project they need to work on
- Set up their project environment in 15 minutes
- Make their first contribution in 30 minutes

**For SINGLE REPO:**

Once the PR is merged, the repository will have:
- ✅ Single, comprehensive README for onboarding
- ✅ Automatic review routing via CODEOWNERS
- ✅ Version tracking via CHANGELOG
- ✅ Code style enforcement via rules
- ✅ Clean, minimal documentation structure

**New engineers can now:**
- Understand the project in 5 minutes
- Set up their environment in 15 minutes
- Make their first contribution in 30 minutes

---

## Notes

- This workflow is based on the NavGuru SDK documentation structure
- Adapt section content to match your specific project
- Keep documentation concise and action-oriented
- Update CHANGELOG.md with every release
- Review and update documentation quarterly
