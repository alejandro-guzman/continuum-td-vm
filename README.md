# continuum-td-vm

## How To Use

Start the guest machine

```bash
cd dir/with/Vagrantfile
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

To provision and target a provision section use the `--provision-with` option.

```bash
vagrant provision --provision-with testdrive
```

The `docker-continuum` directory should be mounted in to the guest machine at
`/docker-continuum`.

You also have access to the directory the Vagrantfile lives in at `/vagrant` on
the guest machine.

All changes done to files in synced folders on the host will appear on the
guest machine as well.

To add a sync folder on the guest.

```bash
# Vagrantfile

...
config.vm.synced_folder "$HOST_DIR", "$GUEST_DIR"
...
```

Or, to open a port on the host.

```bash
# Vagrantfile

...
config.vm.network "forwarded_port", guest: $GUEST_PORT, host: $HOST_PORT
...
```

To completely destroy the VM in VirtualBox.

```bash
vagrant destroy
```

The `Vagrantfile` is the VM definition so you can repeatedly create and destroy
the VM with confidence.

## Summer 2019 Testdrive machine setup

1. On the Windows machine, download Vagrant and VirtualBox, Git for Windows
(with Bash)

2. Git clone or download the zip file of the `docker-continuum` repository to
the local filesystem. Move it to `$HOME`

3. Git clone or download the zip file of this repository (`continuum-tf-vm`) to
the local filesystem. You can leave it in `Downloads`.

4. `cd /dir/with/this/repo` and `vagrant up`. Follow [how to use section](#how-to-use) 
