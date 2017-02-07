# Docker PHP 7 Cli with MySQL Server 5.6

A Docker image with PHP 7 Cli and MySQL Server 5.6. The main purpose is to be create an environment for Bitbucket Pipelines where it is possible to run PHP and have the MySQL for running unit tests with migration. Specifically for use in BitBuckets Pipelines to test/build and then deploy build artifact to an S3 bucket.

This package contains:

- PHP 7 Cli (with extensions Json, Xml, Curl, PDO MySQL)
- MySQL Server 5.6 (host:127.0.0.1, user:root, password:password)
- PHPUnit 4.7
- ByJG Migrate
- Python/Pip
- AWS CLI
