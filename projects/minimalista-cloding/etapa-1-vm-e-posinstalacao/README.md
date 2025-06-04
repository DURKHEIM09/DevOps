# 🧱 Etapa 1 – Criação da Máquina Virtual e Pós-Instalação DevOps

Esta é a **primeira etapa** do projeto `minimalista-cloding`, focada na construção de uma máquina virtual local, funcional e otimizada para servir como base da stack DevOps.

---

## 💻 Ambiente Físico

- **Host:** Windows 11 limpo
- **Hypervisor:** Hyper-V
- **Processador:** Intel i5-12600KF
- **RAM Total:** 32 GB
- **Armazenamento:** SSD NVMe (alto I/O)
- **Placa de vídeo:** RTX 2060 Super (não utilizada diretamente)

---

## 🧩 Configuração da VM

| Item               | Valor                                        |
|--------------------|----------------------------------------------|
| Nome da VM         | `devops`                                     |
| Sistema            | Ubuntu Server 24.04 LTS                      |
| Memória alocada    | 16 GB                                        |
| vCPUs              | 8                                            |
| Disco (VHDX)       | 300 GB (dinâmico)                            |
| Tipo de rede       | Bridge (Switch externo - IP real)            |
| Geração da VM      | 2                                            |
| Secure Boot        | Desabilitado                                 |
| IP interno         | 192.168.0.67 via DHCP                        |

---

## ⚙️ Instalação do Ubuntu Server

- Idioma: Português (Brasil)
- Teclado: ABNT2
- Instalação: Completa, sem Snaps ou drivers extras
- Particionamento: Automático com LVM
  - `/boot/efi` - 1 GB
  - `/boot` - 2 GB
  - `/` (LVM) - 100 GB
- Usuário: `lucas` com sudo habilitado
- SSH: OpenSSH instalado, autenticação por senha habilitada

---

## 🔧 Pós-Instalação: Script Automatizado

Ferramentas instaladas:

- Docker + Docker Compose v2
- Terraform
- Ansible
- kubectl
- Kind (Kubernetes in Docker)
- Trivy (scanner de imagens)
- Utilitários: `htop`, `neofetch`, `curl`, `vim`, `wget`, `unzip`, `fonts-liberation`

Script utilizado:

```bash
#!/bin/bash

# Atualizações iniciais
sudo apt update && sudo apt upgrade -y

# Utilitários básicos
sudo apt install -y curl wget git vim gnupg lsb-release ca-certificates unzip fonts-liberation

# Docker
curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh
sudo usermod -aG docker $USER

# Docker Compose v2
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" \\
-o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Kind (Kubernetes in Docker)
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

# Terraform
wget https://releases.hashicorp.com/terraform/1.7.5/terraform_1.7.5_linux_amd64.zip
unzip terraform_1.7.5_linux_amd64.zip
sudo mv terraform /usr/local/bin/

# Ansible
sudo apt install -y ansible

# Trivy
curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh
sudo mv trivy /usr/local/bin/

echo "✅ Ambiente DevOps instalado com sucesso!"
Esse script pode ser executado com:

bash
Copiar
Editar
chmod +x postinstall.sh
./postinstall.sh
✅ Resultado da Etapa
VM funcional e conectada

Pronta para orquestração local

Stack DevOps pré-instalada

SSH remoto ativo
```
🗣️ Depoimento Pessoal

Essa etapa de criação da máquina virtual foi relativamente tranquila. Já tenho um bom domínio sobre criação e gerenciamento de VMs no Windows com Hyper-V, então segui um processo direto.

O único detalhe técnico foi a necessidade de desativar o Secure Boot na BIOS. Isso aconteceu porque, por conta do anti-cheat do Valorant (Vanguard), eu havia ativado o Secure Boot anteriormente, e ele conflitou com a ISO personalizada do Ubuntu — que não era reconhecida como segura pela UEFI. Após desativar, o sistema subiu normalmente.

Também optei por utilizar o PowerShell para gerenciar a VM. É mais prático no meu dia a dia, principalmente para ligar a máquina via linha de comando e iniciar sessões SSH rapidamente. Ainda não senti necessidade de automatizar tudo com script — para essa etapa, prefiro a flexibilidade do comando manual.
