node {
  stage 'Checkout'
  git credentialsId: '56666fc2-42cb-48e9-993d-266940d05433', url: 'https://github.com/uppsala-university/sample-project.git'
  sh "git clean -f && git reset --hard origin/master"

  def pom = readMavenPom file: 'pom.xml'
  def version = pom.version.replace("-SNAPSHOT", ".${currentBuild.number}")

  stage 'Build'
  withMaven(
      maven:'apache-maven-3.3.9',
      mavenSettingsConfig:'1a894ab7-9af8-4ea8-abfc-edd4a1da44b8'
  ){
      sh "mvn -DreleaseVersion=${version} -DdevelopmentVersion=${pom.version} -DpushChanges=false -DlocalCheckout=true -DpreparationGoals=initialize release:prepare release:perform -B"
  }

  input 'Publish?'
  stage 'Publish'
  withCredentials([usernamePassword(credentialsId: 'jorans-github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
    sh "echo env.GIT_USERNAME:${env.GIT_USERNAME} GIT_USERNAME:${GIT_USERNAME}"
    sh "git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/uppsala-university/sample-project.git ${pom.artifactId}-${version}"
  }
}
