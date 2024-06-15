pipeline {
    
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', credentialsId: 'GitHub-Server-ID', url: 'https://github.com/NageshPatil321/flaskproject.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                script {
                    // Install virtual environment and dependencies
                    sh '''
                        python3 -m venv $VENV_DIR
                        . $VENV_DIR/bin/activate
                        pip install --upgrade pip
                        pip install -r requirements.txt
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'cp -rf /var/lib/jenkins/workspace/flask-pipeline/* /opt/myFlaskApplication/'
                sh 'zip -r /var/lib/jenkins/workspace/flask-pipeline/artifact.zip /var/lib/jenkins/workspace/flask-pipeline/'
            }
        }

        stage('Application Reload') {
            steps {
                sh 'sudo systemctl restart gunicorn'
                sh 'sudo systemctl restart nginx'
            }
        }

        stage('Backup to S3') {
            steps {
                script {
                    // Upload the zip file to S3 using the S3 Plugin
                    s3Upload consoleLogLevel: 'INFO',
                             dontSetBuildResultOnFailure: false,
                             dontWaitForConcurrentBuildCompletion: false,
                             entries: [[
                                 bucket: 'devops-assignment-nagesh-patil',
                                 gzipFiles: false,
                                 keepForever: false,
                                 managedArtifacts: false,
                                 noUploadOnFailure: true,
                                 selectedRegion: 'ca-central-1',
                                 showDirectlyInBrowser: false,
                                 sourceFile: "**/artifact.zip", // Ant GLOB pattern
                                 storageClass: 'STANDARD',
                                 uploadFromSlave: false,
                                 useServerSideEncryption: false
                             ]],
                             pluginFailureResultConstraint: 'FAILURE',
                             profileName: 'S3-Bucket-Storage',
                             userMetadata: []
                }
            }
        }
        stage('clean WS') {
            steps {
                cleanWs()
            }
        }
    }
}