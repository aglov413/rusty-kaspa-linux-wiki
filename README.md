# Ubuntu 22.04 rusty-kaspa build/install 
## If this is a fresh server/VPS, create a user, add user to sudo group, don't use root ;)!!
### The following are the exact steps you can use to build/setup/run your own linux rusty-kaspa node!!

# 1. First update your repo's
```
cd $home
```
```
sudo apt-get update
```
# 2. Install General prerequisites
```
sudo apt install curl git build-essential libssl-dev pkg-config
```
## pink window --> TAB to 'OK' --> ENTER

# 3. Install Protobuf (required for gRPC)
```
sudo apt install protobuf-compiler libprotobuf-dev
```
## pink window --> TAB to 'OK' --> ENTER

# 4. Install the clang toolchain (required for RocksDB and WASM secp256k1 builds)
```
sudo apt-get install clang-format clang-tidy \
clang-tools clang clangd libc++-dev \
libc++1 libc++abi-dev libc++abi1 \
libclang-dev libclang1 liblldb-dev \
libllvm-ocaml-dev libomp-dev libomp5 \
lld lldb llvm-dev llvm-runtime \
llvm python3-clang
```
## pink window --> TAB to 'OK' --> ENTER

# 5. Install the rust toolchain
```
curl --proto '=https' --tlsv1.3 https://sh.rustup.rs -sSf | sh
```
## Proceed with standard installation (default - just press enter)
```
source $HOME/.cargo/env
```
### Verify rust version
```
rustc --version
```

# 6. Update/Upgrade all Ubuntu packages
```
sudo apt update
```
```
sudo apt upgrade
```
## pink window --> TAB to 'OK' --> ENTER
### restart system if New Kernal

# 7. update rust (should be up to date if new install)
```
rustup update
```

# 8. Install wasm-pack
```
cargo install wasm-pack
```

# 9. Install wasm32 target
```
rustup target add wasm32-unknown-unknown
```

# 10. Build & compile rusty-kaspa
```
git clone https://github.com/kaspanet/rusty-kaspa
```
```
cd rusty-kaspa
```
```
cargo build --release --bin kaspad
```
# 11. Setup kaspad service
```
sudo nano /etc/systemd/system/kaspa-mainnet.service
```

## Paste example kaspad.service config below#
```
[Unit]
Description=Kaspa Mainnet

[Service]
User=kaspa
ExecStart=/home/kaspa/rusty-kaspa/target/release/kaspad --utxoindex --rpclisten=0.0.0.0:16110 --rpclisten-borsh=0.0.0.0:17110 --perf-metrics --perf-metrics-interval-sec=1 --yes --disable-upnp
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```
### replace 'kaspa' with your username(User:kaspa and /home/kaspa) and edit necessary --settings 

# 12. enable & start Kaspa-Mainnet service
```
sudo systemctl enable kaspa-mainnet
```
```
sudo systemctl start kaspa-mainnet
```
# 13. view kaspad logs
```
sudo journalctl -u kaspa-mainnet -n 1000 -f
```
