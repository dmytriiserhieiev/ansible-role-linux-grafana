![](https://img.shields.io/badge/Ansible-Grafana-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments. The author does not guarantee the accuracy, completeness, reliability, and availability of the role content. Under no circumstances will the author be held responsible or liable in any way for any claims, damages, losses, expenses, costs or liabilities whatsoever, including, without limitation, any direct or indirect damages for loss of profits, business interruption or loss of information.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。作者不对角色内容之准确性、完整性、可靠性、可用性做保证。在任何情况下，作者均不对任何索赔，损害，损失，费用，成本或负债承担任何责任，包括但不限于因利润损失，业务中断或信息丢失而造成的任何直接或间接损害。__
___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_grafana.png" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [Versions](#versions)
- [ Role variables](#Role-variables)
  * [Main Configuration](#Main-parameters)
  * [Other Configuration](#Other-parameters)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Hosts inventory file](#Hosts-inventory-file)
  * [Vars in role configuration](#vars-in-role-configuration)
  * [Combination of group vars and playbook](#combination-of-group-vars-and-playbook)
- [License](#license)
- [Author Information](#author-information)
- [Donations](#Donations)

## Overview
Grafana is multi-platform open-source analytics and interactive visualization web application. It provides charts, graphs, and alerts for the web when connected to supported data sources. It is expandable through a plug-in system. End users can create complex monitoring dashboards using interactive query builders.

## Requirements
### Operating systems
This Ansible role installs Grafana on the Linux operating system, including establishing a filesystem structure and server configuration with some common operational features, Will works on the following operating systems:

  * CentOS 7

### Versions

The following list of supported the Grafana releases:

* 6, 7, 8

## Role variables
### Main parameters
There are some variables in defaults/main.yml which can (Or needs to) be overridden:

##### General parameters
* `grafana_path`: Specify the Grafana data directory.
* `grafana_version`: Specify the Grafana version.
* `grafana_admin_user`: The name of the default Grafana admin user.
* `grafana_admin_password`: The password of the default Grafana admin.
* `grafana_https`: A boolean to determine whether or not to Encrypting HTTP client communications.
* `grafana_proxy`: A boolean to determine whether or not running behind a HaProxy.

##### Listen port
* `grafana_port`: Grafana instance listen port.

##### NGinx parameters
* `grafana_ngx_dept`: A boolean to determine whether or not to proxy web interface using NGinx.
* `grafana_ngx_block_agents`: Enables or disables block unsafe User Agents.
* `grafana_ngx_block_string`: Enables or disables block includes Exploits / File injections / Spam / SQL injections.
* `grafana_ngx_compress`: Enables or disables compression.
* `grafana_ngx_domain`: Defines domain name.
* `grafana_ngx_port_http`: NGinx HTTP listen port.
* `grafana_ngx_port_https`: NGinx HTTPs listen port.
* `grafana_ngx_ssl_protocols`: intermediate or modern, defines SSL protocol profile.

##### Server System Variables
* `grafana_arg.allow_anonymous`: A boolean to determine whether or not to allow without any login required by enabling anonymous access.
* `grafana_arg.allow_embedding`: A boolean to determine whether or not to allow Grafana HTTP responses includes the header X-Frame-Options.
* `grafana_arg.default_theme`: Default UI theme.
* `grafana_arg.reporting_enabled`: Sends usage counters to stats.grafana.org every 24 hours.
* `grafana_arg.check_for_updates`: Enable or disable all checks to https://grafana.net for new vesions.
* `grafana_arg.timeout`: How long the data proxy should wait before timing out.
* `grafana_incident_levels_map`: Defines the display mode of incident alarm.

##### Datasource
* `grafana_ds_arg`: DataSource settings.

##### Plugins
* `grafana_plugins`: Grafana plugins list for installs.

##### Dashboard #
* `grafana_dashboard_arg`: DashBoard settings.

##### Service Mesh
* `environments`: Define the service environment.
* `datacenter`: Define the DataCenter.
* `domain`: Define the Domain.
* `customer`: Define the customer name.
* `tags`: Define the service custom label.
* `exporter_is_install`: Whether to install prometheus exporter.
* `consul_public_register`: Whether register a exporter service with public consul client.
* `consul_public_exporter_token`: Public Consul client ACL token.
* `consul_public_http_prot`: The consul Hypertext Transfer Protocol.
* `consul_public_clients`: List of public consul clients.
* `consul_public_http_port`: The consul HTTP API port.

## Dependencies
- Ansible versions >= 2.8
- Python >= 2.7.5
- [NGinx](https://github.com/goldstrike77/ansible-role-linux-nginx.git)

## Example

### Hosts inventory file
See tests/inventory for an example.

    node01 ansible_host='192.168.1.10' grafana_version='8.3.10'

### Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: all
  roles:
     - role: ansible-role-linux-grafana
       grafana_version: '8.3.10'
```

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`.

```yaml
grafana_path: '/data'
grafana_version: '8.3.10'
grafana_admin_user: 'admin'
grafana_admin_password: 'changeme'
grafana_https: true
grafana_proxy: false
grafana_port: '3000'
grafana_ngx_dept: false
grafana_ngx_block_agents: false
grafana_ngx_block_string: false
grafana_ngx_compress: false
grafana_ngx_domain: 'visual.example.com'
grafana_ngx_port_http: '80'
grafana_ngx_port_https: '443'
grafana_ngx_ssl_protocols: 'modern'
grafana_ngx_backend:
  - '127.0.0.1'
grafana_ngx_site:
  - domain: 'visual.example.com'
    type: 'proxy'
    location: '/'
    syntax:
      - 'expires off'
      - 'proxy_set_header Host $http_host'
    backend_address: '127.0.0.1'
    backend_port: '3000'
    sticky: 'ip_hash'
    keepalive: '32'
grafana_arg:
  allow_anonymous: false
  allow_embedding: false
  default_theme: 'dark'
  reporting_enabled: false
  check_for_updates: false
  timeout: '60'
grafana_incident_levels_map:
  critical: 'critical'
  high: 'high'
  warning: 'warning'
  info: 'info'
grafana_ds_arg:
  - name: 'prometheus'
    settings:  
      - server: '127.0.0.1'
        port: '19090'
grafana_plugins:
  - 'grafana-piechart-panel'
grafana_dashboard_arg:
  - category: 'OperatingSystem'
    title:
      - 'Linux_Disk_Performance'
      - 'Linux_Disk_Space'
      - 'Linux_Network_Overview'
      - 'Linux_System_Overview'
      - 'Windows_System_Overview'
environments: 'prd'
datacenter: 'dc01'
domain: 'local'
customer: 'demo'
tags:
  subscription: 'default'
  owner: 'nobody'
  department: 'Infrastructure'
  organization: 'The Company'
  region: 'China'
exporter_is_install: false
consul_public_register: false
consul_public_exporter_token: '00000000-0000-0000-0000-000000000000'
consul_public_http_prot: 'https'
consul_public_http_port: '8500'
consul_public_clients:
  - '127.0.0.1'
```

## License
![](https://img.shields.io/badge/MIT-purple.svg?style=for-the-badge)

## Author Information
Please send your suggestions to make this role better.

## Donations
Please donate to the following monero address.

    46CHVMbb6wQV2PJYEbahb353SYGqXhcdFQVEWdCnHb6JaR5125h3kNQ6bcqLye5G7UF7qz6xL9qHLDSAY3baagfmLZABz75

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/xmr_address.png" align="left" /></p>
