# Alta Foreman

## Descripción
El siguiente repositorio contiene:
- el **Playbook de Ansible** (playbooks + rol / roles) para la instalación de Oracle Internet Directory 12c en el entorno
- el **Dockerfile** para generar la imagen inmutable que contendrá dicho código Ansible junto con el runtime de ejecución Ansible
- el descriptor del Pod k8s **JenkinsAgentPod.yml** que pemite ejecutar el agente Jenkins como pod en Kubernetes con la imagen indicada
- el archivo **Jenkinsfile** que describe el pipeline para la ejecución de una instalación del producto indicado empleando la imagen y agente indicados y que establece los parámetros requeridos así como sus valores por defecto.

## Elementos

| Elemento                                       | Descripción                                                                                       |
|----------------------------------------------- |-------------------------------------------------------------------------------------------------- |
| **[Ansible](src/)**                            | *Playbook de Ansible* para instalación de Oracle Internet Directory 12c                           |
| **[Dockerfile](Dockerfile)**                   | Genera la *imagen docker* para el contenedor                                                      |
| **[Jenkinsfile](Jenkinsfile)**                 | Fichero para la tarea de la *PipeLine de Jenkins*                                                 |
| **[JenkinsAgentPod.yml](JenkinsAgentPod.yml)** | Fichero con la ejecución del pod del agente Jenkins en Kubernetes                                 |
| **[Tests](tests/)**                            | Por definir                                                                                       |

## Docker
El contenedor se forma mediante una sentencia **FROM** :

```
FROM ic-repos.redocu.lan:7090/jenkins-slave-ansible-molecule
```

## Jenkins Agent Pod
Para ejecución de agente Jenkins en cluster Kubernetes.
Dicho pod está descrito en el JenkinsAgentPod.yml

El pod está conformado por 2 contenedores:

| Contenedor                  | Descripción                                  |
| --------------------------- | -------------------------------------------- |
| **jnlp**                    | Para poder usar el agente desde Jenkins      |
| **idm-oid-12-install**      | Contiene el playbook necesario para instalar |

## Jenkinsfile

### Parámetros del pipeline

| Parámetro         | Por defecto    | Descripción                                          |
| ----------------- |--------------- |----------------------------------------------------- |
| **HOSTNAME**      | dummy.uxxi.lan | Nombre del host sobre el que ejecutar el playbook    |
| **OIDVERSION**    | 12.2.1.3.0     | Versión de Oracle Internet Directory a instalar      |

### Stages del pipeline

| #   | Tarea       | Descripción                                                                                                                                               |
| --- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Inventory   | Utiliza el método `createInventory` de *devops-pipeline-library* para generar el inventario de Ansible, introduciendo el hostname de destino y el usuario |
| 2   | Playbook    | Realiza la instalación de Oracle Internet Directory 12c (v.12) mediante Ansible                                                                           |

### Funciones y métodos usados en el pipeline

| Método                                                                                                                         | Argumentos | Valores                | Descripción |
| ------------------------------------------------------------------------------------------------------------------------------ | ---------- | -------                | ----------- |
| **[createInventory](http://git.ocu.es:8081/projects/DEVOPS/repos/devops-pipeline-library/browse/vars/uxxiDefsAnsible.groovy)** | hostname   | params.HOSTNAME        | *TBD*       |
| **[executePlaybook](http://git.ocu.es:8081/projects/DEVOPS/repos/devops-pipeline-library/browse/vars/uxxiDefsAnsible.groovy)** | extravars  | params.OIDVERSION, OID | *TBD*       |

### Tareas principales en Jenkins
| Tarea      | Rama | Directorio       | Jenkins Status                                                                                                                                                                                                 | Agente Jenkins                             |
|------------|------|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------- |
|**PlayBook**|master|[/src](src/)      |[![Build Status](http://ic-orq.redocu.lan/buildStatus/icon?job=iac%2Finstalar-sw%2Fplaybook-idm-oid-12-install)](http://ic-orq.redocu.lan/job/iac/job/instalar-sw/view/Ansible/job/playbook-idm-oid-12-install/)| [JenkinsAgentPod.yml](JenkinsAgentPod.yml) |
|**Testing** |master|[/tests](tests/)  |[![Build Status](http://ic-orq.redocu.lan/buildStatus/icon?job=IAC-ANSIBLE%2Fidm-oid-12-install%2Fmaster)](http://ic-orq.redocu.lan/job/IAC-ANSIBLE/job/idm-oid-12-install/job/master/)                         |*TBD*                                       |
|**Testing** |develop|[/tests](tests/) |[![Build Status](http://ic-orq.redocu.lan/buildStatus/icon?job=IAC-ANSIBLE%2Fidm-oid-12-install%2Fdevelop)](http://ic-orq.redocu.lan/job/IAC-ANSIBLE/job/idm-oid-12-install/job/develop/)                       |*TBD*                                       |