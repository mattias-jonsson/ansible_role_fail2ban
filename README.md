Ansible Role: ansible_role_fail2ban
=========

An Ansible role that installs and configures fail2ban.
This role supports the following Linux distributions:

<ul>
<li>CentOS 7/8
<li>Red Hat Enterprise Linux 7/8
<li>Ubuntu 18/20/22
<li>Debian 11
</ul>

Requirements
------------

This role has no dependencies besides what is included in Ansible Core.

Role Variables
--------------

Available variables are listed below, along with default values where applicable (see defaults/main.yml):


    ansible_role_fail2ban_cleanup: false

Optional, boolean, default value is false. If set to true any .conf or .local file in /etc/fail2ban/fail2ban.d or /etc/fail2ban/jail.d not created by the role will be deactivated/renamed.  

    ansible_role_fail2ban_jails: []

List of jails to configure and enable. Each jail can be configured with the following variables: `action`, `action_`, `backend`, `banaction`, `banaction_allports`, `bantime`, `bantime_increment`, `bantime_rndtime`, `comment`, `enable`, `failregex`, `filter`, `findtime`, `ignorecommand`, `ignoreip`, `ignoreregex`, `logencoding`, `logpath`, `maxmatches`, `maxretry`, `name`, `port`, `usedns`, `knocking_url`, `blocktype`, `returntype` Refer to the role variables or jail.conf manpage for explaination of options.
    
    ansible_role_fail2ban_defaults_action_:

General purpose action. Please refer to the distribution defaults from the vars files in the `vars` folder.

    ansible_role_fail2ban_defaults_action_abuseipdb: abuseipdb

Report ban via abuseipdb.com. See `action.d/abuseipdb.conf` for examples.

    ansible_role_fail2ban_defaults_action_badips_report:

Report ban via badips.com. (this likely nolonger works)

    ansible_role_fail2ban_defaults_action_badips:

Report ban via badips.com. (this likely nolonger works)

    ansible_role_fail2ban_defaults_action_blocklist_de:

Report ban via blocklist.de.

    ansible_role_fail2ban_defaults_action_cf_mwl:

Ban IP on CloudFlare & send an e-mail with whois report and relevant log lines to the destemail.

    ansible_role_fail2ban_defaults_action_mw:

Ban & send an e-mail with whois report to the destemail.

    ansible_role_fail2ban_defaults_action_mwl:

Ban & send an e-mail with whois report and relevant log lines to the destemail.

    ansible_role_fail2ban_defaults_action_xarf:

See the IMPORTANT note in action.d/xarf-login-attack for when to use this action. Ban & send a xarf e-mail to abuse contact of IP address and include relevant log lines to the destemail.

    ansible_role_fail2ban_defaults_action: %(action_)s

Choose default action. To change, just override value of 'action' with the interpolation to the chosen action shortcut (e.g.  action_mw, action_mwl, etc)

    ansible_role_fail2ban_defaults_backend: auto

Specifies the backend used to get files modification. Available options are "pyinotify", "gamin", "polling", "systemd" and "auto".

    ansible_role_fail2ban_defaults_banaction_allports:

the same as banaction but for some "allports" jails like "pam-generic" or "recidive" (default iptables-allports).

    ansible_role_fail2ban_defaults_banaction:

banning action (default iptables-multiport) typically specified in the [DEFAULT] section for all jails. This parameter will be used by the standard substitution of action and can be redefined central in the [DEFAULT] section inside jail.local (to apply it to all jails at once) or separately in each jail, where this substitution will be used.

    ansible_role_fail2ban_defaults_bantime_factor:

`bantime.factor` is a coefficient to calculate exponent growing of the formula or common multiplier, default value of factor is 1 and with default value of formula, the ban time grows by 1, 2, 4, 8, 16 ...

    ansible_role_fail2ban_defaults_bantime_formula:

`bantime.formula` used by default to calculate next value of ban time. Ex:  
`bantime.formula = ban.Time * (1<<(ban.Count if ban.Count<20 else 20)) * banFactor`  
`bantime.formula = ban.Time * math.exp(float(ban.Count+1)*banFactor)/math.exp(1*banFactor)`

    ansible_role_fail2ban_defaults_bantime_increment:

`bantime.increment` allows to use database for searching of previously banned ip's to increase a default ban time using special formula, default it is banTime * 1, 2, 4, 8, 16, 32...

    ansible_role_fail2ban_defaults_bantime_maxtime:

