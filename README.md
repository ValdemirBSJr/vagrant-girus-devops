# Girus Projeto

![DevOps](https://img.shields.io/badge/DevOps-007BFF?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0iI2ZmZmZmZiI+PHBhdGggZD0iTTEyIDBDNS4zNzMgMCAwIDUuMzczIDAgMTJzNS4zNzMgMTIgMTIgMTIgMTItNS4zNzMgMTItMTJTMTguNjI3IDAgMTIgMHptMCAyMmMtNS41MjIgMC0xMC00LjQ3Ny0xMC0xMFM2LjQ3OCAyIDEyIDJzMTAgNC40NzcgMTAgMTAtNC40NzggMTAtMTAgMTB6bTAgME0xMiA0Yy00LjQxOCAwLTggMy41ODItOCg4czMuNTgyIDggOCA4IDgtMy41ODIgOC04LTMuNTgyLTgtOC04em0wIDE0LjRjLTMuNTI4IDAtNi40LTIuODcyLTYuNC02LjRzMi44NzItNi40IDYuNC02LjQgNi40IDIuODcyIDYuNCA2LjRTMUuNTI4IDE4LjQgMTIgMTguNHptMCAwTTEyIDdtNC40IDEuMmMwIDIuNzYxLTUgMi4yMzktNSA1LTUgMi43NjEtNSA1LTIuMjM5LTUgNS01IDUuNCAyLjIzOSA1LTUgMi43NjEtNSA1LTIuMjM5IDUtNXptMCA4LjhjLTEuNTQ2IDAtMi44LTEuMjU0LTIuOC0yLjhzMS4yNTQtMi44IDIuOC0yLjggMi44IDEuMjU0IDIuOCAyLjhTMTMuNTQ2IDE1UjEyIDE1Ljh6Ii8+PC9zdmc+)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Vagrant](https://img.shields.io/badge/Vagrant-1563FF?style=for-the-badge&logo=vagrant&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Hyper-V](https://img.shields.io/badge/Hyper--V-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)

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

### 🛠️ Tecnologias Utilizadas neste Laboratório
* **Vagrant**
* **Hyper-V**
* **VirtualBox**
* **Ubuntu**
* **Docker**
* **Girus**

  ## Artigo original demonstrando o uso:
[[https://www.linkedin.com/pulse/estude-devops-de-forma-gratu%C3%ADta-com-o-girus-da-valdemir-w85nf/?trackingId=IpRnWE%2FkQSWK%2F0K1qA0GYg%3D%3D](https://www.linkedin.com/pulse/estude-devops-de-forma-gratu%C3%ADta-com-o-girus-da-valdemir-w85nf/?trackingId=IpRnWE%2FkQSWK%2F0K1qA0GYg%3D%3D)]

## Contato

* **Valdemir Bezerra de Souza Júnior**
* Analista Infraestrutura | Devops | SRE | Cloud | Oracle Cloud | Linux | Docker | Kubernets | Python | Go | Rust | Lua | N8N | No Code
* [Linkedin](https://www.linkedin.com/in/valdemirbezerra/)
