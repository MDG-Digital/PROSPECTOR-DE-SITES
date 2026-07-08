---
description: Publica as páginas redesenhadas na HostGator e retorna as URLs públicas
argument-hint: "[nome do cliente ou todos]"
---

Publique páginas na HostGator seguindo a skill `deploy-hostgator`.

## Passos

1. Leia `prospector-config.json`. Se os dados da HostGator não estiverem preenchidos, colete-os agora (usuário, domínio, servidor — e oriente o usuário a preencher a senha diretamente no config, nunca no chat) — não prossiga sem eles.
2. Determine o que publicar: `$ARGUMENTS` (um cliente ou "todos"), ou liste as páginas com status `redesenhado` em `leads.md` e pergunte.
3. Para cada página, siga a skill `deploy-hostgator`: envie `sites/[slug]/[slug].html` para `public_html/[pastaBase]/[slug]/index.html` (o arquivo local leva o nome do cliente; no servidor ele vira index.html), tentando primeiro o método programático (FTP) e usando o fallback pelo navegador (cPanel File Manager) se necessário.
4. Verifique cada publicação abrindo a URL pública (`https://[dominio]/[pastaBase]/[slug]/`) e confirmando que a página carrega corretamente.
5. Atualize `leads.md`: status `publicado` + coluna com a URL pública.

## Saída

Liste as URLs públicas de cada cliente. Sugira o próximo passo: `/proposta` para enviar os e-mails.
