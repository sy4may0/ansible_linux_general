Linux General
=========

Perform initial settings for Linux.

Requirements
------------

None.

Role Variables
--------------
Refer to the setting examples below.
### Task run setting
```yml
# Exec tasks.
linux_general_run_chrony: yes
linux_general_run_cron: no
linux_general_run_default_target: no
linux_general_run_dnf: no
linux_general_run_files: no
linux_general_run_firewalld: yes
linux_general_run_hostname: yes
linux_general_run_locale: yes
linux_general_run_logrotate_base: no
linux_general_run_mount: no
linux_general_run_resolve: no
linux_general_run_rsyslog: yes
linux_general_run_selinux: no
linux_general_run_services: no
linux_general_run_sudoers: yes
linux_general_run_sysctl: no
linux_general_run_timezone: no
linux_general_run_users: yes
```

#### default.target setting
```yml
# Linux systemd default target.
linux_general_default_target: multi-user.target
```

#### locale setting
```yml
# defaults file for rhel sub_locale.yml tasks.
linux_general_locale: en_US.utf-8
```

#### Create files, directories and links
```yml
# Create linux file or directory.
# The 'mode' must be enclosed in quotes like "0655".
# Select the 'state' value from the following.
#  - touch:     Create an empty file if it does not exist.
#  - directory: Create a directory if it does not exist.
#  - absent:    Remove the file if it exists.
linux_general_files:
  - path: /data/cloaks
     owner: superman
     group: superman 
     mode: "0755"
     state: directory

# Create linux file link.
# Select the 'state' value from the following.
#  - link:    Create a symbolic link if it does not exist.
#  - hard:    Create a hard link if it does not exist.
#  - absent:  Remove the link if it exists.
linux_general_file_links:
  - src: /usr/local/ext/nuclear_button.sh
    dest: /home/d.trump/nuclear_button.sh
    state: link
```

#### mount filesystem
```yml
# Linux filesystem mount settings.
linux_general_mount:
  - path: /data
    src: 192.168.19.31:/nfs/data
    fstype: nfs
    opts: rw,sync,nfsvers=3
    state: present
```

#### DNS lookup server setting
```yml
# DNS servers.
linux_general_dns_servers: 
 - 1.1.1.1
 - 8.8.8.8 

# DNS search domains.
linux_general_dns_domains:
  - localdomain
```

#### selinux setting
```yml
# SELinux state.
# (enforcing|permissive|disabled)
linux_general_selinux_state: enforcing

# SELinux policy.
linux_general_selinux_policy: targeted
```

#### Enable and Disable servicves
```yml
# Linux systemd enable services.
linux_general_enable_services:
  - smbd

# Linux systemd disable services.
linux_general_disable_services:
  - firewalld
  - NetworkManager
```

#### User and Group setting
```yml
# Linux groups.
linux_general_groups:
  - name: lale
    gid: 10000
    state: present
 
# Linux users.
linux_general_users:
  - name: s_minato
    group: lale
    groups: []
    uid: 10720
    shell: /bin/bash
    home: /home/s_minato
    password: IamFlasher
    state: present

# How to specify password string.
# yes: Specify the hash directly.
# no:  Specify plain text password.
linux_general_password_style_direct_hash: no
```
#### Timezone setting
```yml
# Timezone
linux_general_timezone: UTC
```
#### dnf repository and install package
```yml
# dnf repository
linux_general_dnf_repositories:
  - name: MyRepo
    url: http://192.168.39.11/dist/repo

# Install packages
linux_general_dnf_packages:
  - name: vim
    state: latest
  - name: zsh
    state: latest
```

#### sysctl kernel setting
```yml
# sysctl kernel parameter.
linux_general_sysctl:
  - name: vm.swappiness 
    value: '5'
    state: present
```

#### sudoers setting
```yml
# sudoers settings.
linux_general_sudoers:
  - user_alias: lale_group
    users:
      - s_minato
      - i_kurokane
      - n_krebayashi
      - t_hatae
    cmd_alias: debel_commands
    commands:
      - /usr/bin/make install
```

#### cron setting
```yml
# Cron task settings.
linux_general_cron: []
  - name: run ls command
    weekday: "*"
    month: "*"
    day: "*"
    hour: "2"
    minute: "2"
    job: ls -la
    user: root
```

#### logrotate base setting
```yml
# Log rotation interval.
linux_general_logrotate_interval: weekly
# Log rotation backlog counts.
linux_general_logrotate_backlogs: 12
# Compress rotated log file.
linux_general_logrotate_compress: yes
```

#### rsyslog setting
```yml
# rsyslog facility settings.
linux_general_rsyslog_facilities:
  - facility: local3
    target: /var/log/local3.log
    owner: root
    group: root
    mode: '0644'

# Use rsyslog tcpserver.
linux_general_rsyslog_tcpserver: no
# rsyslog tcpserver port number.
linux_general_rsyslog_tcpport: 512
# Use rsyslog udpserver.
linux_general_rsyslog_udpserver: yes
# rsyslog udpserver port number.
linux_general_rsyslog_udpport: 512

# Special settings for Fujitsu ServerView
linux_general_rsyslog_serverview: no

# rsyslog log forwarding settings.
linxu_general_rsyslog_forward:
  - src: '*.*'
    dest: '@@192.168.39.51:514'
```

#### firewalld setting
```yml
# Permit services
linux_general_firewalld_services:
  - service: http
    zone: public
    state: enabled

# Permit protocols
linux_general_firewalld_protocols:
  - protocol: ospf
    zone: public
    state: enabled

# Permit ports
linux_general_firewalld_ports:
  - port: 3128/tcp
    zone: public
    state: enabled

# Rich rules
linux_general_firewalld_rich_rules:
  - rich_rule: rule service name="ftp" audit limit value="1/m" accept
    zone: public
    state: enabled
```

#### chrony settings
```yml
# Activate chrony
linux_general_chrony: yes
# NTP servers
linux_general_chrony_servers:
  - 2.almalinux.pool.ntp.org
# NTP allow networks
linux_general_chrony_allows:
  - 192.168.0.0/24
# NTP server stratum
linux_general_chrony_stratum: 10
```

Dependencies
------------

None.

Example Playbook
----------------
```yml
- hosts: linux_servers
  become: yes
  roles:
    - linux_general
```

License
-------

MIT

Author Information
------------------
https://github.com/sy4may0
