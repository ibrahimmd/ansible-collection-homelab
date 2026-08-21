# ibrahimmd.homelab.plex

Installs and configures [Plex Media Server](https://plex.tv) on Debian-based systems. The role sets up the Plex apt repository, installs the package, configures media directory permissions, and claims the server against your Plex account.

## Requirements

- Debian-based OS - tested on Raspberry Pi OS Bookworm (Debian 12) and Trixie (Debian 13)
- `ansible.posix` collection
- `community.general` collection

## Assumptions

- Role is only supported on Debian-based systems — unsupported OS families will be skipped
- Role writes local facts after install and will not reinstall on subsequent runs unless `plex_force_install` is set
- Plex claim token expires after 4 minutes — obtain it from https://account.plex.tv/claim only when prompted during the role run

## Dependencies

- `ibrahimmd.homelab.facts` — ensures `/etc/ansible/facts.d` exists

## Role Variables

| Variable | Default | Description |
|---|---|---|
| `plex_media_dir` | `/data/media` | Path to media directory |
| `plex_prompt_claim_token` | `true` | Prompt for Plex claim token during install |
| `plex_media_dir_ownership_set` | `false` | Set ownership of media directory |
| `plex_media_dir_owner` | `plex` | Owner of media directory |
| `plex_media_dir_group` | `plex` | Group of media directory |
| `plex_media_dir_acl_set` | `false` | Set ACL on media directory |
| `plex_media_dir_acl` | See below | ACL entries for media directory — only applied when `plex_media_dir_acl_set` is `true` |
| `plex_signing_key_url` | `https://downloads.plex.tv/plex-keys/PlexSign.v2.key` | URL to Plex signing key |
| `plex_force_install` | `false` | Force reinstall even if already installed. Pass via CLI: `-e "plex_force_install=true"` |

#### `plex_media_dir_acl`

```yaml
plex_media_dir_acl:
  - { etype: user,  entity: plex, permissions: rwx }
  - { etype: group, entity: plex, permissions: rwx }
  - { etype: user,  entity: plex, permissions: rwx, default: true }
  - { etype: group, entity: plex, permissions: rwx, default: true }
  - { etype: group, entity: "",   permissions: r-x, default: true }
  - { etype: other, entity: "",   permissions: r-x, default: true }
  - { etype: mask,  entity: "",   permissions: rwx, default: true }
```

### Internal

The following variables are defined in `vars/` and are not intended to be overridden:

| Variable | Description |
|---|---|
| `plex_supported` | Set to `true` for supported OS families |
| `plex_repo_file` | List of packages to install |
| `plex_signing_key_path` | Destination path for the Plex signing key |


## Tags

| Tag | Description |
|---|---|
| `plex_preinstall` | Setup apt repository and signing key |
| `plex_install` | Install Plex Media Server |
| `plex_postinstall` | Claim Plex server, set ownership and set ACL |
| `plex_postinstall_ownership` | Set media directory ownership |
| `plex_postinstall_acl` | Set media directory ACL |

## Local Facts

The role writes facts to `/etc/ansible/facts.d/software.fact` under the `plex` section:

| Fact | Description |
|---|---|
| `ansible_local.software.plex.installed` | Set to `true` after successful install |

On subsequent runs, if `ansible_local.software.plex.installed` is set, the install tasks are skipped automatically.


Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

MIT

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
