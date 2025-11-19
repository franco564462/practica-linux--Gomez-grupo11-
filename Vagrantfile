Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.hostname = "linux-practice"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 2

    # Crear disco adicional de 2GB si no existe
    unless File.exist?('./disk1.vdi')
      vb.customize ['createhd', '--filename', './disk1.vdi', '--size', 2048]
    end

    # Adjuntar disco
    vb.customize ['storageattach', :id, '--storagectl', 'SCSI',
                  '--port', 2, '--device', 0, '--type', 'hdd', '--medium', './disk1.vdi']
  end

  config.vm.network "public_network"

  config.vm.provision "shell", inline: <<-SHELL
    # Actualizar repos
    apt-get update -y
    apt-get upgrade -y

    # --- Instalar FASTFETCH desde el PPA oficial ---
    add-apt-repository ppa:zhangsongcui3371/fastfetch -y
    apt-get update -y
    apt-get install -y fastfetch

    # --- Instalar dependencias ---
    apt-get install -y git lvm2 ca-certificates curl gnupg

    # --- Instalar Docker desde repositorio oficial ---
    install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    chmod a+r /etc/apt/keyrings/docker.asc

    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
      https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
      > /etc/apt/sources.list.d/docker.list

    apt-get update -y
    apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

    # Agregar usuario vagrant al grupo Docker
    usermod -aG docker vagrant

    # Habilitar Docker
    systemctl enable docker
    systemctl start docker
  SHELL
end