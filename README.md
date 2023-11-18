<!-- markdownlint-disable MD041 -->

<p align="center">
  <h1 align="center">Talos Linux</h1>
  <p align="center">A modern OS for Kubernetes.</p>
  <p align="center">
    <a href="https://github.com/talos-systems/talos/releases/latest">
      <img alt="Release" src="https://img.shields.io/github/release/talos-systems/talos.svg?logo=github&logoColor=white&style=flat-square">
    </a>
    <a href="https://github.com/talos-systems/talos/releases/latest">
      <img alt="Pre-release" src="https://img.shields.io/github/release-pre/talos-systems/talos.svg?label=pre-release&logo=GitHub&logoColor=white&style=flat-square">
    </a>
  </p>
</p>

---
# Friendly fork
This is a friendly fork of [siderolabs/talos](siderolabs/talos). It is only here to support [SBC Turing RK1](https://turingpi.com/product/turing-rk1/). And it will be integrated in some way using [community managed SBCs](https://github.com/siderolabs/talos/issues/8065) in Talos 1.7.

## Using this fork
[![asciicast](https://asciinema.org/a/635709.svg)](https://asciinema.org/a/635709)

### Download the latest release on the right

This describes the CLI commands, you may use the Turing PI webgui. Always first unpack the image yourself. Version 2.06 supports xz images, but it is slower.
```sh
xz -d metal-turing_rk1-arm64.raw.xz
tpi flash -n <NODENUMBER> -i metal-turing_rk1-arm64.raw
tpi power on -n <NODENUMBER> 
```

To check bootmessages:
```sh
tpi uart -n <NODENUMBER> get
```

Make sure when you use talosctl apply-config to have in this config:
```yaml
machine:
  kernel:
    modules:
      - name: rockchip-cpufreq
```

### Updating

Updating can also be done faster using the `talosctl upgrade` command.

```sh
talosctl upgrade -i ghcr.io/nberlee/installer:v1.6.x-rk3588
```
when adding the `-rk3588` to the tag, the rk3588 extension is no longer needed in the machine-config.
For example the `ghcr.io/nberlee/installer:v1.6.4-rk3588` installer image has the rk3588 talos extension included


# Talos
**Talos** is a modern OS for running Kubernetes: secure, immutable, and minimal.
Talos is fully open source, production-ready, and supported by the people at [Sidero Labs](https://www.SideroLabs.com/)
All system management is done via an API - there is no shell or interactive console.
Benefits include:

- **Security**: Talos reduces your attack surface: It's minimal, hardened, and immutable.
  All API access is secured with mutual TLS (mTLS) authentication.
- **Predictability**: Talos eliminates configuration drift, reduces unknown factors by employing immutable infrastructure ideology, and delivers atomic updates.
- **Evolvability**: Talos simplifies your architecture, increases your agility, and always delivers current stable Kubernetes and Linux versions.

## Documentation

For instructions on deploying and managing Talos, see the [Documentation](https://www.talos.dev/docs/latest/).

## Community

- Slack: Join our [slack channel](https://slack.dev.talos-systems.io)
- Support: Questions, bugs, feature requests [GitHub Discussions](https://github.com/talos-systems/talos/discussions)
- Forum: [community](https://groups.google.com/a/SideroLabs.com/forum/#!forum/community)
- Twitter: [@SideroLabs](https://twitter.com/SideroLabs)
- Email: [info@SideroLabs.com](mailto:info@SideroLabs.com)

If you're interested in this project and would like to help in engineering efforts or have general usage questions, we are happy to have you!
We hold a weekly meeting that all audiences are welcome to attend.

We would appreciate your feedback so that we can make Talos even better!
To do so, you can take our [survey](https://docs.google.com/forms/d/1TUna5YTYGCKot68Y9YN_CLobY6z9JzLVCq1G7DoyNjA/edit).

### Office Hours

- When: Mondays at 16:30 UTC.
- Where: [Google Meet](https://meet.google.com/day-pxhv-zky).

You can subscribe to this meeting by joining the community forum above.

> Note: You can convert the meeting hours to your [local time](https://everytimezone.com/s/599e61d6).

## Contributing

Contributions are welcomed and appreciated!
See [Contributing](CONTRIBUTING.md) for our guidelines.

## License

<a href="https://github.com/talos-systems/talos/blob/master/LICENSE">
  <img alt="GitHub" src="https://img.shields.io/github/license/talos-systems/talos?style=flat-square">
</a>

Some software we distribute is under the General Public License family
of licenses or other licenses that require we provide you with the
source code.
If you would like a copy of the source code for this
software, please contact us via email: info at SideroLabs.com.
