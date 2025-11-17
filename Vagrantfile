Vagrant.configure("2") do |config|
	config.vm.box = "hashicorp/precise64"
	config.vm.box_version = "1.1.0"
	config.vm.hostname = "pg84"
	config.vm.network "private_network", ip: "192.168.56.84"

	config.vm.provision "shell", inline: <<-SHELL
	set -eux

	export DEBIAN_FRONTEND=noninteractive
	export TERM=linux

	# Add old-releases repo
	sudo sh -c 'printf "%s\n" "deb http://old-releases.ubuntu.com/ubuntu/ precise main universe multiverse restricted" > /etc/apt/sources.list'

	sudo apt-get update -y

	# Install deps
	sudo apt-get install -y --no-install-recommends wget ca-certificates debconf-utils

	# Deal with "obsolete major version" thing
	echo "postgresql-common postgresql-common/obsolete-major select ignore" | sudo debconf-set-selections

	# Install PostgreSQL 8.4 | non-interactive
	sudo apt-get -y -o Dpkg::Options::="--force-confdef" \
					-o Dpkg::Options::="--force-confold" \
					install postgresql-8.4 || true

	# Configure packages, fix dependencies
	sudo dpkg --configure -a || true
	sudo apt-get -f install -y || true

	# Report status
	if sudo service postgresql status >/dev/null 2>&1; then
	echo "postgresql service appears to be running"
	else
	echo "postgresql service not running (check logs)"
	sudo journalctl -u postgresql --no-pager -n 200 || true
	fi
	SHELL
	end
