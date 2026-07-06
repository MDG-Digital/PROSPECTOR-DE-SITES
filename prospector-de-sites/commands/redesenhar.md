---
description: Redesenha os sites dos leads com estética premium (lote de 5 ou mais)
argument-hint: "[URLs ou nomes dos leads] — opcional, usa os 5+ melhores de leads.md"
---

Redesenhe as páginas dos leads seguindo a skill `redesign-premium`. Ela é obrigatória — leia a skill ANTES de escrever qualquer HTML.

## Seleção dos clientes

1. Leia `prospector-config.json` e `leads.md` na pasta conectada.
2. Se `$ARGUMENTS` trouxer URLs ou nomes, use-os. Senão, selecione os leads com status `novo` mais bem ranqueados — **mínimo de 5 clientes por lote** (se houver menos de 5 leads novos, use todos e avise que rodar `/prospectar` de novo aumenta o lote).
3. Confirme a lista com o usuário antes de começar.

## Para cada cliente do lote

1. **Extração**: abra o site original no Claude in Chrome (o sandbox costuma bloquear fetch direto a esses domínios). Extraia TODO o conteúdo real: textos, serviços, formação/credenciais, endereço, telefone/WhatsApp, e-mail, redes sociais, horários, paleta de cores e — OBRIGATÓRIO — as URLs reais do logo e das fotos (via JavaScript no navegador: colete `img.currentSrc` de todas as imagens; se forem lazy-load, role a página até o fim antes de coletar). Tire um screenshot do site original para referência.
2. **Redesign**: aplique a skill `redesign-premium` na íntegra. Regra de ouro: NADA inventado — é uma nova versão da página do cliente, não uma página nova. O logo original e as fotos originais DEVEM aparecer na página nova (se o cliente não tem site/logo, use composição tipográfica — nunca invente logo).
3. **Salvar** na pasta conectada, com o nome do cliente no arquivo para fácil identificação:
   - `sites/[slug]/[slug].html` — a página final (arquivo único, autocontido, responsivo)
   - `sites/[slug]/[slug]-editor.html` — a MESMA página com a camada de edição visual injetada antes de `</body>` (script completo em `references/editor-visual.md` da skill `redesign-premium`). Gere SEMPRE, sem esperar o usuário pedir.
4. **Comparador**: crie/atua