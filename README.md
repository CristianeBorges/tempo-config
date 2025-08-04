# tempo-config
Adiciona guia de instalação e configuração do Tempo Grafana

# Guia de Instalação e Configuração do Tempo

Este guia mostra como instalar e configurar o Tempo em um sistema baseado em RHEL/CentOS, incluindo passos opcionais para proxy e firewall.

## Passo a passo

**Configurar proxy (opcional)**

```bash
export {http,https,ftp}_proxy="http://SEU_PROXY:8080"
export NO_PROXY="localhost,127.0.0.1,REDE_LOCAL"
```

1. Atualizar sistema

```bash
dnf update -y
reboot
dnf update -y
reboot
dnf update -y
```

2. Instalar dependências básicas

```bash
dnf install -y wget epel-release htop vim open-vm-tools
```

**Desabilitar firewall (opcional)**

```bash
systemctl stop firewalld
systemctl disable firewalld
```

3. Importar chave GPG do Grafana

```bash
wget -q -O gpg.key https://rpm.grafana.com/gpg.key
rpm --import gpg.key
```

4. Instalar Tempo

```bash
dnf install -y tempo
```

5. Configurar Tempo

```bash
vim /etc/tempo/config.yml
```

6. Gerenciar serviço Tempo

```bash
systemctl start tempo
systemctl enable tempo
systemctl status tempo
```

7. Testar consulta via API Prometheus

```bash
curl -G http://<SEU-ENDERECO>:9009/prometheus/api/v1/query --data-urlencode 'query=traces_service_graph_request_total'
```

**Backup arquivo config (opcional)**

```bash
cd /etc/tempo/
cp config.yml config.yml.antigo
```

---

*Substitua `<SEU_PROXY>`, `<REDE_LOCAL>` e `<SEU-ENDERECO>` conforme seu ambiente.*
