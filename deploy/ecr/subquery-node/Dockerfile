# Use the specified image as the base
FROM subquerynetwork/subql-node-stellar:latest

# Set working directory in the container
WORKDIR /app

# Copy files from the local /app folder to the container's /app folder
COPY ./src /app/
COPY ./package.json /app/
COPY ./project.yaml /app/
COPY ./schema.graphql /app/
COPY ./tsconfig.json /app/

# Set environment variables (You can set them here or use docker-compose or Kubernetes to set them)
ENV DB_USER=postgres \
    DB_DATABASE=postgres \
    DB_PASS=please_set_at_runtime \
    DB_HOST=please_set_at_runtime \
    DB_PORT=5432

# Define the healthcheck
HEALTHCHECK --interval=3s --timeout=5s --retries=10 CMD curl --fail http://subquery-node:3000/ready || exit 1

# The command to run when the container starts (can be overridden by docker-compose or Kubernetes)
CMD ["${SUB_COMMAND}", "-f=/app", "--db-schema=app", "--workers=1", "--batch-size=5"]