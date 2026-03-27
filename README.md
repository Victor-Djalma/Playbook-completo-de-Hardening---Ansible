# 🔐 Mini SOC - Auditoria de Hardening em Linux

Projeto prático com foco em **Segurança Defensiva (Blue Team)**, simulando a operação de um **Security Operations Center (SOC)** em um ambiente Linux vulnerável.

O projeto cobre o ciclo completo de segurança:

> Descoberta → Exploração → Monitoramento → Hardening → Revalidação

---

## 🎯 Objetivo

Demonstrar, na prática, competências essenciais de segurança ofensiva e defensiva:

* Identificação de vulnerabilidades
* Exploração controlada
* Monitoramento de eventos (SIEM)
* Hardening de sistemas Linux
* Automação de segurança

---

## 🧱 Arquitetura do Ambiente

| Máquina      | Função                      |
| ------------ | --------------------------- |
| Kali Linux   | Atacante                    |
| Debian       | Alvo vulnerável             |
| Wazuh Server | Monitoramento (SIEM)        |
| OpenVAS      | Scanner de vulnerabilidades |

---

## ⚠️ Vulnerabilidades Iniciais

O ambiente foi propositalmente configurado com falhas críticas:

* SSH com login root habilitado
* Autenticação por senha ativa
* FTP com acesso anônimo
* Firewall desativado (UFW)
* Serviços expostos (Apache, Bind9)
* Permissões inseguras em `/etc/passwd`

---

## 🔍 Reconhecimento e Enumeração

### Nmap

```bash
nmap -sV <IP>
nmap -p 22 --script ssh-auth-methods --script-args="ssh.user=root" <IP>
```

Identificação de serviços expostos:

* FTP (21)
* SSH (22)
* DNS (53)
* HTTP (80)

---

## 🚨 Análise de Vulnerabilidades

### OpenVAS

* Identificação de vulnerabilidades em serviços

### Lynis

```bash
lynis audit system
```

Resultado inicial:

```
Hardening Index: 61/100
```

### OpenSCAP

```bash
oscap xccdf eval --profile xccdf_org.ssgproject.content_profile_standard <arquivo>
```

Principais falhas:

* SSH inseguro
* Falta de auditd
* ASLR desativado
* Ausência de logs

### RKhunter

```bash
rkhunter --check
```

---

## ⚔️ Simulação de Ataque

Ataque de força bruta no SSH:

```bash
hydra -l root -P rockyou.txt ssh://<IP>
```

Monitoramento de logs:

```bash
tail -f /var/log/auth.log
```

---

## 📡 Monitoramento com Wazuh

O Wazuh foi utilizado como SIEM para:

* Detectar tentativas de login inválidas
* Correlacionar eventos de brute force
* Gerar alertas em tempo real
* Centralizar logs

---

## 🛡️ Hardening com Ansible

### Execução do Playbook

```bash
ansible-playbook -i inventory.ini hardening.yml
```

### Principais controles aplicados

**SSH**

```yaml
PermitRootLogin no
PasswordAuthentication no
```

**Firewall (UFW)**

```bash
ufw enable
```

**Fail2Ban**

```bash
systemctl enable fail2ban
systemctl start fail2ban
```

---

## 📈 Resultados

* Hardening Index: **61 → ~75–80**
* Redução significativa da superfície de ataque
* Mitigação de vulnerabilidades críticas

---

## 🧠 Principais Aprendizados

* Configuração insegura é uma das maiores causas de falhas
* Monitoramento contínuo é essencial em um SOC
* Automação aumenta consistência e escalabilidade
* Segurança é um processo contínuo

---

## 💼 Competências Demonstradas

* Vulnerability Assessment
* Linux Hardening
* SIEM (Wazuh)
* Log Analysis
* Incident Detection
* Automação com Ansible

---

## 👨‍💻 Autores

* Paulo Daniel
* Vinicius Marson
* Victor Djalma

---

## 🚀 Sobre

Projeto desenvolvido no curso de Segurança Cibernética com foco em práticas reais de SOC e Blue Team em ambientes Linux.

