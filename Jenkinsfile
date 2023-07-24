node {
    stage ("Git Clone") {
        git 'https://github.com/sagar-amd/spring-boot-mongo-k8s.git'
    }
    stage ("Code Anaysis") {
        def sonarHome = tool 'sonarqube' ;
        withSonarQubeEnv('sonarqube') {
            sh "${sonarHome}/bin/sonar-scanner \
             -Dsonar.projectKey=sonar-app \
             -Dsonar.executions=vendor/**,**/*.* \
             -Dsonar.host.url=http://10.0.0.100:9000 \
             -Dsonar.login=sqp_03b14adef933a99383f2df8a25b2da1a665cdd28 \
             -Dsonar.java.binaries=**/**/**"
        }
    }    
    stage (" Maven Build  ") {
        def mavenHome = tool name: "maven-3.9", type: "maven"
        def mavenCMD = "${mavenHome}/bin/mvn "
        sh "${mavenCMD} clean package"
    }
    stage ("Build docker image") {
        sh "docker build -t sagaramd/springmongo-ks ."
    }
    stage ("Docker Img Push") {
        withCredentials([string(credentialsId: 'docker_hub', variable: 'docker_hub')]) {
          sh "docker login -u sagaramd -p ${docker_hub}"
        }
        sh "docker push sagaramd/springmongo-ks "
    }
    stage ("Deploy application in K8s Cluster") {
        sh "kubectl apply -f springBootMongo-PrivateRepo.yml    "
    }
}