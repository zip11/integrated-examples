介绍：

此配置实现 vless tcp 以 http/1.1 或 http/2 自适应代理科学上网，分流出 ws，非 v2ray 的 web 回落给 caddy2。其应用如下：

1、vless+tcp+tls（回落/分流配置。）

2、vless+ws+tls（tls由vless+tcp+tls提供及处理，不需要另外配置；另可改成vmess+ws+tls或SS+v2ray-plugin+tls或trojan+ws+tls。）

利用 vless tcp 强大的回落/分流特性，实现了共用 443 端口，同时支持 vless tcp 与任意 ws 类应用完美共存。

注意：

1、caddy2 目前只能 json 配置才能开启 h2c server，故要实现回落 h2 就不能采用 Caddyfile 配置；另外 caddy2 版本不能低于 v2.1.0 ，否则不支持 h2c server。

2、caddy2 支持 http/1.1 server 与 h2c server 共用一个端口。

3、caddy2 发行版不支持 PROXY protocol，如要支持 PROXY protocol 需选 caddy2-proxyprotocol 插件定制编译或选使用本人github文件即可。

4、配置1：没有启用 PROXY protocol，仅端口回落。配置2：启用了 PROXY protocol，且端口回落。
