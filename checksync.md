
# Remote reboot with SIP-NOTIFY in FreePBX

Admin - Config Edit - sip_notify_custom.conf 

    [reload-yealink]
    Event=>check-sync\;reboot=false
    [restart-yealink]
    Event=>check-sync\;reboot=true

Admin - Asterisk CLI - CLI command

    pjsip send notify restart-yealink endpoint 111
