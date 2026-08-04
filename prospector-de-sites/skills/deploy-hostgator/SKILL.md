---
name: deploy-hostgator
description: Esta skill deve ser usada ao publicar páginas na hospedagem HostGator — upload via script local automático, FTP ou cPanel, criação de pastas por cliente, verificação da URL pública e HTTPS. Acione quando o usuário disser "publicar", "subir o site", "colocar no ar", "deploy", "hostgator" ou rodar /publicar ou o teste de conexão do /setup.
---

# Deploy na HostGator

Publicar páginas em `public_html/[pastaBase]/[slug]/` e garantir a URL pública `https://[dominio]/[pastaBase]/[slug]/` funcionando.

> ⚠️ **SLUG:** o `[slug]` da pasta remota e da URL DEVE ser o mesmo `slug` gravado no `prospector.db` para o lead (e o mesmo da pasta local `sites/bh/oficinas/<slug>/`). Nunca re-derive do nome na hora de publicar — leia o slug do banco e use-o igual. Assim a URL pública, a pasta local e o banco ficam idênticos. Regra única de slug: skill `dashboard-leads`.

## Credenciais

Tudo vem de `prospector-config.json` (bloco `hostgator`): `usuario`, `dominio`, `servidor`, `senha`, `pastaBase` (padrão `clientes`). **A senha vive SÓ nesse arquivo, no computador do usuário — nunca é digitada no chat, nunca é exibida em nenhuma saída, log ou comando mostrado ao usuário.** Se a senha estiver vazia, oriente o usuário: dashboard → aba Configurações → Conexão HostGator → colar a senha e salvar (ou editar o arquivo na mão). Nunca pelo chat.

## Método 1 — Publicador MANUAL (monte a fila; o usuário roda o publicar-agora.bat) — PADRÃO

A rede do sandbox do Cowork NÃO alcança FTP nem cPanel — isso vale para todo usuário. A publicação roda na máquina do usuário de forma MANUAL: o Claude monta a fila (`fila-publicacao.txt`) e o USUÁRIO roda o `publicar-agora.bat` (Windows) / `publicar-agora.command` (Mac), que sobe tudo de uma vez lendo as credenciais do config. **Regra fixa: NUNCA suba automático nem conte com publicador em segundo plano — depois de montar a fila, SEMPRE aguarde o usuário rodar o publicador e confirmar que subiu; só então verifique as URLs.**

1. **Garanta os arquivos do publicador na pasta conectada** (copie de `references/` desta skill, sobrescrevendo versões antigas), conforme o sistema do usuário — pergunte ou detecte:
   - **Windows**: `publicar-agora.ps1`, `publicar-agora.bat`, `publicador-oculto.vbs`, `instalar-publicador.bat`.
   - **Mac**: `publicar-agora.command` e `instalar-publicador.command` (o instalador registra o publicador no launchd, a cada 60s; desinstalar = `launchctl unload` do plist com.prospector.publicador).
   Em dúvida, copie todos — cada sistema ignora os do outro.
2. **Não instale tarefa agendada automática.** O fluxo é manual: garanta apenas que `publicar-agora.bat` + `publicar-agora.ps1` (Windows) ou `publicar-agora.command` (Mac) estão na pasta. Se existir a tarefa antiga `ProspectorPublicador`, remova-a (`schtasks /Delete /TN ProspectorPublicador /F`) — o usuário publica sempre à mão.
3. **Monte a fila**: escreva `fila-publicacao.txt` na raiz da pasta conectada, uma linha por arquivo: `caminho/local/arquivo.html|public_html/[pastaBase]/[slug]/index.html`. Inclua página (`index.html`) e capa (`proposta.html`) de cada cliente. Depois **PEÇA ao usuário para rodar o `publicar-agora.bat`** (duplo clique): ele sobe a fila e a renomeia para `fila-publicada-[data].txt` (log em `publicador-log.txt`).
4. **AGUARDE o usuário confirmar** que rodou o `publicar-agora.bat` (não espere poller automático, não tente subir sozinho). Só após a confirmação, verifique se a fila virou `fila-publicada-[data].txt` e teste as URLs (verificação abaixo).

## Método 2 — FTP direto do sandbox (só sob pedido explícito — NÃO é o padrão)

Só se o usuário pedir explicitamente para você tentar subir direto (o padrão é a fila manual do Método 1): tente `curl -sS --connect-timeout 15 -T [arquivo] "ftp://[servidor]/public_html/[pastaBase]/[slug]/index.html" --user "[usuario]:[senha do config]" --ftp-create-dirs` (senha lida do arquivo via script — jamais mostrada). Se funcionar, ótimo: zero ação do usuário. Se a rede do sandbox bloquear (timeout/refused), caia SEM DRAMA para o Método 1 — não insista em tentativas repetidas.

## Método 3 — Navegador (último recurso)

Se os métodos 1 e 2 falharem (ex.: curl ausente na máquina do usuário): cPanel File Manager pelo Claude in Chrome — o USUÁRIO faz o login dele (nunca peça a senha no chat), você navega, cria as pastas e faz upload pela interface.

## Verificação (obrigatória, após qualquer método)

1. Abra `https://[dominio]/[pastaBase]/[slug]/` e a capa `.../proposta.html` — confirme que carregam com conteúdo certo.
2. **HTTPS obrigatório**: precisa carregar com cadeado válido. Se der erro de certificado: HostGator tem SSL grátis — guie: cPanel → **SSL/TLS Status** → marcar o domínio → **Run AutoSSL** (minutos). Enquanto o HTTPS não valida, a publicação NÃO está concluída — link `http://` NUNCA vai para cliente.
3. Atualize `leads.md` + dashboard com status `publicado` e a URL.

## Teste de conexão do /setup

Publique `teste.html` simples ("Funcionou!") em `public_html/[pastaBase]/teste/index.html`: deixe os scripts do publicador (`publicar-agora.bat`/`.ps1` ou `.command`) na pasta, monte a fila com o teste e peça UM duplo clique no `publicar-agora.bat` — assim o usuário aprende o fluxo manual logo no setup.
