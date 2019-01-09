pipeline {
    agent any
    stages {
        stage("Preparation") {
            steps {
//                git 'https://github.com/ramonsterj/some.git'
                sh "chmod +x gradlew"
            }
        }
//        stage("Build") {
//            steps {
//                sh "./gradlew clean assemble"
//            }
//        }
        stage('Publish to Artifactory') {
            steps {
                echo('Building...')
                rtGradleResolver (
                        id: "rspca-resolver",
                        serverId: "arti",
                        repo: "gradle-dev"
                )

                rtGradleDeployer (
                        id: "rspca-deployer",
                        serverId: "arti",
                        repo: "gradle-dev-local",
                        deployIvyDescriptors: false,
                        deployMavenDescriptors: true
                )
                rtGradleRun (
                        usesPlugin: true, // Set to true if the Artifactory Plugin is already defined in build script
                        tool: "Gradle4", // Tool name from Jenkins configuration
//                        rootDir: "some",
                        useWrapper: true,
                        buildFile: 'build.gradle',
                        tasks: 'clean install artifactoryPublish',
                        resolverId: "rspca-resolver",
                        deployerId: "rspca-deployer",
                )
            }
        }
    }
}