# mablanco.osint

Ansible role to install several OSINT related tools on a Linux system. This role should work on any modern Linux and Unix platforms.

## Requirements
- git CLI client
- Python 2.6+ or 3.x
- PIP & Virtualenv (for both Python 2 and 3). You can optionally install them with this role (read below)

## Role Variables
If you need/want to install PIP and Virtualenv with this role, give the **install_pip** variable a *true* value.

The following variables control whether a tool is installed (*true*) or not (*false*).

- **the_harvester**: 'The Harvester' is a gathering information tool about an URL.
- **gasmask**: 'GasMasK' is an all in one Information gathering tool.
- **datasploit**: 'DataSploit' is an OSINT Framework to perform various recon techniques, aggregate all the raw data, and give data in multiple formats.

## Example Playbook

Example of how to use this role:

    - hosts: sec-nodes
      vars:
         install_pip: false
         the_harvester: true
         gasmask: true
         datasploit: true
      roles:
         - { role: mablanco.osint }

## TODO

- Add lots of tools!!!

## License

GPLv3
