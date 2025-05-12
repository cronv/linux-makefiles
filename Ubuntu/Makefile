### Telegram ###
TELEGRAM_URL = https://telegram.org/dl/desktop/linux
INSTALL_DIR = $(TEMP_DIR)/Telegram
ICON_FILE = org.telegram.desktop

### PhpStorm ###

### WG ###
WG_CONF := $(HOME)/default.conf

docker-all: docker-install-repo docker-install docker-install-rules

telegram-all: telegram-install telegram-install-clean telegram-install-lnk

firefox-all: firefox-dev-install-ppa

phpstorm-all: phpstorm-install phpstorm-install-lnk phpstorm-reset-trial

docker-install-repo:
	@TEMP_DIR='/tmp/docker-install'; \
	echo "Updating the package list..."; \
	sudo apt-get update; \
	echo "Installing the required packages..."; \
	sudo apt-get install -y ca-certificates curl; \
	echo "Creating a temporary directory..."; \
	sudo install -m 0755 -d $$TEMP_DIR; \
	echo "Adding the Docker GPG key..."; \
	sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o $$TEMP_DIR/docker.asc; \
	sudo mv $$TEMP_DIR/docker.asc /etc/apt/keyrings/; \
	sudo chmod a+r /etc/apt/keyrings/docker.asc; \
	echo "Adding the Docker repository to the Apt sources..."; \
	echo "deb [arch=$$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] " \
    "https://download.docker.com/linux/ubuntu $$(. /etc/os-release && echo "$${UBUNTU_CODENAME:-$$VERSION_CODENAME}") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null; \
	echo "Updating the package list after adding the repository..."; \
	sudo apt-get update

docker-install:
	@echo "Installing Docker..."
	sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
	@echo "Verifying the Docker installation..."
	sudo docker run hello-world
	@$(MAKE) clean

docker-install-rules:
	@echo "Adding user ($$USER) to the Docker group..."
	sudo usermod -aG docker $$USER

telegram-install:
	@TEMP_DIR='/tmp/telegram-install'; \
	@echo "Creating a temporary directory..."
	mkdir -p $(TEMP_DIR)
	@echo "Downloading Telegram..."
	wget $(TELEGRAM_URL) -O $(TEMP_DIR)/telegram.tar.xz
	@echo "Extracting Telegram..."
	tar -xf $(TEMP_DIR)/telegram.tar.xz -C $(TEMP_DIR)
	@echo "Moving Telegram to /opt..."
	sudo mv $(INSTALL_DIR) /opt/
	@echo "Creating a symbolic link..."
	sudo ln -sf /opt/Telegram/Telegram /usr/local/bin/telegram
	@$(MAKE) install-telegram-clean

telegram-install-clean:
	@echo "Establishing a symbolic link..."
	rm -rf $(TEMP_DIR)

telegram-install-lnk:
	echo "Creating a shortcut for the application menu..."; \
	/opt/Telegram/Telegram; \
	echo "Check it in ~/.local/share/applications/"

clean:
	@echo "Removing the temporary directory..."
	sudo rm -rf $(TEMP_DIR)

firefox-dev-install-ppa:
	@read -p "Enter the language for installing Firefox (default: ru): " lang; \
	if [ -z "$$lang" ]; then \
		lang="ru"; \
	fi; \
	sudo add-apt-repository ppa:lyzardking/ubuntu-make -y; \
	sudo apt-get update; \
	sudo apt-get install ubuntu-make -y; \
	umake web firefox-dev --lang "$$lang"; \
	echo "Installation of Firefox Developer Edition $$lang is complete!"
firefox-dev-install-lnk:
	@echo "Creating a shortcut for the applications menu..."

wg-install:
	sudo apt update
	sudo apt-get install wireguard

wg-vpn:
	@read -p "Enter the path to the configuration file (default: '$(WG_CONF)'): " conf; \
	if [ -z "$$conf" ]; then \
		conf="$(WG_CONF)"; \
	fi; \
	if [ ! -f "$$conf" ]; then \
		echo "Error: file '$$conf' does not exist."; \
		exit 1; \
	fi; \
	sudo cp "$$conf" /etc/wireguard/; \
	sudo chmod 600 /etc/wireguard/$$(basename "$$conf")
	@echo "Configuration installation is complete!"

