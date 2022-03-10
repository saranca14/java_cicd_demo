# java_cicd

Pipeline_script

```
node {
    stage("Git Clone"){

            git credentialsId: 'GIT_HUB_CREDENTIALS', url: 'https://github.com/saranca14/java_cicd_demo.git'
        }

        stage('+x script'){
                sh "chmod +x gradlew"
        }

        stage('Gradle Build') {

           sh './gradlew build'

        }

        stage("Docker build"){
            sh 'docker version'
            sh 'docker build -t spring_app .'
            sh 'docker image list'
            sh 'docker tag spring_app saranrnair/spring_app:v1'
        }

        stage("Docker Login"){
            withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
                sh 'docker login -u saranrnair -p $PASSWORD'
            }
        }

        stage("Push Image"){
            sh 'docker push  saranrnair/spring_app:v1'
        }
}
```

<img width="1285" alt="image" src="https://user-images.githubusercontent.com/61488445/157595816-81148ea2-2f00-4f88-bf7b-3ae2a3e0e45d.png">

