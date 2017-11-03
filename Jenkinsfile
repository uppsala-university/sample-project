node {
  stage 'Checkout'
  checkout scm

  stage 'Build'
  withMaven(
      maven:'apache-maven-3.3.9',
  ){
      sh "mvn clean verify"
  }

}
