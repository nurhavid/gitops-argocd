# Start from golang:1.18-alpine base image
FROM golang:1.18-alpine AS build-env

# The latest alpine images don't have some tools like (`git` and `bash`).
# Adding git, bash and openssh to the image
# RUN apk update && apk upgrade && \
#     apk add --no-cache bash git openssh
WORKDIR /app
RUN apk add --no-cache git
COPY . .
RUN go mod download

RUN CGO_ENABLED=0 GOOS=linux go build -o main ./main.go


FROM alpine:latest

WORKDIR /
COPY --from=build-env /app/main main


ENV REDIS_HOST redis.digitaltrx
ENV REDIS_PORT 6379

EXPOSE 80
CMD ["/main"]
