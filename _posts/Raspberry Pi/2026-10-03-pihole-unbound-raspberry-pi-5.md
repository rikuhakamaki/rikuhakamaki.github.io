---
title: "Pi-hole + Unbound on Raspberry Pi 5 with Docker"
date: 2026-10-03 00:00:00 +0300
categories: [Raspberry Pi, Networking]
tags:
  - raspberry pi
  - pi-hole
  - unbound
  - docker
  - dns
  - networking
description: "Pi-hole and Unbound setup on Raspberry Pi 5 with Docker, covering recursive DNS, DNSSEC, network clients, testing, and troubleshooting."
---

<aside class="post-author-note" aria-label="Authorship note">
  <p class="post-author-note-title">NOTE</p>
  <p>This tutorial was written and tested by the author. ChatGPT GPT-5.6 Sol was used to finalize the wording and presentation of the text.</p>
</aside>

## Initial Raspberry Pi setup

Start with a Raspberry Pi 5 Model B with 8 GB of RAM and a 128 GB SanDisk Extreme Pro UHS-I microSDXC card, or equivalent hardware.

> **NOTE:** This tutorial uses a Windows computer for all client-side steps. Commands entered after connecting via SSH are executed on the Raspberry Pi itself.

If your Raspberry Pi is already up and running, skip this part.

### Raspberry Pi Imager

Install <code class="language-plaintext highlighter-rouge">Raspberry Pi Imager</code>, insert your SD card into your computer, and start the setup process by selecting the following options:

- Device: Raspberry Pi 5
- OS: Raspberry Pi OS (other) -> Raspberry Pi OS Lite (64-bit)
- Storage: Your SD card
- Customisation:
  - Hostname: choose a hostname that suits you
  - Localisation: choose the settings that suit you
  - User: choose a username and password
  - Wi-Fi: Leave this unconfigured if you use a wired Ethernet connection
  - Remote access: Enable SSH with password authentication
  - Raspberry Pi Connect: Disabled
  - Writing: Review your settings and click <code class="language-plaintext highlighter-rouge">Write</code> if everything looks correct. Note that the writing process erases all data on the SD card. Make sure there is nothing on the card that you want to keep.

After <code class="language-plaintext highlighter-rouge">Raspberry Pi Imager</code> has finished writing the image, safely eject the SD card. Then, move on to booting and connecting to the Raspberry Pi.

Insert the SD card into the Raspberry Pi, connect it to the network, and then connect the power supply. The Raspberry Pi will boot automatically when it receives power. Give it a few minutes to complete the first boot.

### Connecting to the Raspberry Pi

On a computer connected to the same network as the Raspberry Pi, open <code class="language-plaintext highlighter-rouge">Command Prompt</code> or <code class="language-plaintext highlighter-rouge">PowerShell</code> and connect to it using SSH:

```bash
ssh username@hostname
```

Replace <code class="language-plaintext highlighter-rouge">username</code> and <code class="language-plaintext highlighter-rouge">hostname</code> with the values you configured in <code class="language-plaintext highlighter-rouge">Raspberry Pi Imager</code>. If the hostname does not resolve, use the Raspberry Pi's IP address instead.

> **NOTE:** If you have previously connected to a device over SSH using the same hostname or IP address, you might receive a warning that the remote host identification has changed. If you know this was caused by reinstalling the Raspberry Pi, remove the old key with:

```bash
ssh-keygen -R hostname
```

If you connected using an IP address, use:

```bash
ssh-keygen -R IP-address
```

Enter your password when prompted. After entering it, you should be logged in to your Raspberry Pi.

### Updating the Raspberry Pi

Before updating, the installed OS version can be checked with:

```bash
cat /etc/os-release
```

Then, update the system with:

```bash
sudo apt update && sudo apt full-upgrade -y
```

You might be prompted to restart services that are using outdated libraries. Leave all listed services selected and continue unless you have a specific reason not to. Navigate the list with the arrow keys and select or deselect entries with the spacebar.

