web server to serve web page

proxy server -> to forward the request to others
    load balancer
    cache ( static content )
    security ( only one server is getting exposed ) with TLS
    compression of images and videos ( netflix )

nginx.config file

nginx in kubernetes:
    ingress controller -> load balancer for ingress traffic in k8s , not publicly accesible so we need aws load balancer for that.

apache vs nginx => nginx is better for static, fast, lightweight, k8s integrated