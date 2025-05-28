# Atividade Prática — Explorando o Protocolo **SMTP**

> **Objetivo**  
> Compreender, na prática, o diálogo entre cliente e servidor SMTP realizando o envio manual de um e‑mail por meio de comandos de linha de comando. Ao concluir, você verá cada etapa do processo de autenticação, construção e enfileiramento da mensagem.

---

## 1. Pré‑requisitos

| Recurso | Descrição |
|---------|-----------|
| **Sistema operacional** | Windows 10/11, macOS ou distribuição Linux. |
| **Ferramenta de terminal** | **Opção A – Telnet** (habilite *Telnet Client* em Windows ou use nativo no macOS/Linux)<br>**Opção B – PuTTY** (caso Telnet não esteja disponível no Windows). |
| **Conexão à Internet** | Acessar o host `in-v3.mailjet.com` na porta **587**. |
| **Credenciais (Base64)** | **Usuário**: `<USUARIO_BASE64>`<br>**Senha**: `<SENHA_BASE64>` |

> ℹ️ **Importante**  
> As credenciais oficiais serão fornecidas pelo professor **durante a aula**. Substitua as tags `<USUARIO_BASE64>` e `<SENHA_BASE64>` pelos valores recebidos.

> 💡 **Por que Base64?**  
> O comando `AUTH LOGIN` exige que usuário e senha sejam enviados codificados em Base64. Isso **não** é criptografia; é apenas uma forma de transporte legível pelo protocolo.

---

## 2. Como Conectar

### 2.1 Usando Telnet

```bash
telnet in-v3.mailjet.com 587
```

Se o comando for aceito você verá uma resposta iniciando com `220`.

#### Ativando o Telnet Client (Windows)

1. **Win + R** → `optionalfeatures.exe` → **OK**.  
2. Marque **Telnet Client** → **OK** → aguarde a instalação.

### 2.2 Usando PuTTY (alternativa para Windows)

1. Baixe o executável portátil de PuTTY em <https://www.putty.org> (não requer instalação).  
2. Abra **putty.exe**.  
3. Em **Session**:  
   * **Host Name**: `in-v3.mailjet.com`  
   * **Port**: `587`  
   * **Connection type**: selecione **Telnet** (ou **Raw** se a resposta do servidor não aparecer).  
4. *(Opcional)* Em **Logging** marque **Printable output** para salvar todo o diálogo em arquivo.  
5. Clique **Open**. A janela preta abrirá já conectada ao servidor SMTP (aparecerá `220 ... ESMTP`).  
6. Continue a partir da **etapa 3** do passo‑a‑passo (EHLO) abaixo.

---

## 3. Passo a Passo Detalhado (comandos SMTP)

| # | Comando (cliente) | Resposta esperada (servidor) | Explicação |
|---|-------------------|------------------------------|------------|
| 1 | `ehlo meu-pc` | série de linhas `250-...` | Identifica o cliente e lista extensões do servidor. |
| 2 | `auth login` | `334 VXNlcm5hbWU6` | Solicita usuário (Base64). |
| 3 | `<USUARIO_BASE64>` | `334 UGFzc3dvcmQ6` | Solicita senha (Base64). |
| 4 | `<SENHA_BASE64>` | `235 2.7.0 OK` | Autenticado. |
| 5 | `mail from:<qualquercoisa@aulasrede.com.br>` | `250 2.1.0 OK` | Define remetente. |
| 6 | `rcpt to:<claudio.nunes6@fatec.sp.gov.br>` | `250 2.1.5 OK` | Define destinatário. |
| 7 | `data` | `354 End data with <CR><LF>.<CR><LF>` | Inicia o corpo da mensagem. |
| 8 | ```text<br>Subject: Teste SMTP via comandos<br>From: Qualquer <qualquercoisa@aulasrede.com.br><br>To: Claudio <claudio.nunes6@fatec.sp.gov.br><br><br>Olá! Este e‑mail foi enviado manualmente via SMTP.<br>.``` | `250 2.0.0 Message queued` | Um ponto sozinho finaliza o envio. |
| 9 | `quit` | `221 2.0.0 Bye` | Encerra a sessão SMTP. |

---

## 4. Dicas Rápidas

* **Colar no PuTTY**: clique com o botão direito.  
* **Salvar log no PuTTY**: `Session` → `Logging` → escolha um arquivo `.log`.  
* Porta 587 costuma exigir STARTTLS, mas este sandbox aceita autenticação simples apenas para fins didáticos.

---

## 5. Evidência de Conclusão

1. Abra o terminal (Telnet ou PuTTY) em tela cheia para exibir todo o diálogo.  
2. Faça uma **captura de tela** contendo do primeiro `220` até o `quit`.  
3. Salve em PDF ou insira no Word e exporte para PDF.  
4. Nomeie o arquivo como `atividade_SMTP_seuNomeCompleto.pdf`.

---

## 6. Para Saber Mais

* **RFC 5321** — Simple Mail Transfer Protocol.  
* `openssl s_client -starttls smtp` — exemplo de sessão SMTP segura (TLS).  
* Portas SMTP: 25 (relay), 587 (submission), 465 (SMTPS legado).

---

Boa prática! Se algo não sair como esperado verifique a mensagem de erro do servidor — ela sempre indica o que precisa ser ajustado.
