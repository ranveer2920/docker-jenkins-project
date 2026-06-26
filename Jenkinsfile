pipeline {

    agent any

    environment {

        IMAGE_NAME="myapp"

        TAG="${BUILD_NUMBER}"

        DEV="ec2-user@13.60.75.167"

        STAGE="ec2-user@13.48.24.176"

        PROD="ec2-user@13.61.14.255"

    }

    stages {

        stage('Clone') {

            steps {

                git 'https://github.com/ranveer2920/docker-jenkins-project.git'

            }

        }

        stage('Build Docker Image on Dev') {

            steps {

                sh """

                ssh $DEV '

                docker build -t $IMAGE_NAME:$TAG .

                '

                """

            }

        }

        stage('Run Container on Dev') {

            steps {

                sh """

                ssh $DEV '

                docker rm -f app || true

                docker run -d --name app -p 80:80 $IMAGE_NAME:$TAG

                '

                """

            }

        }

        stage('Deploy Stage') {

            steps {

                sh """

                ssh $STAGE '

                docker build -t $IMAGE_NAME:$TAG .

                docker rm -f app || true

                docker run -d --name app -p 80:80 $IMAGE_NAME:$TAG

                '

                """

            }

        }

        stage('Deploy Production') {

            steps {

                sh """

                ssh $PROD '

                docker build -t $IMAGE_NAME:$TAG

                docker rm -f app || true

                docker run -d --name app -p 80:80 $IMAGE_NAME:$TAG

                '

                """

            }

        }

    }

}
