# puppet_master_config

Description
-----------
Configures the puppet master to use environments with r10k

Requirements
------------

  - learn_puppet_centos-6.5-pe-3.7.1-ptb-ovf
  - VirtualBox 4.3.24 (Network-> Bridge Adapter Promiscuous Mode-> Allaow All)

### Commands:

cd /tmp
git clone https://github.com/victorhdamian/puppet_master_config.git
puppet module install zack/r10k
puppet apply configure_r10k.pp
puppet apply configure_directory_environments.pp
rm -Rf /etc/puppetlabs/puppet/environments/production/*
r10k deploy environment -pv


Example
-------

    [root@learning ~]# cd /tmp
    [root@learning /tmp]# git clone https://github.com/victorhdamian/puppet_master_config.git
    Initialized empty Git repository in /tmp/puppet_master_config/.git/
    remote: Counting objects: 31, done.
    remote: Compressing objects: 100% (31/31), done.
    remote: Total 31 (delta 13), reused 0 (delta 0), pack-reused 0
    Unpacking objects: 100% (31/31), done.
    [root@learning /tmp]# cd puppet*
    [root@learning /tmp/puppet_master_config:production]# puppet module install zack/r10k
    Notice: Preparing to install into /etc/puppetlabs/puppet/environments/production/modules ...
    Notice: Downloading from https://forgeapi.puppetlabs.com ...
    Notice: Installing -- do not interrupt ...
    /etc/puppetlabs/puppet/environments/production/modules
    └─┬ zack-r10k (v2.6.5)
      ├── croddy-make (v0.0.5)
      ├─┬ gentoo-portage (v2.2.0)
      │ └── puppetlabs-concat (v1.2.0)
      ├── puppetlabs-gcc (v0.2.0)
      ├── puppetlabs-git (v0.3.0)
      ├── puppetlabs-inifile (v1.2.0)
      ├── puppetlabs-pe_gem (v0.1.0)
      ├── puppetlabs-ruby (v0.4.0)
      ├── puppetlabs-stdlib (v4.5.1)
      └── puppetlabs-vcsrepo (v1.2.0)
    [root@learning /tmp/puppet_master_config:production]# puppet apply configure_r10k.pp
    Notice: Compiled catalog for learning.puppetlabs.vm in environment production in 1.16 seconds
    Warning: The package type's allow_virtual parameter will be changing its default value from false to true in a future release. If you do not want to allow virtual packages, please explicitly set allow_virtual to false.
       (at /opt/puppet/lib/ruby/site_ruby/1.9.1/puppet/type/package.rb:430:in `block (3 levels) in <module:Puppet>')
    Notice: /Stage[main]/R10k::Install/Package[r10k]/ensure: created
    Notice: /Stage[main]/R10k::Install::Pe_gem/File[/usr/bin/r10k]/ensure: created
    Notice: /Stage[main]/R10k::Config/File[r10k.yaml]/ensure: defined content as '{md5}3d39353eeca760c48cf83610809c743a'
    Notice: /Stage[main]/R10k::Config/Ini_setting[R10k Modulepath]/ensure: created
    Notice: Finished catalog run in 29.05 seconds
    [root@learning /tmp/puppet_master_config:production]# puppet apply configure_directory_environments.pp
    Warning: Setting modulepath is deprecated in puppet.conf. See http://links.puppetlabs.com/env-settings-deprecations
       (at /opt/puppet/lib/ruby/site_ruby/1.9.1/puppet/settings.rb:1141:in `issue_deprecation_warning')
    Notice: Compiled catalog for learning.puppetlabs.vm in environment production in 0.04 seconds
    Notice: /Stage[main]/Main/Ini_setting[Configure environmentpath]/value: value changed '/etc/puppetlabs/puppet/environments' to '$confdir/environments'
    Notice: /Stage[main]/Main/Ini_setting[Configure basemodulepath]/value: value changed '/etc/puppetlabs/puppet/modules:/opt/puppet/share/puppet/modules' to '$confdir/modules:/opt/puppet/share/puppet/modules'
    Notice: Finished catalog run in 0.22 seconds
    [root@learning /tmp/puppet_master_config:production]# ls -lrt /etc/puppetlabs/puppet/environments/production/modules
    total 52
    drwxr-xr-x 6 root root 4096 May 15  2014 portage
    drwxr-xr-x 4 root root 4096 Jul  4  2014 gcc
    drwxr-xr-x 4 root root 4096 Oct 28 18:11 pe_gem
    drwxr-xr-x 5 root root 4096 Nov  4 22:30 vcsrepo
    drwxr-xr-x 5 root root 4096 Nov 10 23:46 inifile
    drwxr-xr-x 7 root root 4096 Nov 18 20:50 git
    drwxr-xr-x 7 root root 4096 Nov 18 20:53 ruby
    drwxr-xr-x 4 root root 4096 Dec 30 12:39 cowsayings
    drwxr-xr-x 4 root root 4096 Dec 30 12:39 lvmguide
    drwxr-xr-x 6 root root 4096 Jan 15 00:40 stdlib
    drwxr-xr-x 6 root root 4096 Jan 16 22:53 make
    drwxr-xr-x 7 root root 4096 Feb 17 23:28 concat
    drwxr-xr-x 8 root root 4096 Mar 18 22:14 r10k
    [root@learning /tmp/puppet_master_config:production]# rm -Rf /etc/puppetlabs/puppet/environments/production/*
    [root@learning /tmp/puppet_master_config:production]# r10k deploy environment -pv
    [R10K::Action::Deploy::Environment - INFO] Deploying environment /etc/puppetlabs/puppet/hiera/hiera_master
    [R10K::Action::Deploy::Environment - INFO] Deploying environment /etc/puppetlabs/puppet/environments/production
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/apache
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/ntp
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/stdlib
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/ruby
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/gcc
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/pe_gem
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/make
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/inifile
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/vcsrepo
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/git
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/concat
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/portage
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/r10k
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/notifyme
    [R10K::Action::Deploy::Environment - INFO] Deploying module /etc/puppetlabs/puppet/environments/production/modules/profiles
    [root@learning /tmp/puppet_master_config:production]# ls -lrt /etc/puppetlabs/puppet/environments/production/modules
    total 60
    drwxr-xr-x 6 root root 4096 May 15  2014 portage
    drwxr-xr-x 4 root root 4096 Jul  4  2014 gcc
    drwxr-xr-x 4 root root 4096 Oct 28 18:11 pe_gem
    drwxr-xr-x 5 root root 4096 Nov  4 22:30 vcsrepo
    drwxr-xr-x 7 root root 4096 Nov  5 18:51 ntp
    drwxr-xr-x 5 root root 4096 Nov 10 23:46 inifile
    drwxr-xr-x 7 root root 4096 Nov 18 20:50 git
    drwxr-xr-x 7 root root 4096 Nov 18 20:53 ruby
    drwxr-xr-x 6 root root 4096 Jan 15 00:40 stdlib
    drwxr-xr-x 6 root root 4096 Jan 16 22:53 make
    drwxr-xr-x 7 root root 4096 Feb 17 23:28 concat
    drwxr-xr-x 8 root root 4096 Mar 17 23:49 apache
    drwxr-xr-x 8 root root 4096 Mar 18 22:14 r10k
    drwxr-xr-x 4 root root 4096 Mar 31 21:45 notifyme
    drwxr-xr-x 4 root root 4096 Mar 31 21:45 profiles
    [root@learning /tmp/puppet_master_config:production]# puppet module list
    Warning: Setting modulepath is deprecated in puppet.conf. See http://links.puppetlabs.com/env-settings-deprecations
       (at /opt/puppet/lib/ruby/site_ruby/1.9.1/puppet/settings.rb:1141:in `issue_deprecation_warning')
    /etc/puppetlabs/puppet/environments/production/modules
    ├── croddy-make (v0.0.5)
    ├── gentoo-portage (v2.2.0)
    ├── notifyme (???)
    ├── profiles (???)
    ├── puppetlabs-apache (v1.4.0)
    ├── puppetlabs-concat (v1.2.0)
    ├── puppetlabs-gcc (v0.2.0)
    ├── puppetlabs-git (v0.3.0)
    ├── puppetlabs-inifile (v1.2.0)
    ├── puppetlabs-ntp (v3.3.0)
    ├── puppetlabs-pe_gem (v0.1.0)
    ├── puppetlabs-ruby (v0.4.0)
    ├── puppetlabs-stdlib (v4.5.1)
    ├── puppetlabs-vcsrepo (v1.2.0)
    └── zack-r10k (v2.6.5)
    /etc/puppetlabs/puppet/modules (no modules installed)
    /opt/puppet/share/puppet/modules
    ├── puppetlabs-pe_accounts (v2.0.2-6-gd2f698c)
    ├── puppetlabs-pe_concat (v1.1.2-4-g2b7bba2)
    ├── puppetlabs-pe_console_prune (v0.1.1-4-g293f45b)
    ├── puppetlabs-pe_inifile (v1.1.4-16-gcb39966)
    ├── puppetlabs-pe_java_ks (v1.2.4-35-g44fbb26)
    ├── puppetlabs-pe_postgresql (v3.4.4-15-g32e56ed)
    ├── puppetlabs-pe_razor (v0.2.1-9-g8d78ec2)
    ├── puppetlabs-pe_repo (v0.7.7-51-g5ba0427)
    ├── puppetlabs-pe_staging (v0.3.3-2-g3ed56f8)
    └── puppetlabs-puppet_enterprise (v3.7.1-90-g4a9e885)
