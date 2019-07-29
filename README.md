# continuum-td-vm

## How To Use

Start the guest machine

```bash
cd <dir-with-Vagrantfile>
vagrant up
```

It will prompt you to select the network device, ususally `1` is the right one.

Once the machine has started you can SSH into it.

```bash
vagrant ssh
```

To reload VM changes done to the Vagrantfile (does not run provisioning
section by default, pass `--provision` if you wish to)

```bash
vagrant reload
```

To rerun the provisioning section of the Vagrantfile.

```bash
vagrant provision
```

The `docker-continuum` directory should be mounted in to the guest machine at
`/docker-continuum`.

You also have access the directory the Vagrantfile lives in at `/vagrant` on
the guest machine.

All changes done to files in synced folders on the host will appear on the
guest machine as well.

To add a sync folder on the guest

```bash
# Vagrantfile

...
config.vm.synced_folder "$HOST_DIR", "$GUEST_DIR"
...
```

Or, to open a port on the host

```bash
# Vagrantfile

...
config.vm.network "forwarded_port", guest: $GUEST_PORT, host: $HOST_PORT
...
```
