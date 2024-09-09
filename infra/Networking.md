OSI 7 layer model:
    
    host part: how the retrieved data can be re-assumbled
        application
        presentation 
        session
        transport

    media part: how data can be moved from 1 point to another point
        Network layer
        Data link layer
        Physical

    Physical laayer:
        hub -> blind, receive info from any port then forward it to all ports.
        no traffic control
        no identificatin of device,I mean no device to device communication
        collisions + data loss

Traditional cloud  into  Virtual cloud
Services levels
inside and outside VPC

**CIDR :**

    Notation
    NID , SID or HID , SubnetMask
    Subnetting
    
**Route Tables:**

    Main Route Table
    Custom Route Table

    If instance has Custom RT then the relation with Main RT will be broken
    If no custom RT for subnet then it will use main RT

**FireWalls:**

    SG : 
        at Instance Level
        inbound -> no default
        outbound -> all to all
        no feature of block
        no order for evaluation
        statefull  -> no need to explicitly allow return traffic
    NACL :
        at Subnet Level
        inbound -> all to all
        outbound -> all to all
        feature of block
        evaluate in an order 
        stateless  -> need to explicitly allow return traffic otherwise it won't work
        ephemeral port range -> return traffic won't use port 80 it will use ephemeral port range.


**Flow :**

	1.create vpc
	2.create internet gateway
		attach it to VPC
	3.create subnets 
		public
		private
	4.create Route tables
		public
			ADD ROUTE : connect any IP to public RT
			connect public subnet to public RT
		private
			connect private subnet to private RT
	5.Create firewalls 
		security group (instance level) : my ip  only
		NALC (subnet level)		: my IP only
	6.Creating EC2 Instance
		1.choose amazon machine image - OS
		2.choose an instance type
		3.configure -> attach VPC and Subnet and enable auto-assign subnet
	

**Internet Connectivity to Private subnets :**

    we can use :
        1. Nat Gateway
            0.0.0.0/0 to NatGateway  -> IGW  -> Internet
        2. Nat Ec2 Instance ( disable source/dest check )
            create Nat EC2 instance in public subnet so that it will be assigned to public Ip.
            0.0.0.0/0 to NatEc2Instance 

**Connectivity between Two Private subnets in different regions :**

    1. Via Internet Gateway  ( Two Nat Instances ) ** Not Recommended **
    2. Using VPC Peering  (Sender , Receiver)
        1. set up two VPC networks with required subnets ( public subnet + private subnet , private subnet ).
        2. create a VPC peering between two private networks at sender side and accept the request at receiver end.
        3. update the route table : add route from one private nw to another private nw through vpcPeering.
        4. update the security group.    

**Connectivity between Private-SN in VPC to S3 or DB :**

    1. Via Internet Gateway  ( Nat Instances ) ** Not Recommended **
    2. VPC Endpoint
        1. Create an end-point for aws s3 services
        2. Select vpc , route table ( this will automatically add route to our route table )
            If any traffic going to aws s3 services then it will go through the vpc end-point.

**Connecting Virtual Cloud with On-Premise Data Center :**

    1. Via Direct Connect ( Using physical wires -> Costly )
    2. VPN Connection  ( Virtual Private Gateway  -> Customer Gateway )
        on public network + secure

**Site to Site Connection:**
    
    Virtual Private Gateway
    Customer Gateway
    AWS VPN CloudHub

**Direct Connect:**
    
    dedicated private network + data in transit not encrypted
    Direct Connect Gateway to connect one or more VPC in many different regions
    Types:
        Dedicated ( 1, 10, 100 GBPS )
        Hosted ( 50MB,500MB,10GB )
    Direct Connect + VPN = IPsec encrypted private connection
    Direct Connect - Resiliency
    Site to Site as a backup

**Transit Gateway:**
    
    having transitive peering between thousands of VPC and on-premises
    Equal-cost multi-path routing ( more STS connections )
    Integration with Direct Connect Gateway, VPN connections
    AWS Resource Access Manager to share Transit Gateway with other accounts.

**Network Address Translation:**

    Shortage of IPV4 => split into public + private. ( We don't have this issue in IPV6 , hence we don't need NAT there )
    for public we can access internet for private we can't.
    So NAT will provide the public Ip address to the private IP so that it can also access internet.
    Static NAT => one private to one public ( consistent )
        same as IGW
        there is a NAT device which maintains a NAT table ( { PR1 : PU1 , PR2:PU2 })
        the traffic will go through the nat device and it will take care of converting Pri to Pub and Pub to Pri
    Dynamic NAT => one private to 1st available from pool of public IP's
        for private Ip the public Ip will be allocated dynamically from a pool of public Ip's.
        this is still 1:1 for the duration of allocation.
        There is a chance for run out of public Ip's.
    PAT ( Port Address Translation )
        same as NAT GW
        many private Ip's -> one public Ip
        many private Ip's + ephemetal port -> one public Ip + randomly assigned port from a pool
        this time NAT table will contains [private ip , private port , public ip , public port]
        Random ga assign chestundhi, aa random number same ayye chance vaste ela ?
        Is there any limit on ports ?
        What is the addvantage that we are getting with Static NAT then this ?

