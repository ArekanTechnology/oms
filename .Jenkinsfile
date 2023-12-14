pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
               git 'https://github.com/ArekanTechnology/oms.git'
            }
        }
        
        stage('Activating virtual environment / Making Migrations/ migrating')
        {
            steps {
                dir('/var/www/oms') {
                    script {
                        sh 'venv/bin/activate'
                        sh 'cd oms/'
                        sh 'pip install -r requirements.txt'
                    }
                }
                dir('/var/www/oms/oms') {
                    script {
                        sh 'python3 manage.py makemigrations'
                        sh 'python3 manage.py migrate'
                    }
                }
            }
            
        }
        stage('Pulling the latest version of the code')
        {
            steps {
                dir('/var/www/oms') {
                    script {
                        sh 'git pull origin master'
                    }
                }
            }
        }
        
        stage('Restarting the web server')
        {
            steps {
                dir('/var/www/oms') {
                    script {
                        sh 'sudo su'
                        sh 'whoami'
                        sh 'sudo systemctl restart apache2'
                        sh 'sudo systemctl status apache2'
                    }
                }
            }
        }
        
    }
    
    
}
