# postgresql_vagrant_libvirt_ansible

This Vagrant setup creates a VM and configures [a PostgreSQL
server](https://www.postgresql.org/). It also creates a VM and
installs the `psql` client command, that allows a user to connect to a
PostgreSQL database.

Default OS is openSUSE Leap 15.6. Although that can be changed in the
Vagrantfile, please beware that this will break the Ansible provisioning.

Ansible adds a file called `postgresql_snippet.txt` with instructions to the
client VM (to both the `vagrant` and the `root` user's home directory). This
file contains the command required to establish a connection to the database on
the PostgreSQL server VM.

For convenience, the commands are also printed out at the end of the Ansible
provisioning.

## Vagrant

1. You need vagrant obviously. And ansible. And git...
1. Fetch the box, per default this is `opensuse/Leap-15.6.x86_64`, using
   `vagrant box add opensuse/Leap-15.6.x86_64`.
1. Make sure the git submodules are fully working by issuing `git submodule init
   && git submodule update`
1. Run `vagrant up`
1. Log in on the client VM using `vagrant ssh postgresql-client`.
1. Run the first command from the file, that looks something like this:

   ```bash
   psql -h 192.0.2.13 -d example-database -U example-user
   ```

   The IP address will vary in your case, of course.

   Exit by typing `\q` followed by the `ENTER` key.

1. For comparison, run the second command that tries the connection using the
   `postgres` superuser. As this user is not allowed to connect over a network
   connection, this will fail.
1. Party!

## Cleaning up

The VMs can be torn down after playing around using `vagrant destroy`.

## Disabling the Ansible provisioning

In case you do not want Ansible to install PostgreSQL (because you want to
install it yourself), just comment out the following lines in the `Vagrantfile`:

```hcl
    node.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
        ansible.limit = "all"
      ansible.playbook = "ansible/playbook-vagrant.yml"
    end # node.vm.provision
```

You also find all of the playbooks in the `ansible` folder.

## License

BSD-3-Clause

## Author Information

I am Johannes Kastl, reachable via git@johannes-kastl.de
