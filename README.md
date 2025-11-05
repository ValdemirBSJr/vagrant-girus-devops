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

### Criando a VM com o Hyper-V

```bash
Vagrant.configure("2") do |config|
  # Usando a box "generic/ubuntu2204" que tem suporte a Hyper-V
  config.vm.box = "generic/ubuntu2204"
  
  # Força o vagrant a usar o Hyper-V
  config.vm.provider "hyperv"
  
  # Aloca mais memória para o Docker
  config.vm.provider "virtualbox" do |vb|
     vb.memory = "2048"
  end
  
  # Conecta a VM ao 'Default Switch' do Hyper-V
  config.vm.network "public_network", bridge: "Default Switch"
  
  # Encaminhamento de portas: frontend e backend
  config.vm.network "forwarded_port", guest: 8000, host: 8000
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  
  # Provisiona com shell para instalar e configurar o UFW
  config.vm.provision "shell", inline: <<-SHELL
    #!/bin/bash
    
	echo ""
    echo ">>> Iniciando provisionamento..."

    # Atualiza os pacotes
    apt-get update -y
    
    echo ">>> Instalando pré-requisitos do Docker..."
    # Instala pré-requisitos
    apt-get install -y \
        ca-certificates \
        curl \
        gnupg

    # Adiciona a chave GPG oficial do Docker
    install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    chmod a+r /etc/apt/keyrings/docker.gpg

    # Adiciona o repositório do Docker
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(. /etc/os-release && echo $VERSION_CODENAME) stable" | \
      tee /etc/apt/sources.list.d/docker.list > /dev/null

    # Atualiza os pacotes novamente após adicionar o novo repositório
    apt-get update -y

    echo ">>> Instalando o Docker Engine..."
    # Instala o Docker (versão mais recente)
    apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin net-tools

    echo ">>> Configurando permissões do Docker..."
	# Adiciona o usuário 'vagrant' ao grupo 'docker'
	usermod -aG docker vagrant
	newgrp docker
	echo ">>> Baixando e instalando o script GIRUS..."
	su - vagrant -c "curl -sSL girus.linuxtips.io | bash"
	
	echo ">>> [PATCH] Verificando e garantindo o port-forward do Girus..."
    
    # Roda o comando como o usuário 'vagrant' (que tem o config do kubectl)
    su - vagrant -c " \
      if ! sudo netstat -tulpn | grep -q ':8000.*kubectl' ; then \
        echo 'AVISO: port-forward do frontend (8000) não estava rodando. Iniciando manualmente...'; \
        nohup kubectl port-forward -n girus svc/girus-frontend 8000:80 --address 0.0.0.0 > /dev/null 2>&1 & \
      else \
        echo 'SUCESSO: port-forward do frontend (8000) já está ativo.'; \
      fi \
    "
	
	echo ""
	echo ">>> Provisionamento concluído!"
	echo "-----------------------------------------------"
    IP=$(hostname -I | awk '{print $1}')
    echo "Acesse a aplicação do Girus em: http://$IP:8000"
	
	echo " --- CONCLUÍDO! ---"
  SHELL
end
```

### Criando a VM com VirtualBox

```bash
Vagrant.configure("2") do |config|
  # Usa uma box compatível com VirtualBox
  config.vm.box = "generic/ubuntu2204"

  # Define o provider como VirtualBox (opcional, pois é o padrão)
  config.vm.provider "virtualbox" do |vb|
    vb.name = "girus-vm"
    vb.memory = "4096"  # Recomendado para Kubernetes + Docker
    vb.cpus = 2
  end

  # Encaminhamento de portas: frontend e backend
  config.vm.network "forwarded_port", guest: 8000, host: 8000
  config.vm.network "forwarded_port", guest: 8080, host: 8080

  # Provisionamento via shell
  config.vm.provision "shell", inline: <<-SHELL
    #!/bin/bash
    
	echo ""
    echo ">>> Iniciando provisionamento..."

    # Atualiza os pacotes
    apt-get update -y

    echo ">>> Instalando pré-requisitos do Docker..."
    apt-get install -y ca-certificates curl gnupg

    # Adiciona chave GPG do Docker
    install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    chmod a+r /etc/apt/keyrings/docker.gpg

    # Adiciona repositório do Docker
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo $VERSION_CODENAME) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

    apt-get update -y

    echo ">>> Instalando Docker Engine e plugins..."
    apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin net-tools

    # Adiciona usuário vagrant ao grupo docker
    usermod -aG docker vagrant

    echo ">>> Baixando e instalando o Girus..."
    su - vagrant -c "curl -sSL https://girus.linuxtips.io | bash"

    echo ">>> Garantindo que o port-forward do frontend esteja ativo..."
    su - vagrant -c "
      if ! pgrep -f 'kubectl.*8000.*girus-frontend' > /dev/null; then
        echo 'Iniciando port-forward do frontend (8000)...'
        nohup kubectl port-forward -n girus svc/girus-frontend 8000:80 --address 0.0.0.0 > /home/vagrant/girus-frontend.log 2>&1 &
      else
        echo 'Port-forward do frontend já está em execução.'
      fi
    "

    echo ">>> Garantindo que o port-forward do backend esteja ativo..."
    su - vagrant -c "
      if ! pgrep -f 'kubectl.*8080.*girus-backend' > /dev/null; then
        echo 'Iniciando port-forward do backend (8080)...'
        nohup kubectl port-forward -n girus svc/girus-backend 8080:80 --address 0.0.0.0 > /home/vagrant/girus-backend.log 2>&1 &
      else
        echo 'Port-forward do backend já está em execução.'
      fi
    "

    echo ">>> Provisionamento concluído!"
  SHELL
end
```

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
