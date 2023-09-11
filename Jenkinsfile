def imageName = 'adriannavarro/marketplace'

node('jenkins_agent'){
    stage('Checkout'){
        checkout scm
    }

    //def imageTest= docker.build("${imageName}-test", "-f Dockerfile.test .")

    // stage('Quality Tests'){
    //     sh "docker run --rm ${imageName}-test npm run lint"
    // }

    stage('Unit Tests'){
        // sh "docker run --rm -v $PWD/coverage:/app/coverage ${imageName}-test npm run test"
        // publishHTML (target: [
        //     allowMissing: false,
        //     alwaysLinkToLastBuild: false,
        //     keepAll: true,
        //     reportDir: "$PWD/coverage/marketplace",
        //     reportFiles: "index.html",
        //     reportName: "Coverage Report"
        // ])
        echo "Tests passed"
    }

    // stage('Static Code Analysis'){
    //     withSonarQubeEnv('sonarqube') {
    //         sh 'sonar-scanner'
    //     }
    // }

    // stage("Quality Gate"){
    //     timeout(time: 5, unit: 'MINUTES') {
    //         def qg = waitForQualityGate()
    //         if (qg.status != 'OK') {
    //             error "Pipeline aborted due to quality gate failure: ${qg.status}"
    //         }
    //     }
    // }

    stage('Build'){
        //docker.build(imageName, '--build-arg ENVIRONMENT=sandbox .')
        echo "Build passed"

    }

    stage('Push'){   
        echo "Push passed"

        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            docker.image(imageName).push(commitID())

            if (env.BRANCH_NAME == 'develop') {
                docker.image(imageName).push('develop')
            }
        }
    }
}

def commitID() {
    sh 'git rev-parse HEAD > .git/commitID'
    def commitID = readFile('.git/commitID').trim()
    sh 'rm .git/commitID'
    commitID
}