# Holberton Softy Pinko Docker

This repository contains a step-by-step Docker exercise that builds the Softy Pinko application from a basic container into a multi-container stack with a Flask API, an Nginx-served front end, and an Nginx reverse proxy.

## Project Overview

The project is organized by task directories. Each task keeps its own Dockerfiles and configuration so the evolution of the deployment can be reviewed independently.

| Task | Purpose |
| --- | --- |
| `task0` | Builds a basic Ubuntu image that prints `Hello, World!`. |
| `task1` | Adds a Flask API container with the `/api/hello` endpoint on port `5252`. |
| `task2` | Adds an Nginx front-end container serving the Softy Pinko static site on port `9000`. |
| `task3` | Updates the front end so it can communicate with the back-end API. |
| `task4` | Introduces Docker Compose for the front-end and back-end services. |
| `task5` | Adds an Nginx reverse proxy that routes traffic to the front end and API. |
| `task6` | Final stack with Compose, proxy routing, and API scaling instructions. |

## Final Architecture

The final implementation in `task6` runs three service types:

- `front-end`: Nginx container serving `softy-pinko-front-end` on port `9000` inside the Docker network.
- `back-end`: Flask API container exposing `/api/hello` on port `5252` inside the Docker network.
- `proxy`: Nginx reverse proxy publishing port `80` on the host.

Proxy routing in `task6/proxy/proxy.conf`:

- `/` routes to `front-end:9000`
- `/api` routes to `back-end:5252`

## Requirements

- Docker
- Docker Compose

## Running the Final Stack

From the `task6` directory:

```bash
docker-compose up --build
```

Then open:

```text
http://localhost
```

To test the API through the proxy:

```bash
curl http://localhost/api/hello
```

Expected response:

```text
Hello, World!
```

## Scaling the API

`task6/2-api-servers.txt` records the command for running two back-end containers:

```bash
docker-compose up --scale back-end=2
```

## Useful Task Commands

Build and run the basic Flask API from `task1`:

```bash
cd task1
docker build -t softy-pinko-api:task1 .
docker run -p 5252:5252 softy-pinko-api:task1
```

Build and run the final application from `task6`:

```bash
cd task6
docker-compose up --build
```

Stop the Compose stack:

```bash
docker-compose down
```

## Repository Notes

The copied Softy Pinko front-end directories are tracked by Git as nested repositories. If a nested front-end directory is committed internally, the parent repository still needs to record the updated nested commit pointer with `git add <path-to-nested-repo>`.

For example:

```bash
git add task6/front-end/softy-pinko-front-end
```

Without that parent-level update, `git status` can show the nested directory as modified even when the nested repository itself is clean.
