pipeline {
    agent any

    stages {
        // مرحلة التحضير (Build/Install)
        stage('Build & Install') {
            steps {
                echo 'Building...'
                // نستخدم 'bat' بدلاً من 'sh' لأننا على Windows
                bat 'pip install -r requirements.txt'
            }
        }

        // مرحلة الاختبار (Test) وتوليد التقارير
        stage('Test') {
            steps {
                echo 'Testing...'
                // تشغيل الاختبارات وحفظ النتائج
                bat 'pytest --junitxml=test-results.xml'
            }
        }

        // مرحلة التشغيل وتسجيل اللوج (Log)
        stage('Run & Log') {
            steps {
                // تشغيل التطبيق وحفظ المخرجات
                bat 'python app.py > application.log'
            }
        }
    }

    // مرحلة ما بعد البناء
    post {
        always {
            // نشر نتائج الاختبار
            junit 'test-results.xml'
            
            // حفظ ملفات اللوج
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