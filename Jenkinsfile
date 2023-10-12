pipeline {
    agent { label 'windows' }

    stages {
        stage('checkout') {
            steps {
                echo "SCM...${env.BUILD_ID} on ${env.BUILD_URL}, ${env.WORKSPACE}"
                git (
                    url: 'https://github.com/babo72/pytest-qt-example.git',
                    branch: 'master',
                    credentialsId: 'babo72-github-token',
                    changelog: true,
                    poll: true
                )
            }
        }
        stage('py test') {
            steps {
                bat 'pip install -r requirements.txt'
                bat 'python -m pytest'
                bat 'python -m pytest --cov-report xml --cov messageboxex'
            }
        }
        stage('SonarQube analysis') {
            steps {
                echo 'sonar scan...'
                script {
                    scannerHome = tool 'Sonar-Scanner'
                }
                withSonarQubeEnv('SonarCloud-babo72'){
                    bat "${scannerHome}/bin/sonar-scanner -Dsonar.python.coverage.reportPaths=coverage.xml -Dsonar.organization=babo72 -Dsonar.projectKey=babo72_pyqt-example -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=9f5fe1f43c696afcaf53387d5ee0ec989da98bdf"
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Res compile') {
            steps {
                echo 'compiling (ui, qrc)...'
            }
        }
        stage('pyinstall') {
            steps {
                echo 'pyinstall...'
                bat 'pip install pyinstaller'
                //bat 'python -m pyinstaller --onefile --windowed messageboxex.py'
                bat 'pyinstaller --onefile --windowed messageboxex.py'
            }
        }
    }
}
