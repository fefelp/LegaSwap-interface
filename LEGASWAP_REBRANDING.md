# LEGASWAP REBRANDING

Guia de rebranding e reestilização para a interface — branch: feature/legaswap-rebrand

Objetivo
- Reestilizar completamente a interface para a identidade LegaSwap sem alterar lógica, fluxos de carteira ou conectores.
- Fornecer um guia claro para qualquer IA/automação que for conectada ao repositório via terminal.

Assets já enviados neste repositório (use estes arquivos como fonte):
- image_3a20842b.png
- image_c78b33d4.png
- image_daba9118.png

Paleta de cores (usar exatamente):
- Fundo principal da aplicação: #060b13
- Fundo de cards/painéis/diálogos: #0d1522
- Cor primária / títulos: #0D2137 (Azul escuro naval)
- Cor secundária / links: #1A7F8E (Teal/Ciano)
- Cor de destaque para botões de ação, gradientes e hovers: #00e5ff (Ciano brilhante)
- Tipografia principal (cor do texto): #FFFFFF (branco)

Tipografia
- Usar uma sans-serif moderna: Inter (preferível) ou Geist.
- Garantir fallback: `font-family: 'Inter', system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial`.

Regras gerais
- Dark Mode deve ser o padrão absoluto.
  - Adicionar `class="dark"` no elemento `<html>` do `index.html` para forçar o modo escuro por padrão.
  - `tailwind.config.ts` já usa `darkMode: 'class'`. O arquivo de estilos deve assumir as variáveis CSS para o tema dark.
- Não alterar nenhuma lógica de wallet/connect (wagmi/wagmi connectors, ethers, wagmi core). Só mexer em estilos, assets e strings de UI.
- Substituir todas as ocorrências textuais de "GMX" por "LegaSwap" (case-sensitive para as ocorrências óbvias). Revisar variações (Gmx, gmx) manualmente.

Arquivos prioritários a serem modificados
1. Configuração de cores e tokens do design
   - `src/config/colors.ts` (substituir valores para refletir a paleta LegaSwap)
   - `tailwind.config.ts` (verificar que os `cssVariables.dark` usam as novas cores e que `darkMode: 'class'` permanece)
   - `landing/tailwind.config.ts` (mesma atualização para a landing page)

2. Forçar Dark Mode por padrão
   - `index.html` — adicionar `class="dark"` ao `<html>` para desenvolvimento/testing e instruir a remoção condicional se desejar toggle runtime.

3. Substituição textual
   - Buscar e substituir em todo o repositório (somente em arquivos de frontend e conteúdo estático):
     - `git grep -n "GMX"` para localizar ocorrências
     - `git grep -n "gmx"` para localizar nomes de arquivos/rotas minúsculas
     - Revisar arquivos localizados antes de aplicar substituições automáticas
   - Sugestão de comando (revise antes de executar):
     - `git grep -l "\bGMX\b" | xargs sed -i 's/\bGMX\b/LegaSwap/g'`
     - `git grep -l "\bgmx\b" | xargs sed -i 's/\bgmx\b/legaswap/g'`
   - Preferível usar um codemod ou script Node que altere apenas strings em JSX/TSX/MD, evitando alterar imports de pacotes/external IDs.

4. Assets (logos / favicon)
   - Localizar logos originais (padrões comuns): `public/`, `public/img`, `src/assets`, `src/img`, `src/icons`, `landing/public`.
   - Comandos úteis:
     - `git ls-files | grep -Ei "logo|gmx|favicon|icon|brand|svg|png|jpg"`
     - `git grep -n "gmx" -- '*.{png,svg,jpg,ico,md,html,tsx,jsx,ts,js}'`
   - Substituir arquivos de logo por: `image_3a20842b.png`, `image_c78b33d4.png`, `image_daba9118.png` conforme o contexto:
     - `image_3a20842b.png` → logo principal (header, app bar)
     - `image_c78b33d4.png` → ícone da rede / token
     - `image_daba9118.png` → favicon / small icon
   - Atualizar referências em código (`<img src="/...">`, imports TypeScript `import logo from '...';`) para apontarem para os novos arquivos.
   - Para o favicon: substituir `public/favicon.ico` ou atualizar `index.html` `link rel="icon" href="/path/to/image_daba9118.png"` (SVG/PNG aceitos, mas preferir ICO para compatibilidade ou gerar ICO a partir do PNG).

