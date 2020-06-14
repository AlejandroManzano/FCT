# FCT
En este proyecto se utilizará SSH, Vagrant, Ansible y Terraform.

# PRE-REQUISITOS
Para realizar este proyecto se necesita instalar Vagrant para lanzar las máquinas virtuales y Virtualbox que será el proveedor de Vagrant en este proyecto.

https://www.vagrantup.com/downloads.html

https://www.virtualbox.org/wiki/Downloads


# USO DE VAGRANT

Los Vagrantfile que se encuentran en las carpetas del proyecto describe el tipo de máquina virtual, cómo se configura y cómo se provisiona.

Para lanzar las máquinas virtuales debemos ejecutar el siguiente comando dentro los directorios donde se encuentran los Vagrantfile.

- vagrant up 

El archivo config-ansible dentro del directorio vagrant-ansible instalará ansible al ser lanzada con el comando de vagrant.

Para acceder a la máquina virtual de ansible que será el controlador ejecutamos el comando:

- ssh vagrant@192.168.33.10

La contraseña por defecto para el usuario vagrant es vagrant

Tras conectarnos hay que añadir en el fichero /etc/ansible/hosts las IPs de todas las máquinas que se vayan a utilizar para probar la utilidad de Ansible. Una vez hecho esto, es recomendable hacer un ping para comprobar la conexión.

Algunos comando importantes para controlar las máquinas virtuales con vagrant son:
- vagrant reload Recarga la configuración de Vagrantfile
- vagrant suspend No se para la máquina, se almacena el estado y cuando se arranca se vuelve donde se quedó.
- vagrant halt Parar la máquina (shutdown)
- vagrant destroy Para y elimina todos los recursos

# Configuración de SSH
Primero debemos acceder a las máquinas con el comando ssh vagrant@ip y cambiar la contraseña del root:
- sudo su 
- passwd

Es importante comprobar el fichero known_host (/home/usuario/.ssh) si tenemos problemas para acceder a las máquinas.

Una vez añadida la contraseña, acceder al fichero /etc/ssh/sshd_config como root y poner la línea "permitrootlogin" a yes para poder entrar a la máquina por ssh como root y reiniciar el servicio SSH también como sudo o añadir antes del comando el sudo.

sudo nano /ect/ssh/sshd_config
service ssh restart

Una vez realizado este paso, desde la máquina de ansible, se creará un par de claves (pública y privada) para poder copiarlas en los demás equipos (tanto al usuario root como a vagrant). Para ello se usan los siguientes comandos:
- ssh-keygen -t rsa
- ssh-copy-id root@IP 
- ssh-copy-id vagrant@IP

# USO DE ANSIBLE

Ansible te permite automatizar el aprovisionamiento de software a gestionar y la configuración de máquinas virtuales. En este infraestructura el controlador será "vagrant-ansible" y los nodo "vagrant-node".

Para comprobar que se puede realizar cualquier acción con ansible sobre estas máquinas se podría usar el siguiente comando para ver el nombre del host.
En caso de comprobar en todos los equipos que estén en el fichero /etc/ansible/hosts a la vez:

- ansible all -u root -a "hostname"

Comprobar en un equipo en concreto:

- ansible IP -u root -a "hostname"

Con la opción -a se puede ejecutar cualquier comando y con el -m se llama a los módulos de ansible -m apt. 
ansible [all o IP] -u root -m apt -a "name=servicio state=latest"


En este caso vamos utilizar los playbooks de Ansible que nos permite orquestar y organizar unas tareas a un grupo de hosts en este caso sería las maquinas "vagrant-node", se instalará los servicios de apache2 y ngnix lamando a los playbooks que están en el directorio /home/vagrant/projects de la máquina de ansible:

- ansible-playbook nombre_fichero_playbook.yaml


Una vez instalados los servicios apache2 y nginx se puede comprobar en el navegador con las IPs de las máquinas para comprobar que se han instalado correctamente.

Para desinstalar un servicio se puede hacer con el siguiente comando:

- ansible [all o IP] -u root -m apt -a "name=servicio state=absent"



