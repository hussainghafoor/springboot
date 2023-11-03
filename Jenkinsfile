pipeline {
    agent any
    stages {
        stage('Debug') {
            steps {
                sh 'whoami'
                sh 'env'
            }
        }

        stage('Checkout Code') {
            steps {
                script {
                    // Download the code to your Jenkins server
                    sh 'git clone https://github.com/adsh641/springboot.git'
                }
            }
        }

        stage('Maven') {
            steps {
                script {
                    // Install Maven on the application server
                    sh 'sudo yum install maven -y' // Add sudo here

                    // Navigate to the springboot directory
                    dir('springboot') {
                        // Build the project and create a WAR file
                        sh 'mvn test'
                        sh 'mvn install'
                    }
                }
            }
        }

        stage('Tomcat Installation') {
            steps {
                script {
                    // Install Tomcat on the server
                    sh 'sudo yum install tomcat -y' // Add sudo here

                    // Install additional web tools
                    sh 'sudo yum install tomcat-webapps tomcat-admin-webapps tomcat-docs-webapp tomcat-javadoc -y' // Add sudo here

                    // Update tomcat.conf with JAVA_OPTS settings
                    sh 'echo \'JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom -Djava.awt.headless=true -Xmx512m -XX:MaxPermSize=256m -XX:+UseConcMarkSweepGC"\' | sudo tee -a /usr/share/tomcat/conf/tomcat.conf' // Add sudo here

                    // Update tomcat-users.xml with admin user and password
                    sh 'echo \'<user username="admin" password="password" roles="manager-gui,admin-gui"/>\' | sudo tee -a