If the Pi does not reboot automatically after the update, reboot it manually with:

```bash
sudo reboot
```

After the Raspberry Pi has rebooted, reconnect over SSH using the same command as earlier.

### Creating a dedicated user

Creating a dedicated user for managing Pi-hole is optional but recommended. If you skip this step, use your existing Raspberry Pi username in the commands that follow.

Create a dedicated user for managing Pi-hole:

- Create the user with:

  ```bash
  sudo useradd -m -s /bin/bash username
  ```

  > **NOTE:** Replace <code class="language-plaintext highlighter-rouge">username</code> with your desired username.

- Optionally, set a password with:

  ```bash
  sudo passwd username
  ```

  > **NOTE:** Replace <code class="language-plaintext highlighter-rouge">password</code> with your desired password. The password needs to be retyped to set it.

## Installing Docker

Next, install Docker and Docker Compose with:

```bash
sudo apt install docker.io docker-compose -y
```

On current Debian 13 installations, Docker Compose is provided as a separate package and uses the modern Docker CLI <code class="language-plaintext highlighter-rouge">docker compose</code> command syntax instead of the older <code class="language-plaintext highlighter-rouge">docker-compose</code> command syntax.

Verify that both Docker and Docker Compose are available:

```bash
docker --version
```

```bash
docker compose version
```

Then, add the Pi-hole management account to the Docker group with:

```bash
sudo usermod -aG docker username
```

This allows that user to run Docker commands without <code class="language-plaintext highlighter-rouge">sudo</code>.

> **NOTE:** Adding a user to the <code class="language-plaintext highlighter-rouge">docker</code> group effectively gives that user root-level control of the Raspberry Pi, because Docker can be used to mount and modify the host filesystem.

## Creating the Pi-hole project

Create a directory for the Pi-hole Docker files with:

```bash
sudo mkdir -p /opt/pihole
```

Then, make the Pi-hole management user the owner with:

```bash
sudo chown username:username /opt/pihole
```

Docker group membership changes take effect after starting a new login session. Switch to the dedicated user with:

```bash
sudo -iu username
```

Verify Docker access with:

```bash
docker ps
```

If the command runs without a permission error, the Docker group membership is working correctly.

Create the project directory structure with:

```bash
mkdir -p /opt/pihole/{pihole,unbound}
```

Then, move into the parent Pi-hole directory with:

```bash
cd /opt/pihole
```

## Configuring Docker Compose

Next, configure Docker Compose.

Create a <code class="language-plaintext highlighter-rouge">docker-compose.yml</code> file with:

```bash
nano docker-compose.yml
```

Add the following content:

```yaml
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "80:80/tcp"
      - "443:443/tcp"
    environment:
      TZ: "Europe/Helsinki"  # Replace with your timezone
      FTLCONF_webserver_api_password: "password"  # Change this!
      FTLCONF_dns_listeningMode: "all"
      FTLCONF_dns_upstreams: "unbound#53"
    volumes:
      - ./pihole:/etc/pihole
    restart: unless-stopped
    networks:
      dns_network:
    depends_on:
      - unbound

  unbound:
    container_name: unbound
    image: klutchell/unbound:latest
    volumes:
      - ./unbound:/etc/unbound/custom.conf.d
    restart: unless-stopped
    networks:
      dns_network:

networks:
  dns_network:
    driver: bridge
```

The Pi-hole and Unbound containers share the same Docker bridge network. This allows Pi-hole to use the Unbound container name directly as its upstream resolver with <code class="language-plaintext highlighter-rouge">unbound#53</code>.

Pi-hole v6 uses <code class="language-plaintext highlighter-rouge">FTLCONF_dns_listeningMode: "all"</code> to provide the required listening behaviour for Docker bridge networking. It does not load <code class="language-plaintext highlighter-rouge">/etc/dnsmasq.d</code> by default.

