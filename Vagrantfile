#####################
# CUIDADO, HYPER-V! #
#####################

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
