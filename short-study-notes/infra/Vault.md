authentication : verifying who you are
authorization : what level of access do you have

We can create new item with same username password authentication, and we can add extra functionalities based on role/group/environment.
    prod -> enabling 2 factor authentication.

Tree with 3 roots
    authentication
    secrets
    authorization

users
    environment:path ( with a specific auth type. It might be usernamepass / kubernetes )
        user1
        user2


secrets
    environment ( with a specific secret type. It might be kv / pki certificates )
        secretname1
            k1 : v1
            k2 : v2
        secretname2
        
    versions

policies:
    path "secret_path_pattern" {
        capabilities = ["list","get","read"]
    }

authentication ( user ) , authorization ( policies ) work together to determine whether you have access to the ( secret ) or not.

Dynamic Admission Controller :
    Manifest  ->  Control Plane  ->  Modify the manifest  -> execute the manifest file

    Lets assume you have a requirement of running prometheus as SIDE_CAR_CONTAINER for each and every pod. You dont need to add the prometheus config to each and every manifest file manually.
    Just create a dynamic admission controller and tell them to create a SIDE_CAR_CONTAINER for every pod. It will take care of it.