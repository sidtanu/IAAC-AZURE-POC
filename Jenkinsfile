node {

    stage ('checkout') {

        echo "- - - - Checkout Code - - - -"
	checkout scm
    }

    stage ('setupenv') {
    	echo " - - - - Setting up environment now - - - - "
	}

    stage ('TerraformActivity') {
    
    echo " - - - - Ready to execute Terraform now - - - - "
    sh '''
   echo "started" 
   export VAULT_ADDR='http://127.0.0.1:8200'
   export VAULT_TOKEN="099ee9ce-68b5-53ed-21d5-c1d7b27c58f3"
   export ARM_SUBSCRIPTION_ID="`/opt/apps/vault kv get -field="subs_id" secret/wrtazr`"
   export ARM_CLIENT_ID="`/opt/apps/vault kv get -field="client_id" secret/wrtazr`"
   export ARM_CLIENT_SECRET="`/opt/apps/vault kv get -field="client_secret" secret/wrtazr`"
   export ARM_TENANT_ID="`/opt/apps/vault kv get -field="tenant_id" secret/wrtazr`"
   cd TF/VM
   echo `/opt/apps/terraform --version`
   /opt/apps/terraform init
    {
    while read -r line
    do
    	platform=$(echo "$line" |awk -F "," '{ print $1 }')
	version=$(echo "$line" |awk -F "," '{ print $2 }')
	package=$(echo "$line" |awk -F "," '{ print $3 }')
	allowed_port=$(echo "$line" |awk -F "," '{ print $4 }')
	db_With_TestData=$(echo "$line" |awk -F "," '{ print $6 }')
	s3_Data_Link=$(echo "$line" |awk -F "," '{ print $7 }')
	project=$(echo "$line" |awk -F "," '{ print $8 }')
	environment=$(echo "$line" |awk -F "," '{ print $9 }')
	Deployment=$(echo "$line" |awk -F "," '{ print $11 }')
	appName=$(echo "$line" |awk -F "," '{ print $12 }')
	infra=$(echo "$line" |awk -F "," '{ print $5 }')
	
	if [ "$infra" = "Y" ];then
		echo "yes" | /opt/apps/terraform plan
		echo "yes" | /opt/apps/terraform apply -var "adminPassword=Thankyou@1" -var "virtualMachineName=$platform"
	fi
	removeInfra=$(echo "$line" |awk -F "," '{ if ($5=="N") print $10; }')
	echo "$removeInfra"
	if [ "$removeInfra" = "Y" ]; then
		echo "yes" | /opt/apps/terraform destroy
	fi
    done
    } < ./../../config.csv
    echo " - - - - - - C O M P L E T E D - - - - - - - "
    
    '''
    }
}
