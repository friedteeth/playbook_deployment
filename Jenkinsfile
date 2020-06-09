pipeline{
	agent any
	stages{
	    stage('Ejecucion de playbook de ansible'){
			steps{
			    sh "ansible-playbook /var/lib/jenkins/playbook_deployment/django_apache_deployment.yml -e 'ansible_python_interpreter=/usr/bin/python3'"
			}
		}
		stage('Activacion de entorno virtual'){
			steps{
				sh ". /envs/scatuaz/bin/activate"
			}
		}
		stage('Verifiacion de codigo y complejidad'){
			steps{
				sh "flake8 --exclude=*migrations*,*settings* ."
				sh "flake8 --max-complexity=1 dinosaurios/archivo.py"		
			}
		}
		stage('Testing'){
			steps{
				echo "Ejecución de pruebas unitarias"
				echo "Ejecución de pruebas de aceptación"
				echo "Verfica coverage"
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
