Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"  # Используем Ubuntu 18.04

  config.vm.provision "shell", inline: <<-SHELL
    # Установка необходимых пакетов
    sudo apt-get update
    sudo apt-get install -y wget gnupg2
    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
    echo "deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main" | sudo tee /etc/apt/sources.list.d/pgdg.list
    sudo apt-get update
    sudo apt-get install -y postgresql-12 postgresql-client-12

    # Запуск службы PostgreSQL
    sudo service postgresql start

    # Настройка PostgreSQL
    sudo -u postgres psql -c "CREATE USER vagrant WITH PASSWORD 'vagrant';"
    sudo -u postgres psql -c "CREATE DATABASE vagrantdb WITH OWNER vagrant;"
  SHELL
end
