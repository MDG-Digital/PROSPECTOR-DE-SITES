---
description: Busca no Google Maps negócios bem avaliados (com ou sem site) e gera a lista de leads
argument-hint: "[nicho] [cidade] — opcional, usa os padrões do config"
---

Prospecte leads qualificados seguindo a skill `prospeccao-maps`.

## Preparação

1. Leia `prospector-config.json` na pasta conectada. Se não existir, oriente a rodar `/setup` primeiro.
2. Determine nicho e cidade: use os argumentos `$ARGUMENTS` se informados; senão, pergunte ao usuário qual dos nichos padrão do config usar (e confirme a cidade). O usuário SEMPRE pode trocar nicho e cidade na hora — nunca trave nos padrões.
3. **Dedup (OBRIGATÓRIO):** carregue o cadastro da fonte da verdade — `prospector.db` (e `leads.md`, se existir). Todo negócio já cadastrado (inclusive `descartado`) é EXCLUÍDO da nova busca. Valide cada candidato com `skills/prospeccao-maps/references/checar-cadastro.py` (casa por gmnCid, telefone/WhatsApp ou slug do nome) — só segue quem voltar `NOVO`. Nunca reprospecte cliente/lead existente.

## Execução

Use as ferramentas do Claude in Chrome (carregue via ToolSearch se necessário) para abrir o Google Maps e executar o fluxo completo descrito na skill `prospeccao-maps`:

- Buscar "[nicho] em [cidade]"
- Avaliar até 25 estabelecimentos ou até atingir o número de leads qualificados do config (padrão 10), o que vier primeiro
- Critério ouro: nota alta (≥ 4.0) + avaliações (≥ 20) + oportunidade de site (SEM site → criar o primeiro; COM site fraco → redesign) + pelo menos um canal de contato (e-mail, WhatsApp ou Instagram). Eliminatórios: site já moderno e bom → pula (baixa oportunidade); sem NENHUM contato (sem e-mail, sem WhatsApp e sem Instagram) → pula. Sempre registrar descartados com o motivo e seguir buscando até bater a meta
- Para cada candidato, abrir o site em nova aba e avaliar a qualidade seguindo os critérios da skill
- Coletar: nome, nota, nº de avaliações, telefone, **WhatsApp em formato 55DDDnúmero** (link wa.me no site ou celular do perfil do Maps — ver skill), e-mail, **Instagram (sempre, mesmo com site)**, **URL + CID do Google Meu Negócio**, URL do site (se houver) e o motivo objetivo (site fraco OU "não tem site próprio")

## Saída — Google Sheets + dashboard + cópia local

1. **Google Sheets**: salve os leads numa PLANILHA DO GOOGLE via conector do Google Drive — `create_file` com `contentMimeType: text/csv` e o CSV como `textContent` (a conversão automática cria uma planilha nativa do Sheets). Título: `Leads Prospector — [nicho] [cidade]`. Colunas: #, Nome, Nota, Avaliações, E-mail, Telefone, Site atual, Motivo, Situação (Qualificado/Descartado + motivo), Status, URL nova. Inclua TODOS os avaliados (qualificados E descartados), ranqueados por potencial (melhor nota + maior oportunidade: sem site, depois pior site). Retorne o link da planilha ao usuário.
2. **Cópia local**: mantenha `leads.md` na pasta conectada como cópia de trabalho (o conector do Drive não edita células — os status `novo → redesenhado → publicado → proposta enviada` são atualizados no leads.md local, e a planilha do Google é regenerada com os dados acumulados ao fim de cada comando que muda status). Em rodadas novas, some os leads novos aos antigos numa planilha só, nunca duplique cliente já avaliado.
3. **Dashboard**: crie/atualize `dashboard.html` na raiz da pasta conectada seguindo a skill `dashboard-leads` (template + merge do JSON embutido) — leads novos entram com `status: novo`, descartados com `status: descartado`. ⚠️ **Ao gravar cada lead, gere o `slug` pela REGRA ÚNICA da skill `dashboard-leads`** (slugify do nome do negócio, sem prefixo de nicho). Esse slug é a identidade imutável do lead — `/redesenhar`, `/publicar` e `/proposta` vão reusá-lo tal e qual para pasta, arquivos e URL.

A entrega final DEVE incluir a confirmação explícita "Dashboard atualizado: [N] leads" (criando o dashboard pela skill `dashboard-leads` se a pasta não tiver um — obrigatório, nunca pule). Mostre a tabela ao usuário com o link da planilha e do `dashboard.html`, e sugira o próximo passo: `/redesenhar` para os 5+ melhores leads.
