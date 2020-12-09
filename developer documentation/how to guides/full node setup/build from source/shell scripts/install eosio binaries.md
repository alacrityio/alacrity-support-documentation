---
# Install ALAIO Binaries
---

## ALAIO install script

For ease of contract development, content can be installed at the `/usr/local` folder using the `alaio_install.sh` script within the `eos/scripts` folder. Adequate permission is required to install on system folders:

```sh
cd ~/alaio/eos
sudo ./scripts/alaio_install.sh
```

## ALAIO manual install

In lieu of the `alaio_install.sh` script, you can install the ALAIO binaries directly by invoking `make install` within the `eos/build` folder. Again, adequate permission is required to install on system folders:

```sh
cd ~/alaio/eos/build
sudo make install
```

> What's Next?]]
| Configure and use [Nodeos](../../../01_nodeos/index.md), or optionally [Test the ALAIO binaries](04_test-alaio-binaries.md).
