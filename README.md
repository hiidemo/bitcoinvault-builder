# Docker

## General Usage

The dockerfiles in this directory can be used to run `depends` builds for various `HOST`s.
The images will contain all the dependencies required for cross compiling.

For example, to build the Debian image and use it for a build targeting the `Linux amd64` host:

```bash
# Build container
docker build --pull --no-cache -t bvault-builder .

# Run with a Bash shell
docker run -it --name bvault-builder --workdir /bitcoinvault bvault-builder /bin/bash

# Inside the container: build depends for RISCV-64 bit, skipping Qt packages
make HOST=x86_64-pc-linux-gnu NO_QT=1 -C depends/ -j5
./autogen.sh
./configure --prefix=/bitcoinvault/depends/x86_64-pc-linux-gnu
make -j5
```

### macOS SDK
Cross compiling for macOS requires the macOSX10.14 SDK.
There are [notes in the bitcoinvault/bitcoinvault repo](https://github.com/bitcoinvault/bitcoinvault/tree/master/contrib/macdeploy#sdk-extraction) about how to create it.

You can copy it into a container with:
```bash
docker cp path/to/MacOSX10.14.sdk.tar.gz debian-depends:bitcoinvault/depends/SDKs
```

### Platform Triplets
Common `host-platform-triplets` for cross compilation are:

- `x86_64-w64-mingw32` for Win64
- `x86_64-apple-darwin16` for macOS
- `i686-pc-linux-gnu` for Linux 32 bit
- `x86_64-pc-linux-gnu` for Linux 64 bit
- `arm-linux-gnueabihf` for Linux ARM 32 bit
- `aarch64-linux-gnu` for Linux ARM 64 bit
- `riscv32-linux-gnu` for Linux RISC-V 32 bit
- `riscv64-linux-gnu` for Linux RISC-V 64 bit

You can read more about host target triplets in `autoconf` [here](https://www.gnu.org/software/autoconf/manual/autoconf-2.69/html_node/Specifying-Target-Triplets.html).
