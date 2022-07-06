# Best Practices for WordPress on AWS


- This whitepaper provides system administrators with specific guidance on how to get started with WordPress on AWS and how to improve both the cost efficiency of the deployment as well as the end user experience. 

- It also outlines a reference architecture that addresses common scalability and high availability requirements.
- When the first version of WordPress was released in 2003, it was not built with modern elastic and scalable cloud-based infrastructures in mind. 

- Through the work of the WordPress community and the release of various WordPress modules, the capabilities of this CMS solution are constantly expanding.
- Today, it is possible to build a WordPress architecture that takes advantage of many of the benefits of the AWS Cloud.

## Simple deployment

- For low-traffic blogs or websites without strict high availability requirements, a simple deployment of a single server might be suitable. 
- This deployment isn’t the most resilient or scalable architecture, but it is the quickest and most economical way to get your website up and running.

### Considerations

- This discussion starts with a single web server deployment. 
- There may be occasions when you outgrow it, for example:

    - The virtual machine that your WordPress website is deployed on is a single point of failure. 
    - A problem with this instance causes a loss of service for your website.
    
    - Scaling resources to improve performance can only be achieved by “vertical scaling;” 
    - That is, by increasing the size of the virtual machine running your WordPress website.



### Available approaches

- AWS has a number of different options for provisioning virtual machines. 
- There are three main ways to host your own WordPress website on AWS:

- Amazon Lightsail
- Amazon Elastic Compute Cloud (Amazon EC2)
- AWS Marketplace


#### Amazon Lightsail

Lightsail is the easiest way to get started on AWS for developers, small businesses, students, and other users who need a simple VPS solution.

The service abstracts many of the more complex elements of infrastructure management away from the user. 

It is, therefore, an ideal starting point if you have less infrastructure experience, or when you need to focus on running your website and a simplified product is sufficient for your needs.

With Amazon Lightsail, you can choose Windows or Linux/Unix operating systems and popular web
applications, including WordPress, and deploy these with a single click from preconfigured templates.

As your needs grow, you have the ability to smoothly step outside of the initial boundaries and connect to additional AWS database, object storage, caching, and content distribution services.


##### Selecting an Amazon Lightsail pricing plan

A Lightsail plan defines the monthly cost of the Lightsail resources you use to host your WordPress website. 

There are a number of plans available to cover a variety of use cases, with varying levels of CPU resource, memory, solid-state drive (SSD) storage, and data transfer. 

If your website is complex, you may need a larger instance with more resources. 

You can achieve this by migrating your server to a larger plan using the web console or as described in the Amazon Lightsail CLI documentation.

#### nstalling WordPress

Lightsail provides templates for commonly used applications such as WordPress. 

This template is a great starting point for running your own WordPress website as it comes pre-installed with most of the software you need. 

You can install additional software or customize the software configuration by using the in-browser terminal or your own SSH client, or via the WordPress administration web interface.

Amazon Lightsail has a partnership with GoDaddy Pro Sites product to help WordPress customers easily manage their instances for free. 

Lightsail WordPress virtual servers are preconfigured and optimized for fast performance and security, making it easy to get your WordPress site up and running in no time.

Customers running multiple WordPress instances find it challenging and time-consuming to update, maintain and manage all of their sites. 

With this integration, you can easily manage your multiple WordPress instances in minutes with only a few clicks. 

For more information about managing WordPress on Lightsail, refer to Getting started using WordPress from your Amazon Lightsail instance. 

Once you are finished customizing your WordPress website, we recommend taking a snapshot of your instance. 

A snapshot is a way to create a backup image of your Lightsail instance. 

It is a copy of the system disk and also stores the original machine configuration (that is, memory, CPU, disk size, and data transfer rate). 

Snapshots can be used to revert to a known good configuration after a bad deployment or upgrade. 

This snapshot allows you to recover your server if needed, but also to launch new instances with the same customizations.


### Recovering from failure

A single web server is a single point of failure, so you must ensure that your website data is backed up.

The snapshot mechanism described earlier can also be used for this purpose. 

To recover from failure, you can restore a new instance from your most recent snapshot. 

To reduce the amount of data that could be lost during a restore, your snapshots must be as recent as possible.

To minimize the potential for data loss, ensure that snapshots are being taken on a regular basis. 

