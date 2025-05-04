pipeline {
    agent {
        docker { image 'debian'
        args '-u root:root'
        }
    }
    stages {
        stage('Clone') {
            steps {
                git branch:'master',url:'https://github.com/josedom24/ic-travis-diccionario.git'
            }
        }
        stage('Install') {
            steps {
                sh 'apt-get update && apt-get install -y aspell-es ' 
            }
        }
        stage('Test')
        {
            steps {
                sh '''
                export LC_ALL=C.UTF-8
                OUTPUT=`cat doc/*.md | aspell list -d es -p ./.aspell.es.pws`; if [ -n "$OUTPUT" ]; then echo $OUTPUT; exit 1; fi'''
            }
        }
    }
    post {
         always {
          mail to: 'daneilarantemartin@gmail.com',
          subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
          body: "${env.BUILD_URL} has result ${currentBuild.result}"
        }
      }
}
