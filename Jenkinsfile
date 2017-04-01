#!groovy

def org = 'duck1123'
def project = 'catalogs'

def err, image

node('docker') {
    ansiColor('xterm') {
        stage('Init') {
            checkout scm
            env.BUILD_TAG = env.BUILD_TAG.replaceAll('%2F', '-')

            def isPR = false

            if (env.CHANGE_ID) {
                echo "PR build detected due to change id"
                isPR = true
            }

            if (env.BRANCH_NAME == 'develop') {
                env.BRANCH_TAG = 'latest'
            } else if (env.BRANCH_NAME == 'master') {
                // TODO: Parse version numbers
                env.BRANCH_TAG = 'stable'
            } else {
                env.BRANCH_TAG = env.BRANCH_NAME.replaceAll('/', '-')
            }

            // Print Environment
            sh 'env | sort'
        }

        stage('Validate Files') {
            docker.image('ruby').inside(["--name ${env.BUILD_TAG}-test"].join(' ')) {
                // Validate Yaml files
                sh "script/validate"
            }
        }

        // stage('Notify') {
        //     sh "docker run perarneng/fortune > fortune.txt"
        //     def fortune = readFile('fortune.txt')

        //     hipchatSend( color: 'GREEN', message: "<a href=\"${env.BUILD_URL}\">${env.BUILD_TAG}</a>: build passed<br/><em>${fortune}</em>",
        //     notify: true, room: '572', sendAs: env.NODE_NAME)
        // }
    }
}
