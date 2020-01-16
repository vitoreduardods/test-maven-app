stage 'SonarCloud'
// Split https://github.com/organization/repository/pull/123
node('test'){
    def urlcomponents = env.CHANGE_URL.split("/")
    def org = urlcomponents[3]
    def repo = urlcomponents[4]
    println(org)
    println(repo)
    echo "${env.CHANGE_ID}"
    echo "${env.CHANGE_BRANCH}"
    git url:  "https://github.com/${org}/${repo}.git"
    withSonarQubeEnv('SonarQube') {
        sh """
        java -version
        export JAVA_HOME="/usr/lib/jvm/java-1.11.0-openjdk-amd64"
        ls -al
        mvn clean package sonar:sonar
        mvn sonar:sonar \
        -Dsonar.pullrequest.provider=GitHub \
        -Dsonar.pullrequest.github.repository=${org}/${repo} \
        -Dsonar.pullrequest.key=${env.CHANGE_ID} \
        -Dsonar.pullrequest.branch=${env.CHANGE_BRANCH}"""
    }
}