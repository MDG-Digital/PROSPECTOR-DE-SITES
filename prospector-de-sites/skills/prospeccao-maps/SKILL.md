---
name: prospeccao-maps
description: Esta skill deve ser usada ao prospectar clientes no Google Maps — buscar negócios bem avaliados (COM OU SEM site), qualificar leads, avaliar a oportunidade (site fraco ou ausência de site) e capturar Instagram e dados do Google Meu Negócio para a planilha de leads. Acione quando o usuário disser "prospectar", "buscar clientes", "achar leads", "clientes com site ruim", "clientes sem site" ou rodar /prospectar.
---

# Prospecção no Google Maps

Encontrar o cliente ouro: negócio que JÁ fatura bem (nota alta, muitas avaliações) mas perde clientes por ter um site fraco OU por não ter site nenhum. Não se cria demanda — conserta-se (ou cria-se) a presença digital onde o dinheiro está escapando.

> ⚠️ **SLUG (identidade do lead — nasce aqui):** ao registrar cada lead no banco/dashboard, gere o `slug` pela **REGRA ÚNICA** da skill `dashboard-leads` (slugify do NOME do negócio: minúsculas, sem acento, sem prefixo de nicho — nada de `of-`/`cl-`). Esse slug é **imutável** e vira o nome da pasta (`sites/<slug>/`), dos arquivos e da URL pública — todos os comandos seguintes o **reutilizam** exatamente. Nunca deixe o slug ser re-derivado depois.

## Fluxo (via Claude in Chrome)

1. Abrir `https://www.google.com/maps` e buscar `[nicho] em [cidade]`.
2. Percorrer os resultados um a um, em ordem. Para cada estabelecimento:
   - Abrir o perfil e ler nota, nº de avaliações e link do site.
   - **Filtro 1 — potencial financeiro**: nota ≥ 4.0 E avaliações ≥ 20. Reprovou → próximo.
   - **Filtro 2 — canal de contato**: o lead precisa ter PELO MENOS UM canal para abordagem — e-mail, WhatsApp ou Instagram. Sem nenhum contato público → descartar (registrar o motivo) e seguir.
   - **Filtro 3 — oportunidade de site (COM OU SEM site)**: **Sem site** (ou fora do ar, ou "site" que é só diretório de terceiros/linktree) → **qualifica**; a oportunidade é criar o primeiro site próprio (conteúdo/fotos vêm do Instagram e do Google — ver `redesign-premium`), motivo = "não tem site próprio". **Com site**: abrir em nova aba e avaliar pelos critérios abaixo — site fraco (2+ problemas) → **qualifica** (motivo = os problemas); site já moderno e bom → descartar (baixa oportunidade), registrar o motivo.
3. Parar ao atingir a meta de leads qualificados (config, padrão 10) ou após avaliar 25 estabelecimentos.
4. Pular estabelecimentos que já estão em `leads.md` (avaliados em buscas anteriores).

## Critérios de site ruim (guardar o motivo específico)

Aplica-se aos leads QUE TÊM site (leads SEM site já qualificam pelo Filtro 3). Qualifica como lead se o site (ativo) tiver 2 ou mais destes problemas:

- Layout datado (aparência de template de 10+ anos, fontes de sistema, imagens esticadas/pixeladas)
- Sem CTA claro de agendamento/contato (nenhum botão de WhatsApp ou agenda visível na primeira dobra)
- Domínio gratuito ou hospedado em plataforma alheia (Google Sites, Wix grátis, subdomínio de terceiros com marca da plataforma)
- Não responsivo (quebra no mobile)
- Conteúdo desorganizado: serviços escondidos, sem hierarquia, texto corrido sem seções
- Sem prova social (nenhuma avaliação/depoimento, apesar da nota alta no Google)

O motivo anotado deve ser objetivo e verificável — ele será citado na proposta. Ex.: "domínio redireciona para Google Sites gratuito, template básico, sem CTA de agendamento".

## Coleta por lead

Nome, nota, nº de avaliações, telefone, WhatsApp, e-mail, URL do site, motivo, **Instagram** e **URL do Google Meu Negócio**.

**WHATSAPP: capture SEMPRE, separado do telefone.** Fontes, na ordem: botão/link de WhatsApp no site do lead (procure `wa.me/`, `api.whatsapp.com` ou ícone de WhatsApp — extraia o número do link); telefone celular do perfil do Maps (números com 9º dígito são celular no Brasil — assuma WhatsApp). Registre no formato internacional `55 + DDD + número` (ex.: `5511999990000`), pronto pra `wa.me`. O WhatsApp alimenta os botões do dashboard e o plano B de abordagem quando o e-mail não responde.

