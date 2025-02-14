node {
    stage('Build') {
        properties([
            parameters([
                string(name: 'IMAGE', defaultValue: 'python:3.13-slim')
            ]),
            pipelineTriggers([
                pollSCM('H/2 * * * *') 
            ])
        ])

        docker.image(params.IMAGE).inside {
            sh 'pip install --upgrade pip'
            sh 'pip install -r requirements.txt'
            sh 'pyinstaller --onefile add2vals.py'
        }
    }

    stage('Test') {
        docker.image('python:3.11-slim').inside {
            sh 'pytest --junitxml=pytest-report.xml'
        }
    }
}
