def server = Artifactory.server 'arti'
server.bypassProxy = true
// If you're using username and password:
server.credentialsId = 'jenkins'

pipeline {
    agent any
    stages {
        stage("Preparation") {
            steps {
                git 'https://github.com/ramonsterj/some.git'
            }
        }
        stage('Build') {
            steps {
                echo('Building...')
                sh 'pwd'
                sh 'ls'
                sh 'chmod +x gradlew'
                sh './gradlew assemble -Dorg.gradle.daemon.debug=true'
            }
        }
        stage('Publish to Artifactory') {
            steps {
                rtUpload (
                        serverId: "arti",
                        spec:
                                """{
                            "files": [
                                {
                                "pattern": "build/libs/*.jar",
                                "target": "gradle-dev-local"
                                }
                            ]
                        }"""
                )
                rtPublishBuildInfo (
                        serverId: "arti",
                )
            }
        }
    }
}