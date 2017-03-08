# Docker Mautic Image

===================

<img src="https://www.mautic.org/media/images/github_readme.png" />

# License

Mautic is distributed under the GPL v3 license. Full details of the license can be found in the [Mautic GitHub repository](https://github.com/mautic/mautic/blob/staging/LICENSE.txt).

# How to use this image

This Docker image is maintained by [Autoize Mautic consultants](https://autoize.com). The latest Mautic version is published under a new tag within a day or two of the binaries being released to the open source community. Our goal is to maintain a constantly updated Docker image that Mautic users can utilize to deploy marketing automation in a containerized environment.

Latest image: The image tagged autoize/mautic:latest is currently Mautic 2.7.0.

Stable image: The image tagged autoize/mautic:stable is currently Mautic 2.5.1.

If you are using Mautic in production, it may be advisable to stay on the stable image while the Mautic team gathers feedback from the community and issues bug fixes for the latest release. The latest image coincides with the latest release available with Mautic's community edition with the bleeding-edge features; the stable image is held back two to three minor point releases.

# Start here
Most users will want to refer to our [step-by-step guide to running Mautic as a Docker container](https://autoize.com/run-mautic-as-a-docker-container/) but we have retained the more technical documentation below for experienced Docker and Docker Compose users.

===================

	docker run --name some-mautic --link some-mysql:mysql -d mautic/mautic

The following environment variables are also honored for configuring your Mautic instance:

-	`-e MAUTIC_DB_HOST=...` (defaults to the IP and port of the linked `mysql` container)
-	`-e MAUTIC_DB_USER=...` (defaults to "root")
-	`-e MAUTIC_DB_PASSWORD=...` (defaults to the value of the `MYSQL_ROOT_PASSWORD` environment variable from the linked `mysql` container)
-	`-e MAUTIC_DB_NAME=...` (defaults to "mautic")

If the `MAUTIC_DB_NAME` specified does not already exist on the given MySQL server, it will be created automatically upon startup of the `mautic` container, provided that the `MAUTIC_DB_USER` specified has the necessary permissions to create it.

If you'd like to be able to access the instance from the host without the container's IP, standard port mappings can be used:

	docker run --name some-mautic --link some-mysql:mysql -p 8080:80 -d mautic

Then, access it via `http://localhost:8080` or `http://host-ip:8080` in a browser.

If you'd like to use an external database instead of a linked `mysql` container, specify the hostname and port with `MAUTIC_DB_HOST` along with the password in `MAUTIC_DB_PASSWORD` and the username in `MAUTIC_DB_USER` (if it is something other than `root`):

	docker run --name some-mautic -e MAUTIC_DB_HOST=10.1.2.3:3306 \
	    -e MAUTIC_DB_USER=... -e MAUTIC_DB_PASSWORD=... -d mautic/mautic

## ... via [`docker-compose`](https://github.com/docker/compose)

Example `docker-compose.yml` for `mautic`:

	mautic:
	  image: mautic/mautic
	  links:
	    - mauticdb:mysql
	  ports:
	    - 8080:80
	
	mauticdb:
	  image: mysql:5.6
	  environment:
	    MYSQL_ROOT_PASSWORD: example

Run `docker-compose up`, wait for it to initialize completely, and visit `http://localhost:8080` or `http://host-ip:8080`.

# Supported Docker versions

This image is officially supported on Docker version 1.12.3.

Support for older versions (down to 1.0) is provided on a best-effort basis.

## Issues

If you have any problems with or questions about this image, please contact us through a [GitHub issue](https://github.com/autoize/docker-mautic/issues) or our [website](https://autoize.com/contact/).

You can also reach the Mautic community through its [online forums](https://www.mautic.org/community/) or the [Mautic Slack channel](https://www.mautic.org/slack/).

## Contributing

You are invited to contribute new features, fixes, or updates, large or small; we are always thrilled to receive pull requests, and do our best to process them as fast as we can.

Before you start to code, we recommend discussing your plans through a [GitHub issue](https://github.com/autoize/docker-mautic/issues), especially for more ambitious contributions. This gives other contributors a chance to point you in the right direction, give you feedback on your design, and help you find out if someone else is working on the same thing.
