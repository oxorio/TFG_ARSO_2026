# Declaramos un array con la configuración de cada VM, incluyendo su nombre e IP fija en la red privada. 
# Esto facilita la gestión y escalabilidad del entorno, permitiendo agregar o modificar máquinas simplemente editando este array.
VMS = [
  { name: "vm-proxy", ip: "10.0.0.10" },
  { name: "vm-blue",  ip: "10.0.0.11" },
  { name: "vm-green", ip: "10.0.0.12" }
]

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04" # Usamos una imagen base de Ubuntu 22.04 LTS, que es estable y ampliamente compatible con Ansible y Docker
  config.vm.box_check_update = false
  VMS.each do |vm|
    config.vm.define vm[:name] do |node|
      node.vm.hostname = vm[:name]

      # Configuración de red del clúster:
      # - vm-proxy actúa como el nodo de entrada y dispone de interfaz pública.
      # - vm-blue y vm-green usan la red privada para la comunicación interna.
      # - Todas las VM tienen una IP fija en la red privada para facilitar el enrutamiento.
      # Red privada para el clúster
      node.vm.network "private_network", ip: vm[:ip], netmask: "255.255.255.0"
      if vm[:name] == "vm-proxy"
        # Interfaz pública solo en el proxy para acceso desde la red externa
        node.vm.network "public_network", bridge: "en0: Wi-Fi"
      end
    
      node.vm.provider "vmware_desktop" do |v|
        v.memory = 1024 # Asignamos 1GB de RAM a cada VM para asegurar un rendimiento adecuado sin sobrecargar el host
        v.cpus   = 2  # Asignamos 2 CPUs a cada VM para permitir un procesamiento fluido, especialmente al ejecutar Nginx y Docker
        v.gui    = false # Desactivamos la interfaz gráfica para optimizar el rendimiento, ya que no es necesaria para este entorno de pruebas basado.
      end

      # Configura el prompt de bash personalizado y establece el hostname de la VM
      node.vm.provision "shell", inline: <<-SHELL
        echo 'PS1="\\[\\033[1;32m\\]#{vm[:name]}\\[\\033[0m\\]@\\h:\\w\\$ "' >> /home/vagrant/.bashrc
        hostnamectl set-hostname #{vm[:name]} 
      SHELL

        # Llamada a Ansible para la gestión de la configuración de cada máquina
        node.vm.provision "ansible" do |ansible|
          ansible.playbook = "playbook.yml"

    end
  end
end