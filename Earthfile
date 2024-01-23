VERSION 0.8

FROM golang:1.16-alpine3.13
WORKDIR /go-example

deps:
    COPY go.mod go.sum ./
    RUN go mod download
    SAVE ARTIFACT go.mod AS LOCAL go.mod
    SAVE ARTIFACT go.sum AS LOCAL go.sum

build:
    FROM +deps
    COPY main.go .
    RUN go build -o build/go-example main.go
    SAVE ARTIFACT build/go-example /go-example AS LOCAL build/go-example

docker:
    COPY +build/go-example .
    ENTRYPOINT ["/go-example/go-example"]
    SAVE IMAGE --push earthly/examples:go

#####
# Provider Specific Targets
#####

# Demo for Google Cloud Build, showing use of a dedicated service account
gcp-cloudbuild:
    FROM gcr.io/cloud-builders/gsutil
    RUN --mount type=secret,id=+secrets/earthly-technologies/google/cloudbuild-service-key,target=/root/key.json gcloud auth activate-service-account --key-file=/root/key.json && gsutil ls -p earthly-jupyterlab
