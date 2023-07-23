node {
    stage ("Git Clone") {
        git 'https://github.com/sagar-amd/spring-boot-mongo-k8s.git'
    }
    stage ("Code anaysis"){
        def sonarHome = tool 'sonarqube' ;
        withSonarQubeEnv('sonarqube') {
            sh "$(sonarHome)/bin/sonar-scanner \
            -D sonar.login=sagar \
            -D sonar.password=sagar123 \
            -D sonar.projectkey=sonarqubetest \
            -D sonar.executions=vendor/**,**/*.java \
            -D sonar.host.url=http://10.0.0.100:9000"
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