Ansible Role: IPBan
===================

[![CI](https://github.com/lsgalves/ansible-role-ipban/actions/workflows/ci.yml/badge.svg)](https://github.com/lsgalves/ansible-role-ipban/actions/workflows/ci.yml)
[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)
[![Ansible Role](https://img.shields.io/badge/ansible%20role-lsgalves.ipban-blue.svg)](https://galaxy.ansible.com/lsgalves/ipban/)
[![GitHub tag](https://img.shields.io/github/tag/lsgalves/ansible-role-ipban.svg)](https://github.com/lsgalves/ansible-role-ipban/tags)

Deploy [IPBan](https://github.com/DigitalRuby/IPBan), a software to block hackers and botnets.

Requirements
------------

This role depends on the `ansible.windows.win_service_info` module, so you must have installed the `ansible.windows >= 1.10.0` collection. To install, run `ansible-galaxy collection install ansible.windows`.

Role Variables
--------------

All variables which can be overridden are stored in [defaults/main.yml](https://github.com/cloudalchemy/ansible-role-ipban/blob/master/defaults/main.yml) file as well as in table below.

| Name | Default Value | Description |
| ---- | ------------- | ----------- |
| `ipban_version` | 1.7.2 | IPBan version |
| `ipban_append_config` | yes | Whether to add or overwrite the default configuration |
| `ipban_log_files_to_parse` | [] | List of log files to check periodically |
| `ipban_expressions_to_block` | [] | Event viewer expression list to check for failed logins on Windows |
| `ipban_expressions_to_notify` | [] | Event viewer expression list to check for successful logins on Windows |
| `ipban_failed_login_attempts_before_ban` | 5 | How many failed logins before banning an ip address |
| `ipban_ban_time` | 01:00:00:00 | Time to ban an ip address in `DD:HH:MM:SS` format to ban forever |
| `ipban_reset_failed_fogin_count_for_unbanned_ip_addresses` | no | Ignored if only a single `ipban_ban_time` is specified. If this value is true, the failed login count will be reset to 0, causing the next failed login to start at 1 and then the ip address will move to the next ban time once the failed login count reaches `ipban_failed_login_attempts_before_ban`. If this value is false, then the failed login count will not be reset, and the next failed login will cause an immediate ban of the ip address into the next ban time in the `ipban_ban_time` list |
| `ipban_clear_banned_ip_addresses_on_restart` | no | Whether to remove all banned ip addresses from the ipban database and firewall upon startup of the service |
| `ipban_clear_failed_logins_on_successful_login` | no | Whether to clear all failed logins for an ip address if there is a successful login from that ip address |
| `ipban_process_internal_ip_addresses` | no | Whether to process internal ip addresses. Set to true if you want to allow internal ip addresses to be detected and/or banned |
| `ipban_expire_time` | 01:00:00:00 | Remove failed logins that have not yet been banned after this time in `DD:HH:MM:SS` format. 00:00:00:00 to never forget a failed login |
| `ipban_cycle_time` | 00:00:00:15 | How often to run processing and housekeeping, in `DD:HH:MM:SS` format |
| `ipban_minimum_time_between_failed_login_attempts` | 00:00:00:01 | The minimum time between failed login attempts for an ip address to increment the ban counter, in `DD:HH:MM:SS` format |
| `ipban_minimum_time_between_successful_login_attempts` | 00:00:00:05 | Same as `ipban_minimum_time_between_failed_login_attempts` but for successful logins |
| `ipban_firewall_rule_prefix` | IPBan_ | The firewall rule prefix |
| `ipban_whitelist` | "" | Comma separated list of ip addresses, urls or DNS names that are never banned. If you use a url, each ip entry in the response should be newline delimited |
| `ipban_whitelist_regex` | "" | Regular expression for whitelisting, allows for pattern matching or range of ip addresses |
| `ipban_blacklist` | "" | Comma separated list of ip addreses, urls or DNS names to always ban and never unban. Same format as `ipban_whitelist` |
| `ipban_blacklist_regex` | "" | Regular expression for blacklisting, allows for pattern matching or banning of range of ip addresses |
| `ipban_user_name_whitelist` | "" | Comma separated list of user names that are allowed |
| `ipban_user_name_whitelist_regex` | "" | Regular expression for whitelisting, allows user names for pattern matching |
| `ipban_user_name_whitelist_minimum_edit_distance` | 2 | If the edit distance of a failed user name is greater than this distance away, the user name is immediately banned |
| `ipban_failed_login_attempts_before_ban_user_name_whitelist` | 20 | If a user name is on the user name whitelist, then the failed login attempts before ban is this value instead of `ipban_failed_login_attempts_before_ban` |
| `ipban_process_to_run_on_ban` | "" | Run an external process when a ban occurs. Separate the process and any arguments with a pipe `|` |
| `ipban_process_to_run_on_unban` | "" | Run an external process when a ban is removed. Separate the process and any arguments with a pipe `|` |
| `ipban_firewall_rules` | "" | Firewall rules to create to allow or block ip addresses, one per line |
| `ipban_use_default_banned_ip_address_handler` | yes | Whether to use the default banned ip address handler for banned ip address sharing. Set to false to turn this off |
| `ipban_external_ip_address_url` | https://checkip.amazonaws.com/ | Specify an external url to get the remote ip address of the machine |
| `ipban_firewall_uri_rules` | "" | External firewall uri rules to block, format is one per line: `[RulePrefix],[Interval in DD:HH:MM:SS],[URI][Newline]` |
| `ipban_get_url_update` | "" |  Url to get when the service cycle runs, empty for none |
| `ipban_get_url_start` | "" |  Url to get when the service starts, empty for none |
| `ipban_get_url_stop` | "" |  Url to get when the service stops, empty for none |
| `ipban_get_url_config` | "" |  Url to get config file from, empty for none |

**For more details on options check the [official configuration](https://github.com/DigitalRuby/IPBan/wiki/Configuration).**

Example Playbook
----------------

Example of executing the role:

```yml
- hosts: all
  roles:
    - lsgalves.ipban
```

License
-------

MIT

Author Information
------------------

Leonardo Galves <leonardosgalves@gmail.com>
