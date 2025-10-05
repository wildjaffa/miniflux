FROM docker.io/library/golang:alpine3.22 AS build
# Install git for `go get`/module support. No need for build-base or make.
RUN apk add --no-cache git
ADD . /go/src/app
WORKDIR /go/src/app
# Build a static binary suitable for a minimal runtime image.
ENV CGO_ENABLED=0
# Use a simple go build instead of make to avoid requiring host tools or extra make targets.
RUN go build -ldflags "-s -w" -o miniflux

FROM docker.io/library/alpine:3.22



EXPOSE 8080
ENV LISTEN_ADDR=0.0.0.0:8080
RUN apk --no-cache add ca-certificates tzdata
COPY --from=build /go/src/app/miniflux /usr/bin/miniflux
USER 65534
CMD ["/usr/bin/miniflux"]