`bantime.maxtime` is the max number of seconds using the ban time can reach (doesn't grow further)

    ansible_role_fail2ban_defaults_bantime_multipliers:

`bantime.multipliers` used to calculate next value of ban time instead of formula, coresponding previously ban count and given "bantime.factor" (for multipliers default is 1)

The following example grows ban time by 1, 2, 4, 8, 16 ... and if last ban count greater as multipliers count, always used last multiplier (64 in example), for factor '1' and original ban time 600 - 10.6 hours  
`bantime.multipliers = 1 2 4 8 16 32 64`

The following example can be used for small initial ban time (bantime=60) - it grows more aggressive at begin, for bantime=60 the multipliers are minutes and equal: 1 min, 5 min, 30 min, 1 hour, 5 hour, 12 hour, 1 day, 2 day  
`bantime.multipliers = 1 5 30 60 300 720 1440 2880`

    ansible_role_fail2ban_defaults_bantime_overalljails:

`bantime.overalljails` (if true) specifies the search of IP in the database will be executed cross over all jails, if false (dafault), only current jail of the ban IP will be searched

    ansible_role_fail2ban_defaults_bantime_rndtime:

`bantime.rndtime` is the max number of seconds using for mixing with random time to prevent "clever" botnets calculate exact time IP can be unbanned again:

    ansible_role_fail2ban_defaults_bantime:

`bantime` is the number of seconds that a host is banned.

    ansible_role_fail2ban_defaults_chain:

Specify chain where jumps would need to be added in ban-actions expecting parameter chain

    ansible_role_fail2ban_defaults_destemail:

Destination email address used solely for the interpolations in jail.{conf,local,d/*} configuration files.

    ansible_role_fail2ban_defaults_enabled:

Enables the jails. By default all jails are disabled, and it should stay this way. Enable only relevant to your setup jails in your .local or jail.d/*.conf
  
`true`:  jail will be enabled and log files will get monitored for changes  
`false`: jail is not enabled

    ansible_role_fail2ban_defaults_fail2ban_agent:

Format of user-agent https://tools.ietf.org/html/rfc7231#section-5.5.3

    ansible_role_fail2ban_defaults_filter:

"filter" defines the filter to use by the jail. By default jails have names matching their filter name

    ansible_role_fail2ban_defaults_findtime:

A host is banned if it has generated "maxretry" during the last "findtime" seconds.

    ansible_role_fail2ban_defaults_ignorecommand:

External command that will take an tagged arguments to ignore, e.g. <ip>, and return true if the IP is to be ignored. False otherwise. `ignorecommand = /path/to/command <ip>`

    ansible_role_fail2ban_defaults_ignoreip:

`ignoreip` can be a list of IP addresses, CIDR masks or DNS hosts. Fail2ban will not ban a host which matches an address in this list. Several addresses can be defined using space (and/or comma) separator.

    ansible_role_fail2ban_defaults_ignoreself:

`ignoreself` specifies whether the local resp. own IP addresses should be ignored (default is true). Fail2ban will not ban a host which matches such addresses.

    ansible_role_fail2ban_defaults_logencoding:

`logencoding` specifies the encoding of the log files handled by the jail. This is used to decode the lines from the log file. Typical examples:  `ascii`, `utf-8`, `auto` will use the system locale setting

    ansible_role_fail2ban_defaults_maxmatches:

`maxmatches` is the number of matches stored in ticket (resolvable via tag <matches> in actions).

    ansible_role_fail2ban_defaults_maxretry:

`maxretry` is the number of failures before a host get banned.

    ansible_role_fail2ban_defaults_mode:

`mode` defines the mode of the filter (see corresponding filter implementation for more info).

    ansible_role_fail2ban_defaults_mta:

E-mail action. Since 0.8.1 Fail2Ban uses sendmail MTA for the mailing. Change mta configuration parameter to mail if you want to revert to conventional 'mail'.

    ansible_role_fail2ban_defaults_port:

Ports to be banned, usually should be overridden in a particular jail

    ansible_role_fail2ban_defaults_protocol:

Default protocol

    ansible_role_fail2ban_defaults_sender:

Sender email address used solely for some actions

    ansible_role_fail2ban_defaults_usedns:

`usedns` specifies if jails should trust hostnames in logs, warn when DNS lookups are performed, or ignore all hostnames in logs  
  
`yes` if a hostname is encountered, a DNS lookup will be performed.  
`warn` if a hostname is encountered, a DNS lookup will be performed, but it will be logged as a warning.  
`no` if a hostname is encountered, will not be used for banning, but it will be logged as info.  
`raw` use raw value (no hostname), allow use it for no-host filters/actions (example user)
    


Dependencies
------------

This role is dependent on `https://github.com/mattias-jonsson/ansible_role_epel`.

Example Playbook
----------------

Setups some fail2ban jails

    - hosts: servers

      vars:

        ansible_role_fail2ban_cleanup: true
        ansible_role_fail2ban_jails:
        - name: 'sshd'
            enabled: 'true'
            bantime.increment: 'true'
            bantime.rndtime: '360'
            ignoreip: '192.168.0.1'
            bantime: '3600'
            findtime: '3600'
            maxretry: '5'
            maxmatches: '%(maxretry)s'
            backend: 'auto'
            usedns: 'no'
            logencoding: 'auto'

      roles:
         - role: ansible_role_fail2ban

License
-------

MIT

Author Information
------------------

Mattias Jonsson