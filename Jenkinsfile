node {
    def venvDir = 'venv'

    try {
        stage('Checkout Code') {
            git branch: 'main', url: 'https://github.com/your-repo/flask-app.git'
        }

        stage('Setup Python Environment') {
            sh """
                python3 -m venv ${venvDir}
                source ${venvDir}/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
            """
        }

        stage('Run Integration Tests') {
            sh """
                source ${venvDir}/bin/activate
                pytest tests/ --junitxml=test-results.xml
                deactivate
            """
        }

        stage('Publish Test Results') {
            junit 'test-results.xml'
        }

        echo '🎉 Integration Tests Passed Successfully!'

    } catch (Exception e) {
        echo '❌ Integration Tests Failed! Check the logs.'
        currentBuild.result = 'FAILURE'
    } finally {
        stage('Cleanup') {
            sh "rm -rf ${venvDir}"  // Cleanup virtual environment
        }
    }
}
