## Quick start

```console
$ cd /path/to/svmm
$ mkdir -p "$HOME/.local/bin" && export PATH="$HOME/.local/bin:$PATH"
$ for f in svmm*; do ln -sfr "./$f" "$HOME/.local/bin/$f"; done
# /path/to/svmm/svmm-setup-network
# /path/to/svmm/svmm-dnsmasq
$ svmm run --kvm --cpu host --cores 2 --memory 4G --disk-size 10G --gtk --log-serial --net tap=svmmdynamictap0 --net-dhcp --boot-drive-url http://ftp.altlinux.ru/pub/distributions/ALTLinux/p11/images/cloud/x86_64/alt-server-11.1-p11-cloud-x86_64.qcow2 --guest-agent alt-server-11
$ svmm run --kvm --cpu host --cores 2 --memory 4G --disk-size 10G --gtk --log-serial --net tap=svmmstatictap0 --net-address-cidr 172.16.1.2/24 --net-gateway 172.16.1.1 --uefi --uefi-empty-varstore --tpm --cloud-init-network-config-version 1 --boot-drive-url https://factory.altlinux.space/image/9ed5fecdacb36b5c5427b87d409f1065cfb2df69b0f71c58b868d9d466d8dab3/v11.0-rc.1/alt-orchestra-11.0-rc.1-nocloud-amd64-secureboot.raw.xz --cloud-init-user-data ~/dev/alt-orchestra-clusters/uefi/controlplane.yaml --guest-agent test-nocloud-raw
```

## Dependencies

- `sh`
- `qemu-img` and `qemu-system-x86_64` (uses `qemu-img create -B`, the
  backing-format option name in QEMU > 10.0; with older releases use `-F`)
- `socat`
- `crc32`
- `curl`
- `ss`
- `uuidgen`
- `jq`
- `xz` (for xz-compressed boot images)
- `ssh` and `ssh-keygen`
- `swtpm` (for `--tpm`)
- `cloud-localds`
- `ip` and `nft` (for `svmm-setup-network`)
- `dnsmasq` (for `svmm-dnsmasq`)

## Security note

The default cloud-init user-data sets the root password to "123" and
enables password login. With `--net-tap` the VM is reachable from the
LAN, so use `--cloud-init-user-data` with your own credentials for
anything beyond local testing.
