# Contributing to Amazon Connect Outbound Campaigns

Thank you for your interest in contributing! This document provides guidelines for contributing to this project.

## Code of Conduct

Please be respectful and professional in all interactions.

## How to Contribute

### Reporting Bugs

1. Check existing issues to avoid duplicates
2. Create a new issue with:
   - Clear description of the bug
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment details (Python version, AWS region, etc.)

### Suggesting Features

1. Check existing issues/discussions
2. Create a new issue with:
   - Clear description of the feature
   - Use case and benefits
   - Proposed implementation (if any)

### Pull Requests

1. Fork the repository
2. Create a feature branch from `main`
3. Make your changes
4. Add/update tests if applicable
5. Update documentation
6. Submit a pull request

### Development Setup

```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/connect-outbound-campaigns.git
cd connect-outbound-campaigns

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows

# Install dependencies
pip install boto3 pytest

# Run tests
pytest tests/
```

### Code Style

- Follow PEP 8 for Python code
- Use meaningful variable and function names
- Add docstrings to functions and classes
- Keep functions focused and small

### Commit Messages

Use clear, descriptive commit messages:

```
Add predictive dialer configuration support

- Add capacity parameter to telephony config
- Update documentation with dialer modes
- Add example for predictive campaign
```

## Questions?

Open an issue for any questions about contributing.
