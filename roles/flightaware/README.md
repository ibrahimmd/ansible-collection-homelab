# ibrahimmd.homelab.flightaware

Installs and configures FlightAware PiAware and dump1090-fa on Debian-based systems. The role sets up the FlightAware apt repository, installs the required packages, and configures PiAware to feed ADS-B data to FlightAware.

## Requirements

- Raspberry Pi OS Bookworm (Debian 12) or Trixie (Debian 13)
- SDR dongle (e.g. RTL-SDR) connected to the host
- FlightAware feeder ID — obtain from https://flightaware.com/adsb/piaware/claim or retrieve existing id from `https://www.flightaware.com/adsb/stats/user/<username>`
- `community.general` collection

## Dependencies

- `ibrahimmd.homelab.facts` — ensures `/etc/ansible/facts.d` exists

## Assumptions

- Role is only supported on Debian-based systems — unsupported OS families and versions will be skipped
- `flightaware_feeder_id` must be set and kept encrypted
- SDR hardware dongle must be connected for `dump1090-fa` to work
- Reboot may be required after install for `dump1090-fa` to start working

## Role Variables

### Required

| Variable | Description |
|---|---|
| `flightaware_feeder_id` | FlightAware feeder ID. Keep it encrypted |

### Optional

| Variable | Default | Description |
|---|---|---|
| `flightaware_repo_deb_url` | `https://www.flightaware.com/.../flightaware-apt-repository_1.3_all.deb` | URL to FlightAware apt repository deb package |
| `flightaware_config` | see below | PiAware configuration |
| `flightaware_config_file_path` | see below | Config file paths |
| `flightaware_enabled` | `true` | Enable flightaware services |
| `flightaware_force_install` | `false` | Force reinstall even if already installed. Pass via CLI: `-e "flightaware_force_install=true"` |


#### `flightaware_config`

```yaml
flightaware_config:
  feeder_id: "{{ flightaware_feeder_id }}"
  allow_mlat: "yes"
  mlat_results: "yes"
  receiver_type: "beast"
  allow_auto_updates: "yes"
  allow_manual_updates: "yes"
```

#### `flightaware_config_file_path`

```yaml
flightaware_config_file_path:
  piaware: "/etc/piaware.conf"
  dump1090: "/etc/default/dump1090-fa"
  ansible_fact: "/etc/ansible/facts.d/software.fact"
```

## Internal

The following variables are defined in `vars/` and are not intended to be overridden:

| Variable | Description |
|---|---|
| `flightaware_supported` | Set to `true` for supported OS families and versions |
| `flightaware_packages` | List of packages to install |
| `flightaware_services` | List of services to manage |

## License

MIT - see [LICENSE](../../LICENSE) for details.

## Author

ibrahim — [GitHub](https://github.com/ibrahimmd)
