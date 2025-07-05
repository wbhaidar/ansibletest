# Data Center Network Migration Playbooks

## Overview

This repository contains a collection of Ansible playbooks designed to **simulate data center (DC) network migrations**. These playbooks were developed and tested on a **virtual network topology** environment for **experimentation, learning, and validation** purposes.

The playbooks automate common migration tasks including:

- VLAN information gathering and validation
- Pre-migration data collection from network devices (switches, firewalls)
- Generation of pre-migration configuration files
- Applying pre-migration configurations to network devices
- VLAN migration sequencing (e.g., VRRP priority adjustments, static route updates, MAC forwarding)
  
This set is intended as a **reference framework** or starting point for teams working on DC network migration automation, lab testing, or network operations training.

---

## Playbook Summary

| Playbook                                   | Description                                                  | Target Devices            |
|--------------------------------------------|--------------------------------------------------------------|---------------------------|
| `collect_vlan_info.yml`                     | Collect VLAN details and VRRP info from core switches         | Core Switches              |
| `collect_network_info.yml`                   | Gather pre-migration config and interface data from core and edge switches | Core & Edge Switches        |
| `collect_firewall_info.yml`                   | Collect pre-migration configs and health info from firewalls  | Firewalls                  |
| `generate_switch_prep_config.yml`            | Generate pre-migration config files for core and edge switches | Core & Edge Switches        |
| `generate_firewall_prep_config.yml`          | Generate pre-migration system and user context config files for firewalls | Firewalls                  |
| `apply_switch_prep_config.yml`                 | Apply generated pre-migration configs to core and edge switches | Core & Edge Switches        |
| `apply_firewall_prep_config.yml`               | Apply generated pre-migration configs to firewalls            | Firewalls                  |
| `vlan_migration_sequence.yml`                   | Automate VLAN migration sequence: VRRP, static routes, interfaces, MAC forwarding | Gateways & Leafs           |
| `vlan_active_change_loop.yml`                    | Loop through VLANs and invoke migration subtasks              | Network Devices            |

---

## Key Features

- **Modular design:** Tasks are split across multiple playbooks and include task files for reuse.
- **Generic templating:** Device- and site-specific details are abstracted via variables and Jinja2 templates, making it adaptable.
- **Debug support:** Conditional debug output controlled by `PRINT_DEBUG` variable.
- **Safe defaults:** Error comments inserted for unknown sites or misconfigurations to avoid pushing invalid config.
- **Virtual topology tested:** Developed and validated on virtual network labs to support experimentation without impacting production.

---

## Requirements

- Ansible 2.9 or later
- Network modules for NX-OS (`nxos_*`) and ASA firewalls (`asa_*`)
- A virtual lab environment simulating Cisco NX-OS and ASA devices
- Jinja2 templates and variable files configured per your environment

---

## Usage

1. Customize the `vars.yml` and inventory files with your site-specific variables and device groups.
2. Review and adjust the Jinja2 templates in the `templates/` directory to match your network device configurations.
3. Run playbooks in logical order, for example:
   - `ansible-playbook collect_vlan_info.yml`
   - `ansible-playbook collect_network_info.yml`
   - `ansible-playbook generate_switch_prep_config.yml`
   - `ansible-playbook generate_firewall_prep_config.yml`
   - `ansible-playbook apply_switch_prep_config.yml`
   - `ansible-playbook apply_firewall_prep_config.yml`
   - `ansible-playbook vlan_migration_sequence.yml`
4. Use `PRINT_DEBUG=true` to enable verbose debug output during runs.

---

## Disclaimer

This playbook collection is intended for **learning, testing, and simulation only**. It is **not production-ready** and should be carefully reviewed and adapted before deployment in any live environment.
