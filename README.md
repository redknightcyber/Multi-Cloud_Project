# Multi-Cloud_Projects
This is a Repo to store Files, Code, and Lab documents for Multi-Cloud based projects.

Steps to implement Hands-on Project - Mission 1
Amazon Web Services
•	Access AWS console and go to IAM service
•	Under Access management, Click in "Users", then "Add user" and create a programmatic user "terraform-en-1"
•	On Set permissions, click on "Attach existing policies directly" button.
•	Select AmazonS3FullAccess.
•	Click on Next: Tags
•	Click on Next: Review
•	Click on Create user
•	Click on Download .csv
•	After download, rename .csv to accessKeys.csv
Google Cloud Platform (GCP)
•	**CLICK HERE to download the hands-on files**.
•	Access GCP Console and open Cloud Shell
•	Upload accessKeys.csv and .zip hands-on file to GCP Cloud Shell
•	Hands-on files preparation
mkdir mission1_en
mv mission1.zip mission1_en
cd mission1_en
unzip mission1.zip
mv ~/accessKeys.csv mission1/en
cd mission1/en
chmod +x *.sh
•	Run the following commands to prepare AWS and GCP environment. Authorize when asked.
./aws_set_credentials.sh accessKeys.csv
gcloud config set project <project_id>
•	Execute the command below
./gcp_set_project.sh
•	Enable the Container Registry API, Kubernetes Engine API and the Cloud SQL API
gcloud services enable containerregistry.googleapis.com 
gcloud services enable container.googleapis.com 
gcloud services enable sqladmin.googleapis.com 
IMPORTANT (DO NOT SKIP):
•	Before executing the Terraform commands, open the Google Editor and update the file tcb_aws_storage.tf replacing the bucket name with an unique name (AWS requires unique bucket names).
o	Open the tcb_aws_storage.tf using Google Editor
o	On line 4 of the file tcb_aws_storage.tf: 
	Replace xxxx with your name initials plus two random numbers: Example: luxxy-covid-testing-system-pdf-en-jr29
•	Run the following commands to finish provision infrastructure steps
cd ~/mission1_en/mission1/en/terraform/

terraform init
terraform plan
terraform apply
•	Once the CloudSQL instance is provisioned, access the Cloud SQL service
•	Click on your Cloud SQL instance.
•	On the left side, under Primary Instance, click on Connections.
•	Under Instance IP assignment, enable Private IP. 
o	Click Set up Connection and Use an automatically allocated IP range in your network.
o	Click Continue
o	Click Create Connection and wait a minutes.
•	Under Associated networking, select "Default"
•	Under Authorized Networks, click "Add Network".
•	Under New Network, enter the following information: 
o	Name: Public Access (For testing purposes only)
o	Network:** 0.0.0.0/0
o	Click Done.
PS: For production environments, it is recommended to use only the Private Network for database access. ⚠️ Never grant public network access (0.0.0.0/0) to production databases.
•	After that, click on Save and wait to conclude the update.


Excited to try my hand at building Infrastructure in the cloud using Terraform...
I am gaining tremendous insight and knowledge of cloud automation with this 
Multi-Cloud integration with AWS and Google Cloud thanks to the 
@thecloudbootcamp  #clouds #cloudcomputing #aws #googlecloud #azure #terraform #kubernets


