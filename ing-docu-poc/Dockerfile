#Build the site
FROM python:3.11-slim as builder

# Copy entire repo as context to allow conditional inclusion of team docs
WORKDIR /src
COPY . .

# Prepare MkDocs app directory
WORKDIR /app
RUN pip install mkdocs mkdocs-material mkdocs-pdf

# Copy main module config and docs
RUN cp /src/ing-docu-poc/mkdocs.yml ./ && \
    mkdir -p ./docs && \
    cp -r /src/ing-docu-poc/docs/. ./docs/

# Optionally include team docs if present
RUN if [ -d "/src/ing-docu-poc-team1/docs" ]; then \
      mkdir -p ./docs/team1 && cp -r /src/ing-docu-poc-team1/docs/. ./docs/team1/; \
    fi && \
    if [ -d "/src/ing-docu-poc-team2/docs" ]; then \
      mkdir -p ./docs/team2 && cp -r /src/ing-docu-poc-team2/docs/. ./docs/team2/; \
    fi

RUN mkdocs build

#Serve with NGINX
FROM nginx:alpine
COPY --from=builder /app/site /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]