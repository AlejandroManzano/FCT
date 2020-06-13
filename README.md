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


