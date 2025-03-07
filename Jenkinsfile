node {
    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }

    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        junit 'test-reports/results.xml'
    }
    
    stage('Manual Approval') {
        def userInput = input(
            message: 'Lanjutkan ke tahap Deploy?',
            parameters: [choice(name: 'ACTION', choices: ['Proceed', 'Abort'], description: 'Pilih aksi')]
        )
        
        if (userInput == "Abort") {
            echo 'Menghentikan eksekusi pipeline'
            return
        }
        
        echo 'Melanjutkan eksekusi pipeline ke tahap Deploy'
    }
    
    stage('Deploy') {
        def appContainer = docker.image('python:2-alpine').run('-d', 'sleep 60')
        sleep 60
        sh "docker stop ${appContainer.id}"
    }
}
