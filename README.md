# woodpecker_vagrant_libvirt_ansible

This Vagrant setup creates two VMs and installs the
[WoodpeckerCI](https://github.com/woodpecker-ci/woodpecker) server and agent
packages on one machine each.

Default OS is openSUSE Leap 15.6. Although that can be changed in the
Vagrantfile, please beware that this will break the Ansible provisioning.

## Vagrant

1. You need vagrant obviously. And ansible. And git...
1. Fetch the box, per default this is `opensuse/Leap-15.6.x86_64`, using
   `vagrant box add opensuse/Leap-15.6.x86_64`.
1. Make sure the git submodules are fully working by issuing `git submodule init
   && git submodule update`
1. Run `vagrant up`
1. The Ansible provisioning will fail during the woodpecker server
   configuration. This is to be expected, as there is a manual step to be done.
1. Log into the Forgejo instance, the URL and credentials were printed shortly
   before the provisioning stopped. Go to the user settings, click on
   **Applications** and create a new OAUTH application called `Woodpecker`. Set
   the "Redirect URIs" to `http://192.2.0.13:8000/authorize/` (using the same IP
   address you used to log into Forgejo). Copy the Client ID and secret to a new
   file in `ansible/group_vars/all/woodpecker_oauth_credentials.yml`:

   ```yaml
   ---

   woodpecker_forgejo_client: oauth-client-id-goes-here
   woodpecker_forgejo_secret: oauth-secret-goes-here
   ```

   This file is being ignored by Git, so it will not be checked in by accident.
   In addition, it will be removed once you destroy the Vagrant VMs.

1. Run `vagrant provision` which will start the Ansible provisioning again. This
   time, it should not fail but should setup the Woodpecker server instance
   properly.
1. Open `http://192.2.0.13:8000` and click on `Log in with 192.2.0.13`.
   Authorize "Woodpecker" to access your account by clicking on the **Authorized
   Application** button.
1. You are now logged in on your Woodpecker server.
1. Party!

## Cleaning up

The VMs can be torn down after playing around using `vagrant destroy`. This also
removes the `ansible/group_vars/all/woodpecker_oauth_credentials.yml` file.

## License

BSD-3-Clause

## Author Information

I am Johannes Kastl, reachable via git@johannes-kastl.de
