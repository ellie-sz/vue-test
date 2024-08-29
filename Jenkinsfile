pipeline {
    agent any
    options {
        skipDefaultCheckout()  // 자동 체크아웃 방지
    }
    tools {
        nodejs 'NodeJS-16'
    }
    environment {
        DEPLOY_DIR = '/var/www/html/v8/demo/vue'
        DEPLOY_HTML = '/var/www/html'
        NVM_DIR = "${env.HOME}/.nvm"
        REPO_URL = 'https://github.com/ellie-sz/vue-test.git'
        

    }
    stages {
        stage('Checkout') {
            steps {
                script { // groovy
                    def branchName = env.BRANCH_NAME  
                    echo "Building branch: ${branchName}"  
                    checkout([$class: 'GitSCM', branches: [[name: "refs/heads/${branchName}"]], // checkout branch 지정
                              userRemoteConfigs: [[url: env.REPO_URL]], // 위 repo url 
                              doGenerateSubmoduleConfigurations: false, submoduleCfg: [], // Git 서브모듈 구성 생성 X 
                              extensions: [[$class: 'CloneOption', depth: 1, shallow: true]]]) // Git 리포지토리를 Clone, 최근 커밋 클론
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                 sh '''
                npm install --save-dev @vue/cli-service
                npm install
                '''
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
/*
        stage('File Delivery') {
            steps {
                script {
                    def distDir = 'dist'
                    def serverUser = 'freedream'
                    def serverHost = '192.168.31.250'
                    def serverDestDir = '/usr/local/var/www/v8/demo/vue'
                    def sshKeyPath = '~/.ssh/id_rsa'

                    // SCP 명령어를 사용하여 dist 디렉토리 내의 파일을 서버로 전송
                    sh """
                    sudo -u user scp -i ${sshKeyPath} -r ${distDir}/* ${serverUser}@${serverHost}:${serverDestDir}
                    """            
                
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def buildFiles = sh(script: "ls dist", returnStdout: true).trim()
                    echo "Built files: ${buildFiles}"
                }
                sh "sudo rm -rf ${DEPLOY_DIR}/*"
                sh "sudo rm -rf ${DEPLOY_HTML}/*"
                sh "sudo mkdir -p /var/www/html/v8/demo/vue"
                sh "sudo cp -r dist/. ${DEPLOY_DIR}/"
                sh "sudo cp -r dist/. ${DEPLOY_HTML}/"
            }
        }
        */
    }
}
