# Nginx with SSL Behind Traefik Reverse Proxy

This project demonstrates how to set up an Nginx container with SSL enabled, routed through a Traefik reverse proxy, making it publicly accessible. It leverages Docker Compose for container orchestration and includes configurations for SSL certificate management and reverse proxy routing.

## Features

- **Nginx Docker Container**: Serves web content on HTTPS.
- **Traefik Reverse Proxy**: Handles routing and SSL termination.
- **Public Accessibility**: Accessible via a domain name (`example.com`).
- **SSL Configuration**: Utilizes Let's Encrypt certificates.

## Prerequisites

- Docker and Docker Compose installed.
- A domain name pointing to your server.
- Ports `80` and `443` available on your host machine.
- External Docker network configured for Traefik and Nginx communication.

## Setup Instructions

### Step 1: Clone the Repository

```bash
git clone https://github.com/Grey-PHOENIX001/Reverse-Proxy.git
cd Reverse-Proxy
```

### Step 2: Configure External Network

Ensure an external Docker network exists for Traefik and Nginx:

```bash
docker network create my-custom-network
```

### Step 3: Update DNS Records

Point your domain `example.com` to your server's public IP address.

### Step 4: Deploy the Stack

- Ensure to change the domain name with your obtained public domain and forward System IP with 443 port (Not the docker IP, its your host system IP) to your static IP.
- Also create a directory certs in the path mentioned in config and add your ssl certificates there.
   
Start the containers using Docker Compose:

```bash
docker-compose up -d
```

### Step 5: Verify the Setup

Access your application:

- HTTP: [http://example.com](http://example.com) (redirected to HTTPS)
- HTTPS: [https://example.com](https://example.com)

## Project Structure

```plaintext
.
├── docker-compose.yml  # Docker Compose configuration for traefik
├──acme.json/           # Contains SSL Certificates
├── config/             # Traefik configuration 
├── certs/              # SSL certificates - cloned from host for all cert references
├── nginx-ubuntu/       # Contains docker compose and nginx config
└── README.md           # Project documentation
```

## Docker Compose Configuration

### `docker-compose.yml`

```yaml
services:
  nginx:
    container_name: nginx_demo
    image: nginx:latest
    networks:
      - my-custom-network
    labels:
      - traefik.enable=true
      - traefik.http.routers.nginx-http.rule=Host(`example.com`)
      - traefik.http.routers.nginx-http.entrypoints=web
      - traefik.http.routers.nginx-https.rule=Host(`example.com`)
      - traefik.http.routers.nginx-https.entrypoints=websecure
      - traefik.http.routers.nginx-https.tls=true
      - traefik.http.routers.nginx-https.tls.certresolver=myresolver
    restart: unless-stopped

networks:
  my-custom-network:
    external: true
```

## SSL Certificates

Traefik automatically generates and manages SSL certificates using Let's Encrypt. Ensure the `certresolver` configuration in Traefik is properly set up.

## Troubleshooting

- **Port Already in Use**: Stop any service using ports `80` or `443` on the host.
- **DNS Issues**: Verify that `example.com` resolves to your server's IP.
- **Certificate Errors**: Check Traefik logs for certificate generation issues.

## Contribution

Feel free to submit issues or pull requests to improve this project.

## License

This project is licensed under the MIT License. See `LICENSE` for details.
