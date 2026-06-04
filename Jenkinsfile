pipeline {
    agent any
    
    tools {
        maven 'Maven'
    }
    
    stages {
        stage('Maven构建') {
            steps {
                echo '开始编译项目...'
                bat 'mvn clean package -DskipTests'
            }
        }
        
        stage('PMD代码检查') {
            steps {
                echo '开始进行PMD静态代码检查...'
                bat 'mvn pmd:pmd'
            }
        }
        
        stage('运行测试') {
            steps {
                echo '开始运行单元测试...'
                bat 'mvn test'
            }
        }
        
        stage('生成测试报告') {
            steps {
                echo '正在生成测试报告...'
                archiveArtifacts artifacts: '**/target/surefire-reports/**/*', allowEmptyArchive: true
            }
        }
        
        stage('生成JavaDoc项目文档') {
            steps {
                echo '开始生成 JavaDoc 文档...'
                bat 'mvn javadoc:jar'
            }
        }
    }
    
    post {
        always {
            echo '正在收集实验要求的产物...'
            archiveArtifacts artifacts: '**/target/*.jar, **/target/site/pmd.html, **/target/surefire-reports/*.xml', allowEmptyArchive: true
        }
    }
}
