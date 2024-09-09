Helm: Package manager for kubernetes
    -   bunch of yaml files
    -   create help chart
    -   push to helm repository
    -   download and use it

Instead of creating whole yaml files for common components , we can search that component on helm and we can use the helm chart to do the same.

Uses:
    Templating engine : 
        like terragrunt for terraform files.
         we can create a blueprint and pass the values to the blueprint to create the yaml files.
    Release Management:
        Helm 2:
            client  :   sends request( which yaml files should be installed ) to tiller
            server (Tiller) : will create components from yaml file inside the k8s cluster.

            change yaml files -> do helm update -> the changes will be applies to existing components.
            you can roll back to previous revision if you had any issues on new one.

            Problem: tiller has high power over k8s. 
        
        Helm 3:
            removed tiller part.
            difficult to manage.

Structure:
    mychart/
        Charts.yaml     version
        values.yaml
        charts/         other chart dependencies
        templates/

    template file will be filled with the values from values.yaml file.

Overriding values:
    providing another file
    --set key=value