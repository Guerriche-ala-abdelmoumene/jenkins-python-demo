pipeline {
    agent any

    environment {
        // لاحظ: استخدمنا شرطتين (\\) بدلاً من واحدة لأن لغة Groovy تتطلب ذلك
        // ولاحظ أننا سنستخدم علامات التنصيص لاحقاً للتعامل مع المسافة في الاسم
        PYTHON_PATH = 'C:\\Users\\Alla Guerriche\\AppData\\Local\\Programs\\Python\\Python311\\python.exe'
    }

    stages {
        stage('Build & Install') {
            steps {
                echo 'Building and creating venv...'
                
                // 1. إنشاء البيئة الوهمية
                // نضع المتغير بين علامات اقتباس مزدوجة "%...%" لكي يفهم النظام الاسم الذي يحتوي على مسافة
                bat '"%PYTHON_PATH%" -m venv venv'
                
                // 2. تحديث pip داخل البيئة الوهمية (نستدعي python الموجود داخل venv الجديد)
                bat 'venv\\Scripts\\python.exe -m pip install --upgrade pip'
                
                // 3. تثبيت المتطلبات
                bat 'venv\\Scripts\\pip.exe install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
                // تشغيل الاختبارات
                bat 'venv\\Scripts\\pytest.exe --junitxml=test-results.xml'
            }
        }

        stage('Run & Log') {
            steps {
                // تشغيل التطبيق وحفظ المخرجات
                bat 'venv\\Scripts\\python.exe app.py > application.log'
            }
        }
    }

    post {
        always {
            junit 'test-results.xml'
            archiveArtifacts artifacts: 'application.log', allowEmptyArchive: true
        }
        success {
            echo 'Build Passed Successfully!'
        }
    }
}