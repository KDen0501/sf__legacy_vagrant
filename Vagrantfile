Vagrant.configure("2") do |config|
  # Используем базовый образ Ubuntu 24.04
  config.vm.box = "ubuntu/focal64"
  config.vm.hostname = "postgres-vm"

  # Настраиваем публичную сеть (например, для доступа через IP)
  config.vm.network "public_network", type: "dhcp"

  # Настраиваем виртуальную машину
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end

  # Устанавливаем PostgreSQL 8.4
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y wget build-essential

    # Скачиваем и добавляем старый репозиторий
    wget https://ftp.postgresql.org/pub/source/v8.4.22/postgresql-8.4.22.tar.gz
    tar -xzf postgresql-8.4.22.tar.gz
    cd postgresql-8.4.22

    # Устанавливаем зависимости и собираем PostgreSQL 8.4
    sudo apt-get install -y libreadline-dev zlib1g-dev
    ./configure
    make
    sudo make install

    # Настройка системного пользователя для PostgreSQL
    sudo adduser --disabled-password --gecos "" postgres
    sudo mkdir /usr/local/pgsql/data
    sudo chown postgres /usr/local/pgsql/data

    # Инициализация базы данных и запуск сервера
    sudo su - postgres -c "/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data"
    sudo su - postgres -c "/usr/local/pgsql/bin/pg_ctl -D /usr/local/pgsql/data -l logfile start"

    # Добавляем PostgreSQL в PATH
    echo 'export PATH=/usr/local/pgsql/bin:$PATH' >> ~/.bashrc
    source ~/.bashrc
  SHELL
end
