# Build from Sources
If you're planning to make modifications to SCION implementation, you can build SCION from sources and run your SCIONLab AS with your own version of SCION.
For developer's convenience, SCIONLab supports generating configurations that are compatible with the scripts and machinery intended to run SCION in a development environment.

Building SCION from sources requires following a lengthy setup procedure and installing various development dependencies.
The development setup is currently supported/documented for **Ubuntu 16.04 _only_**.
It is possible to build SCION on other systems, but no guidance is provided. To keep it simple, just run Ubuntu 16.04 in a VM or container if you can't/don't want to set it up on your workstation.

Please follow the instructions in the [GitHub README](https://github.com/netsec-ethz/netsec-scion/) to clone and build SCION.

!!! Note
    SCIONLab runs a version of SCION built from the branch `scionlab` in netsec-ethz/netsec-scion.
    This branch (intenionally) lags behind the scionproto/scion master. As there are still (rarely) breaking changes in the SCION protocol, running `master` may or may not be compatible with `scionlab`.


!!! Tip
    If you only want to develop applications _using_ SCION, you may still rely on the convenience of our [pre-built binary packages](../install/pkg.md).

    The [scion-apps](https://github.com/netsec-ethz/scion-apps/) repository contains examples for applications that run
    on top of SCION.


## Configuration

After having managed to build SCION and after [creating or modifying your AS](../config/create_as.md) in the SCIONLab coordination website, you can deploy the generated configuration to your machine.

1. Download the configuration tarfile from the SCIONLab coordination website.
2. If using VPN, unpack the `client.conf` to `/etc/openvpn/` and start OpenVPN

        #!shell
        sudo systemctl restart openvpn@client

3. Extract the `gen/` subdirectory to your `$SC` directory.

4. Restart SCION

        #!shell
        cd $SC
        scion.sh stop
        supervisor/supervisor.sh reload  # necessary if supervisor.conf files changed
        scion.sh start


!!! Note
    The configuration installed with the `scionlab-config` script as used in the [packaged installation](../install/pkg.md#configuration), is *not* directly compatible
    with `supervisord` and the `scion.sh` machinery.


## Running SCION

To build, start and stop SCION, use the `scion.sh` developer script, located in the SCION repository.

```shell
cd $SC
scion.sh start          # Build and start SCION services
scion.sh start nobuild  # Start, without building
scion.sh status         # Lists failed SCION services
scion.sh stop           # Stop all SCION services
```

The commands above are essentially a wrapper around supervisord.
Alternatively, use the supervisor commands directly, using the `supervisor/supervisor.sh` helper script.
```shell
cd $SC
supervisor/supervisor.sh reload   # Reload supervisord configuration
supervisor/supervisor.sh status   # Show status of all SCION services
supervisor/supervisor.sh          # Starts the supervisor shell
```
