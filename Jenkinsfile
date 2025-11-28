pipeline {
    agent any

    stages {
        // مرحلة التحضير وإنشاء البيئة الوهمية
        stage('Build & Install') {
            steps {
                echo 'Building and creating Virtual Environment...'
                // 1. إنشاء بيئة وهمية باسم venv
                bat 'python -m venv venv'
                
                // 2. تحديث pip داخل البيئة الوهمية
                bat 'venv\\Scripts\\python.exe -m pip install --upgrade pip'
                
                // 3. تثبيت المتطلبات داخل البيئة الوهمية
                bat 'venv\\Scripts\\pip.exe install -r requirements.txt'
            }
        }

        // مرحلة الاختبار
        stage('Test') {
            steps {
                echo 'Testing...'
                // نستخدم pytest الموجود داخل البيئة الوهمية
                bat 'venv\\Scripts\\pytest.exe --junitxml=test-results.xml'
            }
        }

        // مرحلة التشغيل
        stage('Run & Log') {
            steps {
                // تشغيل التطبيق باستخدام بايثون الموجود في البيئة الوهمية
                bat 'venv\\Scripts\\python.exe app.py > application.log'
            }
        }
    }

    // إجراءات ما بعد البناء
    post {
        always {
            junit 'test-results.xml'
            archiveArtifacts artifacts: 'application.log', allowEmptyArchive: true
        }
        success {
            echo 'Great! The build passed successfully.'
        }
        failure {
            echo 'Oops! The build failed.'
        }
    }
}