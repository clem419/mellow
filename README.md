# Mellow
A V2Ray client with TUN support.

## Build
```sh
yarn && yarn dist
```

## Run
1. Install the helper from tray
2. Edit your V2Ray config file ~/Library/Application\ Support/mellow/cfg.json
3. Start from tray
4. tail -f ~/Library/Logs/Mellow/log.log

## TODO
- [x] macOS Support
- [ ] Windows Support
- [ ] Linux Support

## A Sample Config
```json
{
    "log": {
        "loglevel": "info"
    },
    "dns": {
        "hosts": {
            "localhost": "127.0.0.1"
        },
        "servers": [
            {
                "address": "8.8.8.8",
                "port": 53
            },
            {
                "address": "223.5.5.5",
                "port": 53,
                "domains": [
                    "geosite:cn"
                ]
            }
        ]
    },
    "outbounds": [
        {
            "protocol": "vmess",
            "settings": {},
            "tag": "economic_vps_1"
        },
        {
            "protocol": "vmess",
            "settings": {},
            "tag": "economic_vps_2"
        },
        {
            "protocol": "vmess",
            "settings": {},
            "tag": "bittorrent_vps_1"
        },
        {
            "protocol": "vmess",
            "settings": {},
            "tag": "expensive_vps_1"
        },
        {
            "protocol": "freedom",
            "settings": {},
            "tag": "direct"
        },
        {
            "settings": {},
            "protocol": "blackhole",
            "tag": "block"
        }
    ],
    "policy": {
        "levels": {
            "0": {
                "connIdle": 300,
                "downlinkOnly": 0,
                "uplinkOnly": 0,
                "handshake": 4
            }
        }
    },
    "routing": {
        "domainStrategy": "IPIfNonMatch",
        "balancers": [
            {
                "tag": "limited",
                "selector": [
                    "expensive_vps_1",
                    "economic_vps_1"
                ],
                "strategy": "latency",
                "totalMeasures": 2,
                "interval": 300
            },
            {
                "tag": "bt",
                "selector": [
                    "bittorrent_vps_1"
                ]
            },
            {
                "tag": "nolimit",
                "selector": [
                    "economic_vps_1",
                    "economic_vps_2"
                ],
                "strategy": "latency",
                "totalMeasures": 2,
                "interval": 120
            }
        ],
        "rules": [
            {
                "domain": [
                    "domain:doubleclick.net"
                ],
                "type": "field",
                "outboundTag": "block"
            },
            {
                "type": "field",
                "ip": [
                    "1.1.1.1",
                    "9.9.9.9",
                    "8.8.8.8",
                    "8.8.4.4"
                ],
                "balancerTag": "limited"
            },
            {
                "type": "field",
                "domain": [
                    "geosite:cn"
                ],
                "outboundTag": "direct"
            },
            {
                "type": "field",
                "ip": [
                    "geoip:cn",
                    "geoip:private"
                ],
                "outboundTag": "direct"
            },
            {
                "type": "field",
                "port": "1,1024-65535",
                "balancerTag": "bt"
            },
            {
                "type": "field",
                "domain": [
                    "googlevideo",
                    "dl.google.com",
                    "ytimg"
                ],
                "balancerTag": "nolimit"
            },
            {
                "type": "field",
                "domain": [
                    "domain:youtube.com",
                    "android",
                    "google",
                    "nyaa",
                    "git"
                ],
                "balancerTag": "limited"
            },
            {
                "ip": [
                    "0.0.0.0/0",
                    "::/0"
                ],
                "type": "field",
                "balancerTag": "nolimit"
            }
        ]
    }
}
```
