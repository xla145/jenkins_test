node("master") {
    checkout scm
    def workspace = pwd()
    def TOMCAT_HOME = "/home/xla_test/apache-tomcat-7.0.84"
    def MVN_HOME = tool 'apache-maven-3.5.4'
    def MVN_BIN = "${MVN_HOME}/bin/mvn"
    stage('Test') {
        echo "Test start..."
        sh """
            ${MVN_BIN} clean && ${MVN_BIN} test
        """
    }
    stage('Deploy') {
        echo "Deploy start..."
        sh """
            ${MVN_BIN} package -Dmaven.test.skip=true
            ${TOMCAT_HOME}/bin/catalina.sh stop || true
            rm -rf ${TOMCAT_HOME}/webapps/ROOT/*
            cp target/jenkins_test-1.0.0.war ${TOMCAT_HOME}/webapps/ROOT/
            unzip ${TOMCAT_HOME}/webapps/ROOT/jenkins_test-1.0.0.war -d ${TOMCAT_HOME}/webapps/ROOT >/dev/null
            rm -f ${TOMCAT_HOME}/webapps/ROOT/jenkins_test-1.0.0.war
            ${TOMCAT_HOME}/bin/catalina.sh start
        """
    }
}