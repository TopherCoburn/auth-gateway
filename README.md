# auth-gateway

## Description

`auth-gateway` is a lightweight and scalable API gateway designed to handle authentication, authorization, and rate limiting for your microservices architecture. It acts as a single entry point for all external requests, verifying user identity and permissions before routing traffic to the appropriate backend services. This centralized approach enhances security, simplifies management, and improves overall application performance. It's built with performance and ease of integration in mind, making it a valuable addition to any modern application stack.

## Features

*   **Authentication:**
    *   Supports multiple authentication mechanisms, including:
        *   JSON Web Tokens (JWT)
        *   API Keys
        *   OAuth 2.0 (with extension points for custom providers)
    *   Configurable token validation and revocation.
    *   Integration with external identity providers (IdPs).
*   **Authorization:**
    *   Role-based access control (RBAC).
    *   Attribute-based access control (ABAC) (future development).
    *   Fine-grained permission management.
    *   Centralized policy enforcement.
*   **Rate Limiting:**
    *   Configurable rate limits based on IP address, user ID, or API key.
    *   Protection against denial-of-service (DoS) attacks.
    *   Dynamic rate limit adjustments.
*   **Request Routing:**
    *   Intelligent routing based on request path, headers, or other criteria.
    *   Load balancing across multiple backend instances.
    *   Health checks for backend services.
*   **Logging and Monitoring:**
    *   Comprehensive logging of all requests and responses.
    *   Integration with popular monitoring tools (e.g., Prometheus, Grafana).
    *   Real-time performance metrics.
*   **Configuration:**
    *   Configuration via environment variables and YAML files.
    *   Hot reloading of configuration changes.
    *   Centralized configuration management (e.g., using Consul or etcd).
*   **Extensibility:**
    *   Plugin architecture for custom authentication, authorization, and logging.
    *   Middleware support for request and response manipulation.
    *   Easy integration with existing infrastructure.

## Technologies Used

*   **Programming Language:** Go
*   **Web Framework:** Gin Gonic
*   **Configuration Management:** Viper
*   **Logging:** Zap
*   **Testing:** Go Testing Library, Mockery
*   **Docker:** Docker, Docker Compose

## Installation

### Prerequisites

*   Go (version 1.20 or higher)
*   Docker (optional, for containerized deployment)
*   Docker Compose (optional, for local development)

### Building from Source

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/[your-username]/auth-gateway.git
    cd auth-gateway
    ```

2.  **Build the `auth-gateway` binary:**

    ```bash
    go build -o auth-gateway cmd/gateway/main.go
    ```

### Running with Docker

1.  **Build the Docker image:**

    ```bash
    docker build -t auth-gateway .
    ```

2.  **Run the Docker container:**

    ```bash
    docker run -p 8080:8080 auth-gateway
    ```

### Running with Docker Compose (Development)

1. Ensure you have Docker Compose installed.

2.  **Start the application:**

    ```bash
    docker-compose up -d
    ```

This will start the `auth-gateway` container with pre-configured defaults.  You may need to adjust the `docker-compose.yml` file to suit your specific needs (e.g., mapping volumes for configuration).

### Configuration

The `auth-gateway` can be configured using environment variables or a YAML configuration file.  By default, it will look for a file named `config.yaml` in the same directory as the executable.

**Example `config.yaml`:**

```yaml
port: 8080
log_level: info

authentication:
  type: jwt
  jwt:
    secret: your-super-secret-key
    issuer: auth-gateway

routes:
  - path: /api/v1/users
    methods: [GET, POST]
    upstream_url: http://user-service:8081
    authentication_required: true
    authorization_required: false
  - path: /api/v1/public
    methods: [GET]
    upstream_url: http://public-service:8082
    authentication_required: false
    authorization_required: false


rate_limiting:
  enabled: true
  requests_per_minute: 100
  burst_size: 20
```

**Environment Variables:**

Environment variables can be used to override the values in the configuration file. The variables should be prefixed with `AUTH_GATEWAY_`. For example, to set the port using an environment variable, you would use `AUTH_GATEWAY_PORT=8080`.

### Running the Binary Directly

After building from source, you can run the binary directly:

```bash
./auth-gateway
```

Make sure the `config.yaml` file is in the same directory or specify the configuration file path using the `--config` flag:

```bash
./auth-gateway --config /path/to/your/config.yaml
```

## Usage

Once the `auth-gateway` is running, you can send requests to its port (default: 8080). The gateway will then authenticate and authorize the request before routing it to the appropriate backend service.

For example, if you have configured a route for `/api/v1/users` that requires authentication, you will need to include a valid JWT token in the `Authorization` header of your request:

```
Authorization: Bearer <your-jwt-token>
```

## Contributing

Contributions are welcome! Please submit a pull request with your changes.  Follow these guidelines:

*   Write clear and concise commit messages.
*   Include tests for any new features or bug fixes.
*   Adhere to the coding style of the project.
*   Create a new branch from `main` for your changes.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.