**Elastic IP:**

    If you stop the Ec2 instance and start it again then the public ip address will be changed. If you don't want this behaviour just buy an Ip address and attach it to ec2. ( Elastic IP )
    You can allocate upto 5 elastic ips' to your account
    can attach it to one ec2 at a time

**Elastic Network Interface:**

    elastic ip
    private ip
    subnet group
    security group

    you just need to select eni while creating the instance.
    you can achieve failover by using this. If an instance down, you can simply attach another instance to this eni.

**IPV4 : Private Addressing**

    A :
        10.0.0.0  -> 10.255.255.255
        1 x A
    B:
        172.16.0.0  -> 172.31.255.255
        16 x B
    C :
        192.168.0.0  -> 192.168.255.255
        256 x C

**DDOS Attack:**

    Application Layer : HTTP Flood
        botnet will make many connections to the application
    Protocol Attack : SYN Flood
        botnet will send SYN protocol with source as a spoofed IP.
        application will try to send SYN-ACK but the Ip is not a valid one
    Volumetric / Amplification : Data flood
        botnet will send request to many DNS servers with source as a application IP. ( easy )
        Dns will send large response to the application.
        botnet req(AppIp,DnsIP)  -> Dns
        dns req(DnsIP,AppIP) -> App

**DNS :**

    Root
        NameServer
            Zone
                Domain Records
    Basically zone will contains the DNS records. So we need zone to get the IP details.

    The job of DNS is to help you locate and get the query response from the authorative zone which hosts the DNS record you need.
    walking the tree

    Domain Registration:
        Domain Registrar : will register the domain
        DNS Hosting Provider : will host the zone for the domain in name server
        TLD Registry : Store NS info related to domain
        Add cat.com NS to the .com TLD Zone

    Record Types:
        A : domain -> address ( ipv4 )
        AAAA : domain -> address ( ipv6 )
        CNAME : domain -> sub_domain
        MX : mail server domain
            If you wanna send email to person@example.com then it will query for the server so that it can send mail.
        SOA : state of autority
            store administrative information about a DNS zone.
        NS : the name of the Administrative Name Server within a domain.
        PTR : reverse of A / AAAA 
        TXT : contact info about domain

**DNSSEC :**

    Data Origin Authentication: this data comes from this zone
    Data Integrity: this data havn't modified in transit
    Ex:
        Poisoned Cache:
            evil google.com IP kosam DNS ki request pampadu, so adhi tree walk chestadhi, so aa madyalo evil nene authenticative NS and this is the IP address for google.com ani chepthe( Origin Authentication Gone ), DNS resolver aa fake data ni cache lo store chestadhi ( Integrity Gone )
            next manam google.com IP adiginapudu fake IP istadhi until TTL ends.


**SSL/TLS certificate:**
    
    secure communication:
        privacy
            using session key, will be encrypted session key using public key then at server it will be decrypted using private key
        integrity
            using hash_function we can hash the data and send it along with data then at server side it will fetch the data and perform hashing then compare both the hashes to verify the integrity.
        authenticity
            using ssl certificate.
            certificate authority only gives the certificate if the requested server is owner.
            how client believes the certificate is original ?
                every client would be having trust store where it stores few of the ssl signatures. If the signature of the received certificate matches with one of the signatures in trust store then it trusts the ssl certificate.
    on ssl handshake ssl_cerficate + public_key + cipher_suites
    where & how this trust store will be created in my local machine ?
        it would be created when install os / update your os.
    Ref: https://www.youtube.com/watch?v=cLYv4uSFJA8&t=1193s&ab_channel=AWSwithChetan

Devops:

    https://www.youtube.com/watch?v=Xrgk023l4lI
    https://www.youtube.com/watch?v=9pZ2xmsSDdo

**Aws Services intro :**

    https://www.youtube.com/watch?v=Z3SYDTMP3ME
    https://thoughtworks.udemy.com/course/aws-certified-cloud-practitioner-new/learn/lecture/19891628?start=165#overview   till section-7

**Ec2 :**

    https://thoughtworks.udemy.com/course/networking-in-aws/learn/lecture/13988344#overview  till section-2

**Container :**

    Docker : https://thoughtworks.udemy.com/course/learn-docker/
    Kubernetes : https://thoughtworks.udemy.com/course/learn-kubernetes/learn/lecture/10177126#overview
