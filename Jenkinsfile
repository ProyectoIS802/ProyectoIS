pipeline {
    agent any

    stages {
        stage('Integración en Desarrollo') {
            steps {
                script {
                    // Clonar el repositorio
                    git branch: 'desarrollo', url: 'https://tu-repo.git'
                }
            }
        }

        stage('Pruebas') {
            steps {
                script {
                    // Fusionar cambios de desarrollo a pruebas
                    sh 'git checkout pruebas'
                    sh 'git merge desarrollo'
                    // Desplegar en servidor de pruebas
                    // Aquí puedes agregar el comando para desplegar en el servidor de pruebas
                    // Por ejemplo, usando SSH
                }
            }
        }

        stage('Aprobación de Pruebas') {
            steps {
                input '¿Aprobar cambios en pruebas?'
            }
        }

        stage('Producción') {
            steps {
                script {
                    // Hacer respaldo del directorio de producción
                    sh 'cp -r /ruta/a/produccion /ruta/a/produccion_backup_$(date +%Y%m%d%H%M%S)'
                    
                    // Fusionar cambios de pruebas a producción
                    sh 'git checkout produccion'
                    sh 'git merge pruebas'
                    
                    // Desplegar en producción
                    // Aquí puedes agregar el comando para copiar el contenido a producción
                    sh 'cp -r ./src/* /ruta/a/produccion/'
                }
            }
        }
    }

    post {
        failure {
            script {
                // Si falla, restaurar desde el respaldo
                sh 'cp -r /ruta/a/produccion_backup_* /ruta/a/produccion/'
                echo 'Restauración completada.'
            }
        }
        always {
            script {
                // Registrar logs
                sh 'echo "Despliegue realizado en $(date)" >> logs/deploy.log'
            }
        }
    }
}