You can schedule automatic snapshots of your Lightsail Linux/Unix instances. 

For steps, refer to Enabling or disabling automatic snapshots for instances or disks in Amazon Lightsail.

AWS recommends that you use a static IP: a fixed, public IP address that is dedicated to your Lightsail account. 

If you need to replace your instance with another one, you can reassign the static IP to the new instance. 

In this way, you don’t have to reconfigure any external systems (such as DNS records) to point to a new IP address every time you want to replace your instance.


### Improving performance and cost efficiency

You may eventually outgrow your single-server deployment. 

In this case, you may need to consider options for improving your website’s performance. 

Before migrating to a multi-server, scalable deployment , there are a number of performance and cost efficiencies you can apply. 

These are good practices that you should follow anyway, even if you do move to a multi-server architecture.

The following sections introduce a number of options that can improve aspects of your WordPress website’s performance and scalability. 

Some can be applied to a single-server deployment, whereas others take advantage of the scalability of multiple servers. 

Many of those modifications require the use of one or more WordPress plugins. 

Although various options are available, W3 Total Cache is a popular choice that combines many of those modifications in a single plugin.


#### Accelerating content delivery

- Any WordPress website needs to deliver a mix of static and dynamic content. Static content includes images, JavaScript files, or style sheets. Dynamic content includes anything generated on the server side using the WordPress PHP code, for example, elements of your site that are generated from the database or personalized to each viewer.

An important aspect of the end-user experience is the network latency involved when delivering the previous content to users around the world. Accelerating the delivery of the previous content improves the end-user experience, especially users geographically spread across the globe. This can be achieved with a Content Delivery Network (CDN) such as Amazon CloudFront.

Amazon CloudFront is a web service that provides an easy and cost-effective way to distribute content with low latency and high data transfer speeds through multiple edge locations across the globe. Viewer requests are automatically routed to a suitable CloudFront edge location to lower the latency. 

If the content can be cached (for a few seconds, minutes, or even days) and is already stored in a particular edge location, CloudFront delivers it immediately. If the content should not be cached, has expired, or isn’t currently in that edge location, CloudFront retrieves content from one or more sources of truth, referred to as the origin(s) (in this case, the Lightsail instance) in the CloudFront configuration. 

This retrieval takes place over optimized network connections, which work to speed up the delivery of content on your website. Apart from improving the end-user experience, the model discussed also reduces the load on your origin servers and has the potential to create significant cost savings.

#### Static content offload

This includes CSS, JavaScript, and image files – either those that are part of your WordPress themes or those media files uploaded by the content administrators. 

All these files can be stored in Amazon Simple Storage Service (Amazon S3) using a plugin such as W3 Total Cache and served to users in a scalable and highly available manner. 

Amazon S3 offers a highly scalable, reliable, and low-latency data storage infrastructure at low cost, which is accessible via REST APIs. 

Amazon S3 redundantly stores your objects, not only on multiple devices, but also across multiple facilities in an AWS Region, thus providing exceptionally high levels of durability.

This has the positive side effect of offloading this workload from your Lightsail instance and letting it focus on dynamic content generation. 

This reduces the load on the server and is an important step towards creating a stateless architecture (a prerequisite before implementing automatic scaling).

You can subsequently configure Amazon S3 as an origin for CloudFront to improve delivery of those
static assets to users around the world. 

Although WordPress isn’t integrated with Amazon S3 and CloudFront out-of-the-box, a variety of plugins add support for these services (for example, W3 Total Cache)


#### Dynamic content

Dynamic content includes the output of server-side WordPress PHP scripts. Dynamic content can also be served via CloudFront by configuring the WordPress website as an origin. 

Since dynamic content includes personalized content, you need to configure CloudFront to forward certain HTTP cookies and HTTP headers as part of a request to your custom origin server.

CloudFront uses the forwarded cookie values as part of the key that identifies a unique object in its cache. 

To ensure that you maximize the caching efficiency, you should configure CloudFront to only forward those HTTP cookies and HTTP headers that really vary the content (not cookies that are only used on the client side or by third-party applications, for example, for web analytics).


