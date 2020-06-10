pipeline{
	agent any
	stages{
	    stage('Preparacion de sistema'){
			steps{
			    echo "Proyecto instalado"
			}
		}
		stage('Verifica estandar de codigo y complejidad ciclomática'){
			steps{
				sh """
				. /var/lib/jenkins/env/bin/activate
				cd /var/lib/jenkins/scatuaz
				flake8 --exclude=*migrations*,*settings*,*pruebas_aceptacion*,*test* --max-complexity=5 .
				"""
			}
		}
		stage('Pruebas unitarias y coverage'){
			steps{
				sh """
				. /var/lib/jenkins/env/bin/activate
				cd /var/lib/jenkins/scatuaz
				coverage run --source='.' --omit=*migrations*,*__init__*,*test*,*apps* manage.py test
				coverage report
				"""
			}
		}
		stage('Pruebas de aceptacion'){
			steps{
				sh '''
				. /var/lib/jenkins/env/bin/activate
				cd /var/lib/jenkins/scatuaz
				python manage.py runserver 0:8000 &> /dev/null &
				echo $! > my_process.log
				Xvfb :0 &> /dev/null &
				echo $! >> my_process.log
				export DISPLAY=:0
				cd /var/lib/jenkins/scatuaz/pruebas_aceptacion
				behave
				
				'''
			}
		}
		stage('Servidor de producción'){
			steps{
				sh """
				cd /var/lib/jenkins/
				rm -rf playbooks
				git clone https://github.com/friedteeth/playbook_deployment.git playbooks
				ansible-playbook /var/lib/jenkins/playbooks/django_apache_deployment.yml -e \\'ansible_user=ubuntu ansible_host=18.225.32.191\\'
				"""
			}
		}
	}
}