**CONTATO: pelo menos UM canal é obrigatório (e-mail preferido).** O e-mail é o canal ideal (a proposta padrão vai por e-mail), mas NÃO é mais eliminatório sozinho — um lead só com WhatsApp ou Instagram também fecha o ciclo (proposta por WhatsApp/DM, ver `proposta-email`). Procure o e-mail nesta ordem: site (rodapé e página de contato), links `mailto:`, busca no Google por "[nome] + email/contato". Sem e-mail mas com WhatsApp ou Instagram → **mantenha o lead** e marque o canal de abordagem. Só descarte por contato quando NÃO houver e-mail, nem WhatsApp, nem Instagram. Atenção: "site" que aponta para diretório de terceiros (localtreino, acheioprofissional etc.) não conta como site próprio — trate como SEM site (qualifica pelo Filtro 3).

**INSTAGRAM — SEMPRE, para TODO lead (com ou sem site) (captura + revisão manual).** Capture o @ ou a URL do perfil MESMO quando o lead já tem site — o Instagram é fonte rica de fotos, serviços, horários e novidades que alimentam a criação/redesign da página. Procure nesta ordem: site do lead (ícone/link do Instagram no cabeçalho ou rodapé — pegue o `instagram.com/...`), perfil do Google Maps, e busca `[nome] [cidade] instagram`. Ao abrir o perfil, anote sinais úteis (nº de seguidores, se está ativo, principais serviços/fotos). Grave o @ ou a URL em `instagram`. Se não achar com segurança, deixe em branco — o usuário revisa depois no dashboard.

**GOOGLE MEU NEGÓCIO — link + CID (captura + revisão manual).** O objetivo é um link que abre o perfil COMPLETO do negócio em 1 clique, mais uma chave estável pra não prospectar o mesmo lugar 2x. Faça assim, com o perfil do negócio ABERTO no Google Maps:

1. **Leia a URL da barra de endereço** — ela tem o formato `.../maps/place/Nome/@lat,lng,zoom/data=!...!1s0x<HEX_A>:0x<HEX_B>!...`.
2. **Extraia o CID:** é o valor logo depois dos dois-pontos, `0x<HEX_B>`. Converta esse hexadecimal para DECIMAL — o número decimal é o CID. Grave o número puro em `gmnCid`.
3. **Monte o `gmnUrl` no formato preferido:** `https://www.google.com/maps?cid=<CID>` (limpo, estável, abre o painel completo). Antes de gravar, ABRA esse link numa aba nova e confirme que caiu no negócio certo (revisão).
4. **Fallbacks quando não der pra extrair o CID:** grave em `gmnUrl` a própria URL longa `/maps/place/...` da barra, OU o link do botão **Compartilhar → Copiar link** (`maps.app.goo.gl/...`), e deixe `gmnCid` vazio.
5. Se nada abrir com segurança o perfil, deixe os dois campos vazios para revisão manual depois.

> Observação técnica: o `0x<HEX_A>` antes dos dois-pontos é o Feature ID (auxiliar); o Place ID oficial (`ChIJ...`) NÃO aparece na URL — só via API Places, por isso não é usado aqui.

## Saída — Google Sheets + leads.md local

Destino principal: PLANILHA DO GOOGLE (via conector do Google Drive: `create_file` com CSV em `textContent` e `contentMimeType: text/csv` — converte automaticamente para Sheets). Título `Leads Prospector — [nicho] [cidade]`; incluir qualificados e descartados, ranqueados por potencial (nota alta + maior oportunidade: sem site, depois site pior). Entregar o link ao usuário.

Cópia de trabalho local `leads.md` (mesmas colunas) para controle de status, já que o conector do Drive não edita células:

```markdown
| # | Nome | Nota | Aval. | E-mail | Telefone | Site atual | Motivo | Status | URL nova |
```

Status possíveis: `novo`, `redesenhado`, `publicado`, `proposta enviada`. Quando um status mudar (redesenhar/publicar/proposta), regenerar a planilha do Google com os dados acumulados e atualizar o `dashboard.html` (skill `dashboard-leads`). Nunca sobrescrever leads antigos — apenas acrescentar e atualizar.

## Boas práticas

- Trabalhar por região dá vantagem: menos concorrência na oferta e conhecimento local.
- Enquanto o navegador trabalha, não interromper o fluxo com perguntas — só reportar a tabela final.
- Se o Google Maps pedir login/captcha, pausar e avisar o usuário.
