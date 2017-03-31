# Jenkins- jobs for hadoop and spark setup and TPCDS-run

### Pre-requisites:
1. Jenkins is installed on CI machine.Execute below commands for removing useSecurity tag from Jenkins config.xml to remove authentication 
    ex +g/useSecurity/d +g/authorizationStrategy/d -scwq /var/lib/jenkins/config.xml
	sudo -S /etc/init.d/jenkins restart

2. Set passowrdless ssh login for Jenkins user on CI machine to master machine

	ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa 
	cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
	chmod 0600 ~/.ssh/authorized_keys

	ssh-copy-id -i ~/.ssh/id_rsa.pub unix_user@master_ip
	ssh unix_user@master_ip

3. Changes in sudo file to make sudo access passwordless for linux user on master machine 
	Comment out below lines in sudo visudo file
	#Defaults    requiretty
	#Defaults   !visiblepw

	Also add below line in file
	unix_user        ALL=(ALL)       NOPASSWD: ALL
	e.g. testuser        ALL=(ALL)       NOPASSWD: ALL

### How to use:
  ```bash
1. To setup jekins job to use follow below steps one time on CI machine,

  git clone https://github.com/nkalband/Jenkins-hadoop-spark-setup-jobs.git
  
2. cd Jenkins-hadoop-spark-setup-jobs
  
3. Execute below command on linux prompt to import Jenkins jobs

  For hadoop setup -
  java -jar jenkins-cli.jar -noKeyAuth -s  http://localhost:8080/ create-job Setup_hadoop_spark_cluster < ./hadoop-spark-setup/Setup_hadoop_spark_cluster_config.xml
  
  For TPCDS jobs -
  java -jar jenkins-cli.jar -noKeyAuth -s  http://localhost:8080/ create-job Run_setup_tpcds < ./TPCDS-jobs/Run_setup_tpcds_config.xml
  java -jar jenkins-cli.jar -noKeyAuth -s  http://localhost:8080/ create-job Run_benchmark_tpcds < ./TPCDS-jobs/Run_benchmark_tpcds_config.xml

4. Access Jenkins using url http://CI-machine-ip:8080 in web browser and then run job `Setup_hadoop_spark_cluster` to setup haddop and spark cluster

5. To setup TPCDS execute jenkins job `Run_setup_tpcds`. While running at start, you can change the default parameters as per memory and core configurations of machines where you want to run the benchmark. At the end of `Run_setup_tpcds` will trigger downstream job for running the benchmark `Run_benchmark_tpcds`
  
  ```


