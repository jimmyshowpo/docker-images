# Docker PHP 7 Cli with MySQL Server 5.6

A Docker image with PHP 7 Cli and MySQL Server 5.6 and a whole bunch of related deployment tools for end to end testing, builds and deployments of a PHP7 application.

This package contains:

- PHP 7 Cli (with extensions Json, Xml, Curl, PDO MySQL)
- MySQL Server 5.6 (host:127.0.0.1, user:root, password:password)
- PHPUnit 4.7
- ByJG Migrate
- Python/Pip
- AWS CLI
- Node/NPM/Grunt

Example pipelines script for building/testing/deploying Wordpress instance:

image: jimmybeats/php7-mysql-python-aws-bitbucketpipelines

pipelines:
  default:
    - step:
        script:
          # Setup an empty mysql database called "wordpress"
          - service mysql start
          - echo "create database wordpress" | mysql -u root -ppassword
          # Install dependencies with Composer
          - composer install --no-dev --no-interaction --optimize-autoloader
          # Install new clean Wordpress database
          - wp core install --url=http://localhost/ --title=WordPress --admin_user=admin --admin_password=password --admin_email=test@example.com --allow-root
          # Switch to your custom theme
          - wp theme activate mytheme  --allow-root
          # Activate any plugins that need to be activated for the tests to work
          - wp plugin activate myplugin --allow-root
          # Run the unit tests
          - phpunit --debug -vvv
          # Successful build so we can package and deploy to S3 (looks for a file called .exclude in the root to determine
          # which files to exclude from the deployment package
          - tar -czvf /tmp/artifact.tar.gz -X './.exclude' ./
          - aws s3 cp /tmp/artifact.tar.gz "s3://$S3_BUCKET/$APPLICATION_NAME/$BITBUCKET_BRANCH.tar.gz"
