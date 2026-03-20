# Developer Tools Role

Installs and maintains developer CLI tools for homelab environments.

## Description

This role installs and keeps updated:
- **Claude Code CLI** - Anthropic's Claude AI assistant CLI
- **OpenCode CLI** - AI-powered code assistant

Both tools are installed system-wide and automatically updated on each playbook run.

## Requirements

- RHEL/Fedora-based system
- Internet connectivity for downloading installers
- curl installed (usually part of common role)

## Role Variables

Available variables in `defaults/main.yml`:

```yaml
# Enable/disable Claude Code installation
install_claude_code: true
claude_code_install_url: "https://claude.ai/install.sh"

# Enable/disable OpenCode installation
install_opencode: true
opencode_install_url: "https://opencode.ai/install"

# Force update on each run (ensures latest version)
force_update: true
```

## Dependencies

None.

## Example Playbook

### Basic Usage

```yaml
- hosts: hypervisors
  become: true
  roles:
    - cnovak.homelab_config.developer_tools
```

### Custom Configuration

```yaml
- hosts: hypervisors
  become: true
  roles:
    - role: cnovak.homelab_config.developer_tools
      vars:
        install_claude_code: true
        install_opencode: false  # Only install Claude Code
```

### With Other Roles

```yaml
- hosts: hypervisors
  become: true
  roles:
    - cnovak.homelab_config.common
    - cnovak.homelab_config.configure_users
    - cnovak.homelab_config.developer_tools
```

## Installation Locations

- **Claude Code**: System-wide (typically `/usr/local/bin/claude` or `~/.local/bin/claude`)
- **OpenCode**: System-wide (typically `/usr/bin/opencode` or npm global path)

Both installers handle binary placement automatically.

## Update Behavior

With `force_update: true` (default):
- Installation scripts run on every playbook execution
- Scripts detect existing installations and update if newer versions are available
- Provides `state: latest` behavior without manual version management

Set `force_update: false` to disable automatic updates.

## Verification

After running the role, verify installation:

```bash
# Check binaries exist
which claude
which opencode

# Check versions
claude --version
opencode --version
```

## Testing

Run molecule tests:

```bash
cd roles/developer_tools
molecule test
```

## License

MIT

## Author

Christopher Novak
