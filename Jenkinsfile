pipFunc() {
   // git 'http://snowdevops@dnbpuls.sndevops.net:81/scm/dnbpul/dnb-web.git'
    git 'https://snowdevops@github.com/SealTeam79/dnbpuls.git'
    def mvn_version = 'Maven'
    stage('compile') {
	/*script {
             env.COMPILE_STEPSYSID = '6480283b533733007109ddeeff7b1241'
	     env.TEST_STEPSYSID = 'e480283b533733007109ddeeff7b1241'
        }*/
        //snDevOpsStep ("${env.COMPILE_STEPSYSID}")
	snDevOpsStep()
        //snDevOpsChange()
        printBuildinfo {
        	name = "Compiling..."
        }
        withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
        	sh 'mvn clean install -DskipTests'
		}
    }
    stage('test') {
     
	//snDevOpsStep (stepSysId:"${env.TEST_STEPSYSID}")
	snDevOpsStep()    
        printBuildinfo {
        	name = "Testing....."
        }
        withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
        	sh 'mvn test -Dpublish'
        }
        junit '**/target/surefire-reports/*.xml'
    }
    stage('package') {
        snDevOpsStep ()
        printBuildinfo {
        	name = "Packaging...."
        }
        withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
		sh 'mvn package -Dmaven.test.skip=true --batch-mode'
        }
        //deploy()
    }
    if (env.BRANCH_NAME == "dev") {   
    	stage('deploy to dev') {   
		snDevOpsStep ()
		printBuildinfo {
			name = "Docker build...."
		}
    	}
    }
    if (env.BRANCH_NAME == "master") { 	
    	stage('deploy to prod') {   
		snDevOpsStep ()
		snDevOpsChange()
		printBuildinfo {
		     name = "Deploying...."
		}
		checkStatus()
    	}
    }
}
