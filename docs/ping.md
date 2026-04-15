# Ping

The ping module requires no ticket or roles. It only requires the pre-shared API key. Takes no parameters and returns a blob of data about the server and the client.

<details>
<summary><b>Example response</b></summary>

```json
{
    "server": {
        "node": "api.example.com",
        "dns_name": "nodey.taild0af6d.ts.net",
        "ip": "100.108.212.120"
    },
    "client": {
        "ip": "100.65.200.27",
        "dns_name": "nodex.taild0af6d.ts.net"
    },
    "_uuid": "54865d6a-d17e-4ec9-9725-4d6bd43bba47"
}

```

</details>
