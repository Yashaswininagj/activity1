pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Yashaswininagj/activity1.git'
            }
        }

        stage('Setup Python Virtual Environment') {
            steps {
                sh '''
                echo "yourpassword" | sudo -S apt update
                sudo apt install -y python3-venv
                python3 -m venv venv
                '''


                // sh '''
                // sudo apt update
                // sudo apt install -y python3-venv
                // python3 -m venv venv
                // '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                # Use bash explicitly
                bash -c "source venv/bin/activate && pip install -r app/requirements.txt"
                '''
            }
        }

        stage('Run Ansible Deployment') {
            steps {
                sh '''
                echo "Running Ansible Playbook..."
                ansible-playbook -i ansible-playbooks/inventory.ini ansible-playbooks/deploy.yml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                echo "Checking Flask App Deployment..."
                curl -v --retry 5 --retry-delay 10 http://54.209.24.19:5000 || echo "Flask App is not responding!"
                '''
            }
        }
    }
}
