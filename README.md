
# 🛠️ Laboratório Conceitual de Ataques de Força Bruta com Medusa (Kali Linux)

## 📌 Visão Geral

Este projeto apresenta a implementação de um laboratório prático de cibersegurança utilizando **Kali Linux** e **Metasploitable 2**, com foco na simulação de ataques de força bruta em diferentes serviços.

Foram explorados cenários reais em ambiente controlado, incluindo:

* FTP
* Aplicações Web (DVWA)
* SMB

Além da execução dos ataques, o projeto também aborda **análise de risco, detecção e estratégias de mitigação**, conectando práticas ofensivas com visão defensiva.

---

## ⚠️ Aviso Legal

Este projeto foi desenvolvido exclusivamente para fins educacionais.

* Todos os testes foram realizados em ambiente isolado
* Não utilize essas técnicas sem autorização
* O uso indevido pode ser ilegal

---

## 🖥️ Ambiente de Testes

### 🔧 Configuração

* Kali Linux (máquina atacante)
* Metasploitable 2 (máquina vulnerável)
* VirtualBox
* Rede: Host-only

### 🎯 Objetivo

Simular ataques em ambiente isolado para análise de comportamento e impacto.

---

## 💥 Cenários de Ataque — Detalhamento Técnico (Ambiente Controlado)

### 🌐 Contexto de Rede

* 🐉 Kali Linux (Atacante): 192.168.56.10
* 🎯 Metasploitable 2 (Alvo): 192.168.56.101
* 🔗 Tipo de rede: Host-only (VirtualBox)

---

## 🔐 Cenário 1 — Ataque de Força Bruta em FTP

### 🎯 Objetivo

Obter acesso ao serviço FTP explorando credenciais fracas.

---

### 🔎 Etapa 1 — Enumeração de serviços

```bash
nmap -sV 192.168.56.101
```

**Análise:**

* Identificação da porta 21 (FTP) aberta
* Descoberta da versão do serviço
* Confirmação de alvo válido para ataque

---

### ⚙️ Etapa 2 — Execução do ataque

```bash
medusa -h 192.168.56.101 -u msfadmin -P wordlists/passwords.txt -M ftp
```

**Explicação dos parâmetros:**

* `-h` → IP do alvo
* `-u` → usuário específico
* `-P` → wordlist de senhas
* `-M ftp` → módulo do protocolo

---

### 🧠 Raciocínio técnico

* O FTP é um protocolo frequentemente mal configurado
* Ambientes vulneráveis utilizam credenciais padrão
* Ataques automatizados permitem testar centenas de senhas rapidamente

---

### ✅ Resultado esperado

* Credenciais válidas encontradas
* Acesso ao servidor FTP
* Possibilidade de manipulação de arquivos

---

### ⚠️ Vulnerabilidade explorada

* Senhas fracas
* Ausência de bloqueio por tentativa
* Falta de monitoramento

---

---

## 🌐 Cenário 2 — Ataque de Força Bruta em Aplicação Web (DVWA)

### 🎯 Objetivo

Explorar falhas de autenticação em formulário web.

---

### 🔎 Etapa 1 — Acesso à aplicação

```bash
http://192.168.56.101/dvwa
```

* Login na plataforma DVWA
* Configuração de segurança em nível **Low**

---

### ⚙️ Etapa 2 — Execução do ataque

```bash
medusa -h 192.168.56.101 -u admin -P wordlists/passwords.txt -M http
```

---

### 🧠 Raciocínio técnico

* Aplicações web são um dos principais vetores de ataque
* Falta de proteção contra automação permite brute force
* DVWA simula esse cenário de forma controlada

---

### ⚠️ Considerações técnicas

Ataques web podem envolver:

* Cookies de sessão
* Tokens CSRF
* Identificação de respostas HTTP

---

### ✅ Resultado esperado

* Descoberta de credenciais válidas
* Acesso ao painel da aplicação

---

### ⚠️ Vulnerabilidade explorada

* Ausência de rate limiting
* Falta de CAPTCHA
* Senhas previsíveis

---

---

## 🗂️ Cenário 3 — Password Spraying em SMB

### 🎯 Objetivo

Identificar contas com senhas fracas sem gerar bloqueio.

---

### 🔎 Etapa 1 — Enumeração de usuários

