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
                    sh 'yum install maven -y'
                    
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
                    sh 'yum install tomcat -y'
                    
                    // Install additional web tools
                    sh 'yum install tomcat-webapps tomcat-admin-webapps tomcat-docs-webapp tomcat-javadoc -y'
                    
                    // Update tomcat.conf with JAVA_OPTS settings
                    sh 'echo \'JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom -Djava.awt.headless=true -Xmx512m -XX:MaxPermSize=256m -XX:+UseConcMarkSweepGC"\' | sudo tee -a /usr/share/tomcat/conf/tomcat.conf'
                    
                    // Update tomcat-users.xml with admin user and password
                    sh 'echo \'<user username="admin" password="password" roles="manager-gui,admin-gui"/>\' | sudo tee -a /usr/share/tomcat/conf/tomcat-users.xml'
                    
                    // Restart Tomcat service
                    sh 'sudo systemctl restart tomcat'
                }
            }
        }

        stage('Deploy Web Application') {
            steps {
                script {
                    // Generate the WAR file using Maven
                    dir('springboot') {
                        sh 'mvn install'
                    }
                    
                    // List the files in the target directory to see the actual WAR file name
                    sh 'ls -l target/'
                    
                    // Copy the WAR file to Tomcat's webapps directory
                    sh 'sudo cp target/hello-world-0.0.1-SNAPSHOT.war /var/lib/tomcat/webapps'
                }
            }
        }
    }
}
