// Uses Declarative syntax to run commands inside a container.

pipeline {

    agent {

        kubernetes {

            // Rather than inline YAML, in a multibranch Pipeline you could use: yamlFile 'jenkins-pod.yaml'

            // Or, to avoid YAML:

            // containerTemplate {

            //     name 'shell'

            //     image 'ubuntu'

            //     command 'sleep'

            //     args 'infinity'

            // }

            yaml '''

apiVersion: v1

kind: Pod

spec:

  containers:

  - name: shell

    image: anandsadhu/dotnet-jenkins-slave

    command:

    - sleep

    args:

    - infinity

    tty: true

    resources:

      requests:

        memory: "2Gi"

    volumeMounts:

     - name: docker-sock

       mountPath: /var/run/docker.sock

  volumes:

    - name: docker-sock

      hostPath:

        path: /var/run/docker.sock      

'''

            // Can also wrap individual steps:

            // container('shell') {

            //     sh 'hostname'

            // }

            defaultContainer 'shell'

        }

    }

   stages {

        stage('docker build') {

            steps {

                sh 'docker build -t killivinay69/myapp .'

       }

    }

   stage('docker login') {

       steps {

            sh 'docker login -u killivinay69 -p @Vinay1999'

        }

    }

    stage('docker push') {

        steps {

            sh 'docker push killivinay69/myapp'

           }

    }

       stage('Deploy') {


    steps{

        script{

    sh 'kubectl apply -f Deployment.yaml'

    sh 'kubectl apply -f service.yaml'

        }
       }
      }
    stage("install helm"){
       steps {
            sh 'wget https://get.helm.sh/helm-v3.6.1-linux-amd64.tar.gz'
            sh 'ls -a'
            sh 'tar -xvzf helm-v3.6.1-linux-amd64.tar.gz'
            sh 'cp linux-amd64/helm /usr/bin'
            sh 'helm version'
          }
     }
      stage('helm') {
      steps {
          sh 'rm -f *.tgz'
          sh 'helm install demohelm .'
         }
     }

}

 }
}
