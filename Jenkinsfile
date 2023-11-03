pipeline {
    agent any
    
    stages {
        stage('Checkout Code') {
            steps {
                sh 'git clone https://github.com/adsh641/springboot.git'
            }
        }
        
        stage('Maven') {
            steps {
                sh 'sudo yum install maven -y'
                
                dir('springboot') {
                    sh 'mvn test'
                    sh 'mvn install'
                }
            }
        }
        
        stage('Tomcat Installation') {
            steps {
                sh 'sudo yum install tomcat -y'
                sh 'sudo yum install tomcat-webapps tomcat-admin-webapps tomcat-docs-webapp tomcat-javadoc -y'
                
                // Update Tomcat server.xml to change the port to 8090
                sh 'sudo sed -i "s/Connector port=\"8080\"/Connector port=\"8090\"/" /etc/tomcat/server.xml'
                
                // Update tomcat.conf with JAVA_OPTS settings
                sh 'echo \'JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom -Djava.awt.headless=true -Xmx512m -XX:MaxPermSize=256m -XX:+UseConcMarkSweepGC"\' | tee -a /usr/share/tomcat/conf/tomcat.conf'
                
                // Update tomcat-users.xml with admin user and password
                sh 'echo \'<user username="admin" password="password" roles="manager-gui,admin-gui"/>\' | tee -a /usr/share/tomcat/conf/tomcat-users.xml'
                
                // Restart Tomcat service
                sh 'sudo systemctl restart tomcat'
            }
        }
        
        stage('Deploy Web Application') {
            steps {
                dir('springboot') {
                    sh 'mvn install'
                }
                
                sh 'cp target/your-app.war /var/lib/tomcat/webapps'
            }
        }
    }
}
