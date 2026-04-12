# Ball Ansible Collection

[![Version](https://img.shields.io/badge/version-1.0.2-blue.svg)](galaxy.yml)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

A collection of miscellaneous [Ansible](https://www.ansible.com/) roles.

## Overview

**Collection Details:**
- **Namespace**: `ball`
- **Name**: `ansible`
- **Version**: 1.0.2
- **License**: MIT
- **Repository**: https://github.com/allen-ball/ball-ansible-collection.git
- **Author**: Allen D. Ball <ball@hcf.dev>
- **Description**: A collection of miscellaneous Ansible roles

## Installation

Add to your `requirements.yml`:

```yaml
collections:
  - name: https://github.com/allen-ball/ball-ansible-collection.git
    type: git
    version: trunk
```

Install the collection:

```bash
ansible-galaxy collection install -r requirements.yml
```

## Prerequisites

- Ansible 2.9+
- Python 3.8+

## Available Roles

| Role | Description | Documentation |
|:-----|:------------|:--------------|
| [dump-variables] | Debug role that dumps all Ansible variables to console for troubleshooting | [README](roles/dump-variables/README.md) |

[dump-variables]: roles/dump-variables

## Quick Start

```yaml
# playbook-debug.yml
---
- hosts: localhost
  connection: local
  gather_facts: yes
  collections:
    - ball.ansible
  roles:
    - role: dump-variables
```

## Key Features

- **Variable Debugging**: Comprehensive variable dumping for playbook troubleshooting
- **Development Helper**: Essential tool for Ansible role development and testing

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## Support

For issues and questions:
- Open an issue on [GitHub](https://github.com/allen-ball/ball-ansible-collection/issues)
- Contact: ball@hcf.dev

## License

This collection is licensed under the MIT License. See [LICENSE](LICENSE) file for details.

---

**Copyright © 2022-2025 Allen D. Ball. All rights reserved.**
