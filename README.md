# MAQ RAI SDK

[![PyPI version](https://badge.fury.io/py/maq-rai-sdk-v1.svg)](https://badge.fury.io/py/maq-rai-sdk-v1)
[![Python](https://img.shields.io/pypi/pyversions/maq-rai-sdk-v1.svg)](https://pypi.org/project/maq-rai-sdk-v1/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A Python SDK for MAQ's Responsible AI (RAI) services, providing Azure functions for reviewing and updating prompts with responsible AI capabilities.

## Features

- **Reviewer Operations**: Review and analyze AI prompts for responsible AI compliance
- **Test Case Operations**: Manage and execute test cases for AI models
- **Azure Integration**: Built on Azure Core for seamless cloud integration
- **Async Support**: Full async/await support for non-blocking operations
- **Type Hints**: Complete type annotations for better development experience

## Installation

Install the MAQ RAI SDK using pip:

```bash
pip install maq-rai-sdk-v1
```

## Quick Start

### Basic Usage

```python
from azure.core.credentials import AzureKeyCredential
from maq_rai_sdk import MAQRAISDK

# Initialize the client with your Azure credentials
credential = AzureKeyCredential("your-api-key-here")
client = MAQRAISDK(credential=credential)

# Use reviewer operations
reviewer_result = client.reviewer.some_operation()

# Use testcase operations
testcase_result = client.testcase.some_operation()
```

### Async Usage

```python
import asyncio
from azure.core.credentials import AzureKeyCredential
from maq_rai_sdk.aio import MAQRAISDK

async def main():
    credential = AzureKeyCredential("your-api-key-here")
    async with MAQRAISDK(credential=credential) as client:
        # Async reviewer operations
        reviewer_result = await client.reviewer.some_operation()
        
        # Async testcase operations
        testcase_result = await client.testcase.some_operation()

# Run the async function
asyncio.run(main())
```

### Custom Endpoint

If you need to use a custom endpoint:

```python
from azure.core.credentials import AzureKeyCredential
from maq_rai_sdk import MAQRAISDK

credential = AzureKeyCredential("your-api-key-here")
client = MAQRAISDK(
    credential=credential,
    endpoint="https://your-custom-endpoint.azurewebsites.net/api"
)
```

## Authentication

The SDK uses Azure Key Credential for authentication. You'll need:

1. An API key from your MAQ RAI service
2. The endpoint URL (uses default if not specified)

```python
from azure.core.credentials import AzureKeyCredential

credential = AzureKeyCredential("your-api-key-here")
```

## API Reference

### Client Classes

- **`MAQRAISDK`**: Main synchronous client
- **`MAQRAISDK` (from `maq_rai_sdk.aio`)**: Asynchronous client

### Operations

- **`reviewer`**: Operations for reviewing AI prompts and content
- **`testcase`**: Operations for managing and executing test cases

### Configuration

The SDK supports various configuration options:

```python
client = MAQRAISDK(
    credential=credential,
    endpoint="https://custom-endpoint.com/api",  # Custom endpoint
    # Add other configuration options as needed
)
```

## Error Handling

The SDK uses Azure Core's error handling mechanisms:

```python
from azure.core.exceptions import HttpResponseError

try:
    result = client.reviewer.some_operation()
except HttpResponseError as e:
    print(f"HTTP error occurred: {e}")
except Exception as e:
    print(f"An error occurred: {e}")
```

## Development

### Prerequisites

- Python 3.8 or higher
- Azure Core library
- An active Azure subscription with MAQ RAI services

### Installing Development Dependencies

```bash
pip install maq-rai-sdk-v1[dev]
```

### Running Tests

```bash
pytest tests/
```

## Requirements

- Python >= 3.8
- azure-core >= 1.24.0
- azure-core-experimental >= 1.0.0b1
- typing-extensions >= 4.0.0

## Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Submit a pull request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For support and questions:

- **Issues**: [GitHub Issues](https://github.com/DivyanshuMaq/maq-rai-sdk-v1/issues)
- **Documentation**: [README](https://github.com/DivyanshuMaq/maq-rai-sdk-v1#readme)
- **Email**: customersuccess@maqsoftware.com

## Changelog

### Version 1.0.0

- Initial release
- Reviewer operations support
- Testcase operations support
- Async/await support
- Azure Core integration

## About MAQ Software

MAQ Software is a technology consulting firm focused on accelerating business value through cloud analytics and AI solutions. Learn more at [maqsoftware.com](https://maqsoftware.com).