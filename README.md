# Project Title

Converter Platform

## Description

A massively over-engineered video to mp3 converter using Python, Kubernetes, mongodb, mysql, and rabbitMQ.

Clients can login to be issued a JWT, which can then be used to authenticate against the validator. This allows them to post video files and then download them as MP3 files.

## Architecture

![Architecture](architecture.png?raw=true "Architecture diagram")

Every service is built as a docker image and deployed using Kubernetes manifests into a minikube cluster.

Technologies used for each component:\
Gateway - Python, Flask\
Auth - Python, JWT, MySQL\
Video Queue - RabbitMQ\
Converter - Python, MongoDB\
MP3 queue - RabbitMQ\
Notifier - Python

## Converting a file

To convert a file, a user needs to make the following calls in order:

1. Login via a POST request to http://mp3converter.com/login with a username and password that matches their account details in the Auth DB. This will return a valid JWT.
2. Send the video file as a POST request to http://mp3converter.com/upload to initiate a conversion, the authorization header is required in the format of "Authorization: Bearer token".
3. A notification will be sent to the registered email address with the MP3 file id when the file has been successfully converted, this can be used to download the MP3 file.
4. A GET request to http://mp3converter.com/download?fid=<file-id> will download the MP3 file from the server. This request also requires the authorization header in the format of "Authorization: Bearer token".
