node {
    stage ("Git Clone") {
        git 'https://github.com/sagar-amd/spring-boot-mongo-k8s.git'
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
        sh "docker push sagaramd/springmongo "
    }
}