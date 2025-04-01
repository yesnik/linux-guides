# Graylog

[Graylog](https://github.com/Graylog2/graylog2-server) is a powerful open-source log management and analysis platform that helps organizations collect, store, search, and analyze log data from various sources. 

## System requirements

The Graylog server application has the following prerequisites:

- Some modern Linux distribution (Debian Linux, Ubuntu Linux, or CentOS recommended)
- Elasticsearch 2.3.5 or later
- MongoDB 2.4 or later (latest stable version is recommended)
- Oracle Java SE 8 (OpenJDK 8 also works; latest stable update is recommended)

### Recommended resources

- CPU: 4
- RAM: 8Gb

## Advantages

- **Unified View**: Graylog aggregates logs from multiple sources (servers, applications, network devices, etc.) into a single centralized location, making it easier to monitor and troubleshoot issues.
- **Scalability**: It can handle large volumes of log data, scaling horizontally across multiple servers or nodes.
- **Real-Time Monitoring and Customizable Alerts**: Users can set up alerts based on specific conditions or thresholds in the log data, ensuring timely notifications for critical events.
- **Powerful Query Language**: Graylog uses an intuitive query syntax (similar to Elasticsearch) that allows users to perform complex searches and filter logs efficiently.
- **Multiple Input Sources**: Graylog supports various input methods, including Syslog, GELF (Graylog Extended Log Format), Beats, HTTP, Kafka, and more. Users can configure custom inputs to ingest data from unique systems.
- **Elasticsearch Backend**: Graylog leverages Elasticsearch for storage and indexing, ensuring high performance and scalability.
- **Compatibility**: It integrates seamlessly with tools like Grafana, Prometheus, PagerDuty, Slack, etc.
- **Extractors**: Graylog allows users to extract and transform data from logs using built-in extractors or custom scripts.
- **Pipelines**: The pipeline processor enables advanced data manipulation, such as modifying fields, routing messages, or applying conditional logic.
- **Dashboarding**: Graylog provides a web-based interface with customizable dashboards for visualizing log data and metrics.
- **Open Source**: Graylog is free to use under the open-source license, making it an attractive option for organizations looking to reduce costs compared to commercial alternatives like Splunk.
- **Enterprise Features**: For organizations needing additional capabilities, Graylog offers a paid enterprise version with advanced features like role-based access control (RBAC), audit logging, and archiving.
- **Cluster Support**: Graylog can be deployed in a clustered environment to ensure high availability and fault tolerance.
- **Data Retention Policies**: Users can define retention policies to manage log storage and automatically archive or delete old data.
- **Multi-OS Support**: Graylog can be deployed on various operating systems, including Linux, Windows, and macOS.
- **Cloud-Native**: It supports cloud deployments on platforms like AWS, Azure, and Google Cloud, as well as containerized environments using Docker and Kubernetes.

## Create input

- Menu System > Inputs > Select "GELF UDP", press "Launch new input"
- In popup window:
  * Title: UDP Gelf
  * Port: 12201
  * Press "Launch input"
 
## Create index set

- Menu System > Indices > Create index set:
  * Title: Nginx logs
  * Description: Nginx logs
  * Index prefix: nginx
  * Select retention strategy: Delete index

