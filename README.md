# vrouter-vagrant

Vagrant for building contrail.

Create key ~/.ssh/other_keys/vms_key

    vagrant up
    vagrant ssh
    cd contrail-vnc
    scons -j 8

You can clean SConscript from Openstack stuff for faster build.
