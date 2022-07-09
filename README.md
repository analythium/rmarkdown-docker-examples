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

## License

MIT License (c) 2022 Analythium Solutions Inc.
