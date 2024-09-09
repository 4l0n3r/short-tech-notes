End to End Monitoring tool
Observability Service
Requirements:
    Collect data
    Store data
    Visualize / Alert
    Scalable / Performance
    Integration
    Security / Access control

Architecture:

    Collecter
                http
                        TCP  --> Forwarder  Buffer | https / 443  -->  Backend
                http
    DogStatsD