wg-up:
	@read -p "Enter the configuration name to activate (default:  'default'): " conf; \
	if [ -z "$$conf" ]; then \
		conf="default"; \
	fi; \
	sudo wg-quick up $$conf
	@echo "The wg configuration has been initiated!"

wg-auto-run-enable:
	@read -p "Enter the configuration name to enable for autostart (default: 'default'): " conf; \
	if [ -z "$$conf" ]; then \
		conf="default"; \
	fi; \
	sudo systemctl enable wg-quick@$$conf

wg-auto-run-disable:
	@read -p "Enter the configuration name to disable from autostart (default: 'default'): " conf; \
	if [ -z "$$conf" ]; then \
		conf="default"; \
	fi; \
	sudo systemctl disable wg-quick@$$conf

phpstorm-install:
	@echo "Fetching the latest PhpStorm Linux download link..."; \
	LINK=$$(curl -s "https://data.services.jetbrains.com/products/releases?code=PS&latest=true&type=release" | jq -r '.PS[0].downloads.linux.link' | sed 's|download.|download-cdn.|'); \
	sudo apt update -y; \
	sudo apt install -y openjdk-17-jdk; \
	echo "Downloading PhpStorm from: $$LINK"; \
	mkdir -p /tmp/phpstorm; \
	sudo wget --show-progress -q $$LINK -P /tmp/phpstorm; \
	tar -xf /tmp/phpstorm/*.tar.gz -C /tmp/phpstorm; \
	sudo mv /tmp/phpstorm/PhpStorm-* /opt/phpstorm; \
	rm -rv /tmp/phpstorm; \
	@echo "The installation of PhpStorm is complete!"

phpstorm-install-lnk:
	@echo "Creating a shortcut for PHPStorm..."; \
	DESKTOP_FILE="/usr/share/applications/phpstorm.desktop"; \
	if [ -f "$$DESKTOP_FILE" ]; then \
		sudo rm -rv $$DESKTOP_FILE; \
	fi; \
	echo "[Desktop Entry]" | sudo tee $$DESKTOP_FILE > /dev/null; \
	echo "Version=1.0" | sudo tee -a $$DESKTOP_FILE > /dev/null; \
	echo "Type=Application" | sudo tee -a $$DESKTOP_FILE > /dev/null; \
	echo "Name=PHPStorm" | sudo tee -a $$DESKTOP_FILE > /dev/null; \
	echo "Icon=/opt/phpstorm/bin/phpstorm.png" | sudo tee -a $$DESKTOP_FILE > /dev/null; \
	echo "Exec=/opt/phpstorm/bin/phpstorm" | sudo tee -a $$DESKTOP_FILE > /dev/null; \
	echo "Comment=PHPStorm IDE" | sudo tee -a $$DESKTOP_FILE > /dev/null; \
	echo "Categories=Development;IDE;" | sudo tee -a $$DESKTOP_FILE > /dev/null; \
	echo "Terminal=false" | sudo tee -a $$DESKTOP_FILE > /dev/null; \
	echo "StartupNotify=true" | sudo tee -a $$DESKTOP_FILE > /dev/null; \
	echo "StartupWMClass=jetbrains-phpstorm" | sudo tee -a $$DESKTOP_FILE > /dev/null; \
	echo "PHPStorm has been installed and the shortcut has been created!"

phpstorm-reset-trial:
	@echo "PhpStorm reset trial..."; \
	if [ -d "$$HOME/.java/.userPrefs/" ]; then \
		sudo rm -rv ~/.java/.userPrefs/; \
	fi

set-keybinding:
	@echo "Choose a key combination:"
	@echo "1. Shift + Alt (Desktop)"
	@echo "2. Shift + Ctrl (Mac)"
	@read -p "Enter your choice number (1 or 2): " choice; \
	if [ "$$choice" -eq 1 ]; then \
		keybinding='<Shift>Alt_L'; \
		echo "Setting up the key combination for Desktop..."; \
	elif [ "$$choice" -eq 2 ]; then \
		keybinding='<Shift>Control_L'; \
		echo "Setting up the key combination for Mac..."; \
	else \
		echo "Invalid choice. Please select 1 or 2."; \
		exit 1; \
	fi; \
	gsettings set org.gnome.desktop.wm.keybindings switch-input-source "['$$keybinding']"; \
	echo "Key combination set: $$keybinding"
