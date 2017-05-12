# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"

  # config.vm.box_check_update = false

  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.21.77"

  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
     vb.memory = "256"
  end

  config.vm.hostname ="openldap"


  config.vm.provision "shell", inline: <<-SHELL
      sed -i s/^proxy/#proxy/ /etc/yum.conf
      yum -y -q install vim openldap* migrationtools
      systemctl stop slapd
      echo "olcRootPW: `slappasswd -s jfrog`" >> /etc/openldap/slapd.d/cn\=config/olcDatabase\=\{2\}hdb.ldif
      sed -i s/my-domain/jfrog/g /etc/openldap/slapd.d/cn\=config/olcDatabase\=\{2\}hdb.ldif
      sed -i s/my-domain/jfrog/g  /etc/openldap/slapd.d/cn\=config/olcDatabase\=\{1\}monitor.ldif
      cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
      chown -R ldap:ldap /var/lib/ldap/
      systemctl enable slapd
      systemctl start slapd
      ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
      ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
      ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
      ldapadd -Y EXTERNAL -H ldapi:/// -f /vagrant/memberof.ldif
      ldapadd -D  cn=Manager,dc=jfrog,dc=com -w jfrog -f /vagrant/root.ldif
      ldapadd -D  cn=Manager,dc=jfrog,dc=com -w jfrog -f /vagrant/users.ldif
      ldapadd -D  cn=Manager,dc=jfrog,dc=com -w jfrog -f /vagrant/groupofnames.ldif
      ldapsearch -b dc=jfrog,dc=com -x memberof 
      cat /vagrant/motd > /etc/motd
      cp /vagrant/memo_ldap.txt /root/
    SHELL
end
