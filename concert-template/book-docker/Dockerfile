FROM ubuntu:latest
WORKDIR /app

RUN apt update && apt install -y ca-certificates
RUN update-ca-certificates

COPY book .
RUN chmod +x ./book

EXPOSE 8080
CMD ["./book"]
