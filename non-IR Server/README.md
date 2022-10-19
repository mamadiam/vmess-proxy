## Use the following instruction on your server</br></br>
#### 1. update & upgrade the packages </br>
```shell script
# apt update && apt upgrade -y
``` 
#### 2. Create & edit docker compose </br>
```shell script
$ nano docker-compose.yaml
```
#### 3. Add the following to the docker-compose.yaml file and save: </br>
```shell script
version: "3"
services:
  v2ray443:
    image: v2fly/v2fly-core
    restart: always
    network_mode: host
    environment:
      - V2RAY_VMESS_AEAD_FORCED=false
    volumes:
      - ./config.json:/etc/v2ray/config.json:ro
```
#### 4.  Create & edit docker compose </br>
```shell script
$ nano config.json
```
#### 5. Add the following to the config.json file: </br>
Navigate to [UUID Generator](https://www.uuidgenerator.net/version1) and grab a UUID V1, and replace it with `xxx-xxx-xxx` </br>
Write your UUID in notepad, you gonna need it later.</br>
```shell script
{
  "log": {
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "listen": "0.0.0.0",
      "port": 80,
      "protocol": "vmess",
      "allocate": {
        "strategy": "always"
      },
      "settings": {
        "UDP": true,
        "clients": [
          {
            "id": "xxx-xxx-xxx",
            "level": 1,
            "alterId": 0
          }
        ],
        "disableInsecureEncryption": true
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
          "connectionReuse": true,
          "path": "/graphql"
        },
        "security": "",
        "tcpSettings": {
          "header": {
            "type": "http",
            "response": {
              "version": "1.1",
              "status": "200",
              "reason": "OK",
              "headers": {
                "Content-Type": [
                  "application/octet-stream",
                  "application/x-msdownload",
                  "text/html",
                  "application/x-shockwave-flash"
                ],
                "Transfer-Encoding": ["chunked"],
                "Connection": ["keep-alive"],
                "Pragma": "no-cache"
              }
            }
          }
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```
#### 6. Save & close your `config.json`.</br>
#### 7. Or instead, Replace the `YOUR_NEW_UUID` in the following command with your newly generated UUID</br>
```shell script
$ sed -i 's/xxx-xxx-xxx/YOUR_NEW_UUID/g' ./config.json
```
Note: the `docker-compose.yaml` and `config.json` files need to be under the same directory.</br>
#### 8. Install docker and docker-compose: </br>
```shell script
# apt install docker docker-compose
```
#### 9. Start the Vmess Server
```shell script
$ docker-compose up -d
```
