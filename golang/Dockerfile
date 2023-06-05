# Start from the latest Golang base image
FROM golang:latest AS builder

# Set the Current Working Directory inside the builder container
WORKDIR /app

# Copy go mod and sum files
COPY go.mod go.sum ./

# Download all dependencies. They will be cached if the go.mod and go.sum files do not change
RUN go mod download

# Copy the source from the current directory to the Working Directory inside the builder container
COPY . .

# Build the Go app
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -installsuffix cgo -o build/fizzbuzz .

# Use Distroless as minimal base image to package the Go application
FROM gcr.io/distroless/base-debian10

# Set the Current Working Directory inside the container
WORKDIR /app

# Copy the binary from builder
COPY --from=builder /app/build/fizzbuzz .

COPY . .

# This container exposes port 8080 to the outside world
EXPOSE 8080

# Run the binary
CMD ["./fizzbuzz", "serve"]
