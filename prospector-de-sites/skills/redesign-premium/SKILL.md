---
name: redesign-premium
description: Esta skill deve ser usada ao redesenhar o site de um cliente prospectado — criar uma versão nova, premium e de alta conversão da página existente, mantendo conteúdo, logo e paleta do cliente. Acione quando o usuário disser "redesenhar site", "melhorar página", "refazer o site do cliente" ou rodar /redesenhar ou /editor.
---

# Redesign premium de páginas

Criar uma NOVA VERSÃO da página do cliente — não uma página nova. O cliente precisa reconhecer o próprio negócio, só que elevado ao padrão que o faturamento dele merece.

## Regras invioláveis

1. **Nenhum FATO inventado — mas o texto deve ser APRIMORADO.** Todo serviço, credencial, número, endereço e contato vem do site original (ou do perfil do Google). Sem dados fictícios, sem depoimentos criados, sem serviços que o cliente não oferece. Porém o TEXTO não é copiado cru: reescreva com copy melhor — títulos mais fortes, frases mais claras, hierarquia de leitura — sempre dizendo a mesma verdade que o original diz.
2. **Fotos e logo originais são OBRIGATÓRIOS no site novo.** Toda foto utilizável do site existente (profissional, consultório, logo) deve constar na página nova, pelas URLs originais (colete via `img.currentSrc` no navegador, rolando a página inteira para vencer lazy-load). O cliente precisa se reconhecer na hora.
3. **Identidade preservada.** Manter logo, paleta de cores e fotos do cliente. Se a paleta original for fraca (ex.: cores puras saturadas), refinar os tons — nunca trocar a família de cores.
4. **Mais completo que o original.** O site novo deve ser MUITO mais profissional e bem estruturado. Se o original tem poucas seções, CRIE as seções relevantes que faltam — desde que preenchidas só com informação real: prova social (nota + avaliações reais do Google), "como funciona o atendimento" (se dedutível do original), localização com mapa, horários (do perfil do Maps), FAQ com dúvidas respondíveis pelo conteúdo real. Seção que exigiria inventar fato = não criar.
5. **Arquivo único.** `sites/[slug]/[slug].html` autocontido: CSS inline no `<head>`, sem build, sem dependências além de Google Fonts.
6. **Mobile-first.** A página será aberta pelo cliente no celular. Testar mentalmente cada seção em 375px.
7. **Editor sempre.** Todo redesign gera junto o `sites/[slug]/[slug]-editor.html` (camada de edição de `references/editor-visual.md`) — nunca entregar página sem a versão editável.
8. **Comparador sempre.** Todo lote de redesign termina com `comparar.html` na raiz da pasta conectada, gerado a partir de `references/comparador-template.html` (substituir `__CLIENTES__` pelo array JSON; mesclar com clientes já existentes). A entrega padrão de cada cliente são 3 arquivos: página + editor + aba no comparador.

## Estrutura da página (adaptar à profissão)

1. **Hero**: nome + especialidade, promessa clara em 1 linha, CTA primário (WhatsApp) visível sem rolar, foto do profissional/clínica.
2. **Prova social**: nota do Google em destaque ("5.0 ★ · 121 avaliações no Google") — é real e verificável. Citar 2-3 trechos de avaliações reais do Google Maps se coletados.
3. **Serviços/áreas de atuação**: cards clicáveis — cada card leva à âncora da seção detalhada ou direto ao WhatsApp com mensagem pré-preenchida (`https://wa.me/55DDDNUMERO?text=Olá! Vim pelo site e quero saber sobre [serviço]`).
4. **Sobre**: formação e credenciais reais (geram autoridade — nunca cortar).
5. **Oferta estruturada** (quando fizer sentido): transformar "agende uma consulta" em opções de engajamento (ex.: sessão pontual, acompanhamento 90 dias, plano semestral) — SEM preços, apenas nomes e o que incluem, todos levando ao WhatsApp. Só criar planos que sejam agrupamento óbvio do serviço já oferecido.
6. **Localização e c