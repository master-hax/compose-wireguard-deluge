# compose-wireguard-deluge

a multi-container Docker application to run [Deluge](https://hub.docker.com/r/linuxserver/deluge) behind a [Wireguard](https://hub.docker.com/r/linuxserver/wireguard) client

## how to set it up

1. download [docker-compose.yml](/docker-compose.yml)
1. put your `wg0.conf` file into `./wireguard`
1. run `docker-compose up`

if everything works correctly, Deluge should be running behind your VPN!

## how to use it

the Deluge web UI should be accessible at http://localhost:8112

if you want to use this persistently, you should probably
1. change the locations of the `deluge-data-volume` & `downloads-volume`
1. forward a port with your VPN provider, add port forwarding to your `wg0.conf` using the rules proposed [here](https://github.com/linuxserver/docker-wireguard/issues/58#issuecomment-723702782), then set up that same port in Deluge as the incoming port.

## how it works

the `torrent-client` (Deluge) service shares the network stack of the `vpn-sidecar` service (Wireguard), which is tunneled through your VPN provider. to maintain local connectivity to the `torrent-client` container's web UI, we proxy to it to through the `web-proxy` service (Nginx) using [Docker container links](https://docs.docker.com/network/links/).

## note: an [OpenVPN version](https://github.com/master-hax/compose-openvpn-deluge) is also available
