monothilic :
    all services connected to single unit.
    scalling will be happen as a unit, we cant scale only one service.
    must be developed by one tech stack.
    on every change the entire application needs to be build, test, redeploy.
    diff services needs diff versions of same resource.
    if bug in any service, total application will be down.

micro services:
    split based on the functionality.
    services should be loosly coupled.

    communication between services:
        1. API calls - Synchronous
        2. RabbitMq - Asynchronous
        3. Service Mesh ( Kubernetes )

MonoRepo VS PolyRepo :
    coupling
    pipeline trigger count
    if main branch fails all pipelines will block
    git conflicts
    separate logic to tell the pipeline to deploy only the changed service.
