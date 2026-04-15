# ProcPS

The procps module lists processes, gets process information or sends signals to 
running processes.

## Endpoint Format

With the exception of the list endpoint, each endpoint is specified as
`procps/{pid}/function/[parameter]`. or `procps/list`

* [list](#list) - Get a list of processes.
* [info](#info) - Get info for a process.
* [kill](#kill) - Send a signal (kill) a process.

### list

* Role required: process_view
* Usage: `procps/list`

Returns a summary list of all procedses active at the tim of call.

<details>
<summary><b>Example Process List</b></summary>

```json
{
    "1": {
        "status": "sleeping",
        "name": "systemd",
        "pid": 1,
        "username": "root"
    },
    "2": {
        "status": "sleeping",
        "name": "kthreadd",
        "pid": 2,
        "username": "root"
    },
    "3": {
        "status": "sleeping",
        "name": "pool_workqueue_release",
        "pid": 3,
        "username": "root"
    },
    "4": {
        "status": "idle",
        "name": "kworker/R-kvfree_rcu_reclaim",
        "pid": 4,
        "username": "root"
    },
    "5": {
        "status": "idle",
        "name": "kworker/R-rcu_gp",
        "pid": 5,
        "username": "root"
    },
    "6": {
        "status": "idle",
        "name": "kworker/R-sync_wq",
        "pid": 6,
        "username": "root"
    },
    "7": {
        "status": "idle",
        "name": "kworker/R-slub_flushwq",
        "pid": 7,
        "username": "root"
    },
    "8": {
        "status": "idle",
        "name": "kworker/R-netns",
        "pid": 8,
        "username": "root"
    },
    "10": {
        "status": "idle",
        "name": "kworker/0:0H-events_highpri",
        "pid": 10,
        "username": "root"
    },
    "12": {
        "status": "idle",
        "name": "kworker/u16:0-ipv6_addrconf",
        "pid": 12,
        "username": "root"
    },
    "13": {
        "status": "idle",
        "name": "kworker/R-mm_percpu_wq",
        "pid": 13,
        "username": "root"
    },
    "14": {
        "status": "idle",
        "name": "rcu_tasks_kthread",
        "pid": 14,
        "username": "root"
    },
    "15": {
        "status": "idle",
        "name": "rcu_tasks_rude_kthread",
        "pid": 15,
        "username": "root"
    },
    "16": {
        "status": "idle",
        "name": "rcu_tasks_trace_kthread",
        "pid": 16,
        "username": "root"
    },
    "17": {
        "status": "sleeping",
        "name": "ksoftirqd/0",
        "pid": 17,
        "username": "root"
    },
    "18": {
        "status": "idle",
        "name": "rcu_preempt",
        "pid": 18,
        "username": "root"
    },
    "19": {
        "status": "sleeping",
        "name": "rcu_exp_par_gp_kthread_worker/0",
        "pid": 19,
        "username": "root"
    },
    "20": {
        "status": "sleeping",
        "name": "rcu_exp_gp_kthread_worker",
        "pid": 20,
        "username": "root"
    },
    "21": {
        "status": "sleeping",
        "name": "migration/0",
        "pid": 21,
        "username": "root"
    },
    "22": {
        "status": "sleeping",
        "name": "cpuhp/0",
        "pid": 22,
        "username": "root"
    },
    "23": {
        "status": "sleeping",
        "name": "cpuhp/1",
        "pid": 23,
        "username": "root"
    },
    "24": {
        "status": "sleeping",
        "name": "migration/1",
        "pid": 24,
        "username": "root"
    },
    "25": {
        "status": "sleeping",
        "name": "ksoftirqd/1",
        "pid": 25,
        "username": "root"
    },
    "27": {
        "status": "idle",
        "name": "kworker/1:0H-events_highpri",
        "pid": 27,
        "username": "root"
    },
    "28": {
        "status": "sleeping",
        "name": "cpuhp/2",
        "pid": 28,
        "username": "root"
    },
    "29": {
        "status": "sleeping",
        "name": "migration/2",
        "pid": 29,
        "username": "root"
    },
    "30": {
        "status": "sleeping",
        "name": "ksoftirqd/2",
        "pid": 30,
        "username": "root"
    },
    "32": {
        "status": "idle",
        "name": "kworker/2:0H-events_highpri",
        "pid": 32,
        "username": "root"
    },
    "33": {
        "status": "sleeping",
        "name": "cpuhp/3",
        "pid": 33,
        "username": "root"
    },
    "34": {
        "status": "sleeping",
        "name": "migration/3",
        "pid": 34,
        "username": "root"
    },
    "35": {
        "status": "sleeping",
        "name": "ksoftirqd/3",
        "pid": 35,
        "username": "root"
    },
    "37": {
        "status": "idle",
        "name": "kworker/3:0H-events_highpri",
        "pid": 37,
        "username": "root"
    },
    "42": {
        "status": "sleeping",
        "name": "kdevtmpfs",
        "pid": 42,
        "username": "root"
    },
    "43": {
        "status": "idle",
        "name": "kworker/R-inet_frag_wq",
        "pid": 43,
        "username": "root"
    },
    "44": {
        "status": "sleeping",
        "name": "kauditd",
        "pid": 44,
        "username": "root"
    },
    "45": {
        "status": "sleeping",
        "name": "khungtaskd",
        "pid": 45,
        "username": "root"
    },
    "46": {
        "status": "sleeping",
        "name": "oom_reaper",
        "pid": 46,
        "username": "root"
    },
    "48": {
        "status": "idle",
        "name": "kworker/R-writeback",
        "pid": 48,
        "username": "root"
    },
    "49": {
        "status": "sleeping",
        "name": "kcompactd0",
        "pid": 49,
        "username": "root"
    },
    "50": {
        "status": "sleeping",
        "name": "kcompactd1",
        "pid": 50,
        "username": "root"
    },
    "51": {
        "status": "idle",
        "name": "kworker/R-kintegrityd",
        "pid": 51,
        "username": "root"
    },
    "52": {
        "status": "idle",
        "name": "kworker/R-kblockd",
        "pid": 52,
        "username": "root"
    },
    "53": {
        "status": "idle",
        "name": "kworker/R-blkcg_punt_bio",
        "pid": 53,
        "username": "root"
    },
    "57": {
        "status": "sleeping",
        "name": "watchdogd",
        "pid": 57,
        "username": "root"
    },
    "58": {
        "status": "idle",
        "name": "kworker/R-quota_events_unbound",
        "pid": 58,
        "username": "root"
    },
    "61": {
        "status": "idle",
        "name": "kworker/2:1H-kblockd",
        "pid": 61,
        "username": "root"
    },
    "62": {
        "status": "idle",
        "name": "kworker/R-rpciod",
        "pid": 62,
        "username": "root"
    },
    "63": {
        "status": "idle",
        "name": "kworker/R-xprtiod",
        "pid": 63,
        "username": "root"
    },
    "65": {
        "status": "sleeping",
        "name": "kswapd0",
        "pid": 65,
        "username": "root"
    },
    "66": {
        "status": "sleeping",
        "name": "kswapd1",
        "pid": 66,
        "username": "root"
    },
    "67": {
        "status": "idle",
        "name": "kworker/R-nfsiod",
        "pid": 67,
        "username": "root"
    },
    "68": {
        "status": "idle",
        "name": "kworker/R-kthrotld",
        "pid": 68,
        "username": "root"
    },
    "76": {
        "status": "sleeping",
        "name": "irq/27-aerdrv",
        "pid": 76,
        "username": "root"
    },
    "77": {
        "status": "sleeping",
        "name": "hwrng",
        "pid": 77,
        "username": "root"
    },
    "78": {
        "status": "idle",
        "name": "kworker/R-iscsi_conn_cleanup",
        "pid": 78,
        "username": "root"
    },
    "79": {
        "status": "idle",
        "name": "kworker/R-nvme-wq",
        "pid": 79,
        "username": "root"
    },
    "80": {
        "status": "idle",
        "name": "kworker/R-nvme-reset-wq",
        "pid": 80,
        "username": "root"
    },
    "81": {
        "status": "idle",
        "name": "kworker/R-nvme-delete-wq",
        "pid": 81,
        "username": "root"
    },
    "84": {
        "status": "idle",
        "name": "kworker/R-DWC Notification WorkQ",
        "pid": 84,
        "username": "root"
    },
    "85": {
        "status": "idle",
        "name": "kworker/R-uas",
        "pid": 85,
        "username": "root"
    },
    "86": {
        "status": "sleeping",
        "name": "vchiq-slot/0",
        "pid": 86,
        "username": "root"
    },
    "87": {
        "status": "sleeping",
        "name": "vchiq-recy/0",
        "pid": 87,
        "username": "root"
    },
    "88": {
        "status": "sleeping",
        "name": "vchiq-sync/0",
        "pid": 88,
        "username": "root"
    },
    "89": {
        "status": "idle",
        "name": "kworker/0:1H-kblockd",
        "pid": 89,
        "username": "root"
    },
    "90": {
        "status": "idle",
        "name": "kworker/u21:0-brcmf_wq/mmc1:0001:1",
        "pid": 90,
        "username": "root"
    },
    "91": {
        "status": "idle",
        "name": "kworker/u22:0",
        "pid": 91,
        "username": "root"
    },
    "92": {
        "status": "idle",
        "name": "kworker/u23:0",
        "pid": 92,
        "username": "root"
    },
    "93": {
        "status": "idle",
        "name": "kworker/u24:0",
        "pid": 93,
        "username": "root"
    },
    "94": {
        "status": "idle",
        "name": "kworker/u25:0",
        "pid": 94,
        "username": "root"
    },
    "95": {
        "status": "idle",
        "name": "kworker/R-sdhci",
        "pid": 95,
        "username": "root"
    },
    "96": {
        "status": "sleeping",
        "name": "irq/40-mmc0",
        "pid": 96,
        "username": "root"
    },
    "123": {
        "status": "idle",
        "name": "kworker/3:1H-kblockd",
        "pid": 123,
        "username": "root"
    },
    "127": {
        "status": "idle",
        "name": "kworker/1:1H-kblockd",
        "pid": 127,
        "username": "root"
    },
    "155": {
        "status": "idle",
        "name": "kworker/R-v3d_bin",
        "pid": 155,
        "username": "root"
    },
    "156": {
        "status": "idle",
        "name": "kworker/R-v3d_render",
        "pid": 156,
        "username": "root"
    },
    "157": {
        "status": "idle",
        "name": "kworker/R-v3d_tfu",
        "pid": 157,
        "username": "root"
    },
    "158": {
        "status": "idle",
        "name": "kworker/R-v3d_csd",
        "pid": 158,
        "username": "root"
    },
    "159": {
        "status": "idle",
        "name": "kworker/R-v3d_cache_clean",
        "pid": 159,
        "username": "root"
    },
    "160": {
        "status": "idle",
        "name": "kworker/R-v3d_cpu",
        "pid": 160,
        "username": "root"
    },
    "162": {
        "status": "sleeping",
        "name": "irq/43-vc4 hdmi hpd connected",
        "pid": 162,
        "username": "root"
    },
    "163": {
        "status": "sleeping",
        "name": "irq/44-vc4 hdmi hpd disconnected",
        "pid": 163,
        "username": "root"
    },
    "164": {
        "status": "sleeping",
        "name": "cec-vc4-hdmi-0",
        "pid": 164,
        "username": "root"
    },
    "165": {
        "status": "sleeping",
        "name": "irq/45-vc4 hdmi cec rx",
        "pid": 165,
        "username": "root"
    },
    "166": {
        "status": "sleeping",
        "name": "irq/46-vc4 hdmi cec tx",
        "pid": 166,
        "username": "root"
    },
    "167": {
        "status": "sleeping",
        "name": "irq/47-vc4 hdmi hpd connected",
        "pid": 167,
        "username": "root"
    },
    "168": {
        "status": "sleeping",
        "name": "irq/48-vc4 hdmi hpd disconnected",
        "pid": 168,
        "username": "root"
    },
    "169": {
        "status": "sleeping",
        "name": "cec-vc4-hdmi-1",
        "pid": 169,
        "username": "root"
    },
    "170": {
        "status": "sleeping",
        "name": "irq/49-vc4 hdmi cec rx",
        "pid": 170,
        "username": "root"
    },
    "171": {
        "status": "sleeping",
        "name": "irq/50-vc4 hdmi cec tx",
        "pid": 171,
        "username": "root"
    },
    "172": {
        "status": "sleeping",
        "name": "card1-crtc0",
        "pid": 172,
        "username": "root"
    },
    "173": {
        "status": "sleeping",
        "name": "card1-crtc1",
        "pid": 173,
        "username": "root"
    },
    "174": {
        "status": "sleeping",
        "name": "card1-crtc2",
        "pid": 174,
        "username": "root"
    },
    "175": {
        "status": "sleeping",
        "name": "card1-crtc3",
        "pid": 175,
        "username": "root"
    },
    "176": {
        "status": "sleeping",
        "name": "card1-crtc4",
        "pid": 176,
        "username": "root"
    },
    "177": {
        "status": "sleeping",
        "name": "card1-crtc5",
        "pid": 177,
        "username": "root"
    },
    "180": {
        "status": "sleeping",
        "name": "scsi_eh_0",
        "pid": 180,
        "username": "root"
    },
    "181": {
        "status": "idle",
        "name": "kworker/R-scsi_tmf_0",
        "pid": 181,
        "username": "root"
    },
    "182": {
        "status": "sleeping",
        "name": "usb-storage",
        "pid": 182,
        "username": "root"
    },
    "220": {
        "status": "sleeping",
        "name": "jbd2/sda2-8",
        "pid": 220,
        "username": "root"
    },
    "221": {
        "status": "idle",
        "name": "kworker/R-ext4-rsv-conversion",
        "pid": 221,
        "username": "root"
    },
    "241": {
        "status": "idle",
        "name": "kworker/R-mld",
        "pid": 241,
        "username": "root"
    },
    "242": {
        "status": "idle",
        "name": "kworker/R-ipv6_addrconf",
        "pid": 242,
        "username": "root"
    },
    "243": {
        "status": "idle",
        "name": "kworker/u16:1",
        "pid": 243,
        "username": "root"
    },
    "344": {
        "status": "sleeping",
        "name": "systemd-journald",
        "pid": 344,
        "username": "root"
    },
    "373": {
        "status": "sleeping",
        "name": "systemd-timesyncd",
        "pid": 373,
        "username": "systemd-timesync"
    },
    "417": {
        "status": "sleeping",
        "name": "systemd-udevd",
        "pid": 417,
        "username": "root"
    },
    "490": {
        "status": "sleeping",
        "name": "vchiq-keep/0",
        "pid": 490,
        "username": "root"
    },
    "550": {
        "status": "idle",
        "name": "kworker/R-cfg80211",
        "pid": 550,
        "username": "root"
    },
    "551": {
        "status": "sleeping",
        "name": "SMIO",
        "pid": 551,
        "username": "root"
    },
    "553": {
        "status": "idle",
        "name": "kworker/R-mmal-vchiq",
        "pid": 553,
        "username": "root"
    },
    "554": {
        "status": "idle",
        "name": "kworker/R-mmal-vchiq",
        "pid": 554,
        "username": "root"
    },
    "557": {
        "status": "sleeping",
        "name": "irq/56-feb00000.codec",
        "pid": 557,
        "username": "root"
    },
    "561": {
        "status": "idle",
        "name": "kworker/R-brcmf_wq/mmc1:0001:1",
        "pid": 561,
        "username": "root"
    },
    "565": {
        "status": "sleeping",
        "name": "brcmf_wdog/mmc1:0001:1",
        "pid": 565,
        "username": "root"
    },
    "577": {
        "status": "idle",
        "name": "kworker/R-mmal-vchiq",
        "pid": 577,
        "username": "root"
    },
    "578": {
        "status": "idle",
        "name": "kworker/R-mmal-vchiq",
        "pid": 578,
        "username": "root"
    },
    "579": {
        "status": "idle",
        "name": "kworker/R-mmal-vchiq",
        "pid": 579,
        "username": "root"
    },
    "580": {
        "status": "idle",
        "name": "kworker/R-mmal-vchiq",
        "pid": 580,
        "username": "root"
    },
    "581": {
        "status": "idle",
        "name": "kworker/R-mmal-vchiq",
        "pid": 581,
        "username": "root"
    },
    "593": {
        "status": "idle",
        "name": "kworker/u21:1-hci0",
        "pid": 593,
        "username": "root"
    },
    "622": {
        "status": "sleeping",
        "name": "rpcbind",
        "pid": 622,
        "username": "_rpc"
    },
    "625": {
        "status": "sleeping",
        "name": "blkmapd",
        "pid": 625,
        "username": "root"
    },
    "651": {
        "status": "sleeping",
        "name": "avahi-daemon",
        "pid": 651,
        "username": "avahi"
    },
    "652": {
        "status": "sleeping",
        "name": "bluetoothd",
        "pid": 652,
        "username": "root"
    },
    "653": {
        "status": "sleeping",
        "name": "cron",
        "pid": 653,
        "username": "root"
    },
    "654": {
        "status": "sleeping",
        "name": "dbus-daemon",
        "pid": 654,
        "username": "messagebus"
    },
    "658": {
        "status": "sleeping",
        "name": "polkitd",
        "pid": 658,
        "username": "polkitd"
    },
    "659": {
        "status": "sleeping",
        "name": "avahi-daemon",
        "pid": 659,
        "username": "avahi"
    },
    "663": {
        "status": "sleeping",
        "name": "systemd-logind",
        "pid": 663,
        "username": "root"
    },
    "741": {
        "status": "sleeping",
        "name": "NetworkManager",
        "pid": 741,
        "username": "root"
    },
    "743": {
        "status": "sleeping",
        "name": "wpa_supplicant",
        "pid": 743,
        "username": "root"
    },
    "778": {
        "status": "sleeping",
        "name": "ModemManager",
        "pid": 778,
        "username": "root"
    },
    "806": {
        "status": "sleeping",
        "name": "krfcommd",
        "pid": 806,
        "username": "root"
    },
    "917": {
        "status": "sleeping",
        "name": "tailscaled",
        "pid": 917,
        "username": "root"
    },
    "931": {
        "status": "sleeping",
        "name": "sshd",
        "pid": 931,
        "username": "root"
    },
    "934": {
        "status": "sleeping",
        "name": "login",
        "pid": 934,
        "username": "root"
    },
    "971": {
        "status": "sleeping",
        "name": "rapi-server",
        "pid": 971,
        "username": "root"
    },
    "982": {
        "status": "running",
        "name": "pserve",
        "pid": 982,
        "username": "root"
    },
    "996": {
        "status": "sleeping",
        "name": "systemd",
        "pid": 996,
        "username": "nicci"
    },
    "998": {
        "status": "sleeping",
        "name": "(sd-pam)",
        "pid": 998,
        "username": "nicci"
    },
    "1019": {
        "status": "sleeping",
        "name": "dbus-daemon",
        "pid": 1019,
        "username": "nicci"
    },
    "1020": {
        "status": "sleeping",
        "name": "pipewire",
        "pid": 1020,
        "username": "nicci"
    },
    "1024": {
        "status": "sleeping",
        "name": "zsh",
        "pid": 1024,
        "username": "nicci"
    },
    "1025": {
        "status": "sleeping",
        "name": "pipewire",
        "pid": 1025,
        "username": "nicci"
    },
    "1029": {
        "status": "sleeping",
        "name": "wireplumber",
        "pid": 1029,
        "username": "nicci"
    },
    "1030": {
        "status": "sleeping",
        "name": "pipewire-pulse",
        "pid": 1030,
        "username": "nicci"
    },
    "1034": {
        "status": "sleeping",
        "name": "mpris-proxy",
        "pid": 1034,
        "username": "nicci"
    },
    "1118": {
        "status": "sleeping",
        "name": "zsh",
        "pid": 1118,
        "username": "nicci"
    },
    "1119": {
        "status": "sleeping",
        "name": "zsh",
        "pid": 1119,
        "username": "nicci"
    },
    "1122": {
        "status": "sleeping",
        "name": "zsh",
        "pid": 1122,
        "username": "nicci"
    },
    "1127": {
        "status": "sleeping",
        "name": "gitstatusd-linux-aarch64",
        "pid": 1127,
        "username": "nicci"
    },
    "1138": {
        "status": "sleeping",
        "name": "colord",
        "pid": 1138,
        "username": "colord"
    },
    "1142": {
        "status": "sleeping",
        "name": "xdg-permission-store",
        "pid": 1142,
        "username": "nicci"
    },
    "55229": {
        "status": "sleeping",
        "name": "cupsd",
        "pid": 55229,
        "username": "root"
    },
    "55230": {
        "status": "sleeping",
        "name": "cups-browsed",
        "pid": 55230,
        "username": "root"
    },
    "55237": {
        "status": "sleeping",
        "name": "dbus",
        "pid": 55237,
        "username": "lp"
    },
    "56597": {
        "status": "idle",
        "name": "kworker/u18:0-kvfree_rcu_reclaim",
        "pid": 56597,
        "username": "root"
    },
    "58942": {
        "status": "idle",
        "name": "kworker/u19:3-kvfree_rcu_reclaim",
        "pid": 58942,
        "username": "root"
    },
    "59072": {
        "status": "idle",
        "name": "kworker/u19:1-kvfree_rcu_reclaim",
        "pid": 59072,
        "username": "root"
    },
    "59194": {
        "status": "idle",
        "name": "kworker/u19:0-flush-8:0",
        "pid": 59194,
        "username": "root"
    },
    "59420": {
        "status": "idle",
        "name": "kworker/u17:2-flush-8:0",
        "pid": 59420,
        "username": "root"
    },
    "59594": {
        "status": "idle",
        "name": "kworker/u17:1-kvfree_rcu_reclaim",
        "pid": 59594,
        "username": "root"
    },
    "59720": {
        "status": "idle",
        "name": "kworker/u18:1-kvfree_rcu_reclaim",
        "pid": 59720,
        "username": "root"
    },
    "59722": {
        "status": "idle",
        "name": "kworker/u17:0-kvfree_rcu_reclaim",
        "pid": 59722,
        "username": "root"
    },
    "59832": {
        "status": "idle",
        "name": "kworker/u20:3-kvfree_rcu_reclaim",
        "pid": 59832,
        "username": "root"
    },
    "60136": {
        "status": "idle",
        "name": "kworker/0:2-events",
        "pid": 60136,
        "username": "root"
    },
    "60256": {
        "status": "idle",
        "name": "kworker/u20:1-flush-8:0",
        "pid": 60256,
        "username": "root"
    },
    "60484": {
        "status": "idle",
        "name": "kworker/0:3-events",
        "pid": 60484,
        "username": "root"
    },
    "60489": {
        "status": "idle",
        "name": "kworker/0:4-events",
        "pid": 60489,
        "username": "root"
    },
    "60494": {
        "status": "idle",
        "name": "kworker/3:0-events",
        "pid": 60494,
        "username": "root"
    },
    "60495": {
        "status": "idle",
        "name": "kworker/u20:0-flush-8:0",
        "pid": 60495,
        "username": "root"
    },
    "60496": {
        "status": "idle",
        "name": "kworker/2:1-events",
        "pid": 60496,
        "username": "root"
    },
    "60497": {
        "status": "idle",
        "name": "kworker/1:0-events",
        "pid": 60497,
        "username": "root"
    },
    "60498": {
        "status": "idle",
        "name": "kworker/3:1-events",
        "pid": 60498,
        "username": "root"
    },
    "60543": {
        "status": "idle",
        "name": "kworker/2:3-events_power_efficient",
        "pid": 60543,
        "username": "root"
    },
    "60548": {
        "status": "idle",
        "name": "kworker/1:1-events",
        "pid": 60548,
        "username": "root"
    },
    "60549": {
        "status": "idle",
        "name": "kworker/3:2-events",
        "pid": 60549,
        "username": "root"
    },
    "60551": {
        "status": "idle",
        "name": "kworker/0:0-events",
        "pid": 60551,
        "username": "root"
    },
    "60552": {
        "status": "idle",
        "name": "kworker/1:2-events",
        "pid": 60552,
        "username": "root"
    },
    "60553": {
        "status": "idle",
        "name": "kworker/2:0-events",
        "pid": 60553,
        "username": "root"
    },
    "60554": {
        "status": "idle",
        "name": "kworker/2:2-mm_percpu_wq",
        "pid": 60554,
        "username": "root"
    },
    "_uuid": "f9c1e3b9-2e40-4c9d-b08b-b0ef4189b13a"
}

```

</details>


### info

* Role required: process_view
* Usage: `procps/{pid}/info`

Returns details about a process.

</details>

<details>
<summary><b>Example Process Details</b></summary>

```json
{
    "exe": "/usr/sbin/avahi-daemon",
    "nice": 0,
    "environ": {},
    "net_connections": [],
    "open_files": [],
    "cpu_percent": 0.0,
    "memory_full_info": [
        1671168,
        5791744,
        1421312,
        135168,
        0,
        413696,
        0,
        94208,
        347136,
        0
    ],
    "num_ctx_switches": [
        3,
        0
    ],
    "num_fds": 6,
    "memory_maps": [
        [
            "/usr/sbin/avahi-daemon",
            143360,
            147456,
            73728,
            131072,
            8192,
            0,
            4096,
            143360,
            12288,
            0
        ],
        [
            "[heap]",
            16384,
            135168,
            16384,
            0,
            0,
            0,
            16384,
            8192,
            16384,
            0
        ],
        [
            "/usr/lib/aarch64-linux-gnu/libm.so.6",
            8192,
            659456,
            4096,
            0,
            8192,
            0,
            0,
            0,
            8192,
            0
        ],
        [
            "/usr/lib/aarch64-linux-gnu/libc.so.6",
            1056768,
            1777664,
            41984,
            1036288,
            12288,
            0,
            8192,
            1048576,
            20480,
            0
        ],
        [
            "[anon]",
            53248,
            90112,
            45056,
            0,
            16384,
            0,
            36864,
            20480,
            53248,
            0
        ],
        [
            "/usr/lib/aarch64-linux-gnu/libdbus-1.so.3.38.3",
            12288,
            462848,
            8192,
            0,
            8192,
            0,
            4096,
            0,
            12288,
            0
        ],
        [
            "/usr/lib/aarch64-linux-gnu/libcap.so.2.75",
            45056,
            135168,
            5120,
            36864,
            8192,
            0,
            0,
            45056,
            8192,
            0
        ],
        [
            "/usr/lib/aarch64-linux-gnu/libsystemd.so.0.40.0",
            126976,
            1187840,
            37888,
            65536,
            57344,
            0,
            4096,
            69632,
            61440,
            0
        ],
        [
            "/usr/lib/aarch64-linux-gnu/libexpat.so.1.10.2",
            12288,
            266240,
            6144,
            0,
            12288,
            0,
            0,
            0,
            12288,
            0
        ],
        [
            "/usr/lib/aarch64-linux-gnu/libdaemon.so.0.5.0",
            32768,
            135168,
            18432,
            24576,
            4096,
            0,
            4096,
            28672,
            8192,
            0
        ],
        [
            "/usr/lib/aarch64-linux-gnu/libavahi-core.so.7.1.0",
            135168,
            331776,
            67584,
            126976,
            8192,
            0,
            0,
            135168,
            8192,
            0
        ],
        [
            "/usr/lib/aarch64-linux-gnu/libavahi-common.so.3.5.4",
            8192,
            135168,
            6144,
            0,
            4096,
            0,
            4096,
            0,
            8192,
            0
        ],
        [
            "/usr/lib/aarch64-linux-gnu/ld-linux-aarch64.so.1",
            12288,
            176128,
            8192,
            0,
            8192,
            0,
            4096,
            8192,
            12288,
            0
        ],
        [
            "[vvar]",
            0,
            8192,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0
        ],
        [
            "[vdso]",
            0,
            8192,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0
        ],
        [
            "[stack]",
            8192,
            135168,
            8192,
            0,
            0,
            0,
            8192,
            8192,
            8192,
            0
        ]
    ],
    "cpu_affinity": [
        0,
        1,
        2,
        3
    ],
    "gids": [
        104,
        104,
        104
    ],
    "cpu_times": [
        0.0,
        0.0,
        0.0,
        0.0,
        0.0
    ],
    "io_counters": [
        1,
        0,
        0,
        0,
        1,
        0
    ],
    "threads": [
        [
            659,
            0.0,
            0.0
        ]
    ],
    "cpu_num": 1,
    "memory_info": [
        1671168,
        5791744,
        1421312,
        135168,
        0,
        413696,
        0
    ],
    "cmdline": [
        "avahi-daemon:",
        "chroot",
        "helper"
    ],
    "name": "avahi-daemon",
    "pid": 659,
    "num_threads": 1,
    "memory_percent": 0.04198848404077411,
    "ionice": [
        0,
        0
    ],
    "terminal": null,
    "ppid": 651,
    "cwd": "/",
    "username": "avahi",
    "create_time": 1774840400.79,
    "uids": [
        101,
        101,
        101
    ],
    "status": "sleeping",
    "_uuid": "728802e2-b727-4ada-8ba7-4510459290a6"
}
```

</details>

### kill

* Role required: process_control
* Usage: `procps/{pid}/kill[/signal]`

Kills a process with signal or SIGINT if not specified.

<details>
<summary><b>Example Response from Kill</b></summary>

```json
{
    "pid": 2238156,
    "signal": "sigterm",
    "_uuid": "88d07a38-8830-4fed-b7b8-df491609eb35"
}
```

</details>