FROM golang:1.17-alpine

ENV GIN_MODE=release \
    GOOS=linux \
    GOARCH=amd64

WORKDIR /app

COPY go.mod ./
COPY go.sum ./
RUN go mod download

COPY . .

RUN go build cmd/server/main.go

EXPOSE 8080

CMD [ "./main" ]