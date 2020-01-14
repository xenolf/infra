# Personal Cloud IaC

This is the repo I use for my personal cloud server, hosted as a VPS. This draws heavily from [Funky Penguin's Geek Cookbook](https://geek-cookbook.funkypenguin.co.nz/), but uses a single node rather than a swarm and is targeted against Traefik 2.

## Services

- Traefik load balancer, with dashboard protected by Keycloak.
- Keycloak IDM.
- Portainer management console.

## Deploying a new service

Services each live in their own directory, and are driven primarily by a `docker-compose.yml` file. The `example` directory includes several configuration options that can be used to design other services.

## Starting from scratch

The following process is necessary to "bootstrap" a machine.

1. Set the `BASE_DOMAIN` and `DATA_DIR` environment variables.

2. Create the traefik network.

   `docker network create traefik`

3. Start the traefik service.

   `docker-compose -f traefik/docker-compose.yml up -d`

4. Start keycloak.

   `docker-compose -f keycloak/docker-compose.yml up -d app`

5. Open `keycloak.$BASE_DOMAIN` and log in with admin / password.

6. Change the admin credentials.

7. Create a new client called `cloud-admin`

   - Access type: confidential
   - Redirect URI: `http://forwardauth.$BASE_DOMAIN/*`

8. Copy `keycloak/forward-auth.env.example` to `keycloak/forward-auth.env` and edit.

9. Start keycloak forwardauth.

   `docker-compose -f keycloak/docker-compose.yml up -d`