5. Componentes de UI e classes Tailwind
   - Atualizar classes utilitárias/paleta customizada para usar variáveis CSS com os hexes acima.
   - Garantir que botões principais `Connect Wallet`, `Buy`, `Sell`, `Swap`, etc. usem o destaque `#00e5ff` para estados default/hover/active, ou gradientes baseados nesse tom.
   - Atualizar utilitários personalizados no `tailwind.config.ts` (plugins) se necessário.

Execução sugerida (passo-a-passo)
1. Já existe o branch `feature/legaswap-rebrand`. Trabalhe nele localmente:
   - git fetch origin
   - git checkout feature/legaswap-rebrand
2. Atualize `src/config/colors.ts` com a paleta LegaSwap.
3. Atualize `tailwind.config.ts` e `landing/tailwind.config.ts` para refletir as cores e garantir `darkMode: 'class'`.
4. Adicione `class="dark"` em `index.html` para desenvolvimento.
5. Rodar build/servidor local e revisar visual:
   - yarn
   - yarn start-app  # abre em :3011 (ver package.json)
6. Procurar e substituir textos "GMX" por "LegaSwap":
   - git grep -n "GMX" && git grep -n "gmx"
   - Revisar e aplicar substituições com cuidado (preferível fazer manualmente por arquivo para evitar regressões).
7. Substituir logos e favicon utilizando os assets já enviados (veja mapeamento acima).
8. Testar flows críticos:
   - Conectar carteira (MetaMask / WalletConnect)
   - Abrir telas de swap, deposit, withdraw e confirmar que as integrações com Li.Fi / LayerZero não quebraram
   - Testar histórico e balance display
9. Commitar alterações por tópico (cores, logos, strings) e push no branch `feature/legaswap-rebrand`.
10. Abrir PR para `release` com descrição clara do que foi alterado e screenshots comparativas.

Boas práticas e precauções
- Nunca editar contratos, endpoints ou chaves secretas neste processo.
- Evitar alterações em arquivos do `sdk/` que afetem apis/nomes exportados a menos que seja estritamente necessário.
- Se for usar substituição automática, crie um commit de backup antes (`git commit -m "backup before mass replace" --allow-empty`) e use um branch separado.
- Verifique CI e lint (yarn lint) antes de abrir PR.

Comandos úteis rápidos
- Criar branch local baseado no remoto:
  - `git fetch origin && git checkout -b feature/legaswap-rebrand origin/feature/legaswap-rebrand`
- Localizar arquivos com "GMX":
  - `git grep -n "\bGMX\b"`
- Substituir (exemplo, revisar antes):
  - `git grep -l "\bGMX\b" | xargs sed -i 's/\bGMX\b/LegaSwap/g'`
- Listar imagens no repo:
  - `git ls-files | grep -Ei "\.(png|svg|ico|jpg|jpeg)"`

Notas finais
- Este arquivo serve como guia operacional para qualquer IA ou dev humano que automatize a rebrand via terminal.
- Mantenha o foco em alterar apenas o visual e as strings locais — NÃO altere integrações de rede, contratos, endpoints RPC ou credenciais.

Se quiser, eu já posso aplicar as mudanças nos arquivos de configuração de cores (ex: `src/config/colors.ts`, `tailwind.config.ts`) e substituir as strings "GMX" por "LegaSwap" automaticamente neste branch — confirme se quer que eu execute esses passos agora. 
