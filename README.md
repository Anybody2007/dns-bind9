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

**Docker Compose**

    The docker-compose.yml file sets up the Bind9 DNS server with the following specifications:

    - Image: ubuntu/bind9:latest
    - Ports: 53 (TCP and UDP)
    - Volumes:
        - Config directory mapped to `/etc/bind`
        - Cache directory mapped to `/var/cache/bind`
        - Records directory mapped to `/var/lib/bind`
    - Environment Variables:
        - `BIND9_USER`: Set to `root`
        - `TZ:` Timezone set to `Asia/Kolkata`

**DNS Zone Files**

    `local-home.zone`

    - Defines the DNS settings for `local.home`.
    - Contains SOA, NS, and A records.
    - Remember to update the IP addresses as per your network setup.

    `sub-domain-com.zone`

    - Defines the DNS settings for `sub.domain.com`.
    - Similar structure to `local-home.zone` with customizable A records.

**Named Configuration (`named.conf`)**

    - Includes ACLs and options like forwarders and allow-query settings.
    - Defines the zones `sub.domain.com` and `local.home` with their respective zone files.

## Customization

    You can customize the DNS settings by editing the zone files and named.conf as per your requirements. Make sure to update the IP addresses in the zone files to match your network configuration.