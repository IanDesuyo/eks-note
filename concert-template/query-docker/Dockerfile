FROM ubuntu:latest
WORKDIR /app

RUN apt update && apt install -y ca-certificates
RUN update-ca-certificates

COPY query .
RUN chmod +x ./query

EXPOSE 8080
CMD ["./query"]
