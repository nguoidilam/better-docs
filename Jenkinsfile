pipeline {
    agent any

    environment {
        NPM_RELEASE = 'https://artifact.nguoidilam.com/api/npm/npm-release/'
        // GH_TOKEN= "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"
        GH_TOKEN= credentials('github_nguoidilam')
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '3'))
    }

    stages {
        stage('Checkout code') {
            steps {
                sh 'printenv'
                checkout scm
            }
        }

        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                withNPM(npmrcConfig: 'artifactory-npm') {
                    sh '''
                        npm version 1.0.${BUILD_NUMBER} --no-git-tag-version;
                        yarn;
                        yarn build;
                        yarn release --registry ${NPM_RELEASE};
                    '''
                }
            }
        }
    }
}