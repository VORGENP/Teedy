pipeline {
    agent any

    tools {
        // 确保你在 Jenkins 全局工具配置里配置了名为 'Maven' 的工具
        maven 'Maven'
    }

    stages {
        stage('Maven构建与安装') {
            steps {
                echo '>>> 开始编译项目并安装依赖...'
                // 解释：clean清理旧数据，install将模块安装到本地仓库以解决依赖问题，-DskipTests跳过测试加快速度
                bat 'mvn clean install -Dmaven.test.skip=true'
            }
        }

        stage('PMD代码静态检查') {
            steps {
                echo '>>> 正在进行 PMD 代码质量检查...'
                // 解释：单独运行 PMD 插件
                bat 'mvn pmd:pmd -DskipTests'
            }
        }

        stage('运行单元测试') {
            steps {
                echo '>>> 开始运行单元测试...'
                // 解释：正式运行测试，生成 surefire-reports
                bat 'mvn test'
            }
        }

        stage('生成 JavaDoc 文档包') {
            steps {
                echo '>>> 正在生成 JavaDoc JAR 包...'
                // 解释：任务特别要求生成 .jar 格式的文档，使用 javadoc:jar 目标
                bat 'mvn javadoc:jar -DskipTests'
            }
        }

        stage('收集产物') {
            steps {
                echo '>>> 正在归档实验要求的产物...'
                // 解释：严格按照任务书要求匹配文件
                archiveArtifacts artifacts: '**/target/*.jar, **/target/site/pmd.html, **/target/surefire-reports/*.xml', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            echo '流水线执行结束。'
            // 可以在这里添加清理工作空间的步骤，防止磁盘占满
            // cleanWs()
        }
    }
}
