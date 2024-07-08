Aurora:
    not open source
    compatable with Postgres and MySql
    5X performance than MySql on RDS & 3X performance than Postgres
    Automatically increment 10GB when you use more space ( upto 128TB)
    15 Postgres, 5 MySql replicas
    faster replication process
    more cost than RDS ( 20% )

    High Availability :
        store 6 copies across 3 regions
            4 copies needed for write
            3 copies needed for read

        Master : 
            One Aurora instance takes writes
            automated failover 30 sec
        Read Replicas:
            15 replicas
            if master fails one of this will became Master.

                                    client
            writer end point                        reader end point
            master                                  read_replicas
                    shared storage volume (10Gb, upto 64TB)
            
            Custom endpoints:
                you can create couple of endpoints for different size of replicas for different use cases.
    
    Aurora Multi-Master :
        all r/w replicas

    Aurora global:
        1 primary region, 5 secondary regions -> supports 16 read replicas
        decrease latency
        disaster recovery ( RTO < 1m )
    
    Backup & Restore
        snapshot
        store the backup file in s3 and restore it back

    Clone:
        faster than backup & restore.
        copy on write protocol

    Encryption using KMS
    Authentation using IAM
    Cant use SSH
