---
description: Busca no Google Maps negócios bem avaliados com sites ruins e gera a lista de leads
argument-hint: "[nicho] [cidade] — opcional, usa os padrões do config"
---

Prospecte leads qualificados seguindo a skill `prospeccao-maps`.

## Preparação

1. Leia `prospector-config.json` na pasta conectada. Se não existir, oriente a rodar `/setup` primeiro.
2. Determine nicho e cidade: use os argumentos `$ARGUMENTS` se informados; senão, pergunte ao usuário qual dos nichos padrão do config usar (e confirme a cidade). O usuário SEMPRE pode trocar nicho e cidade na hora — nunca trave nos padrões.
3. Leia `leads.md` na pasta conectada (se existir) para saber quais profissionais já foram avaliados — estes devem ser EXCLUÍDOS da nova busca.

## Execução

Use as ferramentas do Claude in Chrome (carregue via ToolSearch se necessário) para abrir o Google Maps e executar o fluxo completo descrito na skill `prospeccao-maps`:

- Buscar "[nicho] em [cidade]"
- Avaliar até 25 estabelecimentos ou até atingir o número de leads qualificados do config (padrão 10), o que vier primeiro
- Critério ouro: nota alta (≥ 4.7) + muitas avaliações (≥ 40) + site ATIVO porém ruim + e-mail público. Os três eliminatórios: sem site (ou site fora do ar/diretório de terceiros) → pula; site bom → pula; sem e-mail → pula. Sempre registrar descartados com o motivo e seguir buscando até bater a meta
- Para cada candidato, abrir o site em nova aba e avaliar a qualidade seguindo os critérios da skill
- Coletar: nome, nota, nº de avaliações, telefone/WhatsApp, e-mail, URL do site e o motivo objetivo pelo qual o site é ruim

## Saída — Google Sheets (obrigatório) + cópia local

1. **Google Sheets**: salve os leads numa PLANILHA DO GOOGLE via conector do Google Drive 