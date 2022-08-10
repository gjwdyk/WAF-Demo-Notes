# Decrypt TLS TCPDump on Big-IP

```
╔══════════════════════════════════════════════════╗
║   Reference:                                     ║
║   https://support.f5.com/csp/article/K31793632   ║
╚══════════════════════════════════════════════════╝
```

Prerequisite:
- [ ] Big-IP version 15.0.0 or later.



## Enable TLS Session Secret Ethernet Trailers on Big-IP

SSH into the Big-IP and run the following command: `tmsh modify sys db tcpdump.sslprovider value enable` .

```
[admin@ip-10-1-1-245:Active:Standalone] ~ # tmsh list sys db tcpdump.sslprovider value
sys db tcpdump.sslprovider {
    value "disable"
}
[admin@ip-10-1-1-245:Active:Standalone] ~ # tmsh modify sys db tcpdump.sslprovider value enable
[admin@ip-10-1-1-245:Active:Standalone] ~ # tmsh list sys db tcpdump.sslprovider value
sys db tcpdump.sslprovider {
    value "enable"
}
[admin@ip-10-1-1-245:Active:Standalone] ~ #
```



## Capture Traffic with `--f5 ssl` flag

To utilize the enabled functionality above, use `--f5 ssl` flag/option when doing the TCPDump. Example: `tcpdump -vvv -s0 -nni 0.0:nnnp --f5 ssl -w /var/tmp/`/bin/hostname`_`date +%Y%m%d%H%M%S`.pcap` .

```
[admin@ip-10-1-1-245:Active:Standalone] ~ # tcpdump -vvv -s0 -nni 0.0:nnnp --f5 ssl -w /var/tmp/`/bin/hostname`_`date +%Y%m%d%H%M%S`.pcap
tcpdump: WARNING: Using the "ssl" option captures additional information related to the SSL/TLS connections, such as master secrets. This enables some packet capture analysis tools to decrypt the SSL/TLS payload in the captured packets. Use only as needed for troubleshooting purposes, and handle captured data with caution.
tcpdump: listening on 0.0:nnnp, link-type EN10MB (Ethernet), capture size 65535 bytes
Got 9876543210
9876543210 packets received by filter
0 packets dropped by kernel
[admin@ip-10-1-1-245:Active:Standalone] ~ #
```



## Create Pre-Master Secret Log File Using tshark



To copy my files using winscp, I use right botton mouse > copy on file and rigth botton mouse  > paste on my "Downloads" folder.



C:\Program Files\Wireshark>tshark -r C:\Users\HC\Downloads\ip-10-1-1-245.ap-southeast-1.compute.internal_20220810123945.pcap -Y f5ethtrailer.tls.keylog -Tfields -e f5ethtrailer.tls.keylog > C:\Users\HC\Downloads\pre_master_log.pms

C:\Program Files\Wireshark>



Note that you don't use space in the file path and name:
C:\Users\HC\Downloads\ip-10-1-1-245.ap-southeast-1.compute.internal_20220810123945.pcap
C:\Users\HC\Downloads\pre_master_log.pms


C:\Users\HC\Downloads>type pre_master_log.pms
CLIENT_RANDOM e442c47ca13717c4f7217175cf74c474b64e9262cc1ed8f29fb136885ce03dd0 21d0829467bdb91f32bf5c8aa97d6a7f1bf73f208bfdfe80005cd393d9f517f5c2a0a0e14cc93d4588f6dfd4bfd54c05
CLIENT_RANDOM f6c16dc760af06c21e8b020869b537dd5253d23a5b8f1594aaee5841f9450f1b 21d0829467bdb91f32bf5c8aa97d6a7f1bf73f208bfdfe80005cd393d9f517f5c2a0a0e14cc93d4588f6dfd4bfd54c05
CLIENT_RANDOM 4aa1a56d4bd45f738e77c28aeab093daf25cbbbd6bb8e984c2be7d223610ff33 21d0829467bdb91f32bf5c8aa97d6a7f1bf73f208bfdfe80005cd393d9f517f5c2a0a0e14cc93d4588f6dfd4bfd54c05
CLIENT_RANDOM 0b22c0132ecf9efaf658a330b3c5291403c2e4a080d9238ce8d5d24925791083 314d4cb6515c042ca97f7668931d0644ad8a0899dc07c66e43f6993704a226b104b4f349baf4ec8c3e9e47c6a82877d7
CLIENT_RANDOM bd7e553ad72f5649012af52770279d586b079c9a48b9f982f24aa1a43291b731 314d4cb6515c042ca97f7668931d0644ad8a0899dc07c66e43f6993704a226b104b4f349baf4ec8c3e9e47c6a82877d7
CLIENT_RANDOM 928810474f9d6ff8f1fdc5c382050f69ba981e434e782214b71f3160a4786a19 314d4cb6515c042ca97f7668931d0644ad8a0899dc07c66e43f6993704a226b104b4f349baf4ec8c3e9e47c6a82877d7
CLIENT_RANDOM 76eaab5a55092ac94e7a2ccf5813e1f1424222545311eaa79af1b6fde3820be6 7789b547f931ed20bf694f7a6342fdcc711ebf378d5793441a0ab284ad0f876983db11bd98a10f686716945daac21a26
CLIENT_RANDOM b4609d23105ac019b9ca3d9ea044a01f54b4a4eec24f30db0b9d460b70da986c 7789b547f931ed20bf694f7a6342fdcc711ebf378d5793441a0ab284ad0f876983db11bd98a10f686716945daac21a26
CLIENT_RANDOM 221b7e5438d07996adee457ec199b07d8987f1e1286b58df92d78e71123a9777 7789b547f931ed20bf694f7a6342fdcc711ebf378d5793441a0ab284ad0f876983db11bd98a10f686716945daac21a26

C:\Users\HC\Downloads>



Disable TLS Session Secret Ethernet Trailers
--------------------------------------------

[admin@ip-10-1-1-245:Active:Standalone] ~ # tmsh list sys db tcpdump.sslprovider value
sys db tcpdump.sslprovider {
    value "enable"
}
[admin@ip-10-1-1-245:Active:Standalone] ~ # tmsh modify sys db tcpdump.sslprovider value disable
[admin@ip-10-1-1-245:Active:Standalone] ~ # tmsh list sys db tcpdump.sslprovider value
sys db tcpdump.sslprovider {
    value "disable"
}
[admin@ip-10-1-1-245:Active:Standalone] ~ #



Enable F5 Ethernet Trailers for TLS
-----------------------------------

<<< Pictures for Dummies >>>



Create Pre-Master Secret Log File By Hand
-----------------------------------------

<<< Pictures for Dummies >>>




To search decrypted packages faster, try to filter:
tcp.port==443
then look for HTTP protocol (usually coloured differently), and then 














<br><br><br>
```
╔═╦═════════════════╦═╗
╠═╬═════════════════╬═╣
║ ║ End of Document ║ ║
╠═╬═════════════════╬═╣
╚═╩═════════════════╩═╝
```
<br><br><br>

