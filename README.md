# DNS with Bind9 Docker Configuration

This repository contains a Docker Compose setup for running a Bind9 DNS server, alongside the necessary DNS configuration files. It is designed to be easily customizable and deployable.

You can use this DNS for your home, so that you make your internet faster.

## Contents

- `docker-compose.yml`: Docker Compose file to set up the Bind9 DNS server.
- `config/`: Directory containing configuration files for the DNS server.
  - `local-home.zone`: DNS zone file for `local.home`. You can rename the file according to your need.
  - `named.conf`: Named configuration file for DNS server settings.
  - `sub-domain-com.zone`: DNS zone file for `sub.domain.com`.

## Quick Start

1. **Clone the Repository**

   Clone this repository to your local machine to get started.

   ```bash
   git clone git@github.com:Anybody2007/dns-bind9.git
   ```

2. **Configure DNS Records**

    Suggestion - It's better to avoid .local doamin, because internet does have this domain. So that means you will be forwarding your queries to the Root DNS for a local query.

    Update the DNS records in config/local-home.zone and config/sub-domain-com.zone as per your requirements.

3. **Start Docker Compose**

    Run the following command in the root directory of this project:

    ```bash
    docker-compose up -d
    ```

    This will start the DNS server in a detached mode.

## Configuration Details

### 1. Docker Compose

The `docker-compose.yml` file sets up the Bind9 DNS server with the following specifications:

- **Image:** `ubuntu/bind9:latest`
- **Ports:** 53 (TCP and UDP)
- **Volumes:**
  - Config directory mapped to `/etc/bind`
  - Cache directory mapped to `/var/cache/bind`
  - Records directory mapped to `/var/lib/bind`
- **Environment Variables:**
  - `BIND9_USER`: Set to `root`
  - `TZ`: Timezone set to `Asia/Kolkata`

### 2. DNS Zone Files

#### `local-home.zone`

- Defines the DNS settings for the local network domain `local.home`.
- Contains SOA, NS, and A records.
- The IP addresses should be updated according to your network setup.

#### `sub-domain-com.zone`

- Provides DNS settings for the external domain `sub.domain.com`.
- Has a structure similar to `local-home.zone` with customizable A records.
- This file is an example of how you can set up a zone for any domain you own.

### 3. Named Configuration (`named.conf`)

- **ACLs (Access Control Lists):** The configuration starts with defining ACLs to restrict DNS access. This is a crucial step to ensure that your DNS server is not used outside your intended network, enhancing security.

- **Forwarders:** 
  - Forwarders are DNS servers to which queries are forwarded if the local server does not have the answer.
  - In this setup, Cloudflare's malware protection DNS servers (`1.1.1.2` and `1.0.0.2`) are used as forwarders. Feel free to substitute these with your preferred DNS servers.

- **Zone Configuration:**
  - Each domain managed by the DNS server has its own zone configuration.
  - The `named.conf` file includes definitions for two zones:
    - `sub.domain.com`: A sample external domain. The corresponding zone file (`sub-domain-com.zone`) includes the necessary DNS records for this domain.
    - `local.home`: A domain for the local network. The `local-home.zone` file outlines the DNS settings for devices and services within your local network.
  - These zone files demonstrate how to configure DNS for both local and external domains, providing a template that can be adapted for other domains.

This section of the README gives users a comprehensive understanding of the DNS server's configuration, highlighting the purpose and customization possibilities of each component. It guides you through setting up your own domains and emphasizes the importance of security with ACLs.


## Customization

You can customize the DNS settings by editing the zone files and named.conf as per your requirements. Make sure to update the IP addresses in the zone files to match your network configuration.
