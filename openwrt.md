# Open WRT

## "Signature check failed." /var/opkg-lists/chaos_calmer_packages.

This has something to do with where and who compiles and signs firmware packages. I found [this posting](https://community.onion.io/topic/93/signature-check-failed-var-opkg-lists-chaos_calmer_packages/4)


Open `/etc/opkg.conf` delete the line that reads `option check_signature 1`

