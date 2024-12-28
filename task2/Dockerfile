FROM alpine:latest
RUN apk add --no-cache python3
EXPOSE 8000
COPY ./main.sh ./main.sh
CMD ["/bin/sh", "./main.sh"]

