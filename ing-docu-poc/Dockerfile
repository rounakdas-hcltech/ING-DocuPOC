#Build the site
FROM python:3.11-slim as builder
WORKDIR /app
RUN pip install mkdocs mkdocs-material mkdocs-pdf
COPY mkdocs.yml .
COPY docs/ ./docs/
RUN mkdocs build

#Serve with NGINX
FROM nginx:alpine
COPY --from=builder /app/site /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]