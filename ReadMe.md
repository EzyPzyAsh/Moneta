# Moneta

Designed for Linux, compatible with WSL.

## Installing Dependencies

Instructions for Arch:

```bash
yay -S --needed base-devel cmake pkgconf openssl zeromq libsodium unbound \
  libunwind xz readline expat libpgm qt5-tools hidapi libusb protobuf \
  systemd-libs boost boost-libs python ccache doxygen graphviz nettle libevent
```

## Building from Source

```bash
git clone --recurse-submodules <repo-url>
cd Moneta
```

If you already cloned without `--recurse-submodules`, fetch the submodules with:

```bash
git submodule update --init --recursive
```

Build external dependencies (this builds monero-project's static libraries; Moneta's own
CMake build compiles monero-cpp itself, so nothing needs to be linked or copied here):

```bash
cd external/monero-project
make release-static -j$(nproc)   # on WSL, use about half your processors to prevent crash
cd ../../
```

Build Moneta:

```bash
mkdir build
cd build
cmake .. -DMONETA_TUI=ON        # OFF to build without the TUI
make -j$(nproc)                 # same WSL warning as above
```

## Configuration

Pass a config file with `-cfg <path>` or place it at ~/.config/moneta.conf

```ini
wallet=/wallet/dir/walletfile
password=password
daemon=http://xmr-node.cakewallet.com:18081
contact=Name:Address
```

## Usage

```
moneta usage:
  -w <wallet> <password>       wallet file
  -cw <wallet> <password>      create wallet
  -ca <label>                  create subaddress with label
  -a <range>                   address selection: 1 | 2:4 | 2,4
  -la                          list created subaddresses
  -b                           show balance
  -s <amount> <dest address>   send/spend monero
  -sc <amount> <name>          send to a saved contact
  -lc                          list contacts
  -tx <N>                      show tx history (0 for all)
  -c                           clean output (CSV output)
  -d <host:port>               daemon address
  -h, --help                   show help
  -cfg <path>                  config file
  -tui                         launch interactive TUI
```
