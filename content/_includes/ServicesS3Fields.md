---
---

**S3 Configuration Options**

| Name | Description |
|------|-------------|
| IP Address | Enter the IP address that runs the S3 service. *0.0.0.0* tells the server to listen on all addresses. |
| Port | Enter the TCP port that provides the S3 service. |
| Access Key | Enter the S3 access ID. See [Access keys](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) for more information. |
| Secret Key | Enter the S3 secret access key. See [Access keys](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) for more information. |
| Disk | Browse to a directory to define the path for the S3 filesystem. |
| Enable Browser | Set to enable the web user interface for the S3 service. Access the minio web interface by entering the IP address and port number separated by a colon in the browser address bar. Example: *192.168.1.0:9000*. |
| Certificate | Use an SSL [certificate]({{< relref "/CORE/System/Certificates.md" >}}) created or imported in **System > Certificates** for secure S3 connections. |
