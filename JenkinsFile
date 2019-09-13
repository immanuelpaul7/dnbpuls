@Library('shared-lib')_
pipFunc() {
    git 'http://admin@dnbpuls.sndevops.net:81/scm/dnbpul/dnb-web.git'
    def mvn_version = 'Maven'
    stage('compile') {
        snDevOpsStep (stepSysId:'a72e5a86dbb373003e87f5861d9619e8')
        printBuildinfo {
        	name = "Compiling..."
        }
        withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
        	sh 'mvn clean install -DskipTests'
		}
    }
    stage('test') {
        snDevOpsStep (stepSysId:'272e9a86dbb373003e87f5861d961965')
        printBuildinfo {
        	name = "Testing....."
        }
        withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
        	sh 'mvn test -Dpublish'
        }
        junit '**/target/surefire-reports/*.xml'
    }
    stage('package') {
        snDevOpsStep (stepSysId:'ab2e9a86dbb373003e87f5861d961965')
        printBuildinfo {
        	name = "Packaging...."
        }
        withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
			sh 'mvn package -Dmaven.test.skip=true --batch-mode'
        }
        deploy()
    }
    stage('docker build') {
        snDevOpsStep (stepSysId:'a72e9a86dbb373003e87f5861d961965')
        printBuildinfo {
        	name = "Docker build...."
        }
    }
    stage('deploy') {
        snDevOpsStep (stepSysId:'2b2e9a86dbb373003e87f5861d961965')
        snDevOpsChange()
        printBuildinfo {
             name = "Deploying...."
        }
		checkStatus()
    }
}

