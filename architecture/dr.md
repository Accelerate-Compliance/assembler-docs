# Disaster Recovery
Assembler uses [Velero](https://velero.io/docs/v1.6/disaster-case/) for disaster recovery.

Each application can have a configured backup schedule, allowing for a completely customisable RPO depending on the criticality of the application. 

The frequency and size of backup  can impact the resource consumption of the environment, potentially requiring larger resource limits to be applied.

Once triggered, restores are entirely automated. Restore times however will vary based on the amount of data stored in the application. Logical backups can be selected instead of full, physical backups in order to reduce the RTO if required. Typical applications see RTO times of less than 1 hour.