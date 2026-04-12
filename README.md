# Dockerized ZAP

Runs [OWASP ZAP](https://www.zaproxy.org/) as a persistent daemon via Docker Compose, accessible over its REST API.

## Prerequisites

- Docker & Docker Compose

## Setup

1. Copy the sample env file and fill in your values:

   ```bash
   cp .sample.env .env
   ```

2. Edit `.env`:

   | Variable      | Description                        | Example  |
   |---------------|------------------------------------|----------|
   | `ZAP_PORT`    | Port ZAP listens on                | `9876`   |
   | `ZAP_API_KEY` | API key for authenticating requests | `abc`    |

## Usage

Start ZAP:

```bash
docker compose up -d
```

The ZAP API will be available at `http://localhost:<ZAP_PORT>`. See the [ZAP REST API docs](https://www.zaproxy.org/docs/api/) for available endpoints.

Example API call:

```bash
curl "http://localhost:9876/JSON/core/view/version/?apikey=<ZAP_API_KEY>"
```

Stop ZAP:

```bash
docker compose down
```