![2](https://user-images.githubusercontent.com/23625821/138556834-6619ab4a-6b64-4907-a3b6-c0aca8f27eca.png)

CloudFront uses standard cache control headers to identify if and for how long it should cache specific HTTP responses. 

The same cache control headers are also used by web browsers to decide when and for how long to cache content locally for a more optimal end user experience (for example, a .css file that is already downloaded will not be re-downloaded every time a returning visitor views a page).

You can configure cache control headers on the web server level (for example, via .htaccess files or modifications of the httpd.conf file) or install a WordPress plugin (for example, W3 Total Cache) to dictate how those headers are set for both static and dynamic content.


### Database caching

Database caching can significantly reduce latency and increase throughput for read-heavy application workloads like WordPress. 

Application performance is improved by storing frequently accessed pieces of data in memory for low-latency access (for example, the results of I O-intensive database queries). 

When a large percentage of the queries are served from the cache, the number of queries that need to hit the database is reduced, resulting in a lower cost associated with running the database.

Although WordPress has limited caching capabilities out-of-the-box, a variety of plugins support integration with Memcached, a widely adopted memory object caching system. 

The W3 Total Cache plugin is a good example.

In the simplest scenarios, you install Memcached on your web server and capture the result as a new snapshot. In this case, you are responsible for the administrative tasks associated with running a cache.

Another option is to take advantage of a managed service such as Amazon ElastiCache and avoid that operational burden. 

ElastiCache makes it easy to deploy, operate, and scale a distributed in-memory cache in the cloud. 

You can find information about how to connect to your ElastiCache cluster nodes in the  Amazon ElastiCache documentation.

If you are using Lightsail and wish to access an ElastiCache cluster in your AWS account privately, you can do so by using VPC peering. 

For instructions to enable VPC peering, refer to Set up Amazon VPC peering to work with AWS resources outside of Amazon Lightsail.


### Bytecode caching

Each time a PHP script is run, it gets parsed and compiled. By using a PHP bytecode cache, the output of the PHP compilation is stored in RAM so that the same script doesn’t have to be compiled again and again. 

This reduces the overhead related to executing PHP scripts, resulting in better performance and lower CPU requirements.

A bytecode cache can be installed on any Lightsail instance that hosts WordPress and can greatly reduce its load. 

For PHP 5.5 and later, AWS recommends the use of OPcache, a bundled extension with that PHP version.

Note that OPcache is enabled by default in the Bitnami WordPress Lightsail template, so no further action is required.


### Elastic deployment

There are many scenarios where a single-server deployment may not be sufficient for your website. In these situations, you need a multi-server, scalable architecture.

Check <a href="https://github.com/awslabs/aws-refarch-wordpress"> here </a> to see refere architecure


### Scaling the web tier

To evolve your single-server architecture into a multi-server, scalable architecture, you must use five key components:

- Amazon EC2 instances
- Amazon Machine Images (AMIs)
- Load balancers
- Automatic scaling
- Health checks

AWS provides a wide variety of EC2 instance types so you can choose the best server configuration for both performance and cost. Generally speaking, the compute-optimized (for example, C4) instance type may be a good choice for a WordPress web server. 

You can deploy your instances across multiple Availability Zones within an AWS Region to increase the reliability of the overall architecture.

Because you have complete control of your EC2 instance, you can log in with root access to install and configure all of the software components required to run a WordPress website. After you are done, you can save that configuration as an AMI, which you can use to launch new instances with all the customizations that you've made.


To distribute end-user requests to multiple web server nodes, you need a load balancing solution. 

AWS provides this capability through Elastic Load Balancing, a highly available service that distributes traffic to multiple EC2 instances. 


Because your website is serving content to your users via HTTP or HTTPS, we recommend that you make use of the Application Load Balancer, an application-layer load balancer with content routing and the ability to run multiple WordPress websites on different domains, if required  Elastic Load Balancing supports distribution of requests across multiple Availability Zones within an AWS Region. 

You can also configure a health check so that the Application Load Balancer automatically stops sending traffic to individual instances that have failed (for example, due to a hardware problem or software crash). 

AWS recommends using the WordPress admin login page (/wp-login.php) for the health check because this page confirms both that the web server is running and that the web server is configured to serve PHP files correctly


### Stateless web tier

To take advantage of multiple web servers in an automatic scaling configuration, your web tier must be stateless. 

A stateless application is one that needs no knowledge of previous interactions and stores no session information. In the case of WordPress, this means that all end users receive the same response, regardless of which web server processed their request. A stateless application can scale horizontally since any request can be serviced by any of the available compute resources (that is, web server instances). 

When that capacity is no longer required, any individual resource can be safely terminated (after running tasks have been drained). Those resources do not need to be aware of the presence of their peers – all that is required is a way to distribute the workload to them.


When it comes to user session data storage, the WordPress core is completely stateless because it relies on cookies that are stored in the client’s web browser. 

Session storage isn’t a concern unless you have installed any custom code (for example, a WordPress plugin) that instead relies on native PHP sessions. However, WordPress was originally designed to run on a single server. As a result, it stores some data on the server’s local file system. 

When running WordPress in a multi-server configuration, this creates a problem because there is inconsistency across web servers. For example, if a user uploads a new image, it is only stored on one of the servers.

This demonstrates why we need to improve the default WordPress running configuration to move important data to shared storage. The best practice architecture has a database as a separate layer outside the web server and makes use of shared storage to store user uploads, themes, and plugins.


#### Shared storage (Amazon S3 and Amazon EFS)


By default, WordPress stores user uploads on the local file system and so isn’t stateless. Therefore, we need to move the WordPress installation and all user customizations (such as configuration, plugins, themes, and user-generated uploads) into a shared data platform to help reduce load on the web servers and to make the web tier stateless.

Amazon Elastic File System (Amazon EFS) provides scalable network file systems for use with EC2 instances. Amazon EFS file systems are distributed across an unconstrained number of storage servers, enabling file systems to grow elastically and allowing massively parallel access from EC2 instances. 

The distributed design of Amazon EFS avoids the bottlenecks and constraints inherent to traditional file servers.

By moving the entire WordPress installation directory onto an EFS file system and mounting it into each of your EC2 instances when they boot, your WordPress site and all its data is automatically stored on a distributed file system that isn’t dependent on any one EC2 instance, making your web tier completely stateless. 

The benefit of this architecture is that you don’t need to install plugins and themes on each new instance launch, and you can significantly speed up the installation and recovery of WordPress instances. It is also easier to deploy changes to plugins and themes in WordPress, as outlined in the Deployment Considerations (p. 18) section of this document.


#### Data tier (Amazon Aurora and Amazon ElastiCache)

With the WordPress installation stored on a distributed, scalable, shared network file system, and static assets being served from Amazon S3, you can focus your attention on the remaining stateful component: the database. 

As with the storage tier, the database should not be reliant on any single server, so it cannot  be hosted on one of the web servers. Instead, host the WordPress database on Amazon Aurora.

Amazon Aurora is a MySQL and PostgreSQL compatible relational database built for the cloud that combines the performance and availability of high-end commercial databases with the simplicity and cost-effectiveness of open source databases. Aurora MySQL increases MySQL performance and availability by tightly integrating the database engine with a purpose-built distributed storage system, backed by SSD. 

It is fault-tolerant and self-healing, replicates six copies of your data across three Availability Zones, is designed for greater than 99.99% availability, and continuously backs up your data in Amazon S3. 

Amazon Aurora is designed to automatically detect database crashes and restart without the need for crash recovery or to rebuild the database cache.

Amazon Aurora provides a number of instances types to suit different application profiles, including memory-optimized and burstable instances. To improve the performance of your database you can select a large instance type to provide more CPU and memory resources.


### WordPress high availability by Bitnami on AWS Quick Start

Quick Starts are built by AWS solutions architects and partners to help you deploy popular technologies on AWS, based on AWS best practices for security and high availability. 

These accelerators reduce hundreds of manual procedures into just a few steps, so you can build your production environment quickly and start using it immediately. 

Each Quick Start includes AWS CloudFormation templates that automate the deployment and a guide that discusses the architecture and provides step-by-step deployment instructions.


![1](https://user-images.githubusercontent.com/23625821/138751233-0f4b7e09-7bbf-47b1-ba1d-2b48408623a4.png)


### Conclusion 

AWS presents many architecture options for running WordPress. The simplest option is a single server installation for low traffic websites. For more advanced websites, site administrators can add several other options, each one representing an incremental improvement in terms of availability and scalability. Administrators can select the features that most closely match their requirements and their budget.



### Reference

<a href="https://docs.aws.amazon.com/whitepapers/latest/best-practices-wordpress/best-practices-wordpress.pdf#welcome"> Original paper </a> 
