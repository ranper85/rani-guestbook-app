# Builder stage
FROM registry.access.redhat.com/ubi10/go-toolset:10.0 AS builder

# Run as root to avoid permission issues
USER 0


WORKDIR /opt/app-root/src

#Copy only module file first (helps caching)
COPY go.mod ./
COPY main.go ./

# Download dependencies 
RUN go mod tidy


# Build the application
RUN  go build -o guestbook-api .

# Runtime stage
FROM registry.access.redhat.com/ubi10-minimal:10.0


WORKDIR /opt/app-root/src


# Copy the compiled binary from the builder stage
COPY --from=builder /opt/app-root/src/guestbook-api /opt/app-root/src/guestbook-api


# Expose the backend port 
EXPOSE 8080
ENV PORT=8080

# Start the backend
CMD ["/opt/app-root/src/guestbook-api"]

