FROM golang:1.20-bookworm as builder
WORKDIR /app
COPY main.go go.mod go.sum /app/
RUN go build -o app

FROM debian:stable-slim
RUN apt-get update
RUN apt-get install -y ca-certificates
COPY --from=builder /app/app /app/app
COPY --from=datadog/serverless-init /datadog-init /app/datadog-init

ENV DD_SERVICE=datadog-demo-run-go
ENV DD_ENV=datadog-demo
ENV DD_VERSION=1

# this env var is needed for trace propagation to work properly in Cloud Run.
# ensure to set this variable for all Datadog-instrumented downstream services.
ENV DD_TRACE_PROPAGATION_STYLE=datadog

ENTRYPOINT ["/app/datadog-init"]
CMD ["/app/app"]
