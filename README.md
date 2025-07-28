# Uptrace Docker Compose

This is a Docker Compose configuration for Uptrace, a distributed tracing tool.

## Services

- **ClickHouse**: A columnar database for storing traces.
- **PostgreSQL**: A relational database for storing metadata.
- **Redis**: An in-memory data structure store.
- **Uptrace**: The main application.
- **OpenTelemetry Collector**: A component for receiving and processing telemetry data.

## Volumes

- `ch_data`: Data volume for ClickHouse.
- `pg_data3`: Data volume for PostgreSQL.

## Usage

1. Ensure you have Docker and Docker Compose installed.
2. Clone this repository or download the `docker-compose.yml` file.
3. Navigate to the directory containing the `docker-compose.yml` file.
4. Run the following command to start the services:

   ```bash
   docker-compose up -d
   ```

5. Access Uptrace UI at `https://localhost:14318`

## Certificates

To use HTTPS, you need to generate or provide your own SSL certificates. Place your certificate files in the `certs` directory and ensure they are named `server.crt` and `server.key`.

For production environments, I'd recommend using Let's Encrypt or Cloudflare for obtaining valid SSL certificates. You can use certbot to generate these certificates, and renew them automatically.

If using in a development environment, you can generate self-signed certificates using the Make file provided inside the folder `certs`:

```bash
make
```

This will create the necessary certificates with a CA. After that, you need to trust the CA in your operating system, if using in a development environment.

In Arch Linux, you can do this by running:

```bash
sudo trust anchor ./Uptrace_CA.crt
```

## Conecting Exporters

To connect exporters to Uptrace, you must configure this (The example bellow is for a Rust application with OpenTelemetry):

```bash
OTEL_RESOURCE_ATTRIBUTES = "service.name=MY_SERVICE,service.version=1.0.0,deployment.environment=development";
OTEL_EXPORTER_OTLP_ENDPOINT = "https://localhost:14317";
OTEL_EXPORTER_OTLP_HEADERS = "uptrace-dsn=https://project1_secret@localhost:14318?grpc=4317";
```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
