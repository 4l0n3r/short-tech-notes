How the aws account login process working with roles ?

What is this aws-auth configmap ?
    mapping AWS IAM roles and users to Kubernetes RBAC ( groups )
    cluster_role_binding lo unde subject name eh aws_auth lo manam iche group name. cluster_role ki permissions anni ichestam avi group ki link avthay. group velli iam role ki aa permissions ichestadhi. aa role evadiki unte vaadu cluster ni access cheyyochu.

Difference between PassRole and AssumeRole ?
    PassRole: The user/service doesn't assume the role; instead, it allows another service to assume the role.
    AssumeRole: The user/service directly assumes the role and gains its permissions temporarily.