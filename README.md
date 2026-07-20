# Laboratório – Sessão 03
## Hardening de Redes Linux e Configuração de Firewalls

**Curso:** Reskilling – Linux e Cibersegurança

---

## Objetivo

Implementar uma política de segurança no servidor Linux utilizando **UFW** e **iptables**, restringindo acessos não autorizados e permitindo apenas o serviço SSH.

---

# Configuração realizada

## 1. Verificação do estado do UFW

Comando:

```bash
sudo ufw status
```

Resultado:

```text
Status: inactive
```

O firewall encontrava-se desativado.

---

## 2. Definição das políticas padrão

Foram aplicadas as seguintes políticas:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

Resultado:

```text
Default incoming policy changed to 'deny'
Default outgoing policy changed to 'allow'
```

Isto significa que:

- Todo o tráfego de entrada é bloqueado por defeito;
- Todo o tráfego de saída é permitido.

---

## 3. Permissão do serviço SSH

Foi criada uma exceção para permitir ligações SSH:

```bash
sudo ufw allow 22/tcp
```

Resultado:

```text
Rules updated
Rules updated (v6)
```

---

## 4. Ativação do UFW

Comando:

```bash
sudo ufw enable
```

Resultado:

```text
Firewall is active and enabled on system startup
```

O firewall ficou ativo e configurado para iniciar automaticamente com o sistema.

---

## 5. Estado final do UFW

Comando:

```bash
sudo ufw status verbose
```

Resultado:

```text
Status: active

Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed)

To          Action      From
22/tcp      ALLOW IN    Anywhere
22/tcp(v6)  ALLOW IN    Anywhere (v6)
```

### Análise

A configuração confirma que:

- o firewall encontra-se ativo;
- todas as ligações de entrada são bloqueadas;
- apenas a porta **22/TCP** está autorizada para acesso remoto via SSH;
- o tráfego de saída continua permitido.

---

# Configuração do iptables

Foi adicionada uma regra para bloquear um endereço IP malicioso fictício:

```bash
sudo iptables -A INPUT -s 203.0.113.50 -j DROP
```

Posteriormente foi verificada a configuração através de:

```bash
sudo iptables -L -v
```

Resultado observado:

- política da chain **INPUT** definida para **DROP**;
- regra de bloqueio para o endereço IP **203.0.113.50**.

---

# Explicação da política aplicada

Foi implementada uma política de segurança baseada no princípio do **menor privilégio**.

As medidas aplicadas foram:

- bloquear todo o tráfego de entrada por defeito;
- permitir apenas ligações SSH na porta 22;
- permitir todas as ligações de saída;
- bloquear explicitamente o IP fictício **203.0.113.50** utilizando o **iptables**, simulando a mitigação de um atacante.

Esta configuração reduz significativamente a superfície de ataque do servidor, permitindo apenas os serviços estritamente necessários.

---

# Conclusão

O laboratório foi concluído com sucesso.

Foram aplicadas regras de hardening utilizando **UFW** e **iptables**, garantindo que apenas o serviço SSH permanece acessível enquanto o restante tráfego de entrada é bloqueado. O bloqueio de um endereço IP específico demonstra a utilização do iptables para criar regras de filtragem adicionais, reforçando a segurança do sistema.
