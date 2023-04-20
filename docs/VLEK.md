# Setup

1. Get VLEK and VCEK

```
ls $HOME/certs
vcek.pem  vlek.bin  vlek.pem
```

2. Load VLEK into the PSP and check platform status if VLEK was loaded

```
sudo sevtool --vlek_load $HOME/certs/vlek.bin
sudo sevtool --platform_status
api_major:      1
api_minor:      54
platform_state: 1
build:          1
vlek_en:        0x1
```
3. Launch SEV-SNP guest

```
sudo ./launch-qemu.sh -hda ../ubuntu-22.04.qcow2 -vnc 1 -sev-snp
```
4. Connect to guest

```
ssh -p 10022 user@localhost
```

5. Get VCEK signed attestation report and validate

```
sudo ./sev-guest-get-report -k 1 $HOME/certs/guest_report.bin
sudo sevtool --ofolder $HOME/certs --validate_guest_report
```

6. Get VLEK signed attestation report and validate

```
sudo ./sev-guest-get-report -k 2 $HOME/certs/vlek_guest_report.bin
sudo sevtool --ofolder $HOME/certs --validate_guest_report_vlek
```
