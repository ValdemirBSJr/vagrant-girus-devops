# Girus Projeto
## Descrição
Este projeto utiliza o Vagrant para criar máquinas virtuais (VMs) com Ubuntu, configuradas para rodar o Girus. O projeto suporta dois provedores: Hyper-V e VirtualBox.

## Principais Conceitos
* **Vagrant**: Ferramenta para criar e gerenciar ambientes de desenvolvimento virtualizados.
* **Girus**: Aplicação que será executada nas VMs criadas pelo Vagrant.
* **Hyper-V** e **VirtualBox**: Provedores de virtualização suportados pelo projeto.
* **Docker**: Utilizado para instalar e configurar o Girus nas VMs.

## Como usar
### Pré-requisitos
* Ter o Vagrant instalado no sistema.
* Ter o Hyper-V ou VirtualBox instalado e configurado.

### Subir as VMs
1. Clone o repositório do projeto.
2. Navegue até a pasta do projeto no terminal.
3. Execute o comando `vagrant up` para criar e iniciar as VMs.
4. Se estiver usando o Hyper-V, vá para a VM e execute o comando `hostname -I` para obter o endereço IP.
5. Acesse a aplicação do Girus em `http://<endereço_IP>:8000`.

### Parar as VMs
1. Execute o comando `vagrant halt` para parar as VMs.

## Contato
Para mais informações ou para relatar problemas, por favor, utilize a seção de issues do repositório do projeto.

## Tecnologias
* **Vagrant**
* **Hyper-V**
* **VirtualBox**
* **Ubuntu**
* **Docker**
* **Girus**