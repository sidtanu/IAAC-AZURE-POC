node {

    stage ('checkout') {

        echo "test test    mm"
	checkout scm
    }
    stage ('setupenv') {
    	echo " - - - - Setting up environment now - - - - "
	////bin////sh setupenv.sh
    }

    stage ('TerraformActivity') {
    	echo " - - - - Ready to execute Terraform now - - - - "
	sh '''
	cd TF/VNET
	 export VAULT_ADDR='http://127.0.0.1:8200'
	 export VAULT_TOKEN="6010297d-a651-e303-5e46-07fc2153f484"
	 export ARM_SUBSCRIPTION_ID="`/opt/apps/vault kv get -field="subs_id" secret/wrtazr`"
	 export ARM_CLIENT_ID="`/opt/apps/vault kv get -field="client_id" secret/wrtazr`"
	 export ARM_CLIENT_SECRET="`/opt/apps/vault kv get -field="client_secret" secret/wrtazr`"
	 export ARM_TENANT_ID="`/opt/apps/vault kv get -field="tenant_id" secret/wrtazr`"
	 
	 /opt/apps/vault --version
	 /opt/apps/vault kv get -field="foo" secret/hello
		
	 echo `/opt/apps/terraform --version`
	 /opt/apps/terraform init
	 echo "yes" | /opt/apps/terraform plan
	 echo "yes" | /opt/apps/terraform apply -var "adminPassword=Thankyou@1"
	 
	 echo " - - - - - - C O M P L E T E D - - - - - - - "


	'''
	}
}
