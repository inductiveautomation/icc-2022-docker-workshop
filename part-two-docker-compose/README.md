# Part Two - Docker Compose Introduction

In part two, we'll explore how to leverage Docker Compose to document your container configurations so they're reproducible and much easier to manage.

![Overview Diagram](.assets/diagram.svg)

## Files

- `.env` - These environment variables will direct Docker Compose functionality.
- `docker-compose.basic.yml` - This Compose YAML defines a stack with very basic configuration.  There is no automated gateway network connectivity or gateway backup restorations.
- `docker-compose.yml` - This Compose YAML uses some more constructs such as secrets and additional container networks.  Additionally, restoration of GWBK's to the Ignition gateways, integrated gateway network / EAM connectivity, and more are packaged in.
- `docker-compose.verbose.yml` - This is an aggressively commented version of `docker-compose.yml`.

## Docker Compose Commands

### General

```bash
# List available Compose commands
docker compose --help
```

```bash
# List help for a given Compose subcommand, such as `ls`
docker compose ls --help
```

```bash
# List "known" Compose stacks and their expected root config file paths
docker compose ls
```

```bash
# Print out the computed "config" of the current Compose solution
# Note: this will also perform validation prior to running lifecycle commands
docker compose config
```

### Lifecycle

```bash
# Bring "up" the stack, which will include creating networks/volumes and launching containers
docker compose up -d
```

```bash
# Stop all containers, but leave them intact.
docker compose stop
```

```bash
# Stop/remove all containers and networks.  Leave volumes in place.
# NOTE: if you want to remove volumes, add a `-v` flag below (USE WITH CAUTION!)
docker compose down
```

```bash
# Stop the `edge` service (i.e. container)
docker compose stop edge
# Start it back up
docker compose start edge
# ... or just restart it
docker compose restart edge
```

### Monitoring

```bash
# Check status of your Compose "services" (i.e. containers) in the current solution
docker compose ps
```

```bash
# Follow the logs (starting with the last 50 lines) of all containers in the current solution.  Use `Ctrl-C` to break out of the "follow".
docker compose logs --tail=50 -f
```

```bash
# Show the last 50 lines of the `gateway` service (i.e. container)
docker compose --tail=50 gateway
```

```bash
# Get really nitty-gritty and debug the events from your containers
docker compose events
```

### Interaction

```bash
# Get a `bash` shell into the `gateway` service (i.e. container)
docker compose exec gateway bash
```

```bash
# Copy a file from within a given service (i.e. container) out to the host.
# In this example, we'll extract the `edge` Gateway Network UUID file from the 
# container to a relative path on our host.
docker compose cp edge:/usr/local/bin/ignition/data/.uuid gw-init/edge-uuid-copied.txt
```

## Other

If you're coming to this after the workshop and just want to launch the stack, make sure you define passwords for each of the configured secrets prior to launching the stack.

## Links

There are various reference links in the solution files.  Below are some links to the services that will be deployed in your Compose stack:

- http://gateway.vcap.me:9088 - Ignition Gateway (Standard Edition)
- http://edge.vcap.me:9089 - Ignition Edge Gateway
- http://pgadmin.vcap.me:9090 - pgAdmin UI for managing your database
