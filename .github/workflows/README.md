# GitHub Actions Workflows

This directory contains GitHub Actions workflows for automated CI/CD, testing, and release management of Plan 9 4th Edition.

## Workflows

### 1. CI Workflow (`ci.yml`)

**Trigger:** Push to main/master/develop, Pull Requests, Manual dispatch

**Purpose:** Continuous Integration for every code change

**Jobs:**
- **Build and Test**: Compiles code for multiple architectures (386, amd64, arm, mips, power, power64, sparc)
- **Lint**: Performs static analysis and code quality checks
- **Integration**: Validates system structure and critical components
- **Summary**: Aggregates results from all jobs

**Matrix Strategy:** Tests across 7 architectures in parallel

### 2. Release Workflow (`release.yml`)

**Trigger:** Git tags (v*.*.*, release-*), Manual dispatch

**Purpose:** Automated release builds and distribution

**Jobs:**
- **Prepare**: Creates GitHub release with release notes
- **Build**: Creates architecture-specific distributions for:
  - 386 (Intel 32-bit)
  - amd64 (AMD/Intel 64-bit)
  - arm (ARM processors)
  - mips (MIPS processors)
  - power (PowerPC 32-bit processors)
  - power64 (PowerPC 64-bit processors)
  - sparc (SPARC processors)
- **Build Source**: Creates complete source distribution
- **Verify**: Validates release completion

**Artifacts:** Each build produces:
- `.tar.gz` archive
- `.sha256` checksum
- `.md5` checksum

### 3. Comprehensive Tests Workflow (`tests.yml`)

**Trigger:** Daily at 2 AM UTC, Pull Requests, Manual dispatch

**Purpose:** In-depth testing and validation

**Jobs:**
- **E2E Tests**: End-to-end system validation
  - Directory structure validation
  - Source code quality tests
  - Library tests
  - Command utilities tests
  - Architecture support tests
  - Documentation validation
- **Security Scan**: Security vulnerability checks
  - Unsafe function detection
  - Secret scanning
- **Performance Tests**: Performance analysis
  - Repository size analysis
  - File type distribution
  - Code complexity analysis
- **Compatibility Tests**: Multi-OS compatibility testing
  - Tests on Ubuntu 20.04, 22.04, and latest
- **Report**: Aggregated test results

### 4. Code Quality Workflow (`quality.yml`)

**Trigger:** Push to main/master/develop, Pull Requests, Weekly (Mondays at 6 AM UTC), Manual dispatch

**Purpose:** Code quality assurance and metrics

**Jobs:**
- **Code Analysis**: Static analysis and complexity metrics
  - Lines of code analysis (cloc)
  - C/C++ static analysis (cppcheck)
  - Shell script analysis (shellcheck)
  - File encoding checks
  - Header guard analysis
  - Code duplication detection
- **Documentation Check**: Documentation completeness
  - README/LICENSE validation
  - Documentation directory checks
  - TODO/FIXME tracking
  - Comment density analysis
- **Build System Check**: Build system validation
  - Mkfile analysis
  - Architecture support verification
  - Dependency analysis
- **Consistency Check**: Code consistency validation
  - File naming conventions
  - Line ending validation
  - File permissions checks
- **Quality Metrics**: Comprehensive quality report
  - Repository statistics
  - Directory structure
  - Architecture support summary

## Usage

### Running Workflows Manually

1. Navigate to the "Actions" tab in GitHub
2. Select the workflow you want to run
3. Click "Run workflow"
4. Choose the branch and provide any required inputs

### Creating a Release

To create a new release:

```bash
# Create and push a version tag
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0
```

The release workflow will automatically:
1. Create a GitHub release
2. Build distributions for all architectures
3. Upload distribution archives with checksums
4. Generate release notes

### Manual Release

Alternatively, trigger a manual release:

1. Go to Actions → Release workflow
2. Click "Run workflow"
3. Enter the version (e.g., v1.0.0)
4. Click "Run workflow"

## Workflow Status Badges

Add these badges to your README to show workflow status:

```markdown
![CI](https://github.com/e9-o9/plan9/workflows/CI/badge.svg)
![Tests](https://github.com/e9-o9/plan9/workflows/Comprehensive%20Tests/badge.svg)
![Quality](https://github.com/e9-o9/plan9/workflows/Code%20Quality/badge.svg)
```

## Customization

### Modifying Build Targets

To add or remove architecture targets, edit the `matrix` section in the workflow files:

```yaml
strategy:
  matrix:
    arch: [386, amd64, arm, mips, power, sparc]
```

### Changing Test Schedules

Modify the `cron` expression in the workflow `on` section:

```yaml
on:
  schedule:
    - cron: '0 2 * * *'  # Daily at 2 AM UTC
```

### Adding Custom Tests

Add new test steps in the appropriate workflow file under the relevant job.

## Dependencies

The workflows use the following GitHub Actions:

- `actions/checkout@v4` - Code checkout
- `actions/create-release@v1` - Release creation
- `actions/upload-release-asset@v1` - Asset uploads

External tools installed via apt:
- build-essential
- gcc/g++
- make
- git
- cloc (code analysis)
- cppcheck (static analysis)
- shellcheck (shell script analysis)
- valgrind (memory checking)

## Troubleshooting

### Build Failures

If builds fail:
1. Check the workflow run logs in the Actions tab
2. Verify that the mkfile syntax is correct
3. Ensure all required files are committed

### Test Failures

If tests fail:
1. Review the specific test step that failed
2. Check for missing files or directories
3. Verify architecture-specific files exist

### Release Issues

If releases fail:
1. Verify you have proper permissions
2. Check that the tag format is correct (v*.*.*)
3. Ensure GITHUB_TOKEN has appropriate permissions

## Best Practices

1. **Always run CI before merging**: Wait for CI checks to pass
2. **Review test results**: Check comprehensive test output regularly
3. **Monitor code quality**: Address issues flagged by quality checks
4. **Keep workflows updated**: Update action versions periodically
5. **Test locally first**: Validate changes locally before pushing

## Contributing

When modifying workflows:

1. Test changes in a fork first
2. Use workflow_dispatch for testing
3. Document any new steps or jobs
4. Update this README with changes

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Plan 9 Documentation](../README)
- [Workflow Syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
