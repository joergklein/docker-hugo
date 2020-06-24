# A Docker Image for static web sites generated with Hugo.

Hugo is a static HTML and CSS website generator written in [Golang][1]. It is
optimized for speed, ease of use, and configurability. Hugo takes a directory
with content and templates and renders them into a full HTML website.
[joergklein/hugo][2] is a [Docker][3] base image for static web sites generated
with [Hugo][4].

Built with love by [bep][5], [spf13][6] and [friends][7].

[1]: https://golang.org
[2]: https://hub.docker.com/r/joergklein/hugo
[3]: https://docker.com
[4]: https://gohugo.io
[5]: https://github.com/bep
[6]: http://spf13.com
[7]: https://github.com/gohugoio/hugo/graphs/contributors

Images derived from this image can either run as a stand-alone server, or
function as a volume image for your web server. You can also use them in a CI/CD
system such as Gitlab CI to build your content prior to bundling it into a
standalone webserver container such as `httpd:alpine`.

### Prerequisites

The image is based on the following directory structure:

```bash
.
├── Dockerfile
└── site
    ├── config.toml
    ├── content
    │   └── ...
    ├── layouts
    │   └── ...
    └── static
    └── ...
```

In other words, your Hugo site resides in the site directory, and you have a
simple Dockerfile:

`FROM joergklein/hugo`

### Building your site

The Repo is on https://github.com/joergklein/docker-hugo

Based on this structure, you can easily build an image for your site:

`docker build -t hugo .`

Your site is automatically generated during this build.

### Using your site

There are two options for using the image you generated:

- as a stand-alone image
- as a volume image for your webserver

Using your image as a stand-alone image is the easiest:

`docker run -p 1313:1313 hugo`

This will automatically `start hugo server`, and your blog is now available on

`http://localhost:1313`.

If you are using boot2docker, you need to adjust the base URL:

`docker run -p 1313:1313 -e HUGO_BASE_URL=http://YOUR_DOCKER_IP:1313 hugo`

The image is also suitable for use as a volume image for a web server, such as nginx

```bash
docker run -d -v /usr/share/nginx/html --name site-data hugo
docker run -d --volumes-from site-data --name site-server -p 80:80 nginx
```