No additional Linux capabilities or <code class="language-plaintext highlighter-rouge">seccomp:unconfined</code> configuration are required for this setup because Pi-hole is not being used as the DHCP server or to set the Raspberry Pi's system time.

Save the file with Ctrl + O, then exit <code class="language-plaintext highlighter-rouge">Nano</code> with Ctrl + X.

## Configuring Unbound

Next, configure Unbound.

Move into the <code class="language-plaintext highlighter-rouge">unbound</code> directory with:

```bash
cd /opt/pihole/unbound
```

Create the Unbound configuration file with:

```bash
nano unbound.conf
```

Add the following content:

```conf
server:
  # Disable IPv6 queries if you do not have native IPv6 connectivity
  do-ip6: no

  # Keep the receive buffer below the typical container host limit
  so-rcvbuf: 400k
```

Although older versions of Unbound configuration tutorials include suggestions to add a custom <code class="language-plaintext highlighter-rouge">root-hints:</code> line, it is not necessary. The <code class="language-plaintext highlighter-rouge">klutchell/unbound</code> image includes its own root hints and DNSSEC trust anchor configuration, so you do not need to download or configure a separate root hints file. Adding another root-hints file results in duplicate root-hints definitions and can produce the warning:

```text
error: second hints for zone . ignored.
```

The EDNS buffer size is set to 1232 bytes. This reduces the risk of fragmented DNS responses and matches the current recommendation commonly used for Unbound.

Save the file with Ctrl + O, then exit <code class="language-plaintext highlighter-rouge">Nano</code> with Ctrl + X.

## Starting the containers

Before starting the containers, return to the parent directory:

```bash
cd /opt/pihole
```

Start the containers with:

```bash
docker compose up -d
```

Check their status with:

```bash
docker compose ps
```

Both the <code class="language-plaintext highlighter-rouge">pihole</code> and <code class="language-plaintext highlighter-rouge">unbound</code> containers should show as running.

Check the Unbound startup logs with:

```bash
docker logs unbound --tail=50
```

Look for a successful startup message such as:

```text
unbound[1:0] info: start of service (unbound 1.26.1).
```

Also check for any syntax errors, permission errors, or problems loading files from <code class="language-plaintext highlighter-rouge">/etc/unbound/custom.conf.d</code>.

You can verify that the Unbound configuration itself is valid with:

```bash
docker exec unbound unbound-checkconf
```

If the configuration is valid, the command should complete without reporting configuration errors.

Next, test Unbound directly:

```bash
docker exec unbound drill-hc @127.0.0.1 dnssec.works
```

A successful response confirms that Unbound can resolve and validate DNS queries.

You can also test DNSSEC failure handling with:

```bash
docker exec unbound drill-hc @127.0.0.1 fail01.dnssec.works
```

The deliberately broken DNSSEC domain should not resolve successfully.

## Configuring Pi-hole

Next, configure Pi-hole. Access the Pi-hole web interface at <code class="language-plaintext highlighter-rouge">http://pi-hole-ip/admin/</code>. 

  > **NOTE:** Replace <code class="language-plaintext highlighter-rouge">pi-hole-ip</code> with your Raspberry Pi's IP address.

Use the password configured in <code class="language-plaintext highlighter-rouge">docker-compose.yml</code>.

Check the following Pi-hole settings:

1. DNS Settings

   - Go to Settings > DNS
   - Verify that <code class="language-plaintext highlighter-rouge">unbound#53</code> is configured as the upstream DNS server
   - Verify that no public upstream DNS providers such as Google, Cloudflare, or Quad9 are enabled
   - Verify that the DNS listening mode is set to permit queries on all interfaces

   Because <code class="language-plaintext highlighter-rouge">FTLCONF_dns_upstreams</code> and <code class="language-plaintext highlighter-rouge">FTLCONF_dns_listeningMode</code> are defined through Docker environment variables, these settings may appear read-only in the Pi-hole web interface. This is expected.

   > **NOTE:** Using the "Permit all origins" / <code class="language-plaintext highlighter-rouge">ALL</code> listening mode is required for this Docker bridge configuration, but make sure TCP and UDP port <code class="language-plaintext highlighter-rouge">53</code> are not exposed to the public Internet. Do not create a router port-forward for DNS port <code class="language-plaintext highlighter-rouge">53</code>.

