pipeline {
    agent { label 'testRepo' }
    properties(
        [
            [
                $class       : 'GithubProjectProperty',
                projectUrlStr: 'https://github.com/harshniA/test-repo'
            ],
            pipelineTriggers([[$class: 'GitHubPushTrigger']])
        ]
    )
    environment {
        // Specify your environment variables.
        APP_VERSION = '1'
    }
    stages {
        stage('Build') {
            steps {
                // Print all the environment variables.
                sh 'printenv'
                echo 'Building the docker image with the current git commit'
                sh 'docker build -f Dockerfile -t registry.MOJtest.com/test_project:$GIT_COMMIT .'
            }
        }
        stage('Test') {
            steps {
                echo 'PHP Unit tests'
                sh 'docker-compose -f docker-compose.yml up -d --build --remove-orphans'
                sh 'sleep 5'
                //run the install target from make file
                sh 'make install'
                //run the tests target from make file to execute php unit tests
                sh 'make tests'
                //run the run target from make file to execute behat tests
                sh 'make run'

            }
        }
        stage('Push') {
            when {
                branch 'master'
            }
            steps {
                echo 'Deploying docker images'
                //Perform the necessary deploy steps to deploy docker containers
            }
        }
    }
}