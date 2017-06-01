pipeline {
  agent none
  options {
        skipDefaultCheckout()
  }
  stages {
        stage('Checkout') {
                agent any
                steps {
                        checkout scm
                        stash(name: 'ws', includes: '**')
                }
        }
        stage('Build Backend') {
                agent {
                        docker {
                                image 'maven:3-alpine'
                                args '-v $HOME/.m2:/root/.m2'
                        }
                }
                steps {
                        unstash 'ws'
                        sh './mvnw -B -DskipTests=true clean compile package'
                        stash name: 'war', includes: 'target/**/*.war'
                }
        }

        stage('Test Backend') {
                agent {
                        docker {
                                image 'maven:3-alpine'
                                args '-v $HOME/.m2:/root/.m2'
                        }
                }
                steps {
                        unstash 'ws'
                        unstash 'war'
                        sh './mvnw -B test findbugs:findbugs'
                }
                post {
                        success {
                                junit '**/surefire-reports/**/*.xml'
                                findbugs pattern: 'target/**/findbugsXml.xml', unstableNewAll: '0' //unstableTotalAll: '0'
                        }
                }
        }

    stage('More tests') {
        steps {
                echo 'more tests'
        }
    }

    stage('Build Staging') {
        steps {
                echo 'build staging'
        }
    }

    stage('Build Production') {
        steps {
                echo 'build production'
        }
    }
  }
}
