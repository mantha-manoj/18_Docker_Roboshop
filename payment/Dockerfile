FROM python:3.9-alpine3.21 AS builder
WORKDIR /app
COPY payment.ini .
COPY *.py .
COPY requirements.txt .
RUN pip3 install -r requirements.txt

FROM python:3.9-alpine3.21
EXPOSE 8080
WORKDIR /app
COPY --from=builder /app /app
ENV CART_HOST="cart" \
    CART_PORT=8080 \
    USER_HOST="user" \
    USER_PORT=8080 \
    AMQP_HOST="rabbitmq" \
    AMQP_USER="roboshop" \
    AMQP_PASS="roboshop123"
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop
RUN chown -R roboshop:roboshop /app
USER roboshop
CMD ["uwsgi", "--ini", "payment.ini"]