# Movie-Review-Feedback-form

Understanding on AWS S3, IAM and EC2:

**Amazon S3 (Simple Storage Service):**
- S3 is a scalable and durable object storage service provided by AWS.
- It allows users to store and retrieve files, such as documents, images, videos, and backups, in the cloud.
- S3 provides high durability, low latency, and unlimited storage capacity.
- It is highly scalable, allowing users to store and retrieve any amount of data.
- S3 provides features such as versioning, access control, and encryption to ensure data security.
- S3 can be used to store data for various use cases, such as data backup and restore, big data analytics, mobile and gaming applications, and content distribution.


**Amazon EC2 (Elastic Compute Cloud):**
- EC2 is a virtual server provided by AWS that allows users to run applications in the cloud.
- It provides resizable compute capacity in the form of virtual machines (VMs) known as instances.
- Users can choose from various pre-configured instance types or create custom instances based on their computing requirements.
- EC2 instances can run a wide range of operating systems, and users have full control over the configuration, security, and networking aspects of the instances.
- EC2 allows users to scale up or down their compute capacity based on demand, making it suitable for a variety of workloads, such as web applications, databases, and data processing.


**AWS IAM (Identity and Access Management):**
- IAM is a service that allows users to manage access to AWS resources securely.
- It provides features for creating and managing AWS users, groups, and roles, and defining their permissions.
- IAM allows users to control access to AWS services and resources based on the principle of least privilege, which means granting only the necessary permissions for users to perform their tasks.
- IAM provides features such as multi-factor authentication (MFA), password policies, and integration with AWS CloudTrail for monitoring and auditing.
- IAM enables users to create and manage fine-grained access control policies to govern the actions that can be performed on AWS resources by different users or groups.

In summary, Amazon S3 is a scalable object storage service, EC2 is a virtual server for running applications, and IAM is a service for managing access to AWS resources securely. Together, they form the foundation of AWS cloud computing, providing storage, compute, and access control capabilities for a wide range of use cases.

- AWS S3 Form Link - https://movie-review-asriram2.s3.us-east-2.amazonaws.com/feedbackform.html

- Bucket name: movie-review-asriram2

- AWS EC2 Instance created - ec2-3-134-77-203.us-east-2.compute.amazonaws.com  (attached .pem file)

- WAR file attached in this code

**feedbackform.html - Description**

This HTML code represents a customer feedback form for movie reviews. Let's go through the different sections and elements of the code:

1. !DOCTYPE html: This is the document type declaration, specifying that the document is an HTML5 document.

2. html: The root element of the HTML document.

3. head: Contains meta-information about the document, such as the title of the web page, external CSS and JavaScript files, and meta tags.

4. title: Specifies the title of the web page that appears in the browser's title bar or tab.

5. link: Defines an external CSS file to be used for styling the web page. In this code, it links to the Bootstrap CSS file.

6. script: Includes an external JavaScript file for adding functionality to the web page. In this code, it links to the Bootstrap JavaScript file and also includes a custom JavaScript code for handling errors.

7. style: Contains CSS rules for styling the web page, such as setting the background color, font color, and layout properties.

8. body: Represents the main content of the web page.

9. header: Contains the header section of the web page, which includes the title "CUSTOMER FEEDBACK FORM" and is styled with a background color, padding, and flexbox properties for alignment.

10. container: A Bootstrap class that adds a responsive container to hold the form elements.

11. form: Represents an HTML form that allows users to input data. It includes various form elements such as text inputs, radio buttons, and a textarea for user input.

12. input: Represents an input field where users can enter data. It includes different types such as text, tel (telephone number), email, and date.

13. textarea: Represents a multiline text input field where users can enter review comments.

14. label: Associates a label with a form element and provides a text description or prompt for that element.

15. h5: Represents a level 5 heading used for grouping related form elements.

16. input type="submit" and input type="reset": Represents submit and reset buttons respectively for the form.

Overall, this code creates a customer feedback form with required fields for name, telephone number, email, date of survey, and movie review comments, and uses Bootstrap for styling and layout. It also includes error handling using JavaScript to redirect to an error page in case of errors.
  
  
**Understanding AWS S3, EC2 and IAM concepts using feedbackform.html
**
**Creating a Docker image and pushing it to DockerHub**

