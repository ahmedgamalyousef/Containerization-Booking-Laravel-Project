pipeline {
    agent any
    options {
    // Force a new workspace each build
        skipDefaultCheckout()
    }
    environment {
        COMPOSE_PROJECT_NAME = 'booking-laravel'
        MYSQL_ROOT_PASSWORD = 'rootpass'
        MYSQL_DATABASE = 'booking'
        MYSQL_USER = 'bookinguser'
        MYSQL_PASSWORD = 'Booking@1234'
    }
    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ahmedgamalyousef/Containerization-Booking-Laravel-Project'
            }
        }

        stage('Stop Old Services') {
            steps {
                sh 'docker compose down -v'
            }
        }

        stage('Build Images') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Prepare .env') {
            steps {
                sh '''
                if [ ! -f .env ]; then
                    cp .env.example .env
                fi
                sed -i "s/DB_HOST=.*/DB_HOST=db/" .env
                sed -i "s/DB_DATABASE=.*/DB_DATABASE=${MYSQL_DATABASE}/" .env
                sed -i "s/DB_USERNAME=.*/DB_USERNAME=${MYSQL_USER}/" .env
                sed -i "s/DB_PASSWORD=.*/DB_PASSWORD=${MYSQL_PASSWORD}/" .env
                '''
            }
        }

        stage('Start Services') {
            steps {
                sh 'docker compose up -d'
            }
        }

        stage('Wait for DB') {
            steps {
                sh '''
                echo "Waiting for MySQL..."
                until docker compose exec -T db mysqladmin ping -uroot -p${MYSQL_ROOT_PASSWORD} --silent; do
                    echo "MySQL is unavailable - sleeping"
                    sleep 3
                done
                echo "MySQL is up!"
                '''
            }
        }

        stage('Laravel Setup') {
            steps {
                sh '''
                docker compose exec -T app chown -R www-data:www-data /var/www/html
                docker compose exec -T app chmod -R 775 /var/www/html/storage /var/www/html/bootstrap/cache
                docker compose exec -T app composer install --no-interaction --prefer-dist
                docker compose exec -T app php artisan key:generate --force
                docker compose exec -T app php artisan migrate --force
                docker compose exec -T app php artisan db:seed --force
                docker compose exec -T app php artisan config:cache
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                echo "Checking app health..."
                for i in {1..10}; do
                    if curl -f http://localhost:8000; then
                        echo "App is healthy!"
                        exit 0
                    else
                        echo "App not ready, retrying in 3 seconds..."
                        sleep 3
                    fi
                done
                echo "App failed health check!"
                exit 1
                '''
            }
        }

    }

    post {
        success {
            echo "✅ Deployment Succeeded!"
        }
        failure {
            echo "❌ Deployment Failed!"
            sh 'docker compose logs --tail=50'
        }
    }
}
