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
        sh "docker build -t sagaramd/springmongo-ks"
    }
}