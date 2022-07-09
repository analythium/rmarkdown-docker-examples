# R Markdown Examples with Docker

This repository is from the blog post [xxx]().

## Runtime: shiny

Read [Introduction to interactive documents](https://shiny.rstudio.com/articles/interactive-docs.html) for details on this runtime.

```bash
docker build -f Dockerfile.shiny -t psolymos/rmd:shiny .

docker run -p 8080:3838 psolymos/rmd:shiny
```

## Runtime: shinyrmd

> The runtime `shinyrmd` was introduced in [rmarkdown version 2.5](https://rmarkdown.rstudio.com/docs/news/index.html#rmarkdown-25) as an alias for the runtime `shiny_prerendered`.

Read [Prerendered Shiny documents](https://rmarkdown.rstudio.com/authoring_shiny_prerendered.HTML) for details on this runtime.

```bash
docker build -f Dockerfile.shinyrmd -t psolymos/rmd:shinyrmd .

docker run -p 8080:3838 psolymos/rmd:shinyrmd
```

## Runtime: static

One option is to render the HTML output locally and copy all these static assets into an Nginx image:

```dockerfile
FROM nginx:alpine
COPY runtime-static /usr/share/nginx/html
CMD ["nginx", "-g", "daemon off;"]
```

This implies that you have all the necessary tools installed on your local machine.

Alternatively, you can use [multi-stage build](https://docs.docker.com/develop/develop-images/multistage-build/) to keep your image size small:

```bash
docker build -f Dockerfile.static -t psolymos/rmd:static .

docker run -p 8080:80 psolymos/rmd:static
```

Another option is to use the static mode of the [of-watchdog](https://github.com/openfaas/of-watchdog) from the [OpenFaaS](https://www.openfaas.com/) project.

> The of-watchdog implements a HTTP server listening on port 8080, and acts as a reverse proxy for running functions and microservices. It can be used independently, or as the entrypoint for a container with OpenFaaS.

```bash
docker build -f Dockerfile.static2 -t psolymos/rmd:static2 .

docker run -p 8080:8080 psolymos/rmd:static2
```

Let's compare the image sizes:

```bash
docker image ls psolymos/rmd --format "{{.Repository}}\t{{.Tag}}\t{{.Size}}"
# psolymos/rmd    static2     32.7MB
# psolymos/rmd    static      24.3MB
# psolymos/rmd    shinyrmd    1.06GB
# psolymos/rmd    shiny       1.05GB
```

## License

MIT License (c) 2022 Analythium Solutions Inc.
