@Library('shared-lib')_
pipFunc() {
    git 'http://snowdevops@dnbpuls.sndevops.net:81/scm/dnbpul/dnb-web.git'
    def mvn_version = 'Maven'
    stage('compile') {
        snDevOpsStep (stepSysId:'28099226533733007109ddeeff7b1264')
        printBuildinfo {
        	name = "Compiling..."
        }
        withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
        	sh 'mvn clean install -DskipTests'
		}
    }
    stage('test') {
        snDevOpsStep (stepSysId:'24099226533733007109ddeeff7b1264')
        printBuildinfo {
        	name = "Testing....."
        }
        withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
        	sh 'mvn test -Dpublish'
        }
        junit '**/target/surefire-reports/*.xml'
    }
    stage('package') {
        snDevOpsStep (stepSysId:'1c099226533733007109ddeeff7b1263')
        printBuildinfo {
        	name = "Packaging...."
        }
        withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
			sh 'mvn package -Dmaven.test.skip=true --batch-mode'
        }
        deploy()
    }
    stage('docker build') {
        snDevOpsStep (stepSysId:'a4099226533733007109ddeeff7b1264')
        printBuildinfo {
        	name = "Docker build...."
        }
    }
    stage('deploy') {
        snDevOpsStep (stepSysId:'a8099226533733007109ddeeff7b1264')
        snDevOpsChange()
        printBuildinfo {
             name = "Deploying...."
        }
		checkStatus()
    }
}