2. Domain Settings (optional)

   - Set your local domain, for example <code class="language-plaintext highlighter-rouge">somethingfun.lan</code>
   - Enable "Expand hostnames" if you want to use simple hostnames on your local network

## Configuring host DNS

Next, configure the Raspberry Pi host itself to use Pi-hole for DNS. Current Raspberry Pi OS versions use <code class="language-plaintext highlighter-rouge">NetworkManager</code> by default, so configure DNS through <code class="language-plaintext highlighter-rouge">NetworkManager</code> instead of manually locking <code class="language-plaintext highlighter-rouge">/etc/resolv.conf</code> or editing <code class="language-plaintext highlighter-rouge">dhclient.conf</code>.

  > **NOTE:** The following <code class="language-plaintext highlighter-rouge">NetworkManager</code> commands require an account with <code class="language-plaintext highlighter-rouge">sudo</code> privileges, so switch back to the original Raspberry Pi user before continuing. If you previously switched to the dedicated Pi-hole user with:
  ```bash
sudo -iu username
```
  > return to the original user with:
  ```bash
exit
```
  > If this closes the SSH session instead of returning you to the original user, reconnect using the original Raspberry Pi account:
  ```bash
ssh username@hostname
```

First, identify the active network connection:

```bash
nmcli connection show --active
```

Look for the connection used by the Raspberry Pi, for example:

<code class="language-plaintext highlighter-rouge">Wired connection 1</code>

Configure the connection to use the local Pi-hole instance as its DNS resolver:

> **NOTE:** Replace <code class="language-plaintext highlighter-rouge">connection-name</code> in the following commands with the actual connection name.

```bash
sudo nmcli connection modify "connection-name" ipv4.dns "127.0.0.1" ipv4.ignore-auto-dns yes ipv6.ignore-auto-dns yes
```

Apply the change:

```bash
sudo nmcli connection up "connection-name"
```

Check the generated resolver configuration with:

```bash
cat /etc/resolv.conf
```

You should see the local resolver being used rather than DNS servers supplied automatically by DHCP.

You can also verify the <code class="language-plaintext highlighter-rouge">NetworkManager</code> DNS configuration with:

```bash
nmcli connection show "connection-name" | grep -E "ipv4.dns|ipv4.ignore-auto-dns|ipv6.ignore-auto-dns"
```

  > **NOTE:** Do not make <code class="language-plaintext highlighter-rouge">/etc/resolv.conf</code> immutable with <code class="language-plaintext highlighter-rouge">chattr +i</code>. <code class="language-plaintext highlighter-rouge">NetworkManager</code> should be allowed to manage the file normally.
  > Do not add a <code class="language-plaintext highlighter-rouge">supersede domain-name-servers</code> entry to <code class="language-plaintext highlighter-rouge">/etc/dhcp/dhclient.conf</code>. Configure host DNS through <code class="language-plaintext highlighter-rouge">NetworkManager</code>, which Raspberry Pi OS uses by default.
  > Also, do not configure Docker's daemon DNS as:
  ```json
  "dns": ["127.0.0.1"]
  ```
  A Docker container has its own network namespace, so <code class="language-plaintext highlighter-rouge">127.0.0.1</code> inside a container refers to that container itself rather than the Raspberry Pi host. Leave Docker's daemon DNS configuration at its default unless you have a specific reason to change it.

## Configuring network clients

