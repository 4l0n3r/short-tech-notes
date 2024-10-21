App Mesh:

    In a micro service architecture, each service is responsible for a separate functionality.
    Services are loosely coupled i.e. each one is capable of working on its own.

There are a few issues that need to addressed when we have this kind of architecture.

Request Logic:

    Since each service has multiple replicas, which replica do we want to route our traffic to ?
    With rolling updates in place, how are we dealing with different version of services ?
    How do we re-route traffic when an instance gets overloaded ?
    How would you define traffic routing rules ?(how does traffic flow between applications ?)

Logging:

    How do you isolate issues and detect point of failures in a plethora of services ?

Security:

    How do you define encryption standards ?
    How do you enforce usage of SSL certificates ?
    All these issues are addressed with App Mesh. It is a mesh that provides application level networking to make service to service communication easier.

Steps involved in App Mesh set up:

    Install the App Mesh controller
    Create the Mesh resource
    Register namespace to the mesh
    Create CRDs → Virtual Node, Virtual Service, Virtual Router
    Enable App Mesh in service specific repos

What are CRDs used by App Mesh ?

    A Virtual Node is logical pointer to the K8S deployment.
    A Virtual Service is an abstraction of a real service that is provided by a virtual node directly or indirectly by means of a virtual router.
    Virtual routers handle traffic for one or more virtual services within your mesh

TLS:

    Why do we need TLS communication ?
    to authenticate services, establish a secure connection and send data in encrypted format over the channel.

Steps to enable TLS communication with App Mesh:

    Install cert manager - a Kubernetes native implementation for automating certificate management
    Create generic secret from TLS credentials stored in GHA
    Create the cluster issuer which will use the secret generated previous step.
    Create a certificate for each of the services in the mesh for which we want to enable TLS. The certificate creation will trigger a respective secret to be created for that service
    Mount certificate onto the deployment and virtual node

cert manager:

    https://aws.amazon.com/blogs/containers/securing-kubernetes-applications-with-aws-app-mesh-and-cert-manager/
    Components:
        CRD'S
            Certificate
            ClusterIssuer
            Issuer
        cert-manager controller
            will keep an eye on above CRD's to create/update/delete
        cert-manager webhook
            will validate the changes happening to CRD's
        Cert-manager CA injector
            will inject the CA data to kubernetes objects ( ConfigMap, Secrets, etc )
