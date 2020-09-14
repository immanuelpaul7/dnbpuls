pipeline {
  agent {
      label 'master'
  }
  stages {
      //This is a top level stage. When you setup your pipeline on the SN instance
      //the stage name 'DevATF' here needs to match the Orchestration stage field
      //on the Steps record. you can find the Orchestration stage field by going to
      //the SN instance, Devops-> App & Pipelines -> Apps, open your app, it's under
      //the Steps related list
      stage('master') {
        //this specifies that this stage will be triggered on event from dev branch
        when {
          branch 'dev'
        }
        steps {
          snDevOpsStep()
          echo "Compiling..."
        }
      }

      //This is a top level stage.  When you setup your pipeline on the SN instance
      //the stage name 'DevHealthScan' here needs to match the Orchestration stage field
      //on the Steps record. you can find the Orchestration stage field by going to
      //the SN instance, Devops-> App & Pipelines -> Apps, open your app, it's under
      //the Steps related list
      stage('dev') {
        //this specifies that this stage will be triggered on event from dev branch
        when {
          branch 'dev'
        }
        steps {
          snDevOpsStep()
          echo "DevHealthScan only in dev..."
        }
      }

      //This is a top level stage.  When you setup your pipeline on the SN instance
      //the stage name 'QADeploy' here needs to match the Orchestration stage field
      //on the Steps record. you can find the Orchestration stage field by going to
      //the SN instance, Devops-> App & Pipelines -> Apps, open your app, it's under
      //the Steps related list.
      stage('feature/central_config') {
        //this specifies that this stage will be triggered on event from qa branch
        when {
          branch 'qa'
        }
        stages {
          //under stages, below are the nested child stages for parent stage QADeploy.
          //DevOps will only see the top level QADeploy stage.  We are grouping
          //deploy from git, publish to app store and deploy to instance into a single
          //parent stage for easier visualization on DevOps
          stage('QADeployFromGit') {
            steps {
              snDevOpsStep()
              echo "QADeployFromGit inside QADeploy only when branch is qa"
            }
          }

          stage('QAPublishToAppRepo') {
            steps {
              snDevOpsStep()
              echo "QAPublishToAppRepo inside QADeploy only when branch is qa"
            }
          }

          stage('QADeployFromAppRepo') {
            steps {
              snDevOpsStep()
              echo "QADeployFromAppRepo inside QADeploy only when branch is qa"
            }
          }
        }
      }
    }
  }