Then, configure your DHCP server or router to provide the Pi-hole server's LAN IP address as the DNS server for your network clients. This step varies depending on your router. Alternatively, configure the Pi-hole IP address manually on individual devices. For example, on Windows:

<code class="language-plaintext highlighter-rouge">Windows Settings > Network and Internet > Ethernet > IP configuration > Manual > IPv4(/6) > DNS</code>

Do not use the loopback address <code class="language-plaintext highlighter-rouge">127.0.0.1</code> on other devices. Use the Raspberry Pi's actual LAN IP address.

### Container autostart

Container autostart does not require a separate Pi-hole <code class="language-plaintext highlighter-rouge">systemd</code> service.

Both containers already use:

```yaml
restart: unless-stopped
```

Docker will therefore restart them automatically when the Docker daemon starts, unless you deliberately stopped them yourself.

Make sure Docker itself is enabled at boot:

```bash
sudo systemctl enable docker
```

You can verify this with:

```bash
systemctl is-enabled docker
```

## Testing

With the main configuration complete, test each part of the DNS chain separately. Change back to the designated Pi-hole user with (or alternatively add <code class="language-plaintext highlighter-rouge">sudo</code> in front of every <code class="language-plaintext highlighter-rouge">docker</code> command):

```bash
sudo -iu username
```

1. Test Unbound directly

   Run:

   ```bash
    docker exec unbound drill-hc @127.0.0.1 example.com
   ```

   This tests whether Unbound itself can perform recursive DNS resolution.

2. Test Pi-hole through Unbound

   Run:

   ```bash
   docker exec -it pihole nslookup example.com 127.0.0.1
   ```

   This sends the query to Pi-hole rather than directly to Unbound.

   Then, open the Pi-hole Query Log in the web interface and confirm that the query was forwarded to <code class="language-plaintext highlighter-rouge">unbound#53</code>. This verifies the complete path.

3. Test from another device

   From a Windows computer using Pi-hole as its DNS server, run:

   ```bash
   nslookup example.com your-pihole-ip
   ```

   Replace <code class="language-plaintext highlighter-rouge">your-pihole-ip</code> with the Raspberry Pi's LAN IP address. The query should return a valid response.

4. Test DNSSEC

   A valid DNSSEC domain should resolve:

   ```bash
   docker exec unbound drill-hc @127.0.0.1 dnssec.works
   ```

   A deliberately broken DNSSEC domain should fail:

   ```bash
   docker exec unbound drill-hc @127.0.0.1 fail01.dnssec.works
   ```

