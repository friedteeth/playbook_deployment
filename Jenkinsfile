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
				. /envs/scatuaz/bin/activate
				flake8 --exclude=*migrations*,*settings*,*pruebas_aceptacion* --max-complexity=5 .
				"""
			}
		}
		stage('Pruebas unitarias y coverage'){
			steps{
				sh """
				. /envs/scatuaz/bin/activate sudo coverage run --source='.' --omit=*migrations*,*__init__*,*test*,*apps* /repos/scatuaz/manage.py test
				cd /repos/scatuaz/
				coverage report
				"""
			}
		}
		stage('Pruebas de aceptacion'){
			steps{
				sh """
				. /envs/scatuaz/bin/activate
				python /repos/scatuaz/manage.py runserver 0:8000 >& /dev/null &
				echo \$! >> my_process.log
				Xvfb :0 >& /dev/null &
				echo \$! >> my_process.log
				export DISPLAY=:0
				cd /repos/scatuaz/pruebas_aceptacion/
				behave
				kill \$(cat my_process.log)
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
