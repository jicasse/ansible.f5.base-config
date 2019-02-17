F5 BIGIP system configuration hardening
=========

The objective of this role is to proceed to a base system configuration.
The idea here is to be able to apply a standard, an hardened configuration for all managed BIGIP devices.

Requirements
------------

Here are the requirements to use the role : 
- ansible >= 2.7 
- f5-sdk python library installed; please refer to the following [link](https://f5-sdk.readthedocs.io/en/latest/) to have further information regarding this sdk
- F5 BIGIP version >= 12

Additionally you'll need to **define all required variables** to have all working as expected.

Role Variables
--------------

A lot of variables are required to use the role.
Available variables are listed below, along with default values (see `defaults/main.yml`)

> f5_validate_certs: 'no'

Define if you want to validate the certificate used on administration interface is trusted or not.

> f5_delegate_to: 'localhost'

Ansible delegate option to delegate API requests to localhost, so the ansible server itself.
This option does not have to be changed.

### BIGIP Access settings

> f5_user: 'ansible'

This variable is the username ansible will use to access the appliance. 

> f5_passwd: 'password'

This variable is the password of the user.
It's recommended to use ansible-vault to encrypt the password. You can find some documentation regarding ansible-vault on the following link: [ansible-vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html)
You can use another vault like CyberArk to 

> f5_host: 1.1.1.1

This variable is the management IP address or hostname ansible will use to access the BIGIP

### Hostname configuration

> f5_set_hostname: False

This variable is set to define if the hostname of the appliance needs to be changed.
Set the variable to true if you want to set the hostname. 

> f5_hostname: 'myhostname'

If you needs to set the hostname, specify the hostname for the host target by using this variable.
This variable should be in host_vars files.

### NTP configuration

> f5_set_time: False

This variable is set to define if ntp configuration needs to be done.
Change the variable to true if you need to define ntp configuration on BIGIP appliances.

> f5_ntp_servers: []

This variable contains the list of the NTP servers you needs to define. 
Here is an example of usage : `f5_ntp_servers: ['myntp1.server.net', 'myntp2.server.net']`

### DNS configuration

> f5_set_dns: False

This variable is set to define if dns configuration needs to be done.
Change the variable to true if you need to define dns configuration on BIGIP appliances.

> f5_dns_search_domains: []

This variable contains the list of dns search domains you need to defined.
This configuration will be done only if `f5_set_dns` is set to True.
Here is an example for this variable definition : `f5_dns_search_domains: ['mydomain1.net', 'mydomain2.net']`

> f5_dns_ip_version: ''

This variable is used to define what IP version is used on dns part. You can use '4' for ipv4 or '6' for ipv6
This configuration will be done only if `f5_set_dns` is set to True.

> f5_dns_cache_status: ''

This variable is used to defined if you need to enable dns cache on the BIGIP.
This configuration will be done only if `f5_set_dns` is set to True.
The possible values for this variable are 'disable' or 'enable'.

### Preferences

> f5_set_prefs: False

Change this variable to True to enable preferences custom configuration.

> f5_prefs_records_per_screen: ''

Define in this variable how the max records count can be disaplyed on the GUI.

> f5_prefs_default_view: ''

Define the default view you need to have on this GUI : 'advanced' or 'basic'.

### GUI and HTTPD settings

> f5_set_gui: False

Change this variable to True if you want to change GUI and httpd settings.

> f5_gui_auth_realm: ''

This variable is used to define the authentication realm on the BIGIP for administrative access.

> f5_gui_idle_timeout: ''

This variable allows to define the idle timeout for the administration interface (in second)

> f5_gui_authorized_subnets: ''

This variable can be defined to set authorized IP addresses which can access the administration interface.

> f5_gui_banner: ''

This string variable is the banner which will be diplayed on the GUI.

> f5_console_timeout: ''

This variable the console timeout you need to define (in second)

> f5_gui_ssl_ciphers_suite: ''

This variable defines the ciphers suite string which will be supported by the appliance on the management GUI.
Here is an example : `f5_gui_ssl_ciphers_suite: 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-SHA:AES256-SHA256'`

> f5_gui_ssl_protocols: ''

This variable defines the SSL protocol enabled on https management access.
Here is an example of the string you can use : `f5_gui_ssl_protocols: 'all -SSLv2 -SSLv3 -TLSv1'`

### SSHD settings 

> f5_set_sshd: False

Change this variable to True if you need to define custom sshd configuration.

> f5_sshd_banner_enabled: ''

Change this variable to True if you need to enable a custom ssh banner.

> f5_sshd_banner: ''

This variable is the sshd banner string. 
Here is an usage example : `f5_sshd_banner: '### MY BANNER ###'`

> f5_sshd_inactivity_timeout: ''

This variable is the inactivity timeout you need to apply on ssh access (in second)

### SNMP settings

> f5_set_snmp: False

Change the variable to True if you want to define custom snmp settings

> f5_snmp_contact: ''

This variable contains the snmp contact you want to define.
This variable should contain a string.

> f5_snmp_allowed_addresses: []

This variable is a list containing the BIGIP will accept snmp requests from.
Here is a usage example : `f5_snmp_allowed_addresses: ['127.0.0.0/8', '172.31.3.3']`

> f5_snmp_agent_authentication_traps: ''

This variable allows you to enable or disable authentication for snmp traps.
The variable should contain 'enable' or 'disable'.

> f5_snmp_agent_status_traps: ''

> f5_snmp_device_warning_traps: ''

> f5_snmp_v3_communities: []

> f5_snmp_rm_default_communities: True

> f5_snmp_default_community: 'public'

### Syslog settings

> f5_set_syslog: False

Change this variable to True to define a syslog server and enable logging using syslog servers

> f5_set_syslog_logging_levels: False

Change this variable to True if you want to defined the security logging level.

> f5_syslog_logging_levels:

This variable is dictionary containing the security level you want to configure per daemon or BIGIP topic.
Here is the default content of this dictionary :
> - { db_key: 'log.arp.level', level: 'Warning' }
> - { db_key: 'log.http.level', level: 'Error' }
> - { db_key: 'log.deflate.level', level: 'Error' }
> - { db_key: 'log.ip.level', level: 'Warning' }
> - { db_key: 'log.layer4.level', level: 'Notice' }
> - { db_key: 'log.mcpd.level', level: 'info' }
> - { db_key: 'log.net.level', level: 'Warning' }
> - { db_key: 'log.rules.level', level: 'Informational' }
> - { db_key: 'log.ssl.level', level: 'Warning' }
> - { db_key: 'log.tmm.level', level: 'Notice' }
> - { db_key: 'log.lind.level', level: 'Notice' }
> - { db_key: 'log.csyncd.level', level: 'Notice' }

This is our recommended configuration for these logging levels.

### Proxy settings

> f5_set_proxy: False

> f5_proxy_authentication: False

> f5_proxy_username: ''

> f5_proxy_password: ''

### Management static routes definitions

> f5_set_mgmt_routes: False

Dependencies
------------

ansible.f5.get-ha-status

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: bigip1
      roles:
         - ansible.f5.get-ha-status
         - ansible.f5.hardening

Please remember you need to define a requirements.yml file containing the source information for the role.
For further information regarding this part you can access the [Ansible Galaxy documentation](https://docs.ansible.com/ansible/latest/reference_appendices/galaxy.html#installing-multiple-roles-from-a-file)

Author Information
------------------

Jimmy Casse
