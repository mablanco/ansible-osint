# mablanco.osint

Ansible role to install OSINT tools:

| Tool | What it does | Installed from |
|---|---|---|
| [theHarvester](https://github.com/laramies/theHarvester) | Gathers emails, names, subdomains, IPs and URLs from public sources | Its release tag on GitHub, with uv |
| [Amass](https://github.com/owasp-amass/amass) | Maps attack surfaces and discovers subdomains | The official release binary |
| [subfinder](https://github.com/projectdiscovery/subfinder) | Discovers subdomains passively | The official release binary |
| [recon-ng](https://github.com/lanmaster53/recon-ng) | Web reconnaissance framework with installable modules | A commit of its repository, with uv |
| [Maigret](https://github.com/soxoj/maigret) | Finds accounts by username across thousands of sites | PyPI, with uv |

The Python tools run on a Python version that [uv](https://github.com/astral-sh/uv) installs, not on the system's, so they work the same on every platform. Every tool's command is in `/usr/local/bin`, and the rest of the files are in `/opt/osint`.

## Supported platforms

Debian 12 and 13, Ubuntu 24.04 and 26.04, EL 9 and 10, and openSUSE Leap 16.0, on x86_64 and aarch64. All of them are tested in CI, on x86_64 only.

On openSUSE, the role needs the `community.general` collection (`ansible-galaxy collection install community.general`).

The hosts need Internet access to GitHub and PyPI.

## Role variables

Each of these installs a tool when `true`, which is the default:

- **osint_theharvester**
- **osint_amass**
- **osint_subfinder**
- **osint_recon_ng**
- **osint_maigret**

Set to `false` the ones you don't want. The role doesn't uninstall a tool when its variable changes to `false`.

Other variables:

- **osint_home**: directory for uv, its Python, the Python tools and the binaries. Defaults to `/opt/osint`.
- **osint_python_version**: Python version for the Python tools. Defaults to `3.14`.
- **osint_uv_version**, **osint_theharvester_version**, **osint_amass_version**, **osint_subfinder_version**, **osint_maigret_version**: release of each tool, without the leading `v`.
- **osint_recon_ng_version**: commit of recon-ng to install. Its last release, 5.1.2, doesn't run on Python 3.12 or later, so the default is a later commit of `master` with the fix.

The binaries' downloads are checked against the checksums published with each release.

## Example playbook

```yaml
- hosts: osint
  become: true
  roles:
    - role: mablanco.osint
      vars:
        osint_recon_ng: false
```

## Upgrading from older versions

This version is a rewrite, and doesn't install GasMasK or DataSploit, which are no longer maintained. The variables have new names (`the_harvester` is now `osint_theharvester`), and every tool is installed by default.

The role removes the `/usr/local/bin/theHarvester` link that older versions created. It doesn't remove the older versions' files, so remove them yourself if you don't need them:

```sh
rm -rf /opt/theHarvester /opt/GasMasK /opt/DataSploit
rm -f /usr/local/bin/gasmask
```

DataSploit's requirements and theHarvester's `requests` were installed with the system's pip.

## Development

The role is tested with [Molecule](https://ansible.readthedocs.io/projects/molecule/) in Docker containers of every supported platform, and linted with [ansible-lint](https://ansible.readthedocs.io/projects/lint/). CI runs both on every change and every week.

```sh
pip install -r requirements-dev.txt
ansible-lint
molecule test
```

## License

GPL-3.0-only
