# ibrahimmd.homelab.tailscale

Installs and configures Tailscale VPN on Debian-based systems.

## Requirements

- Raspberry Pi OS Bookworm (Debian 12) or Trixie (Debian 13)
- Tailscale auth key - obtain it from https://console.tailscale.com/admin/settings/keys
- `community.general` collection

## Dependencies

- `ibrahimmd.homelab.facts` — ensures `/etc/ansible/facts.d` exists

## Assumptions

- Role is only supported on Debian-based systems — unsupported OS families and versions will be skipped

## Role Variables

### Required

None

### Optional

| Variable | Default | Description |
|---|---|---|
| `tailscale_disable_upnp` | `true` | Disable UPNP |
| `tailscale_accept_routes` | `false` | Accept routes from tailscale peers |
| `tailscale_advertise_exit_node` | `true` | Configure as exit node |
| `tailscale_advertise_routes` | See below | Routes to advertise |
| `tailscale_force_install` | `false` | Force reinstall even if already installed. Pass via CLI: `-e "tailscale_force_install=true"` |


#### `tailscale_advertise_routes`

```yaml
tailscale_advertise_routes:
  - "192.168.10.0/24"
```

## Internal

The following variables are defined in `vars/` and are not intended to be overridden:

| Variable | Description |
|---|---|
| `tailscale_supported` | Set to `true` for supported OS families and versions |
| `tailscale_signing_key_url` | GPG signing key url |
| `tailscale_signing_key_path` | GPG singing key install path |
| `tailscale_repo_file` | Debian APT repo file for tailscale |
| `tailscale_package` | List of tailscale packages to install |
| `tailscale_extra_packages` | List of additional packages to install |

## Tags

| Tag | Description |
|---|---|
| `tailscale_preinstall` | Setup apt repository and signing key |
| `tailscale_install` | Install Tailscale VPN |
| `tailscale_postinstall` | Disables UPNP, copies config file, restarts tailscale, writes fact |

## Local Facts

The role writes facts to `/etc/ansible/facts.d/software.fact` under the `tailscale` section:

| Fact | Description |
|---|---|
| `ansible_local.software.tailscale.installed` | Set to `true` after successful install |

On subsequent runs, if `ansible_local.software.tailscale.installed` is set, the install tasks are skipped automatically.


## License

MIT - see [LICENSE](../../LICENSE) for details.

## Author

ibrahim — [GitHub](https://github.com/ibrahimmd)