5. Test DNS leak behaviour

   Visit [dnsleaktest.com](https://dnsleaktest.com) and run a standard test.

   Because Unbound performs recursive DNS resolution itself rather than forwarding everything to a public DNS provider, the results may not look the same as when using services such as Google DNS or Cloudflare. You should not see Google, Cloudflare, Quad9, or another public resolver unless something in your network is explicitly using one of them.

## Blocklists

Once the basic installation is working, add the blocklists of your choice.

For example:

```text
https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts
https://raw.githubusercontent.com/hagezi/dns-blocklists/refs/heads/main/domains/pro.plus.txt
https://gitlab.com/hagezi/mirror/-/raw/main/dns-blocklists/adblock/pro.txt
https://gitlab.com/hagezi/mirror/-/raw/main/dns-blocklists/adblock/tif.txt
https://gitlab.com/hagezi/mirror/-/raw/main/dns-blocklists/adblock/dyndns.txt
https://gitlab.com/hagezi/mirror/-/raw/main/dns-blocklists/adblock/spam-tlds-adblock.txt
```

Add the lists through the Pi-hole web interface. After adding or changing blocklists, update Pi-hole's gravity database with:

```bash
docker exec -it pihole pihole -g
```

## Troubleshooting

### Pi-hole: ignoring query from non-local network

If you see:

```text
ignoring query from non-local network
```

Pi-hole is refusing a query because it does not consider the requesting network local. Because Pi-hole runs behind Docker's bridge network, the correct listening mode for this configuration is already defined in <code class="language-plaintext highlighter-rouge">docker-compose.yml</code>:

```yaml
FTLCONF_dns_listeningMode: "all"
```

Verify that this environment variable is present and then recreate the container if you changed the Compose file:

```bash
cd /opt/pihole
docker compose up -d
```

Do not use <code class="language-plaintext highlighter-rouge">docker compose restart</code> after changing environment variables in the Compose file. A simple restart does not recreate the container with the new environment configuration.

### Unbound not working

If Unbound fails to start or respond to queries:

1. Check the container status:

   ```bash
   docker compose ps
   ```

2. Check the logs:

   ```bash
   docker logs unbound --tail=100
   ```

3. Validate the configuration:

   ```bash
   docker exec unbound unbound-checkconf
   ```

4. Test the resolver directly:

   ```bash
   docker exec unbound drill-hc @127.0.0.1 dnssec.works
   ```

5. Verify that your custom configuration is mounted correctly.

   The local directory should be <code class="language-plaintext highlighter-rouge">/opt/pihole/unbound</code>. The Docker Compose mount should be <code class="language-plaintext highlighter-rouge">./unbound:/etc/unbound/custom.conf.d</code>. The configuration file should be <code class="language-plaintext highlighter-rouge">/opt/pihole/unbound/unbound.conf</code>. The <code class="language-plaintext highlighter-rouge">klutchell/unbound</code> image expects custom configuration files in <code class="language-plaintext highlighter-rouge">/etc/unbound/custom.conf.d/</code>.

6. Check the local configuration file permissions:

   ```bash
   ls -la /opt/pihole/unbound
   ```

   The Unbound Docker image must be able to read the files in this directory. A normal readable configuration file can be set with:

   ```bash
   sudo chmod 644 /opt/pihole/unbound/*.conf
   ```

   And the directory itself can be made traversable with:

   ```bash
   sudo chmod 755 /opt/pihole/unbound
   ```

   Avoid recursively changing the directory to an arbitrary UID such as <code class="language-plaintext highlighter-rouge">1001:1001</code>. The Unbound image runs using its own container user and only requires that the mounted files are readable.

### Unbound DNSSEC failure

If normal domains resolve but DNSSEC validation fails, check:

```bash
docker logs unbound --tail=100
```

Then run:

```bash
docker exec unbound drill-hc @127.0.0.1 dnssec.works
```

If this fails, Unbound may be unable to reach the DNS root or authoritative servers correctly. You can also validate the configuration again with:

```bash
docker exec unbound unbound-checkconf
```

### Duplicate root hints warning

If you see:

```text
error: second hints for zone . ignored.</code>
```

check your <code class="language-plaintext highlighter-rouge">/opt/pihole/unbound/unbound.conf</code> file.There should not be a custom <code class="language-plaintext highlighter-rouge">root-hints:</code> line in this configuration. The <code class="language-plaintext highlighter-rouge">klutchell/unbound</code> image already provides root hints internally, so a second root-hints configuration is unnecessary.

### Pi-hole container permission issues

If Pi-hole reports permission errors, first inspect the actual volume rather than recursively changing its ownership:

```bash
ls -la /opt/pihole/pihole
```

Also inspect the container logs:

```bash
docker logs pihole --tail=100
```

Avoid using commands such as:

```text
chown -R 1001:1001
```

unless you have first confirmed that the UID and GID are actually correct for the container and the specific files involved. Likewise, avoid applying <code class="language-plaintext highlighter-rouge">chmod -R 755</code> to the entire volume. Regular configuration and database files do not need to be executable.

### Host DNS stops working when Pi-hole is down

After configuring <code class="language-plaintext highlighter-rouge">NetworkManager</code> to use <code class="language-plaintext highlighter-rouge">127.0.0.1</code>, the Raspberry Pi itself depends on Pi-hole for DNS. If Docker or Pi-hole is stopped, DNS resolution on the Raspberry Pi will therefore stop as well.

First, try bringing the containers back up:

```bash
cd /opt/pihole
docker compose up -d
```

If you temporarily need the Raspberry Pi to use DNS supplied by DHCP again, find the connection name with:

```bash
nmcli connection show --active
```

Then run:

```bash
sudo nmcli connection modify "connection-name" ipv4.ignore-auto-dns no ipv6.ignore-auto-dns no ipv4.dns ""
```

Apply the connection:

```bash
sudo nmcli connection up "connection-name"
```

After Pi-hole is working again, reapply the local Pi-hole DNS configuration:

```bash
sudo nmcli connection modify "connection-name" ipv4.dns "127.0.0.1" ipv4.ignore-auto-dns yes ipv6.ignore-auto-dns yes
sudo nmcli connection up "connection-name"
```

### Docker container DNS issues

Do not set the Docker daemon DNS server to <code class="language-plaintext highlighter-rouge">127.0.0.1</code>. A Docker container's loopback address points back to the container itself. It does not point to the Raspberry Pi host. If you previously created <code class="language-plaintext highlighter-rouge">/etc/docker/daemon.json</code> containing:

```json
{
  "dns": ["127.0.0.1"]
}
```

remove that DNS override before troubleshooting container DNS. If the file contains nothing else you need, remove it:

```bash
sudo rm /etc/docker/daemon.json
```

Then restart Docker:

```bash
sudo systemctl restart docker
```

Return to the Pi-hole directory and make sure both containers are running:

```bash
cd /opt/pihole
docker compose up -d
```

### Pi-hole gravity list errors

If you see output like:

```text
[i] Target: https://raw.githubusercontent.com/hagezi/dns-blocklists/refs/heads/main/wildcard/nsfw.txt
[✓] Status: No changes detected
[✓] Parsed 0 exact domains and 0 ABP-style domains (blocking, ignored 75907 non-domain entries)
Sample of non-domain entries:
- *.0-porno.com
- *.0000sex.com
- *.000freeproxy.com
- *.000pussy69pornxxxporno.com
- *.000webhostapp.co
```

the list is probably in a format Pi-hole's gravity parser cannot use directly. Use a Pi-hole-compatible domain list instead of a wildcard or <code class="language-plaintext highlighter-rouge">dnsmasq</code>-specific list.

## Updating the containers

Because this configuration currently uses the <code class="language-plaintext highlighter-rouge">latest</code> image tags, Docker will use the latest available images when you explicitly pull them.

To update the images:

```bash
cd /opt/pihole
docker compose pull
```

Then recreate the containers:

```bash
docker compose up -d
```

Check both containers afterwards:

```bash
docker compose ps
```

Then test DNS again:

```bash
docker exec unbound drill-hc @127.0.0.1 dnssec.works
docker exec -it pihole nslookup example.com 127.0.0.1
```

For a setup where reproducibility is more important than automatically following new releases, replace the <code class="language-plaintext highlighter-rouge">latest</code> image tags with specific versions that you have tested.

## Summary

This project turned out to be more demanding than a basic Pi-hole installation, mainly because Pi-hole, Unbound, Docker, and the Raspberry Pi host all had to work together correctly. Most of the problems came from small configuration details, such as the updated Pi-hole misc parameters, duplicate Unbound settings, Docker DNS behaviour, permissions, and making sure the Raspberry Pi itself used Pi-hole without creating circular DNS issues. Troubleshooting often meant checking logs, validating configuration files, and changing one thing at a time to avoid breaking an otherwise working setup.

In the end, the project was a good practical exercise in Linux administration, Docker networking, DNS, and systematic troubleshooting. It also showed how important it is to understand what a Docker image already configures by default instead of blindly duplicating settings in custom files.
