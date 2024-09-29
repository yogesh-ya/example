
# Spring Boot Configuration for GCP and AWS

This documentation outlines the configuration settings for running a Spring Boot application on both Google Cloud Platform (GCP) and Amazon Web Services (AWS). The configuration is divided into common settings and environment-specific settings.

## Common Configuration

The following section lists the common configuration that applies to both GCP and AWS environments.

### Common `application-dev.yml`

```yaml
spring:
  datasource:
    url: jdbc:postgresql://<db-host>:5432/<database-name>?currentSchema=<schema-name>
    username: <db-username>
  jpa:
    properties:
      hibernate:
        dialect: org.hibernate.dialect.PostgreSQLDialect
  host:
    url: https://api.${environment}.${domain}

keycloak:
  host: https://identity.<provider>.nuodata.io/
  realmName: nuodata

server:
  port: 8081
```

### Explanation of Common Configuration:

1. **`spring.datasource.url`**: Defines the JDBC URL for connecting to a PostgreSQL database, with a placeholder for the host, database name, and schema.
2. **`spring.datasource.username`**: Specifies the database user (defined in the specific environment).
3. **`hibernate.dialect`**: Configures Hibernate to use PostgreSQL SQL dialect.
4. **`keycloak`**: Keycloak server configuration for authentication, with a placeholder for the provider (either GCP or AWS).
5. **`server.port`**: The port where the application will run (`8081`).

## GCP Specific Configuration

The following section details the GCP-specific configuration.

### `application-dev.yml` (GCP)

```yaml
gcp:
  secretsmanager:
    secretName: engine_service_dev_password
  project: nuodata-396604
  gcs:
    region: us-west2
    bucket: dev-nuodata-files
  gcs_event:
    region: us-west2
    bucket: nuodata-event-images-dev
```

### Explanation of GCP Configuration:

1. **`cloud.secretsmanager.secretName`**: References the Google Secret Manager key that holds the database password.
2. **`gcp.project`**: The GCP project ID.
3. **`gcs.region` & `gcs.bucket`**: Specifies the region and the Google Cloud Storage bucket for file storage.
4. **`gcs_event.region` & `gcs_event.bucket`**: Configuration for handling event-driven file uploads using GCS.

## AWS Specific Configuration

The following section details the AWS-specific configuration.

### `application-dev-aws.yml`

```yaml
aws:
  secretsmanager:
    secretName: nuodata_rds_dev_master_password
    region: us-west-1
  s3:
    region: us-west-1
    bucket: dev.nuodata.files
  s3_event:
    region: us-west-1
    bucket: nuodata-event-images-dev
```

### Explanation of AWS Configuration:

1. **`aws.secretsmanager.secretName`**: Refers to the AWS Secrets Manager secret containing the database password.
2. **`aws.region`**: Specifies the AWS region where services (S3 and Secrets Manager) are hosted.
3. **`aws.s3.bucket`**: Configures the AWS S3 bucket for file storage.
4. **`aws.s3_event.bucket`**: Specifies the bucket for handling event-driven file uploads using S3.

## Conclusion

This document provides an overview of how to configure a Spring Boot application for GCP and AWS environments. It highlights common configurations and separates GCP-specific and AWS-specific configurations related to database connection, secrets management, and storage services.
