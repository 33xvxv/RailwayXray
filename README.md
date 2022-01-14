# 使用Railway部署Xray高性能代理服务，通过ws传输的 (vmess、vless、trojan shadowsocks、socks)等协议

# Railway已封禁本项目，请Fork到自己的GitHub后部署

> 提醒： 滥用可能导致账户被BAN！！！ 

## 概述

用于在 railway 上部署 vless+websocket+tls，每次部署自动选择最新的 alpine linux 和 Xray core 。  
vless 性能更加优秀，占用资源更少。

* 使用[xray](https://github.com/XTLS/Xray-core)+caddy同时部署通过ws传输的vmess vless trojan shadowsocks socks等协议，并默认已配置好伪装网站。
* 支持tor网络，且可通过自定义网络配置文件启动xray和caddy来按需配置各种功能  
* 支持存储自定义文件,目录及账号密码均为UUID,客户端务必使用TLS连接  
  **Railway 为我们提供了免费的容器服务，我们不应该滥用它，所以本项目不宜做为长期翻墙使用。**


### 服务端

fork 之后 ，在railway的dashboard，选择 new project
[![CHIH8A.png](https://www.helloimg.com/images/2021/09/05/CHIH8A.png)](https://www.helloimg.com/image/CHIH8A)
然后在github中选中本仓库
[![5oSpg.png](https://i.w3tt.com/2021/09/05/5oSpg.png)](https://img.tg/image/5oSpg)

### 客户端
* **务必替换所有的`xxx.railway.app`为railway分配的项目域名**  
* **务必替换所有的`ffffffff-ffff-ffff-ffff-ffffffffffff`为部署时设置的UUID,建议更改,不要每个人都一样**  

**XRay 将在部署时会自动实配安装`最新版本`。**



<details>
<summary>V2rayN(Xray、V2ray)</summary>

```bash
* 客户端下载：https://github.com/2dust/v2rayN/releases
* 代理协议：vless 或 vmess
* 地址：xxx.railway.app
* 端口：443
* 默认UUID：ffffffff-ffff-ffff-ffff-ffffffffffff
* vmess额外id：0
* 加密：none
* 传输协议：ws
* 伪装类型：none
* 伪装域名：xxx.workers.dev(CF Workers反代地址)
* 路径：/ffffffff-ffff-ffff-ffff-ffffffffffff-vless // 默认vless使用(/自定义UUID码-vless)，vmess使用(/自定义UUID码-vmess)
* 底层传输安全：tls
* 跳过证书验证：false
```
</details>

<details>
<summary>Trojan-Go</summary>

```bash
* 客户端下载: https://github.com/p4gefau1t/trojan-go/releases
{
    "run_type": "client",
    "local_addr": "127.0.0.1",
    "local_port": 1080,
    "remote_addr": "xxx.railway.app",
    "remote_port": 443,
    "password": [
        "ffffffff-ffff-ffff-ffff-ffffffffffff"
    ],
    "websocket": {
        "enabled": true,
        "path": "/ffffffff-ffff-ffff-ffff-ffffffffffff-trojan",
        "host": "xxx.railway.app"
    }
}
```
</details>

<details>
<summary>Shadowsocks</summary>

```bash
* 客户端下载：https://github.com/shadowsocks/shadowsocks-windows/releases/
* 服务器地址: xxx.railway.app
* 端口: 443
* 密码：ffffffff-ffff-ffff-ffff-ffffffffffff
* 加密：chacha20-ietf-poly1305
* 插件程序：xray-plugin_windows_amd64.exe  //需将插件https://github.com/shadowsocks/xray-plugin/releases下载解压后放至shadowsocks同目录
* 插件选项: tls;host=xxx.railway.app;path=/ffffffff-ffff-ffff-ffff-ffffffffffff-ss
```
</details>

<details>
<summary>可以使用Cloudflare的Workers来中转流量，（支持VLESS\VMESS\Trojan-Go的WS模式）配置为：</summary>

```js
const SingleDay = 'xxx.railway.app'
const DoubleDay = 'xxx.railway.app'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

## OpenWrt优选IP脚本自动更新：

* [CloudflareST](https://github.com/Lbingyi/CloudflareST)
* [cf-autoupdate](https://github.com/Lbingyi/cf-autoupdate)

## 关于CF筛选IP

* 请参考 [CloudflareSpeedTest](https://github.com/XIU2/CloudflareSpeedTest) `推荐`
* 请参考 [better-cloudflare-ip](https://github.com/badafans/better-cloudflare-ip)

## 📢免费容器📢 

☁️[Railway]`https://railway.app?referralCode=payday4478`
>👥[专属邀请码]`payday4478`(获得5美元)

☁️[Heroku] `https://www.heroku.com`

☁️[Okteto] `https://okteto.com`
