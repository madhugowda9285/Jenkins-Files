pipeline {
    agent any
tools { maven 'maven3'}
    stages {
        stage('git check-out ') {
            steps {
                git branch: 'main', url: 'https://github.com/jaiswaladi246/Ekart.git'
                stash 'chiranth'
            }
        }
        stage('mv compile') {
            agent {
                label 'slave'
                    }
            steps {
                unstash 'chiranth'
                sh 'mvn compile'
                }
            }
            stage('Dependency Check') {
            agent {
                label 'slave'
                    }
            steps {
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DC'
                dependencyCheckPublisher pattern: '**/madhu_chiranth.xml'
                }
            }
            stage('build') {
            agent {
                label 'slave'
                    }
            steps {
                sh "mvn package -DskipTests=true"
                }
            }
            stage('deploy') {
            agent {
                label 'slave'
                    }
            steps {
                sshagent(['jenkins']) {
                sh "scp -o StrictHostKeyChecking=no /home/jenkins/workspace/Pipe-line/target/*.jar jenkins@172.31.10.4:/home/jenkins/apache/webapps"
                #  or
                  #if u are using deploy to container then make use of below one
                 # deploy adapters: [tomcat9(credentialsId: 'admin', path: '', url: 'http://13.232.99.190:8080')], contextPath: null, war: '**/*.jar'
                
                                    }
                }
            }
        }
    }
