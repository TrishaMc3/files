# Databricks platform FAQs
## What is the basic configuration of the Databricks platform? 

The basic configuration of the Databricks platform is where the data resides, what data we have access to, and how it all works. Databricks is divided into three components. The platform separates each component on its own plane, so we distance ourselves as far as possible from your data.

1. **Backend services** in the Databricks account include log storage and analysis. They’re not just customer logs but also infrastructure logs that don’t contain any customer information. Backend services include Genie, which we built as a token-based access control system. This plane also includes the monitoring component (e.g., all the monitoring of our infrastructure that’s needed for a SaaS platform). In a standard SaaS service, these backend services are always controlled by the SaaS provider. 
2. **Frontend services** in the Databricks account are called the *control plane*. They’re the web app that you work from, and they’re the heart of our SaaS platform. The control plane does all the builds, monitoring, and configurations for the customer account. It also has notebooks where customers do their coding and drive the commands to the *data plane* in the customer account layer. 

    The notebooks and query results in the control plane are customer data. For our platform to operate, we have a UI that customers use to build their jobs. Notebooks are where all the work is done, as well as the controls that run all the jobs in the data plane. Customer data on this plane also includes a hive metastore for table names and metadata. 

3. The **customer account** includes the data plane (Spark clusters and local storage) and customer data sources. You create your own VPC, and you provide us with only the specific IM roles and security groups that we need to deploy the platform. All data processing is done in this layer. 

    The data plane connects to your data sources (e.g., your S3 buckets and connections). We don’t have direct access to your data sources, and only you can provide the access to the data plane. 

## Why does Databricks require permissions and cross-account roles? 

We need a full set of roles to do the initial deployment. After the deployment is done, you can restrict that down to just the roles we need to provide the service. For maintenance, we update your platform and monitor its health. And we spin up instances because we provide an auto-scaling approach and methodology to make our product more performant and efficient.

We don’t have access to your S3s or anything else through these roles. We can confirm that, and you can validate it when you go into your VPC after we set everything up.

You give us a role to be able to build your VPC for you. We push in the AMIs that get built into your data plane (e.g., Spark). And we connect to an S3 bucket with the artifacts (AMIs) to build your platform. We need roles to spin up new clusters and to run the platform for you, but we don’t have direct access to the data plane, and we don’t take ownership. 

## Can you limit any of the required permissions? 

See the Databricks Enterprise Guide for the requirements of specific configurations, the groups we need, and why we need the access. There are about 50 different configurations in the guide. 

## How can we achieve the results we need while protecting our information? 

We protect your information while providing our platform:

* The RDS instance can be encrypted with your own KMS key. If you destroy your key, we won’t have access to that RDS instance. That gives you control of your instance. 

* Databricks has multiple deployments, including multi-tenant offerings. But in the case of a single-tenant offering, we have one VPC per customer, so there’s no risk of data bleed between customers. And you can encrypt your RDS instance with your key. 

* We see some logs in your VPC, but those logs can be viewed and sent to you directly. You can put a SIM or something similar on that if you want additional watches of what happens to the platform. 

* We use VPC peering for the VPC connection. But we’re going to be changing that to a tunneling approach in Q3. Right now, we provide protection by monitoring. We have a least-privilege configuration.  

* We don’t have direct access to your data sources, and only you can provide the access to the data plane.

* We can't access your S3s or anything else through our roles. We can confirm that, and you can validate it when you go into your VPC after we set it all up.

* We use the Genie support system to access the customer environment only upon request.

## Will Databricks create the notebooks in the client account layer? 

No. You create the notebooks. We only give you the service to create the notebooks. We deploy the platform, but we don’t need to go into that layer. However, we can go into the control plane with a support ticket from you. 

## What is a Genie, and how is a support ticket created?

Genie is our on-demand access system. There are no engineers in Databricks who have direct access to production. For Databricks engineers to get access to a customer environment, the customer needs to submit a ticket into our support platform. With that ticket, you give Databricks permission to access your platform. 

The engineer goes into Genie with the proper information and what needs to be done, gets a ticket number, and seeks approval. After the engineer’s access to the platform is validated, that access gets a time-based token, from 30 minutes to 3 hours. Then engineers can access the control plane and help with notebooks, restart clusters, and do some actions as the client for support reasons only. 

We log the support actions in the Genie platform, and they’re also recorded in your logs. Your application log will show Databricks, the user who accessed the platform, and all the actions that were taken. You get full visibility of all the actions the individual took on your platform. 

In summary, no individuals in Databricks have access to your platform. All access is restricted to time-based tokens.

## For security assurance, should we create a new, separate account for logging to monitor the use of our customer account?

Yes. We recommend that you create a separate account for logging. Don’t share any other account. Segregation is a good security practice. 

## We want to have logging visible in our own platform. We use Splunk. Does Databricks support it?

Yes. We support using Splunk as your source of truth for your logs. But we also use Databricks for our internal logging and monitoring. It does the same thing as Splunk. 

##  What happens if passwords need to be reset? Will that expand access control to others?

If you have an SSO solution, we can disable the password-setting component. That gives you full access control of your web apps. 

## For SDLC information, how do you make sure that your solution is secure? 

We have an engineer who focuses on SDLC. When we start building, he makes sure that he understands what’s happening with the platform. We’re working on automating scanning, we’re bringing our scanning in house, and we do regular scans as part of SDLC—static and dynamic. Our dynamic scanning is done with Qualys. 

Our full-time pen tester looks at each service that comes online. The tester validates with automated tools to make sure we have the same cadence and methodology. We also have a third-party pen tester, HackerOne. They constantly test our products, and we have a good relationship with that community. 

When we put out a new service, we let the community know, and they test it. We’re working on increasing our automation, but the platform is already set up to do static and dynamic scanning. We do regular dynamic scanning because we need it for our peripheral component interconnect (PCI), and we’re going to increase the frequency of static scanning. 
