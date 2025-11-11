FROM golang:1.22-alpine AS builder 

WORKDIR ./temp

COPY . .

RUN go mod download 

RUN go build -o ./bin/product-catalog.exe ./

FROM alpine AS release

WORKDIR ./temp

COPY ./products ./products 
COPY --from=builder ./bin/product-catalog ./

EXPOSE $(PRODUCT_CATALOG_PORT)

ENTRYPOINT [*./product-catalog]