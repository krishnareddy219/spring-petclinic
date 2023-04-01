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
						id: 'jfrog_id',
						serverId: 'jfrog_id',
						releaseRepo: 'libs-release-local',
						snapshotRepo: 'libs-snapshot-local',
						// By default, 3 threads are used to upload the artifacts to Artifactory. You can override this default by setting:
						threads: 6,
						// Attach custom properties to the published artifacts:
						// properties: ['key1=value1', 'key2=value2']
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
