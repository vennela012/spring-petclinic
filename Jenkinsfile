pipeline {
    agent { label 'OPENJDK-11-JDK' }
    stages {
        stage ( 'vsc' ) {
            steps {
                git url: 'https://github.com/vennela012/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: 'jfrogJUL12',
                    url: 'https://vennagoud.jfrog.io/',
                    credentialsId: 'jfrog'
                )
                rtMavenDeployer (
                    id: 'jfrogJUL12',
                    serverId: 'https://vennagoud.jfrog.io/',
                    releaseRepo: 'batman-libs-release-local',
                    snapshotRepo: 'batman-libs-snapshot-local'
                )
            }
        }
        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: 'mvn-package', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: 'jfrogJUL12'
                )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: 'jfrogJUL12'
                )
            }
        }
    }
}