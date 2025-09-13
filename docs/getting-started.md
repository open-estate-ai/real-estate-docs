# Getting Started

Welcome! This guide will help you set up and use Open Estate AI projects.

## Prerequisites

- Python 3.11+
- Node.js and npm (for AWS CDK)
- AWS CLI configured (`aws configure`)
- Terraform (if using infra-as-code)
- An AWS account with suitable permissions

## Quick Setup

1. Clone the repository:
   ```sh
   git clone https://github.com/open-estate-ai/real-estate-docs.git
   ```

2. Create and activate a Python virtual environment:
   ```sh
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. Install dependencies:
   ```sh
   pip install -r requirements.txt
   ```

4. For documentation:
   ```sh
   mkdocs serve
   ```

5. For infrastructure:
   - See the [Guides](guides/scrapers.md) and [Reference](reference/terraform.md) sections for details.

## Need Help?

- Join our [Slack](https://join.slack.com/t/open-estate-ai/shared_invite/zt-3dk65gu4h-SmBeySssL732C3ReHL_ejQ)
- Open an issue on [GitHub](https://github.com/open-estate-ai/real-estate-docs)

---

Feel free to suggest improvements or ask questions!