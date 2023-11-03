pipeline {
    agent any
stage('Debug') {
    steps {
        sh 'whoami'
        sh 'env'
    }
}
    stages {
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
                    sh 'echo \'JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom -Djava.awt.headless=true -Xmx512m -XX:MaxPermSize=256m -XX:+UseConcMarkSweepGC"\' | tee -a /usr/share/tomcat/conf/tomcat.conf'
                    
                    // Update tomcat-users.xml with admin user and password
                    sh 'echo \'<user username="admin" password="password" roles="manager-gui,admin-gui"/>\' | tee -a /usr/share/tomcat/conf/tomcat-users.xml'
                    
                    // Restart Tomcat service
                    sh 'systemctl restart tomcat'
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
                    
                    // Copy the WAR file to Tomcat's webapps directory
                    sh 'cp target/your-app.war /var/lib/tomcat/webapps'
                }
            }
        }
    }
}
