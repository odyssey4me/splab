ansible-splunk
==============

This role installs Splunk and automates some of the initial configuration.

Requirements
============

* The Splunk tarball must be placed in ``files/binaries``.
* The Splunk application tarballs must be placed in ``files/applications``.

Testing
=======

A test environment is provided using Vagrant and Virtualbox. To implement
a simple all-in-one test environment do the following:

```bash
git clone https://github.com/odyssey4me/splab.git
cd splab
vagrant up
```

