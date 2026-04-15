pipeline {
    agent any

    environment {
        SERVER_IP = "54.87.61.109"
    }

    stages {

        stage('Install Java') {
            steps {
                sshagent(['ec2-ssh']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ec2-user@$SERVER_IP '
                        if ! java -version 2>/dev/null; then
                          sudo dnf install java-17-amazon-corretto -y
                        fi
                    '
                    """
                }
            }
        }

        stage('Install Tomcat') {
            steps {
                sshagent(['ec2-ssh']) {
                    sh """
                    ssh ec2-user@$SERVER_IP '
                        if [ ! -d /opt/tomcat/bin ]; then
                          cd /opt
                          sudo wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.70/bin/apache-tomcat-9.0.70.tar.gz
                          sudo tar -xzf apache-tomcat-9.0.70.tar.gz
                          sudo mv apache-tomcat-9.0.70 tomcat
                        fi
                    '
                    """
                }
            }
        }

        stage('Change Tomcat Port to 8081') {
            steps {
                sshagent(['ec2-ssh']) {
                    sh """
                    ssh ec2-user@$SERVER_IP '
                        sudo sed -i "s/port=\\"8080\\"/port=\\"8081\\"/g" /opt/tomcat/conf/server.xml
                    '
                    """
                }
            }
        }

        stage('Download Sample App') {
            steps {
                sshagent(['ec2-ssh']) {
                    sh """
                    ssh ec2-user@$SERVER_IP '
                        sudo cd /opt/tomcat/webapps
                        sudo wget https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/sample.war
                    '
                    """
                }
            }
        }

        stage('Start Tomcat') {
            steps {
                sshagent(['ec2-ssh']) {
                    sh """
                    ssh ec2-user@$SERVER_IP '
                        sudo /opt/tomcat/bin/startup.sh
                    '
                    """
                }
            }
        }
    }
}
