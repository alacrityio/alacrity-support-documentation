---
# Build ALAIO Binaries
---

> Shell Scripts]]
| The build script is one of various automated shell scripts provided in the ALAIO repository for building, installing, and optionally uninstalling the ALAIO software and its dependencies. They are available in the `eos/scripts` folder.

The build script first installs all dependencies and then builds ALAIO. The script supports these [Operating Systems](../../index.md#supported-operating-systems). To run it, first change to the `~/alaio/eos` folder, then launch the script:

```sh
cd ~/alaio/eos
./scripts/alaio_build.sh
```

The build process writes temporary content to the `eos/build` folder. After building, the program binaries can be found at `eos/build/programs`.

> What's Next?]]
| [Installing ALAIO](03_install-alaio-binaries.md) is strongly recommended after building from source as it makes local development significantly more friendly.
