一、回落终极部署

配置1/配置2/配置3实现 vless tcp 以 http/1.1 代理科学上网，分流出ws，回落给 trojan，由 trojan 处理后再回落给 nginx。（套娃方式）其应用如下：

1、vless+tcp+tls（回落/分流配置。）

2、vless+ws+tls（tls由vless+tcp+tls提供及处理，不需要另外配置；另可改成vmess+ws+tls或SS+v2ray-plugin+tls或trojan+ws+tls。）

3、trojan+tcp+tls（tls由vless+tcp+tls提供及处理，不需要另外配置。）

利用 vless 强大的回落/分流特性，实现了共用 443 端口，同时支持 vless tcp 与任意 ws 类及trojan 应用完美共存。

注意：

1、v2ray v4.31.0 版本及以后才支持 trojan 及完整回落。

2、nginx 支持 h2c server，但不支持 http/1.1 server 与 h2c server 共用一个端口或一个进程；而v2ray的trojan不支持端口或进程分离h2回落，故套娃回落nginx只能全部采用h1回落。

3、nginx 预编译程序包可能不带支持 PROXY protocol 协议的模块。如要使用此项协议应用，需加 http_realip_module（必须加的） 及 stream_realip_module（可选加） 两模块构建自定义模板，再进行源代码编译和安装。另编译时选取源代码版本建议不要低于1.13.11。

4、配置1：没有启用 PROXY protocol，仅端口回落。配置2：启用了 PROXY protocol，且端口回落。配置3：启用了 PROXY protocol，且进程回落。


二、nginx SNI分流优化共用443端口

配置4/配置5/配置6实现 vless tcp 以 http/1.1 或 http/2 自适应代理科学上网，分流出ws，回落给nginx。同时v2ray trojan也以 http/1.1 代理科学上网，回落给nginx。其应用如下：

1、vless+tcp+tls（回落/分流配置。）

2、vless+ws+tls（tls由vless+tcp+tls提供及处理，不需要另外配置；另可改成vmess+ws+tls或SS+v2ray-plugin+tls或trojan+ws+tls。）

3、trojan+tcp+tls（回落配置。）

利用nginx支持SNI分流特性，对vless tcp与trojan进行端口分流（四层转发），实现共用443端口。

注意：

1、v2ray v4.31.0 版本及以后才支持 trojan 及完整回落。

2、nginx 支持 h2c server，但不支持 http/1.1 server 与 h2c server 共用一个端口或一个进程。v2ray的trojan不支持端口或进程分离h2回落，只能采用h1回落。

3、nginx 预编译程序包不带支持SNI分流协议的模块。如要使用此项协议应用，需加stream_ssl_preread_module模块构建自定义模板，再进行源代码编译和安装。

4、nginx 预编译程序包可能不带支持 PROXY protocol 协议的模块。如要使用此项协议应用，需加 http_realip_module 与 stream_realip_module 两模块构建自定义模板，再进行源代码编译和安装。另编译时选取源代码版本建议不要低于1.13.11。

5、配置4：没有启用 PROXY protocol，仅端口回落。配置5：启用了 PROXY protocol，且端口回落。配置6：启用了 PROXY protocol，且进程回落。