1.	Make sure you have docker installed on your machine. I am using Docker Desktop for Mac. You will also need to create an account on https://hub.docker.com/ 
2.	In Eclipse, create a file called Dockerfile. Docker requires the file to be called ‘Dockerfile’ 
3.	Build the project on eclipse and put the war file in the same folder as the Dockerfile. Note that Feedbackform is the display name of my Tomcat application. 
4.	In the DockerFile, use the FROM command to get the initial image for the build. We want to run the war file in Tomcat so we should use the tomcat image: FROM tomcat:9.0-jdk15 
5.	Next, we need to drop the war file in the webapps folder: COPY SurveyForm.war /usr/local/tomcat/webapps/ 
6.	On the command line, use this command: ‘docker build --tag moviereview:0.1 .’ You can use whatever name and tag you want. 
7.	Verify that the image is properly working by running ‘docker run -it -p 8182:8080 moviereview:0.1’ and open a browser at http://localhost:8182/Feedbackform
8.	On the command line, login to docker using ‘docker login -u <your username>’ 
9.	Change the name of you image to be <your username on dockerhub>/<name of the app>:<image tag> using the docker tag command. In my case it is: ‘docker tag moviereview:0.1 abhisri/moviereview:0.1’ 
10.	Verify that your image is on Docker Hub. Your image is accessible from the internet 

**Installing Rancher on AWS **

1.	Log in to the AWS console https://aws.amazon.com/ and create an account. You will need to have a credit card. You can also use an AWS Educate but it has limited capabilities www.awseducate.com 
2.	I am running Rancher on an Ubuntu EC2 but all you need is an instance that has docker running. 
3.	In the AWS Console, click on Services then EC2 and click Instance and ‘Launch instance’. Using the Ubuntu AMI from the list 
4.	Under Instance Type choose t2.micro. I tried to run it on the free tiers but Rancher wouldn't start. Click Next until you get to the 'Security Group'. You need to add ports 443 and 80 to 0.0.0.0/0 
5.	Click 'Review and Launch' and then Launch. Make sure you save your pem key. 
6.	Once the instance is up and running, get the public IP address and public DNS. Ssh into it by running the following command from the terminal (or Putty for windows): ssh -i <YOUR KEY LOCATION> ubuntu@<YOUR PUBLIC IP>. You might have to change the permission on your key to be read-only. 
7.	Once you are in the instance, updated it by running: 'sudo apt-get update' 
8.	Install docker: 'sudo apt install docker.io' 
9.	Verify that docker is installed by running 'docker -v' 
10.	Run this docker command using 'sudo docker run --privileged=true -d --restart=unless- stopped -p 80:80 -p 443:443 rancher/rancher' 
11.	Wait a minute or so then open your browser and copy/paste your public DNS from step f. After some warnings, you should get this screen: 
12.	Create a password to the admin user. Installing Rancher installs Kubernetes.

**Starting a Rancher cluster **

1.	In the global Rancher page, click on ‘Add Cluster’ 
2.	You can choose any type of cloud provider you want; I chose Amazon EC2 
3.	Give your cluster a Name 
4.	Next you will need to create a template for your master/etc/worker nodes. I used the same template for all the nodes types. Click on ‘Add node template’ 
5.	You will need to create a user from your AWS account that can create and manage EC2 instances. For that you will need to go back to the AWS console and click on Service and IAM 
6.	Click on ‘Add User’, give it a name and click on the ‘Programmatic access’ to get the Access key and Secret key. 
7.	Click Next into permission and click on ‘Attach Existing Policy directly’. Add the following permission ‘AdministratorAccess’. 
8.	Click Next until you Reach ‘Create User’. Make sure you save the Access Key and Secret Key 
9.	Copy both keys to the screen on step d. and choose the appropriate region (us-east-1) 
10.	Click Create and don’t change your VPC or security group 
11.	Choose the type of instance you want to use. You will also have to choose an AMI from the AMI list that is provided by Rancher. 
12.	Under ‘IAM Instance Profile Name’, put the name of the IAM user that you created. 
13.	Give a name to the template and click on Create. 
14.	Your template will show under template. It is advisable to have one instance dedicated to runs etc and the ‘Control pane’ and have the workers running by itself. 
15.	Leave all the rest as default and click Create. Rancher will take some time to provision your cluster. 
16.	Once the cluster is ready, you will be able to see the health worker nodes under your cluster. You will also be able to see the new instances created on the AWS console.

**Deploying the docker image to rancher **

1.	In Rancher, click on the top left side and choose your cluster. 
2.	Click on Projects/Namespaces. Create a project and a namespace to make it easier to locate your running pods 
3.	Click on your cluster on the upper left and choose your project 
4.	Click on deploy in the upper right 
5.	Fill out the deploy form. As a docker images, use the image that you pushed to Docker Hub in step 2-j. Choose the appropriate namespace. We need to have three pods running so we choose a load balancer as a Service. 
6.	Click Launch and wait a couple of minute for AWS to setup the load balancer. You can check the status by clicking on the ‘Load Balancing’ tab. 
7.	Under your pods, a ‘8080/tcp’ link will appear. Click on the url and add the display name: /Feedbackform in my case. The application should load and should see the survey. 

