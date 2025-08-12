# f5-aws-cfe-declaration
cfe declaration for ha in AWS 

Step 1
onboard BIGIP in aws with below tags

cost:f5cost<b>
environment:f5env
group:f5group
owner:f5own
f5_cloud_failover_lable: "bigip-deployment"

## Step 2:
# for the external interface add below tags
f5_cloud_failover_lable: "bigip-deployment"
f5_cloud_failover_nic_map:"external"

## Step 3:
# For the Internal inteface add below tags
f5_cloud_failover_lable: "bigip-deployment"
f5_cloud_failover_nic_map:"internal"

## step 4 
create a s3 bucket without any space and special characeter and below tag

f5_cloud_failover_lable: "bigip-deployment"

## step 5
in the internal routing table add the below tag
f5_cloud_failover_lable: "bigip-deployment"

## step 6
add secondary IP (pvt) to the External interface 

## step 7 
associate a elastic ip to the secondary IP of BIGIP A ( which ever active)
and add below tag
f5_cloud_failover_lable: "bigip-deployment"
f5_cloud_failover_vips: 10.10.10.12,10.20.20.12   ; this secondary IP attached to the bigip external interface

## step 8
Note: FOR HA better use internal Selfip ,
You need to create LOCAL Only Route for each devices own routes
1. Create a parition as LOCAL_ONLY
   remove the inherit,and set traffic-group none

2. navigate to the LOCAL_ONLY partition
   create the routes in each biigp, eg: default route, Static route between bigip, server segments
   (as both bigip are in different AZ )
for same zone no need of this step, check f5 offical document

## step 9
Post the CFE declaration 
https://ip/mgmt/shared/cloud-failover/declare 

after positing declaration on both device send a reset POST
https://ip/mgmt/shared/cloud-failover/reset
{"resetStateFile":true}

for testin run the belwo get
https://ip/mgmt/shared/cloud-failover/inspect

dry run on standby device only
/mgmt/shared/cloud-failover/trigger
{"action":"dry-run"}

tail -f /var/log/restnoded/restnoded.log


Key note:
if the Elastic IP attached to the Standby :
POST The declration with some dummy Elstic IP, Make Active to standby 
and then POST actual declration with correct Elastic IP

Make sure
internet access to below url/ip via external selfip
curl http://169.254.169.254/latest/meta-data/
curl -s -I  https://ec2.amazonaws.com

also set DNS and NTP and access via external selip
Cloud Provider	DNS	NTP
AWS	169.254.169.253	169.254.169.123

final touch
In the BIG-IP user interface, navigate to System > Resource Provisioning. Set Management provisioning to Large.

Modify sys db variables using following commands in the CLI (bash):

tmsh modify sys db provision.extramb value 1000

tmsh modify sys db restjavad.useextramb value true
(check this command might changed provision.restjavad.extramb value 1000)
Restart restjavad daemons:

bigstart restart restjavad restnoded
