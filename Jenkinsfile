node {
    stage('Build') {
        properties([
            pipelineTriggers([
                pollSCM('H/2 * * * *')
            ])
        ])

        docker.image('python:3.13-slim').inside {
            stage('Install Dependencies') {
                sh 'python -m venv venv'
                sh '. venv/bin/activate && pip install --upgrade pip'
                sh '. venv/bin/activate && pip install -r requirements.txt'
            }

            stage('Executable') {
                sh 'pyinstaller --onefile add2vals.py'
            }
        }
    }
}