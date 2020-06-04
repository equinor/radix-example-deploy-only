# Docker is a tool designed to make it easier to create, deploy, and run applications by using containers.
# Containers allow a developer to package an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package
# Dockerfile is a definition of how to create a Docker image.


# Build stage using image with go compiler installed
FROM golang:1.14-alpine3.12 as build
ENV GOPATH /go
COPY . /go/src/github.com/equinor/radix-example-deploy-only/
WORKDIR /go/src/github.com/equinor/radix-example-deploy-only/

RUN apk add --no-cache git
RUN go get -t -v ./... # go get -d -v
RUN go build -v main.go


# Runtime stage using simple linux image
FROM alpine:3.12

# Create app directory
WORKDIR /usr/src/app

# Copy executable built by go
COPY --from=build /go/src/github.com/equinor/radix-example-deploy-only/main .

# Container port is not important for the exposure on Radix
EXPOSE 8080

# Command run on docker
CMD [ "sh", "-c", "/usr/src/app/main" ]
