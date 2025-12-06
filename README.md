# Shadow TLS
[![Build Releases](https://github.com/ihciah/shadow-tls/actions/workflows/build-release.yml/badge.svg)](https://github.com/ihciah/shadow-tls/releases) [![Crates.io](https://img.shields.io/crates/v/shadow-tls.svg)](https://crates.io/crates/shadow-tls) [![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fihciah%2Fshadow-tls.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fihciah%2Fshadow-tls?ref=badge_shield)

one**Can use someone else's trusted certificate**的 TLS Pretend to be a proxy。

it and [trojan](https://github.com/trojan-gfw/trojan) performs similarly, but while doing a real TLS handshake, it can directly use someone else's trusted certificate (such as the domain name of some large companies or institutions) without the need to issue the certificate yourself. When opened directly using a browser, the web content corresponding to the trusted domain name can be displayed normally.

---

A proxy to expose real tls handshake to the firewall.

It works like [trojan](https://github.com/trojan-gfw/trojan) but it does not require signing certificate. The firewall will see **real** tls handshake with **valid certificate** that you choose.

## How to Use It
This service requires bilateral deployment，And it generally needs to be paired with an encryption proxy（Because this project does not include data encryption and proxy request encapsulation functions，This is not our goal）。

usually，You can deploy it on the same machine shadowsocks-server 和 shadowtls-server；Then deploy on the other side of the firewall shadowsocks-client 和 shadowtls-client。

There are two ways to deploy this service。
1. Use Docker + Docker Compose

    Modify `docker-compose.yml` directly after `docker-compose up -d`。
2. Use precompiled binaries

    从 [Release Page](https://github.com/ihciah/shadow-tls/releases) to download the binary file of the corresponding platform, and then run it. Run instructions can be seen with `./shadow-tls client --help` or `./shadow-tls server --help`.

For more detailed usage guide, please refer to [Wiki](https://github.com/ihciah/shadow-tls/wiki/How-to-Run)。

---

Normally you need to deploy this service on both sides of the firewall. And it is usually used with an encryption proxy (because this project does not include encryption and proxy request encapsulation, which is not our goal).

1. Run with Docker + Docker Compose
 Modfy `docker-compose.yml` and run `docker-compose up -d`.

2. Use prebuilt binary
    Download the binary from [Release page](https://github.com/ihciah/shadow-tls/releases) and run it.

For more detailed usage guide, please refer to [Wiki](https://github.com/ihciah/shadow-tls/wiki/How-to-Run).

## How it Works
On client side, just do tls handshake. And for server, we have to relay data as well as parsing tls handshake to handshaking server which will provide valid certificate. We need to know when the tls handshaking is finished. Once finished, we can relay data to our real server.

Full design doc is here: [v2](./docs/protocol-en.md) | [v3](./docs/protocol-v3-en.md).

Complete protocol design: [v2](./docs/protocol-zh.md) | [v3](./docs/protocol-v3-zh.md).

## Note
This project relies on [Monoio](https://github.com/bytedance/monoio) which is a high performance rust async runtime with io_uring. However, it does not support windows yet. So this project does not support windows.

However, if this project is used widely, we will support it by conditional compiling.

Also, you may need to [modify some system limitations](https://github.com/bytedance/monoio/blob/master/docs/en/memlock.md) to make it work. If it does not work, you can add environ `MONOIO_FORCE_LEGACY_DRIVER=1` to use epoll instead of io_uring.

You may need to modify some system settings to get this to work，[Reference here](https://github.com/bytedance/monoio/blob/master/docs/en/memlock.md). If it doesn't work, you can add the environment variable `MONOIO_FORCE_LEGACY_DRIVER=1` to use epoll instead of io_uring.

## License
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fihciah%2Fshadow-tls.svg?type=large)](https://app.fossa.com/projects/git%2Bgithub.com%2Fihciah%2Fshadow-tls?ref=badge_large)
