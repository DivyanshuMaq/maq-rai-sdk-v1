# MAQ RAI SDK - Build and Publish Guide

This guide walks you through the process of building and publishing the MAQ RAI SDK to PyPI.

## Prerequisites

Before you begin, make sure you have:

1. **Python 3.8 or higher** installed
2. **pip** and **setuptools** updated to the latest versions
3. **PyPI account** (create one at https://pypi.org/account/register/)
4. **TestPyPI account** (create one at https://test.pypi.org/account/register/) for testing

## Step 1: Install Build Tools

First, install the required build tools:

```bash
pip install --upgrade pip setuptools wheel
pip install --upgrade build twine
```

## Step 2: Prepare Your Environment

Navigate to the SDK directory:

```bash
cd "c:\Users\DivyanshuRastogiMAQS\Downloads\MAQ_RAI_SDK\MAQ_RAI_SDK\MAQ_RAI_SDK"
```

Make sure all required files are present:
- `pyproject.toml` ✓
- `setup.py` ✓
- `README.md` ✓
- `LICENSE` ✓
- `MANIFEST.in` ✓
- `maq_rai_sdk/` directory ✓

## Step 3: Build the Package

Clean any previous builds:

```bash
# Remove old build artifacts
rmdir /s /q build dist *.egg-info 2>nul || echo "No previous builds found"
```

Build the distribution packages:

```bash
python -m build
```

This will create two files in the `dist/` directory:
- `maq_rai_sdk_v1-1.0.0-py3-none-any.whl` (wheel distribution)
- `maq_rai_sdk_v1-1.0.0.tar.gz` (source distribution)

## Step 4: Verify the Build

Check the contents of your built package:

```bash
# List the contents of the wheel file
python -m zipfile -l dist/maq_rai_sdk_v1-1.0.0-py3-none-any.whl

# Check the package metadata
python -m twine check dist/*
```

## Step 5: Test Upload (Recommended)

Before uploading to the main PyPI, test with TestPyPI:

```bash
python -m twine upload --repository testpypi dist/*
```

You'll be prompted for your TestPyPI credentials.

Test the installation from TestPyPI:

```bash
pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ maq-rai-sdk-v1
```

## Step 6: Upload to PyPI

Once you've verified everything works correctly, upload to the main PyPI:

```bash
python -m twine upload dist/*
```

You'll be prompted for your PyPI credentials.

## Step 7: Verify Installation

Test that your package can be installed from PyPI:

```bash
pip install maq-rai-sdk-v1
```

## Authentication Methods

### Option 1: Interactive Authentication
You'll be prompted for username and password when running twine upload.

### Option 2: API Tokens (Recommended)
1. Go to PyPI account settings
2. Generate an API token
3. Use the token as your password with username `__token__`

### Option 3: Configuration File
Create `~/.pypirc`:

```ini
[distutils]
index-servers = pypi testpypi

[pypi]
username = __token__
password = pypi-your-api-token-here

[testpypi]
repository = https://test.pypi.org/legacy/
username = __token__
password = pypi-your-test-api-token-here
```

## Common Issues and Solutions

### Issue: "File already exists"
**Solution**: You cannot overwrite files on PyPI. Increment the version number in `pyproject.toml` and rebuild.

### Issue: "Invalid distribution"
**Solution**: Run `python -m twine check dist/*` to identify issues. Common causes:
- Missing README.md
- Invalid metadata in pyproject.toml
- Malformed description

### Issue: Missing dependencies during installation
**Solution**: Update the `dependencies` section in `pyproject.toml` with all required packages.

## Version Management

When releasing new versions:

1. Update the version in `pyproject.toml`
2. Update the version in `maq_rai_sdk/_configuration.py`
3. Update the version in `maq_rai_sdk/__init__.py`
4. Update the changelog in `README.md`
5. Create a git tag: `git tag v1.0.1`
6. Rebuild and upload

## Automated Publishing (Optional)

For automated publishing using GitHub Actions, create `.github/workflows/publish.yml`:

```yaml
name: Publish to PyPI

on:
  release:
    types: [published]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.x'
    - name: Install build tools
      run: |
        python -m pip install --upgrade pip
        pip install build twine
    - name: Build package
      run: python -m build
    - name: Publish to PyPI
      env:
        TWINE_USERNAME: __token__
        TWINE_PASSWORD: ${{ secrets.PYPI_API_TOKEN }}
      run: twine upload dist/*
```

## Final Checklist

Before publishing:

- [ ] All tests pass
- [ ] Documentation is complete and accurate
- [ ] Version number is correct
- [ ] Dependencies are properly specified
- [ ] License file is included
- [ ] Package builds without errors
- [ ] Package passes twine check
- [ ] Tested on TestPyPI first
- [ ] README.md has clear installation and usage instructions

## Support

If you encounter issues during publishing:

1. Check the [Python Packaging User Guide](https://packaging.python.org/)
2. Consult the [PyPI Help](https://pypi.org/help/)
3. Review the build logs for specific error messages
4. Contact the development team

## Security Notes

- Never commit API tokens to version control
- Use environment variables or secure credential storage
- Regularly rotate your PyPI API tokens
- Consider using trusted publishers for automated deployments