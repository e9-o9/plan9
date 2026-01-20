# CI/CD Documentation

This repository is now equipped with comprehensive GitHub Actions workflows for continuous integration, testing, and automated releases.

## Overview

Four automated workflows have been configured:

1. **CI Workflow** - Runs on every push and pull request
2. **Comprehensive Tests** - Runs daily and on pull requests
3. **Code Quality** - Runs on pushes, pull requests, and weekly
4. **Release Workflow** - Runs on version tags

## Quick Start

### Viewing Workflow Status

Navigate to the **Actions** tab in GitHub to see all workflow runs, their status, and detailed logs.

### Running a CI Build

CI runs automatically when you:
- Push to main, master, or develop branches
- Create a pull request
- Manually trigger via the Actions tab

### Creating a Release

To create a new release with automated builds:

```bash
# Tag the release
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0
```

This will automatically:
- Create a GitHub release
- Build distributions for all architectures (386, amd64, arm, mips, power, power64, sparc)
- Generate checksums (SHA256 and MD5)
- Upload all artifacts to the release

## Architecture Support

The workflows build and test for the following architectures:

- **386** - Intel 32-bit (i386)
- **amd64** - AMD/Intel 64-bit
- **arm** - ARM processors
- **mips** - MIPS processors
- **power** - PowerPC 32-bit processors
- **power64** - PowerPC 64-bit processors
- **sparc** - SPARC processors

## Workflow Details

### CI Workflow (ci.yml)

**Runs on:** Push, Pull Request, Manual

**Jobs:**
- Build and test across multiple architectures
- Lint and static analysis
- Integration testing
- Summary report

**Duration:** ~5-10 minutes

### Comprehensive Tests (tests.yml)

**Runs on:** Daily at 2 AM UTC, Pull Requests, Manual

**Jobs:**
- End-to-end system validation
- Security vulnerability scanning
- Performance analysis
- Cross-platform compatibility testing

**Duration:** ~10-15 minutes

### Code Quality (quality.yml)

**Runs on:** Push, Pull Request, Weekly (Mondays), Manual

**Jobs:**
- Static code analysis (cppcheck)
- Lines of code metrics (cloc)
- Shell script validation (shellcheck)
- Documentation completeness
- Build system validation
- Code consistency checks

**Duration:** ~5-8 minutes

### Release Workflow (release.yml)

**Runs on:** Git tags (v*.*.* or release-*), Manual

**Jobs:**
- Prepare release and generate notes
- Build architecture-specific distributions
- Build source distribution
- Upload all artifacts with checksums

**Duration:** ~15-20 minutes

## Testing Locally

While these workflows are designed for GitHub Actions, you can test elements locally:

### Basic Build Test

```bash
# Navigate to sys/src
cd sys/src

# Attempt to build (requires Plan 9 tools or 9base)
export objtype=386
mk libs
```

### Run Static Analysis

```bash
# Install tools
sudo apt-get install cppcheck shellcheck cloc

# Run analysis
cppcheck --enable=warning sys/src/libc
shellcheck **/*.sh
cloc .
```

## Workflow Configuration

All workflow files are located in `.github/workflows/`:

- `ci.yml` - CI workflow
- `tests.yml` - Comprehensive tests
- `quality.yml` - Code quality checks
- `release.yml` - Release automation
- `README.md` - Detailed workflow documentation

## Customization

To modify workflows:

1. Edit the appropriate `.yml` file in `.github/workflows/`
2. Test using `workflow_dispatch` (manual trigger)
3. Commit and push changes

Common customizations:
- **Change schedule:** Modify `cron` expressions
- **Add architectures:** Update `matrix.arch` arrays
- **Add tests:** Add new steps in job definitions
- **Change triggers:** Modify `on:` sections

## Status Badges

Add workflow status badges to your README:

```markdown
[![CI](https://github.com/e9-o9/plan9/workflows/CI/badge.svg)](https://github.com/e9-o9/plan9/actions/workflows/ci.yml)
[![Tests](https://github.com/e9-o9/plan9/workflows/Comprehensive%20Tests/badge.svg)](https://github.com/e9-o9/plan9/actions/workflows/tests.yml)
[![Quality](https://github.com/e9-o9/plan9/workflows/Code%20Quality/badge.svg)](https://github.com/e9-o9/plan9/actions/workflows/quality.yml)
```

## Troubleshooting

### Workflow Not Running

- Check that the workflow file is in `.github/workflows/`
- Verify YAML syntax is correct
- Ensure triggers match your actions (branch names, tag patterns)

### Build Failures

- Review workflow logs in the Actions tab
- Check for missing dependencies
- Verify file paths are correct

### Permission Issues

- Ensure repository has Actions enabled
- Check that GITHUB_TOKEN has necessary permissions
- For releases, verify write permissions to contents

## Best Practices

1. **Review CI results** before merging pull requests
2. **Monitor scheduled tests** for regression detection
3. **Keep workflows updated** with latest action versions
4. **Test in forks** before modifying production workflows
5. **Use semantic versioning** for releases (v1.0.0, v1.1.0, etc.)

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax Reference](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- [Detailed Workflow README](.github/workflows/README.md)

## Support

For issues with workflows:

1. Check the workflow run logs
2. Review the workflow documentation
3. Test with `workflow_dispatch`
4. Open an issue with logs and error details
