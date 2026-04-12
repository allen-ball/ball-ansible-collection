# GitHub Copilot Instructions for Ball Ansible Collection

## Project Overview

This is an Ansible collection repository containing miscellaneous utility roles for Ansible development and troubleshooting.

- **Collection Namespace**: `ball.ansible`
- **Version**: 1.0.1
- **Primary Language**: YAML (Ansible playbooks and roles)
- **Purpose**: Ansible development utilities and debugging tools
- **License**: MIT
- **Author**: Allen D. Ball <ball@hcf.dev>

## Prerequisites

- Ansible 2.9+
- Python 3.8+

## Repository Structure

```
ball-ansible-collection/
├── galaxy.yml              # Ansible Galaxy collection metadata
├── roles/                  # All Ansible roles
│   └── dump-variables/    # Variable debugging role
└── .github/               # GitHub-specific files
```

## Key Concepts

### Role Descriptions

1. **dump-variables** - Debug role that dumps all Ansible variables to console for troubleshooting

### Use Cases

- **Playbook Development**: Understanding variable scope and precedence
- **Troubleshooting**: Debugging variable issues in complex playbooks
- **Role Testing**: Verifying variables are set correctly
- **Documentation**: Discovering available variables in a playbook context

### Ansible Best Practices for This Project

- **Idempotency**: All roles should be idempotent (safe to run multiple times)
- **Variables**: Use role defaults in `defaults/main.yml`
- **Documentation**: Each role should have a comprehensive README.md
- **Simplicity**: Keep roles focused on specific tasks

## Coding Guidelines

### YAML Style

```yaml
# Use 2-space indentation
- name: Descriptive task name
  module_name:
    parameter: value
    another_parameter: "{{ variable }}"
  tags:
    - debug
    - development
```

### Variable Naming

- Use descriptive names: `debug_variable_filter` not `filter`
- Prefix role-specific vars: `dump_variables_format`
- Use snake_case for all variables

### Task Structure

- Always include `name:` for clarity
- Use appropriate modules
- Add `tags:` for flexibility
- Include `when:` conditions for conditional execution

### File Organization

```
role_name/
├── README.md              # Role documentation
├── defaults/
│   └── main.yml          # Default variables
└── tasks/
    └── main.yml          # Main task file
```

## Common Patterns

### Variable Debugging

The dump-variables role provides comprehensive variable inspection:

```yaml
- name: Debug playbook variables
  collections:
    - ball.ansible
  roles:
    - role: dump-variables
```

### Selective Variable Dumping

Support filtering variables by name or pattern (if implemented):

```yaml
- name: Dump specific variables
  include_role:
    name: ball.ansible.dump-variables
  vars:
    variable_pattern: "^ansible_.*"
```

## Testing and Validation

### Syntax Check
```bash
ansible-playbook --syntax-check playbook.yml
```

### Linting
```bash
ansible-lint roles/role-name/
```

### Testing a Role
Create a test playbook:
```yaml
# test-playbook.yml
---
- hosts: localhost
  connection: local
  gather_facts: yes
  collections:
    - ball.ansible
  roles:
    - role: dump-variables
```

Run it:
```bash
ansible-playbook test-playbook.yml
```

### Integration Testing
Test roles in actual playbooks with various variable scenarios to ensure idempotency and correct behavior.

## When Writing New Roles

1. Follow existing role structure patterns
2. Document all variables in README.md
3. Include examples in the README
4. Keep roles focused (single responsibility)
5. Consider use cases for development and troubleshooting

## When Modifying Existing Roles

1. Maintain backward compatibility when possible
2. Update role version in galaxy.yml
3. Test against existing playbooks
4. Update documentation if behavior changes
5. Preserve existing variable names and structure

## Security Considerations

- Never log sensitive variables (use `no_log: true` where appropriate)
- Warn users about sensitive data exposure in debug output
- Consider filtering sensitive variable names by default

## Documentation Standards

### Role README.md Structure

Each role MUST have a comprehensive README.md following this structure:

1. **Title**: `# ball.ansible.role-name Role`
2. **Brief Description**: One-line summary
3. **Description**: Detailed explanation of purpose and functionality
4. **Requirements**: Ansible version, dependencies
5. **Role Variables**: Tables for required and optional variables
6. **Example Playbook**: Complete working example
7. **Notes**: Important operational notes, security considerations
8. **Author**: Allen D. Ball <ball@hcf.dev>
9. **License**: MIT

### Variable Documentation Format

```markdown
### Required Variables

| Variable | Type | Description | Default |
|:---------|:-----|:------------|:--------|
| `var_name` | string | What it does | N/A |

### Optional Variables

| Variable | Type | Default | Description |
|:---------|:-----|:--------|:------------|
| `var_name` | string | `value` | What it does |
```

## License and Copyright

### Collection License

This collection is **open source software** licensed under the MIT License.

- **License File**: MIT License at repository root: `LICENSE`
- **Copyright**: © 2022-2025 Allen D. Ball
- **Contact**: Allen D. Ball <ball@hcf.dev>

### Permitted Actions

- Free to use, modify, and distribute
- Commercial use allowed
- Attribution required (preserve copyright notices)

## Version Management

- **Semantic Versioning**: MAJOR.MINOR.PATCH (e.g., 1.0.0)
- **galaxy.yml**: Single source of truth for version number
- **Badges**: Update version badge in README.md when bumping version

## Code Review Checklist

Before committing changes:

- [ ] README.md updated if behavior changed
- [ ] All variables documented
- [ ] Example playbook tested and working
- [ ] No credentials or secrets exposed
- [ ] Idempotency verified
- [ ] Version bumped in galaxy.yml if needed
- [ ] License headers present where appropriate
- [ ] Security warnings added for sensitive data handling

## Role Implementation Details

### dump-variables Role

**Purpose**: Dumps all Ansible variables to a YAML file for debugging and troubleshooting.

**How it works**:
1. Uses a Jinja2 template (`templates/template.yml`) to render variables using the `varnames` and `vars` lookups (Ansible 2.9+)
2. Creates temporary file on target host (`/tmp/{{ inventory_hostname }}.yml`)
3. Fetches the file back to `/tmp/` on the control node
4. Outputs three main sections:
   - `hostvars[inventory_hostname]` - Variables specific to the current host
   - `all variables` - All variables in scope, collected via varnames/vars lookups
   - `hostvars` - Variables for all hosts in the inventory

**Variable Collection Approach**:
- Uses the modern `lookup('varnames', ...)` and `lookup('vars', ...)` pattern to avoid deprecated `vars` magic variable
- Explicitly excludes deprecated magic variables (`vars`, `play_hosts`)
- Collects variables by namespace prefix (`ansible_`, `playbook_`, `inventory_`, etc.)
- Compatible with Ansible core 2.20+ deprecation warnings

**Usage**:
```yaml
- hosts: all
  collections:
    - ball.ansible
  roles:
    - dump-variables
```

**Output**: Creates `/tmp/{{ inventory_hostname }}.yml` on the Ansible controller containing all variable data in YAML format.

## Collection Metadata

**Namespace**: ball  
**Name**: ansible  
**Version**: 1.0.2  
**Author**: Allen D. Ball <ball@hcf.dev>  
**License**: MIT  
**Repository**: https://github.com/allen-ball/ball-ansible-collection.git

## Development Philosophy

This collection focuses on:
- **Developer Experience**: Tools that make Ansible development easier
- **Simplicity**: Straightforward, easy-to-use utilities
- **Open Source**: MIT licensed for maximum reusability
- **Quality**: Well-documented, tested, and maintained
