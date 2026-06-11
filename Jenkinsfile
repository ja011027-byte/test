pipeline {
    agent any
    stages {
        // 1. 빌드 시작 전 기존 리포트 폴더를 깨끗하게 지우는 단계를 추가합니다.
        stage('Clean Old Reports') {
            steps {
                bat 'rmdir /s /q newman 2>nul || exit 0'
                bat 'rmdir /s /q Newman_20Reports 2>nul || exit 0'
            }
        }
        
        // 기존에 있던 다른 stage들 (예: Test, Run 등)은 이 아래에 그대로 둡니다.
        stage('Run Tests') {
            steps {
                // ... 기존 코드 ...
            }
        }
    }
}

    stage('Run Postman Collections') {
      steps {
        // 환경/컬렉션 파일이 리포지토리 루트에 있다고 가정
        bat 'newman run Login_API_Test.postman_collection.json -e GameQA_ENV.postman_environment.json -r cli,htmlextra --reporter-htmlextra-export newman\\Login_Report.html --export-environment GameQA_ENV.postman_environment.json'
        bat 'newman run Ranking_API_Test.postman_collection.json -e GameQA_ENV.postman_environment.json -r cli,htmlextra --reporter-htmlextra-export newman\\Ranking_Report.html'
        bat 'newman run Item_Grant_API_Test.postman_collection.json -e GameQA_ENV.postman_environment.json -r cli,htmlextra --reporter-htmlextra-export newman\\ItemGrant_Report.html'
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'newman/*.html', fingerprint: true
      // HTML Publisher 플러그인이 설치되어 있다면 아래 주석 해제
      publishHTML(target: [
      reportDir: 'newman',
      reportFiles: 'Login_Report.html,Ranking_Report.html,ItemGrant_Report.html',
      reportName: 'Newman Reports',
      keepAll: true,
      alwaysLinkToLastBuild: true
       ])
    }
  }
}
