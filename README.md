# Puppet Dev Project: User Management and LAMP Stack Configuration

## Project Overview

This project demonstrates the use of Puppet, an open-source configuration management tool, to automate the creation and management of user accounts, groups, and the deployment of a LAMP (Linux, Apache, MySQL, PHP) stack. The tasks are divided into four parts, showcasing the power and efficiency of Puppet for system administrators.

## Project Structure

Part 1: Setup Directory and Initial Puppet Manifest

Part 2: Implement User and Group Management

Part 3: Enhance Password Security

Part 4: Create a LAMP Stack Role Using Puppet

All manifest files are located within the puppet-dev directory.

## Instructions

### Part 1: Setup Directory and Initial Puppet Manifest

Create and navigate to the working directory:
```
mkdir puppet-dev
cd puppet-dev
```
Create the Puppet manifest file:
```
nano server_users_groups.pp
```

nano server_users_groups.pp

### Part 2: Implement User and Group Management
Define groups group01 and group02 in the manifest.

Create users user04 and user06 with specified groups and passwords:
```
group { 'group01':
  ensure => present,
}

group { 'group02':
  ensure => present,
}

user { 'user04':
  ensure => present,
  shell => '/bin/bash',
  password => '$6$xyz$RS.wHeM.mhNC0lxrp5Zds0ubSAKobEjpYvIWroBijzmu0tuqfQ1C6iBejYnxrEonuCOM0jgFUF3Dc038gW2.D.',
  groups => 'group01',
  managehome => true,
}

user { 'user06':
  ensure => present,
  password => 'user06',
  groups => ['group01', 'group02'],
}
```

Apply the manifest and verify user and group creation:

```
sudo puppet apply server_users_groups.pp
```
Check /etc/passwd and /etc/group to confirm the updates.

### Part 3: Enhance Password Security

Generate a SHA-512 hashed password for user06:
```
openssl passwd -6 -salt xyz user06
```
Update the user06 configuration:
```
user { 'user06':
  ensure => present,
  password => '$6$xyz$0RT6mRekQHKdXyWm/wFA06pcoh9KiH9HgdzELyXsmHKHGV6/h6VnAs44BLlVCXvMvi4Ju9c6VAeTMK3.4TjLm1',
  groups => ['group01', 'group02'],
  shell => '/bin/bash',
  managehome => true,
}
```
Apply the manifest again to update the password securely:
```
sudo puppet apply server_users_groups.pp
```
### Part 4: Create a LAMP Stack Role Using Puppet
Create a new manifest file:
```
touch lamp_stack_server.pp
```
Add LAMP stack automation code:
```
package { 'apache2':
  ensure => installed,
}

package { 'php':
  ensure => installed,
  notify  => Service['apache2'],
  require => Package['apache2'],
}

package { 'mysql-server':
  ensure => installed,
}

service { 'apache2':
  ensure => running,
  enable => true,
}
```
Apply the manifest to install and configure the LAMP stack:
```
sudo puppet apply lamp_stack_server.pp
```
Verify the LAMP stack installation:

Check that Apache is running: systemctl status apache2

Test PHP processing with a phpinfo() page.


## Verification and Testing
User Management: Ensure users and groups are correctly created and can log in with their assigned credentials.

LAMP Stack: Verify that the LAMP stack components are installed and configured properly by hosting a sample PHP page.

Conclusion

This project highlights the capabilities of Puppet in automating routine administrative tasks such as user management and application stack deployment. By following these steps, system administrators can reduce manual errors, ensure consistency, and enhance operational efficiency.



