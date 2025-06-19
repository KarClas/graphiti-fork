FROM python:3.11-slim

WORKDIR /app

# Install uv for package management
RUN apt-get update && apt-get install -y curl && \
    curl -LsSf https://astral.sh/uv/install.sh | sh && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Add uv to PATH
ENV PATH="/root/.local/bin:${PATH}"

ENV MCP_SERVER_HOST="0.0.0.0"

# Copy entire graphiti-core project
COPY . /app/graphiti_core_project

# Copy MCP server files
COPY mcp_server/pyproject.toml .
COPY mcp_server/graphiti_mcp_server.py .

# Update the pyproject.toml to point to the copied graphiti-core project
RUN sed -i 's|path = "../"|path = "/app/graphiti_core_project"|g' pyproject.toml

# Install dependencies
RUN uv sync

EXPOSE 8000

# Set environment variables
ENV PYTHONUNBUFFERED=1

# Command to run the application
CMD ["uv", "run", "graphiti_mcp_server.py"]
