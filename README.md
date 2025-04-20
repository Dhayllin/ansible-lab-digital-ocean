# Ansible Lab - DigitalOcean 🐧⚙️

Este projeto é um laboratório de automação com Ansible (https://www.ansible.com/) para provisionar e configurar servidores Ubuntu e CentOS hospedados na DigitalOcean (https://www.digitalocean.com/).

## 🔧 Funcionalidades

- Configuração de hostname por host
- Criação de diretórios customizados
- Criação de arquivos com touch, preservando tempo de acesso e modificação
- Instalação do Docker e Neofetch (em Ubuntu)
- Suporte a variáveis externas para segurança e organização

## 🗂️ Estrutura do projeto

    ansible-lab-digital-ocean/
    ├── ansible.cfg                     # Configuração do Ansible
    ├── ansible.log                     # Log da execução dos playbooks
    ├── inventory.yml                   # Inventário dos hosts
    ├── playbook-lab.yml                # Playbook de instalação (Docker e Neofetch)
    ├── playbook-lab-artefacts.yml      # Playbook para criação de artefatos
    ├── vars-example.yml                # Modelo de variáveis (não sensível)
    ├── vars.yml                        # Variáveis sensíveis (NÃO versionar)
    ├── README.md                       # Este arquivo

## 📦 Requisitos

- Ansible >= 2.16
- Python >= 3.8
- Acesso root via chave SSH para os hosts

## 🚀 Como usar

### 1. Instale Dependências no Host Local

    sudo apt update && sudo apt install -y ansible

### 2. Configure Variáveis Sensíveis

Crie o arquivo vars.yml a partir do modelo vars-example.yml:

    cp vars-example.yml vars.yml

Edite o vars.yml com seus dados reais:

    ansible_user: root
    ansible_ssh_private_key_file: /caminho/para/sua/.ssh/id_ed25519
    default_dir: /opt/custom

⚠️ Importante: Nunca suba o vars.yml para o Git. Ele deve ser listado no .gitignore.

### 3. Configure o Inventário (inventory.yml)

    all:
    hosts:
        159.65.255.159:
        ansible_user: "{{ ansible_user }}"
        ansible_ssh_private_key_file: "{{ ansible_ssh_private_key_file }}"
        hostname: centos-lab
        161.35.134.20:
        ansible_user: "{{ ansible_user }}"
        ansible_ssh_private_key_file: "{{ ansible_ssh_private_key_file }}"
        hostname: ubuntu-lab
    children:
        ubuntu:
        hosts:
            161.35.134.20
        centos:
        hosts:
            159.65.255.159

### 4. Execute os Playbooks

    ansible-playbook -i inventory.yml playbook-lab.yml
    ansible-playbook -i inventory.yml playbook-lab-artefacts.yml

##  🔐 Segurança

- Nunca versionar vars.yml nem suas chaves SSH.
- Garanta que vars.yml esteja no .gitignore:

    echo "vars.yml" >> .gitignore

## 📌 Dicas

- Teste os comandos com --check para simular sem executar mudanças:

    ansible-playbook -i inventory.yml playbook-lab.yml --check

- Use --limit para aplicar em apenas um grupo ou host:

    ansible-playbook -i inventory.yml playbook-lab.yml --limit ubuntu

## 📚 Referências

- Documentação do Ansible: https://docs.ansible.com/
- DigitalOcean Cloud: https://cloud.digitalocean.com/