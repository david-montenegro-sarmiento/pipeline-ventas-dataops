pipeline {
    agent { label 'AgenteDocker' } // Usa tu nodo configurado

    stages {
        stage('Checkout') {
            steps {
                checkout scm // Descarga el código de GitHub
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t reporte-app .' // Construye usando tu Dockerfile
            }
        }
        stage('Run & Generate') {
            steps {
                // Ejecuta el contenedor y genera el Excel
                sh 'docker run --name ejecucion_reporte reporte-app'
            }
        }
    }
    post {
        success {
            // Extrae el artefacto del contenedor al espacio de Jenkins
            sh 'docker cp ejecucion_reporte:/app/ComisionesCalculadas.xlsx .'
            // Publica el archivo para que aparezca el botón de descarga
            archiveArtifacts artifacts: 'ComisionesCalculadas.xlsx', fingerprint: true
        }
        always {
            sh 'docker rm -f ejecucion_reporte' // Limpia el contenedor
        }
    }
} 