# helm-hellospringbooteks

# Pre-Requisites
    - Install Git
    - Install Maven
    - Install Docker
    - Install EKS-Cluster
    - Install Helm
# Install Java:
    yum install java-1.8.0-openjdk-devel -y
# Install Git:
    yum install git -y
# Install Apache-Maven:
    wget https://mirrors.estointernet.in/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
    tar xvzf apache-maven-3.6.3-bin.tar.gz

    vi /etc/profile.d/maven.sh
    --------------------------------------------
    export MAVEN_HOME=/opt/apache-maven-3.6.3
    export PATH=$PATH:$MAVEN_HOME/bin
    --------------------------------------------

    source /etc/profile.d/maven.sh
    mvn -version
# Install Docker:
    yum install docker -y
    service docker start
# Clone code from github:
    git clone https://github.com/cloudtechmasters/helm-chart-metricserver-HPA.git
    cd helm-chart-metricserver-HPA/hellospringbooteks
# Build Maven Artifact:
    mvn clean install
# Build Docker image for Springboot Application
    docker build -t cloudtechmasters/hellospringbooteks:v1 .
# Docker login
    docker login
# Push docker image to dockerhub
    docker push cloudtechmasters/hellospringbooteks:v1
  Edit application code at below place and shown in below image
    
    vi src/main/java/com/spjenk/hello/HelloController.java
  ![image](https://user-images.githubusercontent.com/68885738/91663401-3c2af200-eb06-11ea-9ccf-58a02197d28e.png)
# Again build artifact and build docker image
    mvn clean install
    docker build -t cloudtechmasters/hellospringbooteks:v2 .
    docker push cloudtechmasters/hellospringbooteks:v2
# Create helm chart for springboot application
    helm create hellospringbooteks
  Edit values.yaml file and Deployment.yaml files as shown in below images
   
    cd hellospringbooteks   
    vi hellospringbooteks/values.yaml
  ![image](https://user-images.githubusercontent.com/68885738/91663508-db4fe980-eb06-11ea-9d58-37aac1ef24c2.png)
# Deploy springboot application using helm
    helm install hellospringbooteks hellospringbooteks
# Goto web UI and check output
    <loadbalancer>:8080/hello
    http://a03bb109d210749f680a06813c8793d2-1493801453.us-east-1.elb.amazonaws.com:8080/hello
  ![image](https://user-images.githubusercontent.com/68885738/91663839-eefc4f80-eb08-11ea-99a1-f9ae975a2a2b.png)
  
  Again edit values.yaml file and Deployment.yaml files as shown in below images
      
    vi hellospringbooteks/values.yaml
  ![image](https://user-images.githubusercontent.com/68885738/91663904-439fca80-eb09-11ea-8fac-c3ffdab920fd.png)
# Upgrade springboot application using helm
    helm upgrade hellospringbooteks hellospringbooteks
# Check revision history using below command
    helm history hellospringbooteks
  ![image](https://user-images.githubusercontent.com/68885738/91664021-fec86380-eb09-11ea-8dbf-4c5472487e86.png)
# Goto web UI and check output
    <loadbalancer>:8080/hello
    http://a03bb109d210749f680a06813c8793d2-1493801453.us-east-1.elb.amazonaws.com:8080/hello
  ![image](https://user-images.githubusercontent.com/68885738/91663996-d476a600-eb09-11ea-9a98-ba6f1b42cbe3.png)
# Rollback springboot application using helm
    helm rollback hellospringbooteks 1
# Check revision history using below command
    helm history hellospringbooteks
# Goto web UI and check output
    <loadbalancer>:8080/hello
    http://a03bb109d210749f680a06813c8793d2-1493801453.us-east-1.elb.amazonaws.com:8080/hello
  ![image](https://user-images.githubusercontent.com/68885738/91664062-46e78600-eb0a-11ea-80f9-b153413365f3.png)
# Delete springboot application using helm
    helm delete hellospringbooteks
