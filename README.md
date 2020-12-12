# Plataforma de analítica de datos de COVID-19
Durante la pandemia COVID-19 el trabajo de Universidad de Johns Hopkins se ha vuelto de vital importancia para mantener a las personas informadas sobre el progreso del virus en sus comunidades, en sus países y en el mundo.
Johns Hopkins publica los datos gratuitamente para que cualquiera los use.  Estos datos se entregan como archivos CSV que se deben descargar para luego consultar.  Hacer que los datos actualizados sean más accesibles ayudará a que las personas puedan crear análisis y aplicaciones directamente sobre el conjunto de datos. 
En este contexto se propone la construcción de una plataforma analítica de datos, la cual mediante Vagrant permite la instalación de un entorno Multi Machine para la descarga, manipulación y visualización de datos de COVID-19. La plataforma necesita de la instalación de los siguientes elementos: 

- Rundeck
- Jupyter
- MongoBD
Donde Jupyter y MongoBD estarán instalados en VM1y Rundeck en VM2.

### Prerrequisitos de software 
Previamente tener instalado Vangrant y VirtualBox en el Host.
Vagrant (https://www.vagrantup.com/)
VirtualBox (https://www.virtualbox.org/ )

### Instrucciones 
1. Utilice este repositorio en su ordenador y ubíquese en el directorio. Es fundamental que usted tenga en su directorio de trabajo los siguientes archivos o scripts: 
- Vagrantfile
- download-latest-JHU
- script
- script2

2. Inicie la configuración de las máquinas virtuales con vagrant up. La instalación y configuración de las máquinas virtuales puede tarde unos minutos. 

3. Terminado el paso 2 usted contará con un entorno MultiMachine donde tendrá dos máquinas virtuales descritas a continuación: 
- MV Jupyter : máquina virtual con Jupyter y MongoBD. (Para ingresar: vagrant ssh Jupyter ) 
- MV Rundeck: máquina virtual con Rundeck. (Para ingresar: vagrant ssh Rundeck.) 

4. Ingrese a la máquina virtual Jupyter (vagrant ssh Jupyter) y ejecute el siguiente script en bash que le permitirá importar los archivos CSV hacia MongoBD.

```
#!/usr/bin/env bash
exit_code=0
mongo "${1}" --quiet --eval "db.dropDatabase()"
exit_code=$((exit_code + $?))

for file in jhu/csse_covid_19_data/csse_covid_19_daily_reports/10-*.csv; do
mongoimport --uri "${1}" --collection daily --type csv --headerline --file "${file}" &
exit_code=$((exit_code + $?))
done
wait

echo "$0 finished with code $exit_code."
exit $exit_code

```



