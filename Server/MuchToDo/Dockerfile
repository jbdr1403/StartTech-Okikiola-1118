# ---------- Build stage ----------
FROM golang:1.25-alpine AS builder

WORKDIR /app

# Install git (some modules may require it)
RUN apk add --no-cache git

# Cache deps
COPY go.mod go.sum ./
RUN go mod download

# Copy source
COPY . .

# Build binary (entrypoint is cmd/api/main.go)
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o server ./cmd/api

# ---------- Run stage ----------
FROM alpine:3.19

WORKDIR /app

# TLS certs (for MongoDB Atlas SRV/SSL)
RUN apk add --no-cache ca-certificates

COPY --from=builder /app/server /app/server

EXPOSE 8080

# Run
CMD ["/app/server"]