```bash
nmap -p 445 --script smb-enum-users.nse 192.168.56.101
```

---

### 🧠 Análise

* Identificação de usuários válidos
* Redução de tentativas inválidas
* Aumento da eficiência do ataque

---

### ⚙️ Etapa 2 — Execução do ataque

```bash
medusa -h 192.168.56.101 -U wordlists/users.txt -p 123456 -M smbnt
```

---

### 🧠 Raciocínio técnico

* Password spraying evita bloqueios de conta
* Muito usado em ambientes corporativos
* Explora reutilização de senhas

---

### ✅ Resultado esperado

* Identificação de contas comprometidas
* Possível acesso ao sistema

---

### ⚠️ Vulnerabilidade explorada

* Senhas comuns
* Falta de política de senha forte
* Reutilização de credenciais

---

---

## 🔬 Ferramentas Utilizadas

### 🐉 Kali Linux

Sistema operacional focado em testes de segurança.

---

### ⚔️ Medusa

Ferramenta de força bruta:

* Suporte a múltiplos protocolos
* Execução paralela
* Alta performance

---

### 🔎 Nmap

Utilizado para:

* Descoberta de serviços
* Enumeração de rede
* Identificação de portas

---

### 🧪 Metasploitable 2

Ambiente vulnerável para testes práticos.

---

### 🌐 DVWA

Aplicação web vulnerável para simulação de ataques.

---

---

## 🧠 Conclusão Técnica

Os cenários demonstram diferentes abordagens para exploração de credenciais:

* Ataques diretos (FTP)
* Ataques em aplicações web
* Ataques distribuídos (SMB)

Mesmo sendo técnicas consideradas básicas, continuam altamente eficazes em ambientes sem controles adequados de segurança.


## 🧠 Mapeamento MITRE ATT&CK

As técnicas utilizadas neste laboratório podem ser associadas ao framework MITRE ATT&CK:

* **T1110 — Brute Force**
* **T1110.001 — Password Guessing**
* **T1110.003 — Password Spraying**
* **T1078 — Valid Accounts**

Essas técnicas representam métodos comuns de obtenção de acesso inicial através da exploração de credenciais fracas.

---

## 🚨 Indicadores de Comprometimento (IoCs)

Durante ataques de força bruta, é possível identificar padrões como:

* Alto volume de tentativas de login falhas
* Tentativas distribuídas entre múltiplos usuários
* Tráfego repetitivo em portas específicas (21, 445, 80)
* Logs com padrão sequencial de usernames
* Tentativas fora do horário habitual

---

## ⚖️ Brute Force vs Password Spraying

| Técnica           | Característica                        | Risco                  |
| ----------------- | ------------------------------------- | ---------------------- |
| Brute Force       | Muitas tentativas em um único usuário | Alto risco de bloqueio |
| Password Spraying | Uma senha em vários usuários          | Mais discreto          |

---

## 🛡️ Estratégias de Mitigação

### 🔐 Controles preventivos

* Políticas de senha forte
* Autenticação multifator (MFA)
* Bloqueio por tentativas (account lockout)

---

### 📡 Detecção e monitoramento

* Monitoramento de logs de autenticação
* Uso de SIEM para correlação de eventos
* Detecção de padrões anômalos
* Rate limiting

---

### 🧠 Boas práticas

* Princípio do menor privilégio
* Remoção de usuários padrão
* Atualizações de segurança

---

## ⚠️ Riscos Operacionais

* Bloqueio de usuários legítimos
* Possibilidade de DoS via lockout
* Falsos positivos em monitoramento

---

## 📊 Aprendizados

* Ataques simples podem ser eficazes contra configurações fracas
* A ausência de monitoramento facilita a exploração
* Segurança depende tanto de tecnologia quanto de comportamento

---

## 🧠 Insight Final

Ataques de força bruta não exploram vulnerabilidades complexas, mas sim falhas básicas de segurança, como senhas fracas e ausência de controle de acesso.

Isso reforça que a maturidade em cibersegurança está diretamente ligada à implementação de boas práticas e monitoramento contínuo.

---

## 📚 Contexto

Projeto desenvolvido como parte do Bootcamp de Cibersegurança da DIO em parceria com a Riachuelo.

---

## 👩‍💻 Autora

Bruna Portella Cruz
