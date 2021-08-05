node('haimaxy-jnlp'){
    stage("Git 远程仓库拉取"){
        git url: "https://github.com.cnpmjs.org/Menganhaoa/jenkins-demo.git",
            credentialsId: "github.com",
            branch: "master"
    }
    
    stage('Build') {
        echo "3.Build Docker Image Stage"
        sh "docker build -t harbor.od.com/app/jenkins-demo:${build_tag} ."
    }
    
    stage('docker image push') {
            withCredentials([usernamePassword(credentialsId: 'harbor.com', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            sh "docker login -u ${dockerHubUser} -p ${dockerHubPassword} harbor.od.com"
            sh "docker push harbor.od.com/app/jenkins-demo:${build_tag}"
        }
    }
    
    stage('YAML') {
        echo "5. Change YAML File Stage"
        sh "sed -i 's/<BUILD_TAG>/${build_tag}/' k8s.yaml"
        sh "sed -i 's/<BRANCH_NAME>/master/' k8s.yaml"
        sh 'cat k8s.yaml'
    }
    
    stage('Deploy') {
        echo "6. Deploy Stage"
        sh "kubectl apply -f k8s.yaml"
    }
}
