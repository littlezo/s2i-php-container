PHP container images
====================

This repository contains the source for building various versions of
the PHP application as a reproducible container image using
[source-to-image](https://github.com/kubesphere/s2ioperator).
Users can choose between RHEL and CentOS based builder images.
The resulting image can be run using [Docker](http://docker.io).

Versions
---------------
PHP versions currently provided are:
* [PHP 7.3](7.3)
* [PHP 7.4](7.4)
* [PHP 8.0](8.0)

RHEL versions currently supported are:
* RHEL7
* RHEL8

CentOS versions currently supported are:
* CentOS7
* CentOS8


Installation
---------------
To build a PHP image, choose either the CentOS or RHEL based image:

*  **CentOS based image**

    This image is available on DockerHub. To download it run:

    ```
    $ docker pull littleof/php-80-centos8:8.0
    ```

    To build a PHP image from scratch run:

    ```
    $ git clone --recursive https://github.com/littlezo/s2i-php-container.git
    $ cd s2i-php-container
    $ git submodule update --init
    $ make build TARGET=centos8 VERSIONS=7.4
    ```

**Notice: By omitting the `VERSIONS` parameter, the build/test action will be performed
on all provided versions of PHP.**


Usage
---------------------------------
For information about usage of Dockerfile for PHP 7.3,
see [usage documentation](7.3/README.md).
For information about usage of Dockerfile for PHP 7.4,
see [usage documentation](7.4/README.md).
For information about usage of Dockerfile for PHP 8.0,
see [usage documentation](8.0/README.md).

Test
---------------------
This repository also provides a [S2I](https://github.com/kubesphere/s2ioperator) test framework,
which launches tests to check functionality of a simple PHP application built on top of the s2i-php image.

Users can choose between testing a PHP test application based on a RHEL or CentOS image.

*  **RHEL based image**

    To test a RHEL7 based PHP image, you need to run the test on a properly
    subscribed RHEL machine.

    ```
    $ cd s2i-php-container
    $ git submodule update --init
    $ make test TARGET=rhel8 VERSIONS=7.4
    ```

*  **CentOS based image**

    ```
    $ cd s2i-php-container
    $ git submodule update --init
    $ make test TARGET=centos8 VERSIONS=7.4
    ```

**Notice: By omitting the `VERSIONS` parameter, the build/test action will be performed
on all provided versions of PHP.**


Repository organization
------------------------
* **`<php-version>`**

    * **Dockerfile**

        CentOS based Dockerfile.

    * **Dockerfile.rhel7**

        RHEL based Dockerfile. In order to perform build or test actions on this
        Dockerfile you need to run the action on a properly subscribed RHEL machine.

    * **`s2i/bin/`**

        This folder contains scripts that are run by [S2I](https://github.com/kubesphere/s2ioperator):

        *   **assemble**

            Used to install the sources into the location where the application
            will be run and prepare the application for deployment (eg. installing
            modules using npm, etc.)

        *   **run**

            This script is responsible for running the application, by using the
            application web server.

        *   **usage***

            This script prints the usage of this image.

    * **`contrib/`**

        This folder contains a file with commonly used modules.

    * **`test/`**

        This folder contains the [S2I](https://github.com/kubesphere/s2ioperator)
        test framework with simple PHP echo server.

        * **`test-app/`**

            A simple PHP echo server used for testing purposes by the [S2I](https://github.com/kubesphere/s2ioperator) test framework.

        * **run**

            This script runs the [S2I](https://github.com/kubesphere/s2ioperator) test framework.

推送 镜像到镜像仓库 修改 s2i-php-swoole.yaml 中的 defaultBaseImage 和 builderImage 为你的镜像地址 使用如下命令 推送模板到 kubesphere集群

kubectl apply -f deploy/s2i-php-73-template.yaml
kubectl apply -f deploy/s2i-php-74-template.yaml
kubectl apply -f deploy/s2i-php-80-template.yaml