IAM:

    users,groups,policies,roles

    Policy Structure :

    	version 2012-10-17 always
    	id optional
    	statements : list of individual statements
    	sid - optional
    	effect - allow , denny
    	principal - assigned to account / user / role
    	action - list of actions
    	resource - list of resources to which the actions to apply
    	condition - when it is applied

    IAM password policy
    MFA

    Trust Policy ( Assume Role ):

    	Assume to all under the account ( root, this doesn't mean root user it means all under the account )
    	Assume to all under the account which is having the Principal tag as specific one ( ABAC )
    	Assume to single role
    	Assume to single role which is comming from CIDR range
    	Assume to single user
    	Assume to single user who has the MFA enabled

    Access Key :
    	username == access key id
    	password == secret access key
    	creating Access key to access AWS from console

    audit:
    	Credential Report: details of users
    	Access advisor: service level permissions to user

IAM Advanced:

    AWS organizations:
    	organizational units + accounts
    	service control policiese (OU) + account level policies(Users/Roles) & hierarchy

    	Root OU ( admin )
    		management account
    			test OU
    				account-1 ( need an explicit allow )
    				account-2
        Security OU
            audit account
            log archive account
        Sandbox OU
            dev / test accounts
        Prod OU
            prod account

        central logging account

    IAM conditions:
    	SourceIp
    	RequestedRegion
    	MultiFactAuthPresent
    	PricipalOrgId

    s3 bucket level + object level permissions

    assume role:
    	need to give up your current permissions to assume new one
    	ex: ec2 , kinesis

    resource-based:
    	resource will have permissions to allow other resources to have acceess on it.
    	ex: sqs, sns, lambda, logs

    Iam permissions boundaries
    	intersection ( scp + boundaries + roles + resource level permissions )

    Evalution logic

    IAM Identity center:
    	single sign-on
    		to all accounts
    		to all saml applications
    		ec2 windows instances
    	identity provider
    		okta, AD

    	fine-grained permissions
    		multi-account
    		application assignements
    		attribute based access control
    			define permissions first for each attribute, then modify the attributes at user level

    Active Directory:
    	database of user accounts, printers, file shares, security groups
    	central security management

    	managed microsoft ad
    	ad connector
    	simple ad ( aws managed )

    identity center -> active directory

    Control Tower:
    	best way to manage multi-account aws environment in few clicks
    	manage policy management using guardrails ( preventive - scps & detectice - aws config )
    	detect policy voilations
    	compliance dashboard

EC2:

    Config:
    	OS,CPU,RAM,Storage,Firewall rules, network card, user data
    instance type
    	general t2.micro,m5
    	compute c
    	memory r
    	storage i,d
    ec2 purchasing options:
        On-Demand
        Reserved :
            1 / 3 year commitment
            can't cancel, but modify,exchange,buy on marketplace
            instance attributes:
                type, family, os, scope ( Region / Zonal ), tenancy( host / dedicated )
            Offering class:
                Standard:
                    75 %
                    modify / sell
                Convertible:
                    66 %
                    can change the instance attributes
        Savings plan:
            1 / 3 year commitment
            commit to certain type of usage ( 10$/H for 1 Year )
            Compute Savings Plans:
                66 %
            EC2 Instance Savings Plans:
                72 %
                locked to specific family & region
        Spot Instance:
            90 %
            interrupted
        Dedicated Host:
            A physical server 
            BYOL
        Dedicated instance:
            Instnaces on hardware
            may share hardware to other instance
            no control over the placement group
        Capacity reservation:
            reserve On-Demand instance capacity in a az
            no time commitment, no discounts
            will be charged regardless of using it or not
Placement Groups:

    Group of instances
    Cluster:
    	all in one hardware, single az
    	high bandwidth
    	high risk, speed matters

    Spread:
    	each instance in one hardware across different AZ's
    	7 instance per az
    	no risk
    	critical application, availability matters

    Partition:
    	group of instances in one partition ( one hardware )
    	7 partitions per az
    	medium risk
    	100's of instances

EC2 Hibernate:

    Hibernation:
    	Storing the current RAM data in EBS and using it while restarting the instance to save os bootup time.
    	EBS volume >> RAM < 150GB
    	Only EBS, encrypted
    	supported for few from c,m,r,t instance types
    	not more than 60 days

EC2 Instance Storage:

    EBS:
    	preserve data even after instance termination though by default Root EBS will be terminated when instance got terminated
    	network communication, bit of latency
    	only to one EC2 at a time
    	can dettach and attach it to other instance
    	bound to AZ
    	snapshot and move across region
    	move snapshots to archive tier ( 24 to 72 hours to restore )
    	Recovery rule to prevent it from accedental deletion. ( store it even if it deleted )
        snapshot fast restore -> to have faster invitial access

    	Volume Types:
    		based on size / throughput / iops
    		io1 / io2  : SSD  ( Root volume )
    		gp2 / gp3  : SSD  ( Root volume )
    		st 1  : HDD frequntly accessed
    		sc 1  : HDD infrequntly accessed

    		Only gp2/gp3 & io1/io2 can be used as boot volumes

    	Multi-Attach:
    		io1 / io2 can be attached to upto 16 instance at a time within az.

    	Encryption
    		EBS <- Encrypted -> Instance
    		unencrypted volume  -> unencrypted snapshot
    		unencrypted snapshot -> encrypted volume / unencrypted volume
    		1. copy snapshot and toggle on encryption , create volume out of it
    		2. while creating volume from unencrypted snapshot , toggle on encryption

    EFS:
    	network file system, can be mounted to many EC2
    	multi-AZ
    	only for linux based AMI's
    	too expensive ( 3x gp2 EBL )
    	inside security group
    	Encryption at rest by default
    	scales automatically
    	unpredictible workloads

    	Size: Auto scale
    	Throughput: Bursting(Auto scale based on volume), Provisioned(fixed), Elastic(Automatic)
    	Performance: General Purpose, Max IO

    	Types:
    		Standard: Frequntly use case
    		Infrequent Access ( EFS IA ): InFrequntly use case

    		based on life cycle, the files which are not accessed more than X days will move to less expensive storage( EFS IA ).

    EC2 Instance Store:
    	IO matter vaste guarante Instance Store
    	EBS is network driver, not attached directly to ec2
    	If you want high performance (faster i/o) hardware disk, use EC2 Instance store. It's directly attached to hardware.
    	data lose if hardware fails
    	use it for cache / temporary usages

    AMI:
    	customize all your configuration, OS, monitoring
    	Faster boot since all have already installed on AMI
    	bound to region, but you can copy and use it in other

    	process:
    		start instace, customize it ( em em kaavalo anni install cheseyali User data block use chesi or ssh into it and install)
    		stop instance
    		build AMI out of it
    		launch other instance using AMI

High Availability & Scalability

    Scalling:
    	Vertical
    		Increase size
    		non distributed
    		databases
    	Horizontal
    		Increase number of instances
    		distributed
    		modern applications

    Availability:
    	At least on 2 AZs

    Elastic Load Balancer:
    	will forward the traffic based on instance health  ( /health : 4567 )

    	Clasic LB :
    		Http,Https,Tcp,Ssl

    	Application LB :
    		Http,Https,Websocket
    		route to different target groups (ec2,ip addresses,ecs tasks,lambda)
    		redirect

    	Network LB :
    		Tcp, Tls, Udp
            route to different target groups (ec2,ip addresses,alb)
    		million requests
            static ip for each az

    	Gateway Load Balancer :
    		Operates at layer 3
    		The traffic:
    			first will go target group of security virtual appliances
    			if the request is valid then returns to Gateway LB , then go to application
    			else, the request will be dropped off at firewall instance itself

    	public:external / private:internal LB's

    	X-Forwarded for to see the client Ip address since the application will get the request directly from load balancer instead of client.

    	Sticky Sessions:
    		The same person will always redirected to the same instance behind LB.
    		control over expiration time.
    		Cookies:
    			Application-based cookies
    				Custom cookies
    				Application cookies
    					Generated by load balancer : AWSALBAPP
    			Load-balancer cookies
    				Generated by load balancer : AWSALB , AWSELB

    	Cross Zone Load Balancer ( equal traffic to all instance across az's )
    		ALB
    			on by default
    			no charges
    			HTTP,HTTPS,WEBSOCKET
    		NLB
    			off by default
    			charges apply
    			HTTP,HTTPS,TCP
    		GLB
    			off by default
    			no charges

    	SSL/TLS certificates
    		Encrypted communication between client and lb.
    		CLB
    			one SSL for one domain
    			should create multiple CLB for multiple SSL
    		ALB , NLB
    			multiple certificates for multiple domains
    			help of SNI server name indicator

    		Server Name Indication (SNI) allows you to expose multiple HTTPS applications each with its own SSL certificate on the same listener.

    	connection draining / de-registration delay

Target Group instances unhealthy :

    404
    	change the health path
    request timed out
    	connectivity issue
        check the security groups
            lb outbound with target_sg
            target inbound with lb_sg
    less timeout configuration
    	increase the timeout

Auto Scaling group:

    The Auto Scaling Group can't go over the maximum capacity (you configured) during scale-out events.
    launch template
    cloudwatch_alarm -> asg
    Policies:
    	Dynamic policies:
    		Target tracking  : alarms created automatically and scalling happens.
    		Simple / Step : cpu utilization increases -> cloudwatch alarms triggers -> scale up 2 units
    		Scheduled : at a perticuler time
    	Predictive policies:
    		based on history -> forecast will be created -> scalling will scheduled
    metrics to scale:
    	CPUUtilization
    	RequestCountPerTarget
    	Average Network In / Out
    	Any custom metric
    cold down period ( 300 sec )
    better to use ready-to-use ami for less config time

Route53:

    create zone ( public, private )
    	create domain records inside it

    TTL

    CNAME -
    	maps hostname to another hostname
    	For Non Root domain ( something.mydomain.com )

    Alias -
    	maps hostname to Aws resource
    	For Both Root & Non_Root domains ( mydomain.com )
    	Automatically recognizes changes in the target resource IP since no TTL.
    	Always A / AAAA
    	Not possible for EC2 Domain

    Routing Policies:
    	Simple : can return more ips at a time
    	Weighted : type of load balancing. ( Id  + Ip + Weight % + Region )
    	Latency : type of geolocation lb. ( Id + Ip + Region )
    	Failover: Primary & Secondary records.
    	Geolocation: Based on the client location. Default -> if nothing ( from all geolocation records ) matches with the client location
    	GeoProximity: to shift the more traffic to specific region using bias value.
    	Ip Based: based on client Ip ( CIDR block )
    	Multi Value: Simple Routing + Health checks for each record. Only returns the healthy records from the list.

    	You can create domain on 3rd party and use our route 53 as a DNS service. Just update the ns records in 3rd party with the records mentioned in our aws host zone.

    Health Checks:
    	to prevent the traffic to go through the failed instances.
    	from all global locations. If > 18% then healthy.
    	string match for first 5128 bytes.

    	Simple Health Check ( Resources health checks )
    		15 global health checkers
    	Calculated Health Check : childs and parents
    	Private Health Check : based on cloud watch ( for private instances )

    how the ingress is going to create the record in exactly perticuler zone ?

CloudFront:

    216 edge locations
    origins: s3 bucket / custom origin ( ec2, load balancer, s3 website )
    cloudfront vs s3 cross region replication  [ static x dynamic ]
    Geo restriction : allow list and block list
    Price classes => Reduce the edge locations
    cache invalidation

Global Accelerator:

    create instnaces in different regions
    create endpoints with those instances
    attach those endpoints to the global accelerator
    global accelerator will expose 2 static IP's
    traffic will goes to the nearest region instance
    failover within 30 sec
    tcp, udp => good for gaming apps

S3:

    by default private, can't access using public url only through pre-signed url
    5TB, > 5GB ( multi upload )
    website hosting:
    	should be public
    	http://bucket-name.s3-website-aws-region.amazonaws.com
    	http://bucket-name.s3.website-aws-region.amazonaws.com
    version: ability to get back prev version
    replication:
    	versioning is mandatory
    	across same and multi region
    	delete markers can be replicated
    	deletions with version Id not possible
    storage classes
    	standard
    	standart IA ( backup )
    	intelligent tiering
    		frequent
    		in-frequent
    		archive instant access tier
    		archive access tier
    		deep archive access tier
    	one-zone IA ( sencondary backup, can be recretable)
    	Glacier instant retrieval (
    	Glacier flexible retrieval
    	Glacier deep archive
    life cycle rules
    	to move objects between storages
    	expiration actions

    	based on prefix / object tags
    S3 analytics for suggestions ( only for standard, standard IA)
    Requester pays for networking cost
    S3 notifications
    	sns, sqs, lambda
    	eventBridge
    	resource policy instead of user policy
    Performance: per prefix
        3,500 - GET/HEAD
        5,500 - PUT/POST/DELETE
    Uploads:
    	Multi uploads => can be paralellized
    	S3 tranfer acceleration => send it to nearest edge location ( public ) then use private network to upload.
    S3 byte-range fetches:
    	retrieve only partial data
    	can be parallelized
    S3 select & Glacier select
    	server-side-filtering instead of client side
    	less network & less cpu on client side

    	s3 will get csv file then it will filter the data then return only filtered data instead of sending all.
    S3 batch operations:
    	a job with list of objects + action to perform on those + optional parameters
    	like: encrypt all objects, update metadata, invoke lambda to perform something.

    Encryption:
    	server side:
    		default, kms, customer_key
    		api call limitations with kms key.
    	client side:
    		https is mandatory

    	kms limits on api calls for Decrypt + GenerateDataKey API calls

    policy usages:
    	force encryption while uploading by adding condition in policy
    	force use only https

    Allow CORS from another bucket to allow the first s3 bucket host. ( preflight request & response for methods )

    MFA Delete protection
    S3 access logs to another bucket
    Pre-signed url
    	for authorization => the permissions will be similer to the one who created that pre-signed url.
    	expiration: console (12H), cli (7D)
    Glacier vault lock & S3 object lock  =>
    	write once read many
    	Retention mode - compliance, governance
    	retention period
    	legal hold: hold the object regardless of retention
    Access Point
    	dns name + access permission
    	different folders => different access points => different users
    	access point + vpc endpoint ( restrict access from within vpc )
    S3 Object lambda
    	modifying data on fly before retrieval
    	s3 object => access point => lambda => lambda access point => application

    Doubts:
    	who are seeting up the header to encrypt & decrypt data

Storage Extras:

    Snow Family:
        storage , edge processing, data migration / transfer

    	Snowcone:
    		2CPU / 4GB RAM
    		snowcone:
    			8TB
    		snowcone ssd:
    			14TB
    		Data sync
    		migration size: 24TB, offilne online
    	Snowball edge:

    		storage optimized:
    			40 CPU / 80 GB RAM
    			80 TB
    		compute optimized:
    			104 CPU / 416 GB RAM
    			42TB
    		migration size: PetaBytes, offilne
    		large data migrations, DC decommission, disaster recovery
    	SnowMobile:
    		capacity < 100PB
    		migration size: ExaBytes, offilne online
    		better for transfer > 10PB

    	AWS OpsHub: manage snowfamily devices

    FSx:
    	FSx for windows ( SMB,NTFS )
    	FSx for lustre ( HPC parallel computing )
    		Scratch
    		Persistent
    	FSx for NetApp ONTAP ( SMB,NFS,ISCSI )
    	FSx for OpenZFS ( NFS )

    Storage Gateway:
    	S3 File Gateway ( SMB,NFS )
    	FSx file Gateway ( SMB,NTFS )
    	Volume Gateway ( ISCSI )
    		Cached Volumes
    		Stored Volumes
    	Tape Gateway (ISCSI)

    Transfer Family: S3 + EFS ( by connecting with Microsoft AD for authentication along with cognito )

    DataSync:
    	S3, EFS, FSx

Integration & Messaging:

    async communication
    SQS:
        4 - 14 days retention
        256KB
        receive 10 msgs at a time
        delete after processing it
        auto scale based on no of msgs no queue ( cloudwatch metric , alarm )
        security:
            encryption
            user access
            reource based policy
        message visibility ( 30 sec ), ChangeMessageVisibility
        long polling at queue , WaitTimeSeconds: 1-20 sec at consumer
        duplicate, un-order
    FIFO SQS:
        no duplicates, order
        300 msg/s without batching, 3000 msg/s with
    SNS:
        pub/sub model
        12,500,000 subscribers
        1,000,000 topics
        message filtering ( while sending to many queues )
    SNS + SQS Fan Out


Database:

    Security:
    	Encryption using KMS
    	Authentation using IAM
        TLS
    	Security groups
    	Cant use SSH

    BackUp:
    	automated 1 to 35 days retention ( cann't be disable on aurora)
    	PTR
    	long-term -> on demand backup ( manually )

    Restore Options
        create snapshot of on-premise database
        store the backup file in s3
    	restore it back to aws database

    RDS:
    	suitable for unpredictable workloads.
    	automatic scalling storage. ( specify the maximum storage )
    	15 read replicas across region
    	free cost for read replicas in same region.
    	one dns name, standby instance, automatic failover
    	single to multi az with zero downtime ( modify the db settings )

    	RDS : managed by aws
    	Custom: full admin access to human. even we can ssh into it.

    	RDS Proxy:
    		to pool all the requests from all applications and reduce the traffic to go to the db.
    		improve efficiency by reduce the stress on db
    		failover time up by 66%
    		Enforce IAM authencation

    	You can not create encrypted Read Replicas from an unencrypted RDS DB instance.

    Aurora:
    	not open source
    	compatable with Postgres and MySql
    	5X performance than MySql on RDS & 3X performance than Postgres
    	Automatically increment 10GB when you use more space (upto 128TB)
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
    			if master fails one of this will became Master

    									client
    			writer end point                        reader end point
    			master                                  read_replicas
    				shared storage volume (10Gb, upto 64TB)

    			Custom endpoints:
    				you can create couple of endpoints for different size of replicas for different use cases ( data analytics queries )

    	Aurora Multi-Master :
    		all r/w replicas

    	Aurora Serverless:
    		automated database installation + auto scalling

    	Aurora global:
    		1 primary region, 5 secondary regions -> supports 16 read replicas
    		decrease latency
    		promoting another region ( RTO < 1m )
    		replication < 1s
            replication lag 10ms

    	Clone:
    		faster than backup & restore.
    		copy on write protocol ( won't copy until you start writing into it )

    	Aurora machine learning:
    		Sagemaker (ML model)
    		Comprehend (Sentiment analysis)

    RDS proxy :
    	reduce the main instance connections ( less stress )
    	reduce failover time by 66% because instead of our applications handle it single RDS proxy will handle it.
    	never publicly accessble

    ElastiCache:
    	It's same how RDS managing Relation database. It's managing Redis & Memcache
        heavy changes from application side
    	Architecture:
    		Storing recent queries in cache
    		Storing session data for user login in cache

    		Redis Sorted sets -> both uniqeness & element ordering ( leader boarding )

    	only for
    		1. Redis
    			IAM Auth
    			token/password
    			SSL

    			backup & restore
    			high availability / replication
                persistant
    			Sorted sets

    		2. MemCache
    			SASL

    			no backup & restore
    			no high availability
                non-persistant
    			multi-threaded

    DocumentDb
        aws version of mongoDb
        no sql, json data
        same as aurora

    Neptune
    	aws version of graph
    	3 azs, 15 replicas

    Amazon KeySpaces ( serverless )
    	Apache cassandra
    	3 times across multi az
    	Cassandra Query Language
    	IOT devices info

    QLDB ( serverless )
        hightly available,multi az
    	quantum ledger
    	replication across 3 az
    	recording financial transactions: review history of all the changes made to your application data
    	immutable
    	blockchain

    Timestream ( serverless )
        scalable
    	time series database
    	storage tiering
    	Built-in time series analytics functions
    	IOT apps

Serverless:

    It doesn't mean no server, it means you don't manage them
    virtual functions on demand

    lambda:
        memory 128MB - 10GB
        disk capacity 512MB - 10GB
    	maximum execution 15 mins
    	4KB variables
        compressed 50MB, uncompressed 250MB
    lambda snapstart
    lambda in vpc ( eni )
    invoke lambda from rds instance
    connect to sns / eventbridge
    lambda edge:
    	authenticate users at the CloudFront Edge Locations instead of authentication requests go all the way to your origins
        cloudfront function ( javascript, < 1ms )
        lambda@edga ( nodeJs, python  5 - 10 sec )

    DynamoDB:
    	no sql, higly scalable + available , auto scalling, multi az replication
    	400KB per row
    	provision mode -> RCU , WCU  ( decoupled )
    	on-demand mode & auto scale
    	import , export to s3 ( RCU & WCU won't be utilized here )

    	DynamoDB Accelerator (DAX):
    		DAX cluster for in-memory caching data
    		no application changes
    		5 min TTL

    	DynamoDb Streams
    		DynamoDB Streams enable DynamoDB to get a changelog and use that changelog to replicate data across replica tables in other AWS Regions.
    	DynamoDb global tables
    		active-active
    	Backups
            continuos ( 35 days )
            on-demand ( infinite )
        can't connect lambda directly from dynamoDb, you need dynamoDb Stream to do that.
        import & export to s3 ( RCU & WCU won't be used ) -> ETL to transform data inbetween
    
    API gateway:
        integrations:
            lambda
            endpoint
            service ( to expose publicly )
        endpoint types:
            edge optimized ( default )
            regional
            private
        security
            iam roles
            congnito
            certificates
    
    Step function -> to orchastrate lambda functions in a workflow
    
    Congnito:
        User Pool
            Sign in functionality for app users
        Identity Pool
            Provide AWS credentials to users so they can access AWS resources directly

Data & Analysis

    Athena:
    	serverless query executor for s3 access logs.
    	better for adhoc request ( quick to load )
        using data source connector in lambda it will execute federated queries on DynamoDb, Redshift, RDS, Aurora, Elasticache, etc 
		columnar data > 128MB file

    Redshift:
    	online analytical processing
    	columnar storage
    	better for large queries & joins ( but need to load data first )
    	Leader node + Compute nodes =  query plannig & result aggregation + query execution

    	redshift spectrum :
    		query data without loading the data from s3
    		the query will be splitted to thousands of redshift spectrum nodes and those will read data from s3 and perform queries then send the result to compute nodes.

    OpenSearch :
    	search by any filed
		security:
			congnito
			iam
			kms
			tls
    	storage classes:
    		hot : ebs + query + visualization
    		warm : s3 + query degradattion + visualization
    		cold : s3

    EMR : elastic map reduce
		data processing, machine learning, web indexing, big data
        on-demand, reserved, spot
    	hadoop cluster
    		master node
    		core node
    		task node

    Quicksight:
    	serverless interactive dashboards
    	in-memory computation
        column level security
		user , groups, dashboards

    Glue:
    	serverless
    	extract + transform + load

    Lake formation:
        clean, normalise
    	central place for all data ( s3 , rds, aurora )
    	Glue + Central access control for users

    Kinesis data analytics:
    	Kinesis data analytics for sql:
    		sources: data stream , firehose
    		targets: data stream , firehose
    	Kinesis data analytics for apache flik:
    		sources: data stream , msk

    MSK:
    	alternative for kinesis
        MSK creates & manages Kafka brokers nodes & Zookeeper nodes for you
        up to 3 for HA
    	server / serverless
        EBS
        1MB default till 10MB message size

    Data ingestion pipeline:
    	collect data	Kinesis
    	transform		lambda
    	query			athena
    	reports			s3
    	visualization	quicksight

    DynamoDb : serverless + non-relational
    Athena : serverless + s3
    RedShift : server + postgresql
    OpenSearch :  server / serverless

Monitoring:

    CloudWatch:
    	cloudwatch metric
    	cloudwatch metric stream
    	logs insights
    		run query on logs
    	logs export s3: 12 days
    	logs subscription:
            real-time logging
    		cross-accounts + cross-regions
    		filter logs
    	ec2 logs:
    		logs agent: for logs
    		unified agent: for metrics + logs
    	Live tail : to debug the issue
    	alarm:
    		targets:
    			ec2
    			auto scaling
    			sns
    		composite alarm: monitor the state of other alarms
            can be created based on data

    	EventBridge:
    		schedule cron jobs
    		event rules to react on service events

    		filter the events from source and create json format then send it to destination

    		default event bus + partner event bus + custom event bus

    		schema registry

    	CloudWatch Insights:
    		Container insights
    		lambda insights
    		Contributor insights -> top-n contributors
    		Application insights

    Cloudtrail
        90 Days retention period
    	management events + data events
    	management event ( write events ) -> cloudtrial_insights -> insights_events => s3 / cloudtrail_console / eventbridge
    	better combination : API calls + Cloud Trail + Event Bridge

    Aws Config:
    	every resource will be monitored based on complaince rules
    	record config changes : oka resource ni select chesi em em changes / edits ayyayo chuudochu

    	cloudtrail is little different, it records API calls within account by everyone. but aws config will evaluate resources agains compliance rules & config changes to resource & get timeline of changes

    	config remediation: going back ( with retries )
    	config notifications -> event bridge + sns

Machine Learning:

    Rekognition: face detection, labeling, celebrity recognition
    Transcribe: audio to text (ex: subtitles)
    Polly: text to audio
    Translate: translations
    Lex: build conversational bots – chatbots
    Connect: cloud contact center
    Comprehend: natural language processing
    Comprehend medical
    SageMaker: machine learning for every developer and data scientist • Forecast: build highly accurate forecasts
    Forecast
    Kendra: ML-powered search engine
    Personalize: real-time personalized recommendations 
    Textract: detect text and data in documents 

Security:

    KMS key
    	multi-region ( treated as individual keys )
    	s3 replication
    	ami copy
    Parameter store
    	10,000 + 4KB, 100,000 + 8KB
    	serverless
    	policies + notifications ( eventBridge )
    	secureString with kms
    		with_decrypt flag
    Secret manager
    	lambda for rotation
    	connection with RDS => RDS will be updated when there is a rotation happened for credentials
    ACM
    	public + private certificates
    	in-flight encryption
    	can't connect to ec2
    	public will be renewed automatically
    	private certificate:
    		validataion method:
    			dns validataion : oka record create cheyyali aa domain owner maname ani proove cheyyadaniki
    			email validation
    	import chesina certificates ni malli renew chesi import cheyyali
    	events from 45 days prior to expiry
    	Integrations:
    		alb
    		cloudfront
    		api gateway:
    			edge optimized
    			regional
    WAF:
    	10,000 rules
    	http body , http heaaders, geo-match, rate-based , uri
    	not for NLB
    AWS Sheild: DDos attack
    AWS Firewall manager: manage rules on all accounts under organization
    GuardDuty:
    	Threat discovery
    	Cloudtrail logs
    	VPC logs
    	DNS logs
    	Optional features: s3 , ebs, eks audit

    	EventBridge
    Inspector:[cve]
    	Automatic security check for vulnerabilities

    	Ec2 instances
    	Container images
    	lambda function

    	reporting to security hub + event bridge
    Macie:
    	discover & protect sensitive data
    	s3 data

Disaster Recovery:

    RPO
    RTO
    Strategies:

    	Backup and Restore
    	Pilot light
    		critical parts will be up, can create ec2 instances when disaster happens
    	Warm standby
    		will be up with minimul setup
    	Hot site / Multi site
    		everything is up
    	Multi region
    		everything up on different region
    Tips:
        Backup
        High Availability
        Replication
        Automation
        Chaos

Data Migrations:

    DMS ( an ec2 instance with CDC which is used to replicate the data  )
    SCT ( only when migrating to different engine )
    need an compute-intensive ec2 instance to run DMS + SCT
    DMS multi-az ( standby )
    RDS & Aurora migrations:
        Internal:
            1. snapshot from RDS and create Aurora DB
            2. Aurora read replica of RDS and promote it as primary
        External:
            create backup file
            upload to s3
            create db with s3 backup file
        If both are up and running DMS is best suite.
    SMS -> incremental replication of on-premise live servers to AWS
    Application Discovery Service & Migration Hub
    Application migration service
    VMware Cloud

    Aws BackUp
        on-demand & scheduled
    	plan : retention , frequency, window, transition to cold storage
    	Bakup Vault ( WORM ) -> Even the root user cannot delete backups when enabled

    large amount of data transfer:
    	site-to-site - 100Mbps
    	direct connect - 1Gbps ( 1 month to setup )
    	Snowball

    	for on-going use DMS / Datasync with above ones

AWS Networking:

    NAT Gateway
        5 - 100 GBps

    Site to Site Connection:
        Virtual Private Gateway
        Customer Gateway
        AWS VPN CloudHub

    Direct Connect:
        dedicated private network + data in transit not encrypted
        Direct Connect Gateway to connect one or more VPC in many different regions
        Types:
            Dedicated ( 1, 10, 100 GBPS )
            Hosted ( 50MB,500MB,10GB )
        Direct Connect + VPN = IPsec encrypted private connection
        Direct Connect - Resiliency
        Site to Site as a backup

    Transit Gateway:
        having transitive peering between thousands of VPC and on-premises
        Integration with Direct Connect Gateway, VPN connections
        Equal-cost multi-path routing ( more STS connections )
        AWS Resource Access Manager to share Transit Gateway with other accounts.

    VPC - Traffic Mirroring:
        send traffic to security appliances
    
    Egress Only Internet Gateway -> Same as NAT for IPV6
    
    Networking Cost Savings:
        Use private Ip so that traffic won't go via public internet
        Query the db first then send the tiny result over the internet
        Use gateway endpoint for s3 & dynamodb
    
    Aws Network Firewall:
        Protect your entire Amazon VPC

Other Services:

    HPA:
        Intel 82599 VF up to 10 Gbps – LEGACY
        Elastic Network Adapter (ENA) up to 100 Gbps
        Elastic Fabric Adapter ( only for linux )
    Systems Manager:
        Integration with S3 and Cloudwatch & SNS & EventBridge
        Run Command
        Patch Manager
        Maintenance Windows
            Schedule
            Duration
            Ec2 instances
            Tasks
        Automation Runbook
    cost optimizer
        Forecast usage up to 12 months
    aws batch
        Docker images and run on ECS
    aws appflow ( Slack / ServiceNow -> S3 / Redshift )
    Amplify
        deploy mobile / web application

    Well-Architected Tool
        review against 6 pillers
    Trusted Advisor
        provide recommendations over 6 categories

Boring to Remember:

    EBS:
        gp3: 1Gb - 16Tb
            3K,125MB
            16K,1000MB
        gp2: 1Gb - 16Tb
            3K - 16K 
            3:1 (IOPS:Gb)
        io1: 4Gb - 16Tb
            32K, 64K
        io2: 4Gb - 64Tb
            256K
            1000:1 ( IOPS:Gb )
        st1: 125Gb - 16Tb
            500, 500 MB
        sc1: 125Gb - 16Tb
            250, 250 MB
    EFS:
        Performance Mode:
            General Purpose
            Max IO
        Throughput Mode:
            Bursting 1TB = 50 MB + 100
            Provisioned ( 1GB per 1TB )
            Elastic

    Instance types:
        On-Demand : 60 Sec 
        Reserved : 75%
        Spot Instance: 90%
        Savings:
            Ec2 Savings Plan: 72% ( Region, Family fix )
            Compute Savings Plan : 66% ( Anything )

Mock Test Reviews:

    Use AWS Global Accelerator to distribute a portion of traffic to a particular deployment.
    In blue/green deployment if you wanna test the deployment without thinking about DNS cache, then use Global Accelerator.
    lowest latency and provide fast regional failover -> Global Accelerator

    AWS Compute Optimizer to look at instance type recommendations
    
    You cannot use Transfer Acceleration to copy objects across Amazon S3 buckets in different Regions using Amazon S3 console.
    S3 replication: S3 batch & S3 sync

    Can't modify Stadanrd queue to FIFO.Should create new FIFO / delete the stadard and convert it to FIFO.FIFO should have suffex as .fifo
    FIFO will have 3000 thoughput with batching, 300 without batching
    
    Use lambda layer for re-usable code. upto 5 layers in a function.

    With launch template you can provision capacity across multiple instance types, but not with launch configuration.

    Nat instance supports port forwarding & bastion server

    Use VPC sharing to share one or more subnets with other AWS accounts belonging to the same parent organization from AWS Organizations
    
    DMS to migrate from s3 to KDS & databases to Redshift

    high volume of read traffic, reduce latency, and also downsize the instance size to cut costs => ElasticCache ( not read replicas, cost matters )

    Fsx luster -> 
        worlds most high performance file system. designed for applications that require fast storage
        can integrate with s3 and FSx for Lustre file system transparently presents S3 objects as files and allows you to write changed data back to S3.

    user data -> first time at launch, root user.
    
    Redshift spectrum -> s3 data
    
    Aws config -> imported certificate expiry notification

    Direct Connect:
        DTI - free
        DTO - going to outside aws will be changed but transfer to inside aws won't be charged
    
    Throttling is the process of limiting the number of requests an authorized program can submit to a given operation in a given amount of time. API Gateway will provide this
    
    If you specify targets using an instance ID, traffic is routed to instances using the primary private IP address specified in the primary network interface for the instance
    If you specify targets using IP addresses, you can route traffic to an instance using any private IP address from one or more network interfaces
    
    Compute Optimizer -> EC2,ASG,EBS,Lambda 

    dead-letter queue to collect failed messages
    AWS recommend using separate queues when you need to provide prioritization of work
    
    Amazon RDS creates an SSL certificate and installs the certificate on the DB instance when Amazon RDS provisions the instance.

    OriginGroup -> An origin group includes two origins (a primary origin and a second origin to failover to) and a failover criteria that you specify.
    
    tightly-coupled High Performance Computing -> Elastic Fabric Adapter 

    two regions lo low latency queries annadante inka global db eh ( dynamodb, aurora )

    block protocols -> iscsi -> volume gateway (cache )
    file protocols -> nfs  -> file gateway ( cache )

    migrating to s3 / efs / fsx then first think about DataSync then only DMS
    AWS Database Migration Service (DMS) is used for migrating databases, not data on file shares.
    
    Lambda@Edge is not used to direct traffic to on-premises origins.

    Availability for on-premise database -> migrate to RDS multi-az 
    
    task_role -> s3 , dynamodb access
    task_execution_role -> access to pull image, send logs
    container_instance_role -> to the entire ec2 instance ( means all tasks under instance )

    On-Demand Capacity Reservations enable you to reserve compute capacity for your Amazon EC2 instances in a specific Availability Zone for any duration

    maximum performance -> use Direct connect over site-to-site connection

    Mock-Test-1:
        mistake - multi az rds & ec2
        mistake - sqs over kds
        don't know - rds ssl
        mistake - backup with support details using cloudfront
        don't know - cloudfront origin group ( availability of origin )
        mistake - cloudfront to block countries
        mistake - hibernate the instance for instance memory preservation
        mistake - storage must be mounted using the NFS protocol ( EFS , not s3)
        mistake - cache for block & efs file storages (volume gateway, file gateway)
        mistake - notification service with decouple functionality ( SNS )
        mistake - migration from fsx to s3 ( datasync over dms )
        don't know - improve performance of on-premise dynamic app by having cache using cloudfront
        mistake - multi az ( mq,ec2,database )
        don't kmow - use deny over the allow as much as possible ( SCP with a deny rule that denies all but the specific instance types )
        mistake - scale middle tier ec2 instances
        don't know - ec2 when we need it ( capacity reservations(without commitment), zonal reserved instances(with commitment) )
        mistake - made ec2 instance available to internet ( use alb with new public subnets )
        mistake - maximum performance while connecting on-premise to vpc -> Direct connect over site-to-site
        don't know - AWS recommend using separate queues when you need to provide prioritization of work
        mistake - didn't understood it properly at that time. encrypt of read-replica only possible with new encrypted snapshot and new db creation.
    
    Mock Test - 2:
        private connection -> damn sure not SiteToSite connection. either snow family / direct connect ( if time permits )
        millisecond responsiveness  -> DynamoDb over Redshift
        near real-time performance  -> KDS over SQS
        durable and loosely coupled solution for storing jobs  -> SQS
        resilient storage for Windows instances -> Fsx windows server
        block HTTP or HTTPS traffic to domains -> domain list rules in network firewall
        AWS Batch is designed to run jobs across multiple instances
        NLB should be in the service provider VPC & Endpoint should be in the consumer VPC
        AWS recommend you use target tracking in place of step scaling for most use cases
        Amazon ECS uses the AWS Application Auto Scaling service to scales tasks. This is configured through Amazon ECS using Amazon ECS Service Auto Scaling.
        Redshift -> RedShift can also improve performance for repeat queries by caching the result and returning the cached result when queries are re-run.
        hybrid cloud storage -> services that provide on-premises access to virtually unlimited cloud storage -> Storagete gateway
        Accounts can be migrated between organizations using AWS Organizations console.

    Mock Test - 3:
        DNS failover configurations:
            active-passive -> failover
            active-active -> return many 
            combination -> use more policies
        Encryption to Direct connect connection -> VPG 
            A VPG is used to setup an AWS VPN which you can use in combination with Direct Connect to encrypt all data that traverses the Direct Connect link.
            This combination provides an IPsec-encrypted private connection that also reduces network costs, increases bandwidth throughput
        RDS -> KMS uses a customer master key (CMK) to encrypt the DB instance, all logs, backups, and snapshots.
        Multi-AZ RDS Read Replica -> create standby replica for your read replica. when availability matters for read replica regardless of whether the main instance is multi-az
        FSx for Windows File Server does not use Amazon S3
        API Gateway REST API directly accesses the data in the DynamoDB table
        Route 53 health check to route traffic to standby alb
        resilient architecture -> ASG + ALB
        static ip -> global accelerator since cloudfront won't work here.
        Aurora Multi master only works within a Region it does not work across Regions.
        S3 Replication -> Both source and destination buckets must have versioning enabled.
        If multi-az is available no need to create read replica, use the standby replica for read queries.
        It is not supported to connect Fargate to FSx for Lustre.

    Mock-Test-4
        cloudwatch event rule -> sns
        install Linux-based software application -> ec2, not lambda
        aws:Secure Transport header  -> SSL/TLS
        x-amz-server-side-encryption -> encrypt an object at the time of upload 
        RAID-0 : I/O matters then fault tolerance
        RAID-1: fault tolerance matter then I/O
        Ec2 monitoring:
            basic monitoring -> 5 mins periods ( enabled by default )
            detailed monitoring -> 1 min periods
        AWS Data Exchange -> get third-party telemetry data
        AWS Private 5G -> scale your own private cellular network
        management account - enable organization trails option while creating new trail, members don't have write permissions on trials, read permissions on s3 logs.
        AWS License Manager -> manage the software licenses
        Fsx multi-az
        mysql -> 3306
        risk with spot instances that it would increase the cost
        If there is a doubt between lambda and fargate on serverless situation always think about time to procees it.
        SQS does not ingest data. If there is a kinesis data firehose go ahead with that.
        service-managed tightly-coupled HPC deployment across nodes -> Aws Batch multi-node parallel job
        db scale without downtime -> DynamoDb
        AWS Control Tower automates the setup of a new landing zone -> pre-approved account,network configurations
        AWS AppSync is a serverless GraphQL and Pub/Sub API service that simplifies building modern web and mobile applications.
        Backup for Direct Connect -> IPSec VPN connection and use the same BGP prefix
        For VPCs in the same region a VPG is not necessary
        config rule -> state change actions will be integrated to eventBridge to take actions

    Mock-Test-5:
        EBS fast snapshot restore: fully initialized at creation & high I/O performance
        You cannot restore EBS snapshots as instance store volumes
        The Redis engine does not support multiple CPU cores or threads.
        secure and unique URLs for each customer = Registering a wildcard custom domain name in Route 53 + creating a record pointing to API Gateway + Requesting a wildcard certificate in the same AWS region 
        CloudWatch Logs subscription: cloudwatch log group stream data to open search.
        S3 pre-signed URLs : for individual file access
        CloudFront signed cookies : access to multiple files in one or multiple directories
        Cost Explorer's granular filtering feature to perform an in-depth analysis of EC2 costs based on instance types.
        Game : Cognito user pool, Amplify with Cloudfront + Api gateway, Lambda, DynamoDb
        sharing the dashboard from Cloud watch console to the people who don't have direct aws access to your account
        EFS replication RPO - 15Min
        canary release deployment -> to gradually roll out the new API version
        stateless application -> EKS
        benefits of Reserved Instances -> AWS Organization + Sharing is enabled
        A two-way trust is required by IAM Identity Center
        SensitiveData:S3Object/Personal -> macie event
        Data Lifecycle Manager to automate snapshots

        3[A/D],1[B/C],4[A/C],26[A/C],35[a/d]
DK
M
DU
M
M
M
M 15
M
M
M
Doubts:

    how to enable tls/ssl on rds ? Answer: It will be automatic
    backup website ? s3 / ec2 ? Answer: If static files then s3
    cloudfront orgins ? at a time s3 and inkokati undocha ? Answer: Undochu OriginGroup tho at a time two origin pettochu. primary + secondary
    Is efs multi region ? kakapothe multi-region vachinapudu s3 chuskocha ? Answer: replication will be there but not sure about mounting.
    which is secure for rds ? iam / tls ? Answer: both are little different iam evariki access ivvalo chustadhi, tls aa connections secure ga undela chustadhi
    durable ? reliable ? resiliency ?
    can we create route53 record for lambda endpoint ? No
    Is simple and step policies are different ? Yes
    can NLB attached to instances from different regions ? No, it's region specific.
    SwapUtilization metric -> Aws managed
    Differences between aurora & aurora serverless 
    Difference between NAT gateway & virtual private gateway ? NAT will go through public internet, VPG is purely private connection
    what is reduced redundency in s3 life cycle policiies

WhitePapers:

    Global,Available,Scalable
    Higher-level managed services
    Built-in Security
    Archiving for cost
    
    Design Principles:
        scalability
        stateless application
        Distribute load to multiple nodes
            Push : ALB,NLB,Route 53 rounb robin
            Pull : SQS
        stateless components
            all session data
            session id
            s3,efs
        stateful components
            gaming environment ( horizontal scaling using affinity )
        Implement session affinity
            through alb sticky functionality
            custom functionality from client software
        Distributed processing
            Offline ->  aws batch , apache hadoop, glue
            aws -> EMR hadoop
            real-time -> kinesis
        Disposable resources instead of fixed resources
            fixed resource has a problem of drift configuration
        Instantiating Compute Resources
            Bootstrapping
            Golden Images
            Containers
        Automation for events happening on aws services
            IAC
            Beanstalk
            ec2 recovery
            Systems manager
            Auto scaling
            Alarm & Events
        Loose coupling:
            RESTful API
            Asynchronous Integration -> SQS
        Serverless:
            Lambda
            API gateway
            Cognito
            lambda Edge
            Athena
        Databases:
            SQL
            NOSQL
            Data warehouse -> Redshift
            Search
            Managing increase volume of data -> Data lake
        Removing Single Points of Failure
            Redundancy
                standby
                active
            Detect Failure & React
                using metrics and alarms
            Design Good Health Checks
                layered health checks ( better )
                deep health check ( not only instance is up, checking connection btw db )
            Durable Data Storage
                Replication:
                    Synchronous: A transaction can’t be acknowledged before all replicas have performed the write. it is not recommended to maintain many synchronous replicas
                    Asynchronous replication:  decouples the primary node from its replicas at the expense of introducing replication lag.
                    Quorum-based replication: combines synchronous and asynchronous replication
            Automated Multi-Data Center Resilience
                failover ( RDS autofailover )
                Shuffle Sharding
    
    Optimize for Cost:
        Right Sizing
        Elasticity
        Take Advantage of the Variety of Purchasing Options
            Reserved Instances  ->  AWS Trusted Advisor or Amazon EC2 usage report
            Spot Instances
    
    Cache:
        Application Data Caching -> Elasticache
        Edge Caching
            Cloudfront, Edge locations
            Amazon CloudFront reuses existing connections between the Amazon CloudFront edge and the origin server, which reduces connection setup latency for each origin request

    Security:
        Guard -> VPC topology
        human readable -> Data encryption
        long term credentials or service accounts
        Cognito
        Security as Code:
            Golden Environment -> This template is used by AWS CloudFormation and deploys your resources in alignment with your security policy
        Aws cloudwatch -> logs to reproduce it on lower envs and troubleshoot the issue
        Aws config
        Aws cloudtrail


aws.amazon.com/architecture
aws.amazon.com/solutions
https://digitalcloud.training/aws-application-integration-services/

white papers:

    https://d1.awsstatic.com/whitepapers/AWS_Cloud_Best_Practices.pdf
    https://d0.awsstatic.com/whitepapers/AWS_Serverless_Multi-Tier_Architectures.pdf

