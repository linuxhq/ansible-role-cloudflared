# ansible-role-cloudflared

[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-cloudflared-blue.svg?style=flat)](https://galaxy.ansible.com/linuxhq/cloudflared)
[![License](https://img.shields.io/badge/license-GPLv3-brightgreen.svg?style=flat)](https://github.com/linuxhq/ansible-role-cloudflared/blob/master/COPYING)

RHEL/CentOS - Cloudflare Tunnel daemon

## Requirements

This role requires that your tunnel has already been initialized.

Follow steps 1 through 3 in the documentation link below to create your tunnel.

https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-guide

Once you have your certificate and secrets, see the required role variables and example playbook below.

## Role Variables

Available variables are listed below, along with default values:

    cloudflared_dist: "{{ 'el' + ansible_distribution_major_version }}"
    cloudflared_gid: 13335
    cloudflared_group: cloudflared
    cloudflared_home: /home/cloudflared
    cloudflared_ingress:
      - service: http_status:404
    cloudflared_pkg: cloudflare-release-latest
    cloudflared_release: "{{ [cloudflared_repo, cloudflared_pkg + '.' + cloudflared_dist] | join('/') }}"
    cloudflared_repo: 'http://pkg.cloudflare.com'
    cloudflared_shell: /sbin/nologin
    cloudflared_uid: 13335
    cloudflared_user: cloudflared

Additional *required* variables not defined by default

    cloudflared_account_tag
    cloudflared_cert
    cloudflared_tunnel_id
    cloudflared_tunnel_name
    cloudflared_tunnel_secret

## Dependencies

None

## Example Playbook

    - hosts: servers
      roles:
        - role: linuxhq.cloudflared
          cloudflared_account_tag: 7f15527b20d04645e27dd16eb8e350c0
          cloudflared_cert: |
            -----BEGIN PRIVATE KEY-----
            {...}
            -----END PRIVATE KEY-----
            -----BEGIN CERTIFICATE-----
            {...}
            -----END CERTIFICATE-----
            -----BEGIN ARGO TUNNEL TOKEN-----
            {...}
            -----END ARGO TUNNEL TOKEN-----
          cloudflared_ingress:
            - hostname: cf.tunnel.net
              service: ssh://localhost:22
            - service: http_status:404
          cloudflared_tunnel_id: 7cb2da87-c89f-43e1-bda3-91036fb79fb4
          cloudflared_tunnel_name: cf-tunnel
          cloudflared_tunnel_secret: MafXiMK3XmEMfczxsfbrLKdzwTAzRE9ztWWMTgwNbxC=

## License

Copyright (C) 2022 Taylor Kimball <tkimball@linuxhq.org>

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <http://www.gnu.org/licenses/>.
