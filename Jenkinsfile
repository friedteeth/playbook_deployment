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
		stage('Pruebas unitarias'){
			steps{
				sh """
				. /envs/scatuaz/bin/activate
				python /repos/scatuaz/manage.py test /repos/scatuaz/trabajador/tests
				python /repos/scatuaz/manage.py test /repos/scatuaz/usuario/tests
				python /repos/scatuaz/manage.py test /repos/scatuaz/login/tests
				
				"""
			}
		}
		stage('Pruebas de coverage para modulo trabajador'){
			steps{
				sh """
				. /envs/scatuaz/bin/activate
				cd /repos/scatuaz
				coverage erase
				coverage run --source='.' --omit=*migrations*,*__init__*,*test*,*apps* manage.py test trabajador/tests
				coverage report
				"""
		}
		// }
		// stage('Pruebas de coverage para modulo usuario'){
		// 	steps{
		// 		sh """
		// 		. /envs/scatuaz/bin/activate
		// 		cd /repos/scatuaz
		// 		coverage erase
		// 		coverage run --source='.' --omit=*migrations*,*__init__*,*test*,*apps* manage.py test usuario/tests
		// 		coverage report
		// 		"""
		// 	}
		// }
		// stage('Pruebas de coverage para modulo login'){
		// 	steps{
		// 		sh """
		// 		. /envs/scatuaz/bin/activate
		// 		cd /repos/scatuaz
		// 		coverage erase
		// 		coverage run -a --source='.' --omit=*migrations*,*__init__*,*test*,*apps* manage.py test login/tests
		// 		coverage report
		// 		"""
		// 	}
		// }
		stage('Pruebas de aceptacion'){
			steps{
				sh """
				. /envs/scatuaz/bin/activate
				mv /repos/scatuaz/pruebas_aceptacion/features/environment.py /repos/scatuaz/pruebas_aceptacion/features/.firefox_environment.py
				mv /repos/scatuaz/pruebas_aceptacion/features/.chrome_headless_environment.py /repos/scatuaz/pruebas_aceptacion/features/environment.py
				python manage.py runserver 0:8000 >& /dev/null &
				Xvfb :0 >& /dev/null &
				export DISPLAY=:0
				cd /repos/scatuaz/pruebas_aceptacion/

				behave
				"""
			}
		}
		stage('Servidor de producción'){
			steps{
				echo "Copia la aplicación al servidor de pruebas"
			}
		}
	}
}
