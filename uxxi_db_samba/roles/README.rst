Se crea playbook para la configuración de samba, el playbook lo dejo con varias opciones algunas comentadas si necesitan.
1 Comprueba si esta instalado samba  y genera un aviso si esta o no instalado.
2 Si no esta instalado instala la ultima versión y configura el samba.
3 Si esta instalado configura el samba.
4 Reinicia el samba.
opciones comentadas
5 Crea los usuario uxxi uxxidb
6  Habilitad los usuarios en el samba 
esta opciones depende de un archivo encriptado por tener el pasword de los usuarios uxxi y uxxidb, si se desea esta función se debe moverse el archivo a la raíz de default.

[root@serverforeman /etc/ansible/roles/uxxi_db_samba/roles ]# ls -la
total 0
drwxr-xr-x 6 root root 68 May 18 15:28 .
drwxr-xr-x 3 root root 58 May 18 15:56 ..
drwxr-xr-x 3 root root 16 May 18 16:15 defaults
drwxr-xr-x 2 root root 22 May 18 15:28 handlers
drwxr-xr-x 2 root root 79 May 18 16:55 tasks
drwxr-xr-x 2 root root 22 May 18 16:24 templates


[root@serverforeman /etc/ansible/roles/uxxi_db_samba/roles ]# ls -la defaults/bk/
total 8
drwxr-xr-x 2 root root   22 May 18 16:34 .
drwxr-xr-x 3 root root   16 May 18 16:15 ..
-rw-r--r-- 1 root root 7483 May 18 16:13 main.yml

#normal
ansible-playbook -i /etc/ansible/inventory/prueba /etc/ansible/roles/uxxi_db_samba/linux-smb.yml


#con el archivo defaul ansible vault
ansible-playbook -i /etc/ansible/inventory/prueba /etc/ansible/roles/uxxi_db_samba/linux-smb.yml --ask-vault-pass


