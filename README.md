# apresentacoes-moura-siqueira

Skill do Claude Code para criar e revisar apresentações, decks, relatórios visuais e cases da
Moura & Siqueira Consultoria. Entrega sempre dois arquivos: um HTML interativo de slides
(1920×1080) e o PDF correspondente.

## Instalar

Copie a pasta da skill para o diretório de skills do Claude Code, mantendo o nome
`apresentacoes-moura-siqueira`.

**Windows (PowerShell)**

```powershell
git clone https://github.com/Mylla29/apresentacoes-moura-siqueira.git "$env:USERPROFILE\.claude\skills\apresentacoes-moura-siqueira"
```

**macOS / Linux**

```bash
git clone https://github.com/Mylla29/apresentacoes-moura-siqueira.git ~/.claude/skills/apresentacoes-moura-siqueira
```

Reinicie o Claude Code. Para atualizar depois, rode `git pull` dentro dessa pasta.

## Usar

Peça algo como "crie uma apresentação de cases para o cliente X" ou "revise este deck no padrão
da casa". A skill é carregada sozinha quando o pedido envolve apresentações da Moura & Siqueira.

## Antes de usar

- **Logo:** a skill precisa das duas versões do logo (colorida e branca), em PNG transparente ou
  SVG. Coloque em uma pasta `marca/` dentro da pasta de trabalho ou em `~/MouraSiqueira/marca/`.
  Os arquivos do logo não fazem parte deste repositório.
- **PDF:** é gerado a partir do próprio HTML, com Playwright ou com Microsoft Edge/Google Chrome
  em modo headless.

## Conteúdo

- `SKILL.md`: o guia operacional (entrega, interatividade, história, logo, paleta, tipografia,
  composição, gráficos, tipos de slide, proibições e checklist final).
