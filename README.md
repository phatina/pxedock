# DISCLAIMER

This project is based on `slicedbread/pxedock`.


# Overview

This source can be used to build an image for a pxe-boot server.
The image contains:

* H. Peter Anvin's [tftp server](https://git.kernel.org/cgit/network/tftp/tftp-hpa.git/)
* the [ISC DHCP server](https://www.isc.org/downloads/dhcp/)
* default, minimal configuration using [`netboot.xyz`](http://netboot.xyz)
  that you can easily override
* lightweight DHCP managing [script](start)

The image contains a tftpd and dhcp server to PXE-boot your hosts. The default
dhcp configuration will listen for broadcasts on a best guess of the current
subnet.

The default tftpd server will contain only the
[`netboot.xyz.kpxe`](http://netboot.xyz) file.


# Running

## Fetch the pre-built image

The image is published as `phatina/pxedock`.

    docker pull phatina/pxedock

## Run

Run the container, specifying the interface it should listen on (will generate
dhcpd.conf properly to listen to it):

    docker run -d --net=host \
        -p 67:67 -p 67:67/udp -p 69:69/udp \
        -e iface="ens4u1u2"
        -e PXE_target=bootx64.efi \
        -v $PWD/conf.d:/etc/dhcp/conf.d:Z \
        -v $PWD/tftp://tftpboot:Z \
        phatina/pxedock

Run the container, specifying your own DNS server, if necessary (this will be
provided to the clients) (*default is 8.8.8.8*):

    docker run -d --net=host \
        -e DNS=192.168.0.2 \
        -p 67:67 -p 67:67/udp -p 69:69/udp \
        phatina/pxedock


# License

See [`LICENSE`](LICENSE) in this repo.