Steps to implement Hands-on Project - Mission 2
Amazon Web Services
•	Access AWS console and go to IAM service
•	Under Access management, Click in "Users", then "Add user" and create a programmatic user "luxxy-covid-testing-system-en-app1"
•	On Set permissions, click on "Attach existing policies directly" button.
•	Select AmazonS3FullAccess.
•	Click on Next: Tags
•	Click on Next: Review
•	Click on Create user
•	Click on Download .csv
•	After download, rename .csv to luxxy-covid-testing-system-en-app1.csv
Google Cloud Platform (GCP)
•	Navigate to Cloud SQL instance and create a new user app with password welcome123456 on Cloud SQL MySQL database
•	Connect to Google Cloud Shell
•	Download the mission2 files
cd
mkdir mission2_en
cd mission2_en
wget <https://objectstorage.us-ashburn-1.oraclecloud.com/p/t1RPAY7hbbdaSj9fhLJhYetCsFe5CEKufO56vb1l2_6FVz35PJzIukvTJKhRYKGs/n/idqfa2z2mift/b/eventos-thecloudbootcamp/o/ICP2/mission2.zip>
unzip mission2.zip
•	Connect to MySQL DB running on Cloud SQL (once it prompts for the password, provide welcome123456)
mysql --host=<public_ip_cloudsql> --port=3306 -u app -p
•	Once you're connected to the database instance, create the products table for testing purposes
use dbcovidtesting;
source ~/mission2_en/mission2/en/db/create_table.sql;
show tables;
exit;
•	Build the Docker image and push it to Google Container Registry. Please replace the <PROJECT_ID> with your My First Project ID
cd ~/mission2_en/mission2/en/app
gcloud builds submit --tag gcr.io/<PROJECT_ID>/luxxy-covid-testing-system-app-en
•	Open the Cloud Editor and edit the Kubernetes deployment file (luxxy-covid-testing-system.yaml) and update the variables below in red with your <PROJECT_ID> on the Google Container Registry path, AWS Bucket name, AWS Keys (from luxxy-covid-testing-system-en-app1.csv) and Cloud SQL Database Private IP.
cd ~/mission2/en/kubernetes
luxxy-covid-testing-system.yaml

				image: gcr.io/**<PROJECT_ID>/**luxxy-covid-testing-system-app-en:latest
...
				- name: AWS_BUCKET
          value: "**luxxy-covid-testing-system-pdf-en-xxxx**"
        - name: S3_ACCESS_KEY
          value: "**xxxxxxxxxxxxxxxxxxxxx**"
        - name: S3_SECRET_ACCESS_KEY
          value: "**xxxxxxxxxxxxxxxxxxxx**"
        - name: DB_HOST_NAME
          value: "**172.21.0.3**"
•	Connect to the GKE (Google Kubernetes Engine) cluster via Console (follow the video)
•	Deploy the application Pharma Store in the Cluster
cd ~/mission2_en/mission2/en/kubernetes
kubectl apply -f luxxy-covid-testing-system.yaml
•	Get the Public IP and test the application (CLICK HERE to download COVID-19 Testing result sample)
•	You should see the app up & running! Congrats! 🎉

Steps to implement Hands-on Project - Mission 3
Google Cloud Platform - Database Migration steps
•	Connect to Google Cloud Shell
•	Download the dump
cd
mkdir mission3_en
cd mission3_en
wget <https://objectstorage.us-ashburn-1.oraclecloud.com/p/tvk3j_I8Uj0tTvPE2A2JDWCF_TZpmw0vDIiHkD8HKzGYxZsZtWXBfaURIZ4djW1k/n/idqfa2z2mift/b/eventos-thecloudbootcamp/o/ICP2/mission3.zip>
unzip mission3.zip
•	Connect to Cloud SQL MySQL database instance
mysql --host=<public_ip_address> --port=3306 -u app -p
•	Import the dump on Cloud SQL
use dbcovidtesting;
source ~/mission3_en/mission3/en/db/db_dump.sql
•	Check if the data got imported correctly
SELECT * FROM records;
Amazon Web Services - PDF Files Migration steps
•	Connect to the AWS Cloud Shell
•	Download the PDF files
cd
mkdir mission3_en
cd mission3_en
wget <https://objectstorage.us-ashburn-1.oraclecloud.com/p/tvk3j_I8Uj0tTvPE2A2JDWCF_TZpmw0vDIiHkD8HKzGYxZsZtWXBfaURIZ4djW1k/n/idqfa2z2mift/b/eventos-thecloudbootcamp/o/ICP2/mission3.zip>
unzip mission3.zip
•	Sync PDF Files with your AWS S3 used for COVID-19 Testing Status System. Replace the bucket name with yours.
cd mission3/en/pdf_files
aws s3 sync . s3://luxxy-covid-testing-system-pdf-en-**xxxx**
•	Test the application. Upon migrating the data and files, you should be able to see the entries under “View Guest Results” page.
 
Congratulations! You have migrated an "on-premises" application & database to a MultiCloud Architecture!
How to delete the Multiple Cloud Providers resources
First step - Empty your bucket
•	Access the AWS Console and open S3 Service.
•	Select your bucket and click on Empty.
•	To confirm deletion, type permanently delete in the text input field.
•	Click on Empty.
First step concluded! 🎉
Second step - Set the instance deletion_protection from true to false
•	Open the Google Editor, and access the following path: mission1_en/mission1/en/terraform/. Now, open the tcb_gcp_database.tf file
•	On line 11 of the tcb_gcp_database.tf file:
o	Replace from deletion_protection = “true” to deletion_protection = “false” Example: 
	First version: deletion_protection = "true”
	Updated version: deletion_protection = "false”
•	Change from Editor to Terminal
•	Run the commands to apply the terraform file changes:
cd ~/mission1_en/mission1/en/terraform/

terraform apply
	***Enter a value: yes***
Now, you’re ready to delete all the resources with the terraform destroy. 🚀
•	Run the following command:
terraform destroy
	***Enter a value: yes***
 
Congratulations! All resources were deleted from multiple Cloud Providers with just a command! 🚀✅
