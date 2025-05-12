# Ubuntu

Here's a well-structured nested list of examples for using the `make` command based on your Makefile, formatted for a `README.md` file on GitHub. This should help the developer community understand how to use the commands effectively.

## Docker

- **Install Docker Repository and Required Packages**
  ```bash
  make docker-install-repo
  ```

- **Install Docker**
  ```bash
  make docker-install
  ```

- **Add User to Docker Group**
  ```bash
  make docker-install-rules
  ```

- **Complete Docker Installation (Repository, Installation, and User Setup)**
  ```bash
  make docker-all
  ```

## Telegram

- **Install Telegram**
  ```bash
  make telegram-install
  ```

- **Clean Up Temporary Files After Installation**
  ```bash
  make telegram-install-clean
  ```

- **Create a Shortcut for Telegram in the Application Menu**
  ```bash
  make telegram-install-lnk
  ```

- **Complete Telegram Installation (Install, Clean Up, and Create Shortcut)**
  ```bash
  make telegram-all
  ```

## Firefox Developer Edition

- **Install Firefox Developer Edition**
  ```bash
  make firefox-dev-install-ppa
  ```

- **Create a Shortcut for Firefox in the Application Menu**
  ```bash
  make firefox-dev-install-lnk
  ```

## PhpStorm

- **Install PhpStorm**
  ```bash
  make phpstorm-install
  ```

- **Create a Shortcut for PhpStorm in the Application Menu**
  ```bash
  make phpstorm-install-lnk
  ```

- **Reset PhpStorm Trial Period**
  ```bash
  make phpstorm-reset-trial
  ```

- **Complete PhpStorm Installation**
  ```bash
  make phpstorm-all
  ```

## WireGuard

- **Install WireGuard**
  ```bash
  make wg-install
  ```

- **Configure WireGuard VPN**
  ```bash
  make wg-vpn
  ```

- **Activate WireGuard Configuration**
  ```bash
  make wg-up
  ```

- **Enable WireGuard Configuration for Autostart**
  ```bash
  make wg-auto-run-enable
  ```

- **Disable WireGuard Configuration from Autostart**
  ```bash
  make wg-auto-run-disable
  ```

## Miscellaneous

- **Set Keyboard Shortcuts for Input Sources**
  ```bash
  make set-keybinding
  ```

This Makefile provides a comprehensive way to manage installations and configurations for various applications. Use the commands as described above to streamline your setup process.
