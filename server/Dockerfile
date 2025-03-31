# Etapa de construcción
FROM golang:1.24.1-alpine3.21 as builder

# Instalar dependencias de compilación para SQLite3
RUN apk add --no-cache gcc musl-dev

WORKDIR /app

# Copiar dependencias primero para cachear
COPY go.mod go.sum ./
RUN go mod download

# Copiar el código fuente
COPY . .

# Compilar el binario con CGO habilitado
RUN CGO_ENABLED=1 GOOS=linux go build -ldflags="-s -w" -tags sqlite3 -o main .

# Etapa final
FROM alpine:latest

# Instalar runtime dependencies para SQLite
RUN apk add --no-cache sqlite

WORKDIR /app

# Copiar el binario compilado
COPY --from=builder /app/main .

# Puerto expuesto (ajusta según tu aplicación)
EXPOSE 8080

# Comando de ejecución
ENTRYPOINT ["./main"]