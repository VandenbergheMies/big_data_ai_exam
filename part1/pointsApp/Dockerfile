FROM python:3.12.0-alpine as build-image
WORKDIR /app
RUN apk add --no-cache linux-headers g++
COPY ./requirements.txt ./
RUN pip wheel --wheel-dir=/root/wheels -r requirements.txt

FROM python:3.12.0-alpine as production-image
WORKDIR /app
RUN addgroup -S appuser && adduser -S appuser -G appuser
COPY --from=build-image /root/wheels /root/wheels
COPY --from=build-image /app/requirements.txt ./
RUN pip install --no-index --find-links=/root/wheels -r requirements.txt
COPY . .
EXPOSE 5000
USER appuser
CMD ["python", "app.py"]
