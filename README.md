# tailscale-vpn-docker

## Motivation

Setting up a Tailscale VPN server via Docker is particularly useful for those who want to securely access their home or office network from anywhere. For example, you can use this setup to remotely manage smart home devices, monitor security cameras, or access personal file servers while on the go. This solution is also beneficial for IT administrators who need secure remote access to internal systems.

## Project Overview

This project sets up a Tailscale VPN server using Docker. The setup allows your local network to be accessible via Tailscale, enabling secure remote access to your home or office network. Additionally, the Tailscale container acts as an exit node and advertises local subnet routes, allowing you to route traffic through it.

## Prerequisites

* Docker and Docker Compose installed on your server.
* A valid Tailscale account and an **auth key** (obtainable from the Tailscale admin console).
* Administrative (root) access to the server.

## Configuration

### 1. Enabling IP Forwarding

To allow traffic forwarding through the Tailscale VPN, enable IP forwarding in the kernel:

```bash
echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
sudo sysctl -p /etc/sysctl.d/99-tailscale.conf
```

### 2. Docker Compose Configuration

The Docker Compose file is included in the project repository. Please refer to it for the complete setup.

### 3. Environment Variables

Store your Tailscale auth key in an environment file (.env):

```
TS_AUTHKEY=tskey-xxxxxxxxxxxxxxxx
```

## Running the Container

Start the container using Docker Compose:

```bash
docker-compose up -d
```

Check the logs to ensure Tailscale is running properly:

```bash
docker logs tailscaled
```

## Verifying the Setup

To verify that your setup works:

1. Connect to your Tailscale network from another device.
2. Check if you can access local network devices through their IP addresses.
3. Use `tailscale ip -4` to check the Tailscale IP address.

## Troubleshooting

### Common Issues

1. **Cannot access local network devices:**

   * Make sure IP forwarding is enabled as described above.
   * Check that your Tailscale ACLs allow access to the subnet.
2. **Routes not appearing:**

   * Ensure the `--advertise-routes` argument in the Docker Compose file matches your local subnet.

## Stopping the Container

To stop the container, run:

```bash
docker-compose down
```

## References

* [Tailscale Documentation](https://tailscale.com/kb/)
* [Docker Documentation](https://docs.docker.com/)
