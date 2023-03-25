 pipeline {
  agent { label 'krishna' }
  stages {
    stage('vcs') {
      steps {
        git url: 'https://github.com/spring-projects/spring-petclinic.git',
            branch: 'main'
        
            } 
    }
    stage('build') {
      steps {
        sh 'mvn clean package'
            }
          }
    stage ('Artifactoryconfiguration') {
                steps {
                    rtServer (
                        id: "jfrog_id",
                        url: 'https://krishna219.jfrog.io/artifactory',
                        credentialsId: 'jfrog_server'
                    )

                    rtMavenDeployer (
                        id: "jfrog_id",
                        serverId: "jfrog_id",
                        releaseRepo: 'libs_release',
                        snapshotRepo: 'libs_snapshot'
                    )

                    rtMavenResolver (
                        id: "jfrog_id",
                        serverId: "jfrog_id",
                        releaseRepo: 'libs_release',
                        snapshotRepo: 'libs_snapshot'
                    )
                }
            }

            stage ('Exec Maven') {
                steps {
                    sh "mvn clean install"
                /*    rtMavenRun (
                        tool: 'maven_3.6.3',
                        pom: 'pom.xml',
                        goals: 'clean install',
                        deployerId: "jfrog_id",
                        
                    )*/
                }
            }

            stage ('Publish build info') {
                steps {
                    sh "mvn clean install"
                    rtPublishBuildInfo (
                        serverId: "jfrog_id"
                    )
                }
            }
        }
    }
