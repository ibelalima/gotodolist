FROM golang:1.24-alpine AS builder

WORKDIR /app

RUN apk add --no-cache git

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o gotodolist monolito/main.go

FROM alpine:3.21

WORKDIR /app

RUN adduser -D -u 10001 appuser
USER appuser

COPY --from=builder /app/gotodolist .
COPY --from=builder /app/desacoplado/frontend/ ./desacoplado/frontend/

EXPOSE 8081

ENTRYPOINT ["./gotodolist"]