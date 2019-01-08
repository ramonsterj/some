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
        stage("Prepare artifactory") {
            steps {
                rtGradleResolver (
                        id: "rspca-resolver",
                        serverId: "arti",
                        repo: "gradle-dev"
                )

                rtGradleDeployer (
                        id: "respca-deployer",
                        serverId: "arti",
                        repo: "gradle-dev-local",
                )
            }
        }
        stage('Build') {
            steps {
                echo('Building...')
                rtGradleRun (
                        usesPlugin: false, // Set to true if the Artifactory Plugin is already defined in build script
                        tool: GRADLE_TOOL, // Tool name from Jenkins configuration
                        rootDir: "/",
                        buildFile: 'build.gradle',
                        tasks: 'clean artifactoryPublish',
                        resolverId: "rspca-resolver",
                        deployerId: "rspca-deployer",
                )
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