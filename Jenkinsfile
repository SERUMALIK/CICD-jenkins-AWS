node {
    def appDir = '/var/www/nextjs-app'

    stage('Clean Workspace'){
        echo 'Cleaning Jenkins Workspace'
        deleteDir()
    }

    stage('Clone Repo'){
        echo 'Cloning the repo'
        git(
            branch: 'main',
            url: 'https://github.com/SERUMALIK/CICD-jenkins-AWS.git'
        )
    }

    stage('Deploy to EC2'){ 
        echo 'Deploying to EC2'
        stage('Deploy to EC2'){ 
    echo 'Deploying to EC2'
    sh """
        sudo mkdir -p ${appDir}
        sudo chown -R jenkins:jenkins ${appDir}

        rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}

        cd ${appDir}

        # Install dependencies (NO sudo)
        npm install

        # Build app (limit memory to avoid crash)
        export NODE_OPTIONS="--max-old-space-size=512"
        npm run build

        # Stop old app if running
        sudo fuser -k 3000/tcp || true

        # Start app
        npm run start &
    """
}
}