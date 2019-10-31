---
title: "README"
date: 2019-03-29T17:16:43+03:00
draft: true
---

## AWS Cloud Builders' Day Workshop

Collection of workshops to demonstrate best practices in using AWS Services. The objective of Cloud Builders' Day repository is to provide do-it-yourself lab guides for several AWS services including but not limited to EC2, S3, DynamoDB, VPC, Elastic Beanstalk, IoT Core & SageMaker.

Website for this workshops is available at https://cloudayolla.github.io/

## Building the Workshop site

The content of the workshops is built using [hugo](https://gohugo.io/). 

To build the content
 * clone this repository
 * [install hugo](https://gohugo.io/getting-started/installing/)
 * The project uses [hugo learn](https://github.com/matcornic/hugo-theme-learn/) template as a git submodule. To update the content, execute the following code
```bash
pushd themes/learn
git submodule init
git submodule update --checkout --recursive
popd
```
 * Run hugo to generate the site, and point your browser to http://localhost:1313
```bash
hugo serve -D
```

## License

This library is licensed under the Amazon Software License.

