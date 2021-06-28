# Storage and Data security
## Default storage encryption

In Assembler, all data is stored in a Kubernetes Persistent Volume. This includes:

1. Application data stored in PostgreSQL.
2. Application files stored in MinIO.
3. Application caching stored in Redis.
4. Secrets and credentials (stored in Kubernetes Secrets).
5. Configuration (stored in Kuberentes ConfigMaps).

The default storage class we use is encrypted at rest with Advanced Encryption Standard (AES). This occurs at:

1.  The underlying storage devices, either by AES256 or AES128
2. The distributed file system, protected by AES256
3. Databases and file storage, AES256 or AES128

Backups are also stored with the same storage class, allowing for all backups to be encrypted at rest.

The encryption mechanism is [FIPS 140-2 Validated](https://cloud.google.com/security/compliance/fips-140-2-validated).

## Custom storage encryption

Further options are available for workloads with differing encryption requirements. Due to the flexibility of Kubernetes Storage Classes, custom, client specific storage class with customised encryption mechanism can be deployed, either within the public SaaS offering or self-hosted. This includes the ability to leverage customer managed encryption keys.