pipeline{
	agent any
	stages{
	    stage('Ejecucion de playbook de ansible'){
			steps{
			    sh "ansible-playbook /var/lib/jenkins/playbook_deployment/django_apache_deployment.yml -e 'ansible_python_interpreter=/usr/bin/python3'"
			}
		}
		stage('Verifica estandar de codigo y complejidad ciclomática'){
			steps{
				sh """
				. /envs/scatuaz/bin/activate
				flake8 --exclude=*migrations*,*settings*,*pruebas_aceptacion* --max-complexity=5 .
				"""
			}
		}
		stage('Testing'){
			steps{
				sh """
				. /envs/scatuaz/bin/activate
				python manage.py test trabajador/tests
				python manage.py test usuario/tests
				python manage.py test login/tests
				"""
			}
		}
		
		stage('Servidor de pre-producción'){
			steps{
				echo "Copia la aplicación al servidor de pruebas"
			}
		}
		stage('Servidor de producción'){
			steps{
				echo "Copia la aplicación al servidor de pruebas"
			}
		}
	}
}
