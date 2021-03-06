horizon
=======

#### Table of Contents

1. [Overview - What is the horizon module?](#overview)
2. [Module Description - What does the module do?](#module-description)
3. [Setup - The basics of getting started with horizon](#setup)
4. [Implementation - An under-the-hood peek at what the module is doing](#implementation)
5. [Limitations - OS compatibility, etc.](#limitations)
6. [Development - Guide for contributing to the module](#development)
7. [Contributors - Those with commits](#contributors)
8. [Release Notes - Notes on the most recent updates to the module](#release-notes)

Overview
--------

The horizon module is a part of [Stackforge](https://github.com/stackfoge), an effort by the Openstack infrastructure team to provide continuous integration testing and code review for Openstack and Openstack community projects not part of the core software.  The module its self is used to flexibly configure and manage the dashboard service for Openstack.

Module Description
------------------

The horizon module is a thorough attempt to make Puppet capable of managing the entirety of horizon.  Horizon is a fairly classic django application, which results in a fairly simply Puppet module.

This module is tested in combination with other modules needed to build and leverage an entire Openstack software stack.  These modules can be found, all pulled together in the [openstack module](https://github.com/stackfoge/puppet-openstack).

Setup
-----

**What the horizon module affects**

* horizon, the dashboard service for Openstack.

### Installing horizon

    example% puppet module install puppetlabs/horizon

### Beginning with horizon

To utilize the horizon module's functionality you will need to declare multiple resources but you'll find that doing so is much less complicated than the other OpenStack component modules.  The following is a modified excerpt from the [openstack module](https://github.com/stackfoge/puppet-openstack).  We recommend you consult and understand the [openstack module](https://github.com/stackforge/puppet-openstack) and the [core openstack](http://docs.openstack.org) documentation.

**Define a horizon dashboard**

```puppet
class { 'memcached':
  listen_ip => '127.0.0.1',
  tcp_port  => '11211',
  udp_port  => '11211',
}

class { '::horizon':
  cache_server_ip       => '127.0.0.1',
  cache_server_port     => '11211',
  secret_key            => '12345',
  swift                 => false,
  django_debug          => 'True',
  api_result_limit      => '2000',
}
```

Implementation
--------------

### horizon

Horizon is a simple module using the combination of a package, template, and the file_line type.  Most all the configuration lives inside the included local_settings template and the file_line type is for selectively inserting needed lines into configuration files that aren't explicitly managed by the horizon module.

Limitations
------------

* Only supports Apache using mod_wsgi.

Development
-----------

Developer documentation for the entire puppet-openstack project.

* https://wiki.openstack.org/wiki/Puppet-openstack#Developer_documentation

Contributors
------------

* https://github.com/stackforge/puppet-horizon/graphs/contributors

Release Notes
-------------

**2.2.0**

* Fixed apache 0.9.0 incompatability.
* Various lint fixes.

**2.1.0**

* Updated local_settings.py.
* Pinned Apache module version.
* Varioud lint fixes.

**2.0.0**

* Upstream is now part of stackfoge.
* httpd config now managed on every platform.
* Provides option to enable Horizon's display of block device mount points.

