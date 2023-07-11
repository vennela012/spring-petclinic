pipeline {
    agent { label 'OPENJDK-11-JDK' }
    parameters {
        string(name: 'maven_goal', defaultValue: 'package', description: 'this is maven goals')
        choice(name: 'branch', defaultValue: ['main','master','venna'], description: 'this is branch for building code')
    }
    stages {
        stage ('vcs') {
            steps {
                git branch: "${params.branch}",
                    url: 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }
       stage ('Artifactory configuration') {
            steps {
                rtMavenDeployer (
                    id: 'MAVEN_DEPLOYER',
                    serverId: 'jfrog',
                    releaseRepo: 'batman-libs-release-local',
                    snapshotRepo: 'batman-libs-snapshot-local'
                )
            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: mvn-package, // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: 'MAVEN_DEPLOYER'
                )
            }
        } 
    }
}