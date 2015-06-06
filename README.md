# kitematic-box

Install Kitematic in a fresh Mac VM

If you want to get in touch with Kitematic but are afraid of what dependencies are getting installed you can try this Vagrant box.

All you need on your host is Vagrant and VMware Fusion and a Mac OSX Vagrant box. You can create a Vagrant box for yourself with the brilliant https://github.com/boxcutter/osx repo.

And then you can spin up the Vagrant box with the usual

```bash
vagrant up --provider vmware_fusion
```

Kitematic will be installed from source ( https://github.com/kitematic/kitematic ) and you can dig into all details.

## Take a snapshot

If you want to investigate first use experience over and over again, just take a snapshot

```bash
vagrant snap take
```

## Run Kitematic for the first time

To run Kitematic for the first time from source code, open a terminal in the Vagrant box and run these commands

```bash
cd kitematic
npm start
```

If you want to have the same first time experience, just run `vagrant snap rollback` on your host and repeat the commands above.
