# Dockerized ZAP

Runs [OWASP ZAP](https://www.zaproxy.org/) as a persistent daemon on an **Amazon EC2 instance** via Docker Compose, accessible over its REST API for remote security scanning.

## Prerequisites

* **AWS EC2 Instance:** (Ubuntu or Amazon Linux 2023 recommended).
* **Docker & Docker Compose** installed on the instance.
* **Security Group Config:** Ensure your EC2 Security Group allows **Inbound TCP** traffic on your chosen `ZAP_PORT` from your specific IP address (don't leave it open to `0.0.0.0/0`!).

## Setup

1. **Clone the repository** to your EC2 instance.
2. **Copy the sample env file**:

   ```bash
   cp .sample.env .env
   ```

3. **Edit `.env`**:
   The `ZAP_OPTIONS` variable is crucial for EC2; it configures ZAP to allow connections from your public IP, bypassing the default local-only protection.

   | Variable        | Description                               | Example          |
   |-----------------|-------------------------------------------|------------------|
   | `ZAP_PORT`      | Port ZAP listens on                       | `8080`           |
   | `ZAP_API_KEY`   | API key for authenticating requests       | `your_secure_key`|
   | `EC2_PUBLIC_IP` | The Public IPv4 address of your instance | `142.112.199.221`    |

## Usage

### Start ZAP
Run the container in detached mode. The configuration uses the `-host 0.0.0.0` flag to ensure the daemon binds to the network interface accessible via the EC2 public IP.

```bash
docker compose up -d
```

### Accessing the API
The ZAP API will be available at `http://<EC2_PUBLIC_IP>:<ZAP_PORT>`. 

> [!IMPORTANT]  
> If you cannot connect, double-check that your **AWS Security Group** allows traffic on the `ZAP_PORT`.

**Example API call from your local machine:**

```bash
curl "http://142.112.199.221:8080/JSON/core/view/version/?apikey=your_secure_key"
```

### Stop ZAP
To stop the daemon and remove the containers:

```bash
docker compose down
```

## Security Best Practices
* **API Key:** Never leave the `ZAP_API_KEY` as the default. 
* **IP Whitelisting:** In the AWS Console, restrict access to your `ZAP_PORT` only to your office or home IP address to prevent unauthorized actors from using your instance as a scanning proxy.
* **HTTPS:** For production environments, consider putting ZAP behind an Nginx reverse proxy with Let's Encrypt SSL.