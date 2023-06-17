# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.vm.provider 'virtualbox' do |v|
    v.memory = 4096
    v.cpus   = 4
  end

  config.vm.network "private_network", type: "dhcp"

  config.vm.define "Ubuntu" do |ubuntu|
    ubuntu.vm.box = 'ubuntu/trusty64'
  end

  # config.vm.define "SmartOS" do |smartos|
  #   smartos.vm.box = 'livinginthepast/smartos-base64'
  #   smartos.vm.network "private_network", ip: "33.33.33.10"
  #   smartos.vm.communicator = 'smartos'
  #   smartos.ssh.insert_key = false
  #   smartos.global_zone.platform_image = 'latest'
  #   smartos.zone.name = 'base64'
  #   smartos.zone.brand = 'joyent'
  #   smartos.zone.image = '0edf00aa-0562-11e5-b92f-879647d45790'
  #   smartos.zone.memory = 4096
  #   smartos.zone.disk_size = 20
  # end

  microbox_user = ENV["MICROBOX_USER"]
  microbox_base_project = ENV["MICROBOX_BASE_PROJECT"] || "base"
  microbox_gomicro_project = ENV["MICROBOX_GOMICRO_PROJECT"] || "gomicro"
  microbox_base_secret = ENV["MICROBOX_#{microbox_user.upcase}_#{microbox_base_project.upcase}_SECRET"]
  microbox_gomicro_secret = ENV["MICROBOX_#{microbox_user.upcase}_#{microbox_gomicro_project.upcase}_SECRET"]

  config.vm.synced_folder "../distfiles", "/content/distfiles", type: "nfs"
  config.vm.synced_folder "../packages", "/content/packages", type: "nfs"
  config.vm.synced_folder "../pkgsrc-lite", "/content/pkgsrc", type: "nfs"
  config.vm.synced_folder "../pkgsrc-base", "/content/pkgsrc/base", type: "nfs"
  config.vm.synced_folder "../pkgsrc-gomicro", "/content/pkgsrc/gomicro", type: "nfs"

  $script = <<-SCRIPT
  echo # Vagrant environment variables > /etc/profile.d/vagrant.sh
  echo export MICROBOX_USER=#{microbox_user} >> /etc/profile.d/vagrant.sh
  echo export MICROBOX_BASE_PROJECT=#{microbox_base_project} >> /etc/profile.d/vagrant.sh
  echo export MICROBOX_BASE_SECRET=#{microbox_base_secret} >> /etc/profile.d/vagrant.sh
  echo export MICROBOX_GOMICRO_PROJECT=#{microbox_gomicro_project} >> /etc/profile.d/vagrant.sh
  echo export MICROBOX_GOMICRO_SECRET=#{microbox_gomicro_secret} >> /etc/profile.d/vagrant.sh
  echo umask 022 >> /etc/profile.d/vagrant.sh
  chmod +x /etc/profile.d/vagrant.sh
  SCRIPT

  config.vm.provision "shell", inline: $script

end
