# Deatiled Architecture Breakdown - AWS 3-tier Architecture Enterprise
## 1 VPC & NETWORKING
-**CIDR BLOCK**: 10.0.0.0/16
-**SUBNETS**:
 -WEB TIER : 2 Public Subnet accross AZ-a and AZ-b
 -APP TIER : 2 Private Subnet accross AZ-a and AZ-b
 -DB TIER : 2 Private Subnet accross AZ-a and AZ-b (Used in RDS Subnet group)
-**ROUTING**:
 -Internet Gateway for Public Subnets
 -NAT Gateway for Private Subnets (App Tier)
 -Route Tables with proper associations.

## 2 WEB TIER
-**INSTANCES** : EC2 with NGINX, Apache and PHP
-**Security Groups** : 
 -Allow HTTP/HTTPS from ALB
-**ALB** :
 -TYPE : Application Load Balancer (Public Facing)
 -Health Checks enabled on '/'
-**Auto Scaling Group**
 -Target Tracking Scaling based on CPU Utilization

## 3 App Tier
-**Instances** : EC2 (business logic/ PHP-FPM)
-**Internal ALB** to map the traffic accordingly 
-**Security Group** :
 -Only allow traffic from web-tier SG
 -No internet access (private)

## 4 DB Tier
-**Amazon RDS** : MySQL, Multi-AZ enabled
-**Subnet Groups** : Configure with DB private subnets
-**Security Group** :
 -Allow Access Through App Tier SG
-**Backup and Monitoring** :
 -Enabled automated backup
 -CloudWatch Logs and enhanced monitoring

## 5 S3, Cloudfront, Route 53
-**S3** :
 -Used for Static asset storage
-**CloudFront** :
 -Linked to ALB DNS
 -DDoS protection, caching
-**Route 53**:
 -Hosted Zone configured 
 -ALB as recorded target

## 6 Monitoring and Logging
-**CloudWatch** :
 -Custom metrics + alarms or EC2, RDS
 -Log collection from EC2 via agent
-**CloudTrail** :
 -Enabled for auditing API calls

## 7 IAM & Security
-**SSM** :
 -have created a SSM role for more security
 -Secure access without SSL

## 8 Scaling & High Availability
-ASG for web tier and app tier (across AZs)
-RDS Mutli-AZ
-ALB across Multiple Subnets
