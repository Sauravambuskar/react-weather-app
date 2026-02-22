pipeline {
agent any

```
environment {
    IMAGE_NAME = "react-weather-app"
    CONTAINER_NAME = "react-weather-container"
    PORT = "3003"
}

stages {

    stage('Checkout') {
        steps {
            git 'https://github.com/Sauravambuskar/react-weather-app.git'
        }
    }

    stage('Install Dependencies') {
        steps {
            sh '''
            docker run --rm \
            -v $WORKSPACE:/app \
            -w /app \
            node:lts-alpine \
            npm install
            '''
        }
    }

    stage('Test') {
        steps {
            sh '''
            docker run --rm \
            -v $WORKSPACE:/app \
            -w /app \
            node:lts-alpine \
            npm test -- --watchAll=false
            '''
        }
    }

    stage('Build React App') {
        steps {
            sh '''
            docker run --rm \
            -e CI=false \
            -v $WORKSPACE:/app \
            -w /app \
            node:lts-alpine \
            npm run build
            '''
        }
    }

    stage('Docker Build Image') {
        steps {
            sh 'docker build -t $IMAGE_NAME .'
        }
    }

    stage('Deploy Container') {
        steps {
            sh '''
            docker stop $CONTAINER_NAME || true
            docker rm $CONTAINER_NAME || true
            docker run -d -p $PORT:80 --name $CONTAINER_NAME $IMAGE_NAME
            '''
        }
    }
}

post {
    success {
        echo "Application deployed successfully!"
        echo "Access it at: http://<server-ip>:3003"
    }
    failure {
        echo "Pipeline failed!"
    }
}
```

}
