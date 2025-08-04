# f5-aws-cfe-declaration
cfe declaration for ha in AWS 

Step 1
onboard BIGIP in aws with below tags

cost:f5cost
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
f5_cloud_failover_nic_map:"external"

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

