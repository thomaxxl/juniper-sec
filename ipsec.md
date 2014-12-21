IPSEC Troubleshooting

http://rtoodtoo.net/2013/08/23/jncie-sec-traceoptions-ipsec-troubleshooting/
http://kb.juniper.net/InfoCenter/index?page=content&id=KB10099


Successful Phase 1 Log:
```
[Aug 22 20:40:14]ike_calc_mac: Start, initiator = true, local = false
[Aug 22 20:40:14]ike_st_i_cert: Start
[Aug 22 20:40:14]ike_st_i_private: Start
[Aug 22 20:40:14]ike_st_o_wait_done: Marking for waiting for done
[Aug 22 20:40:14]ike_st_o_all_done: MESSAGE: Phase 1 { 0xe4d65d2e a7bf1c17 - 0x498aaa01 01d0dd21 } / 00000000, version = 1.0, xchg = Identity protect, auth_method = Pre shared keys, Initiator, cipher = 3des-cbc, hash = sha1, prf = hmac-s
ha1
[Aug 22 20:40:14]192.168.179.2:500 (Initiator) <-> 212.45.64.2:500 { e4d65d2e a7bf1c17 - 498aaa01 01d0dd21 [-1] / 0x00000000 } IP; MESSAGE: Phase 1 version = 1.0, auth_method = Pre shared keys, cipher = 3des-cbc, hash = sha1, prf = hmac-
sha
[Aug 22 20:40:14]ike_send_notify: Connected, SA = { e4d65d2e a7bf1c17 - 498aaa01 01d0dd21}, nego = -1
[Aug 22 20:40:14]iked_pm_ike_sa_done: local:192.168.179.2, remote:212.45.64.2 IKEv1
[Aug 22 20:40:14]IKE negotiation done for local:192.168.179.2, remote:212.45.64.2 IKEv1 with status: Error ok
[Aug 22 20:40:14]Added (spi=0xaebf2827, protocol=0) entry to the spi table
[Aug 22 20:40:14]Added (spi=0x3037b766, protocol=0) entry to the spi table
[Aug 22 20:40:14]ssh_ike_connect_ipsec: Start, remote_name = :500, flags = 00000000
[Aug 22 20:40:14]ike_sa_find_ip_port: Remote = all:500, Found SA = { e4d65d2e a7bf1c17 - 498aaa01 01d0dd21}
[Aug 22 20:40:14]ike_alloc_negotiation: Start, SA = { e4d65d2e a7bf1c17 - 498aaa01 01d0dd21}
[Aug 22 20:40:14]ssh_ike_connect_ipsec: SA = { e4d65d2e a7bf1c17 - 498aaa01 01d0dd21}, nego = 0
[Aug 22 20:40:14]ike_init_qm_negotiation: Start, initiator = 1, message_id = 5aa9f0f2
[Aug 22 20:40:14]ike_st_o_qm_hash_1: Start
[Aug 22 20:40:14]ike_st_o_qm_sa_proposals: Start
[Aug 22 20:40:14]ike_st_o_qm_nonce: Start
[Aug 22 20:40:14]ike_policy_reply_qm_nonce_data_len: Start
[Aug 22 20:40:14]ike_st_o_qm_optional_ke: Start
[Aug 22 20:40:14]ike_st_o_qm_optional_ids: Start
[Aug 22 20:40:14]ike_st_qm_optional_id: Start
[Aug 22 20:40:14]ike_st_qm_optional_id: Start
[Aug 22 20:40:14]ike_st_o_private: Start
[Aug 22 20:40:14]Construction NHTB payload for  local:192.168.179.2, remote:212.45.64.2 IKEv1 P1 SA index 8160872 sa-cfg vpn-hub
```

Successful Phase 2 Log:
```
[Aug 22 20:40:14]ike_policy_reply_private_payload_out: Start
[Aug 22 20:40:14]ike_st_o_encrypt: Marking encryption for packet
[Aug 22 20:40:14]<none>:500 (Initiator) <-> 212.45.64.2:500 { e4d65d2e a7bf1c17 - 498aaa01 01d0dd21 [0] / 0x5aa9f0f2 } QM; MESSAGE: Phase 2 connection succeeded, No PFS, group = 0
[Aug 22 20:40:14]ike_qm_call_callback: MESSAGE: Phase 2 connection succeeded, No PFS, group = 0
[Aug 22 20:40:14]<none>:500 (Initiator) <-> 212.45.64.2:500 { e4d65d2e a7bf1c17 - 498aaa01 01d0dd21 [0] / 0x5aa9f0f2 } QM; MESSAGE: SA[0][0] = ESP 3des, life = 0 kB/3600 sec, group = 0, tunnel, hmac-sha1-96, Extended seq not used, key le
n =
[Aug 22 20:40:14]ike_qm_call_callback: MESSAGE: SA[0][0] = ESP 3des, life = 0 kB/3600 sec, group = 0, tunnel, hmac-sha1-96, Extended seq not used, key len = 0, key rounds = 0
[Aug 22 20:40:14]iked_pm_ipsec_sa_install: local:192.168.179.2, remote:212.45.64.2  IKEv1 for SA-CFG vpn-hub
[Aug 22 20:40:14]Added (spi=0xaebf2827, protocol=ESP dst=192.168.179.2) entry to the peer hash table
[Aug 22 20:40:14]Added (spi=0xdfa0760c, protocol=ESP dst=212.45.64.2) entry to the peer hash table
[Aug 22 20:40:14]Hardlife timer started for inbound vpn-hub with 3600 seconds/0 kilobytes
[Aug 22 20:40:14]Softlife timer started for inbound vpn-hub with 2978 seconds/0 kilobytes
[Aug 22 20:40:14]In iked_ipsec_sa_pair_add Adding GENCFG msg with key; Tunnel = 131073;SPI-In = 0xaebf2827
[Aug 22 20:40:14]Added dependency on SA config blob with tunnelid = 131073
[Aug 22 20:40:14]Successfully added ipsec SA PAIR
[Aug 22 20:40:14]ike_st_o_qm_wait_done: Marking for waiting for done
[Aug 22 20:40:14]ike_encode_packet: Start, SA = { 0xe4d65d2e a7bf1c17 - 498aaa01 01d0dd21 } / 5aa9f0f2, nego = 0
```

Debugging:
set system syslog file kmd-logs match KMD


Phase 1 error messages

iked_ts_config_template_clean_up_all_gt_gi Failed to find sa_cfg xxx

KMD_INTERNAL_ERROR: iked_ifstate_eoc_handler: EOC msg received


iked_process_ifl_ext_add: ifl tunnel-id lookup failed for ifl ge-0/0/1.0

KMD_INTERNAL_ERROR: iked_update_ha_blob: Error updating blob with type = 80000025. Error: No such file or directory


Phase 2 error messages

---

ike_get_sa: Invalid cookie, no sa found, SA
