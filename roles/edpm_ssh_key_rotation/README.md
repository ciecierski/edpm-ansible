# edpm_ssh_key_rotation

This Ansible role provides SSH authorized keys rotation functionality for EDPM (External Data Plane Management) nodes. It can backup existing authorized keys and add new key entries.


## Role Variables

### Main Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `edpm_ssh_key_rotation_backup_keys` | `true` | Backup existing authorized keys before rotation |
| `edpm_ssh_key_rotation_backup_dir` | `/var/lib/edpm-config/ssh-keys-backup` | Backup directory for old authorized keys |
| `edpm_ssh_key_rotation_user` | `{{ ansible_user \| default('root') }}` | User for authorized keys rotation |
| `edpm_ssh_key_rotation_force` | `false` | Force key rotation even if key exists |
| `edpm_ssh_key_rotation_cleanup_backups` | `false` | Cleanup old backup keys after successful rotation |
| `edpm_ssh_key_rotation_max_backups` | `3` | Maximum number of backup generations to keep |

### New Key Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `edpm_ssh_key_rotation_new_public_key` | `""` | New public key content to add to authorized_keys |

## Example Playbook

```yaml
---
- name: Rotate SSH authorized keys on EDPM nodes
  hosts: edpm_nodes
  become: true
  roles:
    - role: osp.edpm.edpm_ssh_key_rotation
      vars:
        edpm_ssh_key_rotation_backup_keys: true
        edpm_ssh_key_rotation_user: "{{ ansible_user }}"
        edpm_ssh_key_rotation_new_public_key: "{{ lookup('file', 'keys/id_ed25519.pub') }}"
```

## Usage Examples

### Basic Authorized Keys Rotation

```yaml
- name: Basic SSH authorized keys rotation
  hosts: all
  roles:
    - role: osp.edpm.edpm_ssh_key_rotation
      vars:
        edpm_ssh_key_rotation_user: "{{ ansible_user }}"
        edpm_ssh_key_rotation_new_public_key: "{{ lookup('file', 'keys/id_ed25519.pub') }}"
```

### Force Key Rotation

```yaml
- name: Force SSH authorized keys rotation
  hosts: all
  roles:
    - role: osp.edpm.edpm_ssh_key_rotation
      vars:
        edpm_ssh_key_rotation_force: true
        edpm_ssh_key_rotation_user: "{{ ansible_user }}"
        edpm_ssh_key_rotation_new_public_key: "{{ lookup('file', 'keys/id_ed25519.pub') }}"
```

### Rotation with Cleanup

```yaml
- name: SSH authorized keys rotation with cleanup
  hosts: all
  roles:
    - role: osp.edpm.edpm_ssh_key_rotation
      vars:
        edpm_ssh_key_rotation_cleanup_backups: true
        edpm_ssh_key_rotation_max_backups: 5
        edpm_ssh_key_rotation_user: "{{ ansible_user }}"
        edpm_ssh_key_rotation_new_public_key: "{{ lookup('file', 'keys/id_ed25519.pub') }}"
```


