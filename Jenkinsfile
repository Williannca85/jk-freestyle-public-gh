pipeline {
    agent any

    stages {
        stage('Clone Git Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Williannca85/jk-freestyle-public-gh.git'
            }
        }
        
        stage('Building Image') {
            steps {
                sh '''
                    echo "🔨 Buildando imagem Docker..."
                    docker build -t webapp:${BUILD_NUMBER} .
                '''
            }
        }
        
        stage('Deploy Application') {
            steps {
                sh '''
                    echo "🚀 Parando container antigo..."
                    docker stop webapp_ctr 2>/dev/null || true
                    docker rm webapp_ctr 2>/dev/null || true
                    
                    echo "🐳 Executando novo container..."
                    docker run --rm -d -p 3000:3000 --name webapp_ctr webapp:${BUILD_NUMBER}
                    
                    echo "✅ Deploy concluído! Acesse: http://localhost:3000"
                '''
            }
        }
    }
    
    post {
        always {
            echo "🏁 Pipeline finalizado! Build #${BUILD_NUMBER}"
        }
        success {
            echo "✅ Pipeline executado com SUCESSO!"
        }
        failure {
            echo "❌ Pipeline falhou! Verifique os logs."
        }
    }
}