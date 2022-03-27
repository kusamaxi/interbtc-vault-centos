# interbtc-vault-centos
Instructions for setting up interbtc vault for Rocky linux

```
#install rust
curl https://sh.rustup.rs -sSf | sh
#install development tools
sudo yum -y groupinstall 'Development Tools'
sudo yum -y install git wget curl jq ncurses-devel
# i recommend installing screen for 
wget https://ftp.gnu.org/gnu/screen/screen-4.8.0.tar.gz &&
tar xzf screen-4.8.0.tar.gz && cd screen-4.8.0 &&
./configure --prefix=/usr                     \
            --infodir=/usr/share/info         \
            --mandir=/usr/share/man           \
            --with-socket-dir=/run/screen     \
            --with-pty-group=5                \
            --with-sys-screenrc=/etc/screenrc &&
sed -i -e "s%/usr/local/etc/screenrc%/etc/screenrc%" {etc,doc}/* &&
make &&
make install &&
install -m 644 etc/etcscreenrc /etc/screenrc
```
install subkey to create a keyfile, if you want to use existing wallet you can use subkey inspect 'your mnemonic wallet here' to get secret seed for keyfile.json 
```
cargo install --force subkey --git https://github.com/paritytech/substrate --version 2.0.1 --locked
cargo build -p subkey --release
screen -S subkey
subkey vanity --pattern KINT --network kintsugi --output-type json || jq '{(.accountId): .secretPhrase}' > keyfile.json
# detach by pressing ctrl + a, ctrl + d, to retach write screen -r subkey
```


# install bitcoin node
```
wget https://github.com/bitcoin/bitcoin/archive/refs/tags/v22.0.tar.gz ||
tar xzf bitcoin-22.0-x86_64-linux-gnu.tar.gz ||
sudo install -m 0755 -o root -g root -t /usr/local/bin bitcoin-22.0/bin/*
```
