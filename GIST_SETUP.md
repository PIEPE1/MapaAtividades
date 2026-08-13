# Configuração de Sincronização com Gist

Este guia explica como publicar o estado do Mapa de Atividades em um Gist do GitHub.

## Modelo de uso

- **A página lê a URL do Gist no arquivo `gist-config.json`**, que é publicado junto com o site (GitHub Pages). Quem abre o site não configura nada: os cards já vêm preenchidos com o estado do Gist.
- **Somente o administrador**, com um Personal Access Token do GitHub, pode criar/remover itens e publicar o estado no Gist.
- **O token nunca vai para o repositório.** Ele fica só no navegador de quem administra.

```text
gist-config.json (público, no repo)  ──►  página lê o Gist  ──►  todos os visitantes
                                                  ▲
                          token no navegador ──────┘ (só o administrador publica)
```

## Passo 1 — Criar o Gist

1. Acesse [gist.github.com](https://gist.github.com)
2. Crie um arquivo `.json` (ex.: `MapaPIEPE.json`) com o conteúdo inicial:

```json
{
  "staging-pool": [],
  "tier-ensino": [],
  "tier-pesquisa": [],
  "tier-extensao": []
}
```

3. Clique em **"Create secret gist"** (recomendado) ou **"Create public gist"**
   - **Secret**: não aparece em buscas/perfil, mas a URL raw continua acessível por quem tem o link — é exatamente isso que permite a leitura sem token.
   - Em ambos os casos, **editar** o Gist exige o token do dono.
4. Clique no botão **"Raw"** e copie a URL.

## Passo 2 — Preencher `gist-config.json`

A URL do botão "Raw" costuma vir com o SHA da revisão:

```text
https://gist.githubusercontent.com/usuario/ID/raw/<SHA>/MapaPIEPE.json
```

Essa URL **congela** a versão. Remova o SHA para sempre buscar a versão mais recente:

```json
{
  "gistUrl": "https://gist.githubusercontent.com/usuario/ID/raw/MapaPIEPE.json",
  "gistId": "ID",
  "gistFile": "MapaPIEPE.json",
  "enabled": true
}
```

- `gistId` e `gistFile` são opcionais (extraídos de `gistUrl` quando ausentes), mas deixá-los explícitos evita surpresas.
- Se você colar a URL com o SHA, a página remove o SHA automaticamente ao carregar.
- `enabled: false` desliga a leitura do Gist e a página passa a usar apenas o armazenamento local.

Faça commit do `gist-config.json` — é ele que o GitHub Pages vai servir.

## Passo 3 — Gerar o token do administrador

1. Acesse [github.com/settings/tokens](https://github.com/settings/tokens)
2. **Generate new token** (classic) com o escopo **`gist`**
   - Fine-grained tokens também funcionam, desde que tenham permissão de **Gists: Read and write**
3. Copie o token (ele só aparece uma vez)

O token precisa pertencer à **mesma conta dona do Gist** — a página verifica isso no login e recusa tokens de outra conta.

## Passo 4 — Usar como administrador

1. Abra o site e clique em **"⚙️ Administração"**
2. Cole o token em **"Token de Acesso (GitHub)"**
   - Marque **"Lembrar token neste navegador"** só em máquina pessoal (`localStorage`). Sem marcar, o token vive apenas na aba atual (`sessionStorage`).
3. Clique em **"🔓 Entrar como administrador"** — o selo no topo muda de `leitura` para `admin`
4. Use a seção **"Criar itens"** do modal para adicionar post-its e imagens
5. Organize os itens com drag & drop na página
6. Clique em **"☁️ Publicar estado no Gist"**

Para encerrar, clique em **"🔒 Sair"** (apaga o token do navegador).

## Como o estado é carregado

Ao abrir a página:

1. Lê `gist-config.json`
2. Busca o JSON pela URL raw (sem cache); se falhar, tenta a API do GitHub
3. Compara com o que está salvo no navegador:
   - **Mesma versão do Gist** → mantém a organização local (o que a turma arrastou na aula continua lá)
   - **Versão diferente** (o administrador publicou algo novo) → aplica o estado do Gist
4. Se o Gist não puder ser lido, cai para o estado local

O botão **"🔄 Recarregar"** força a volta ao estado publicado, descartando a organização local.

## Estrutura do JSON

```json
{
  "staging-pool": [
    {
      "type": "postit",
      "text": "Texto do post-it",
      "color": "#ffe066",
      "rotation": "2.5deg"
    },
    {
      "type": "image",
      "src": "data:image/png;base64,..."
    }
  ],
  "tier-ensino": [],
  "tier-pesquisa": [],
  "tier-extensao": []
}
```

### Tipos de itens

- **postit**
  - `text`: conteúdo (máx. 25 caracteres)
  - `color`: cor em hex
  - `rotation`: rotação (ex.: `"2.5deg"`)
- **image**
  - `src`: URL da imagem ou Data URL (base64)

⚠️ Imagens em base64 vão inteiras para o Gist. Muitas imagens grandes deixam o carregamento lento e podem estourar o limite do Gist — prefira URLs de imagens hospedadas quando possível.

## Troubleshooting

### "Token inválido ou expirado (401)"

Token errado, revogado ou expirado. Gere outro em [github.com/settings/tokens](https://github.com/settings/tokens) com escopo `gist`.

### "Sem permissão — verifique o escopo 'gist' (403)"

O token existe mas não tem permissão de gist, ou você atingiu o rate limit da API.

### "Gist não encontrado ou token sem acesso a ele (404)"

O `gistId` está errado, o Gist foi deletado, ou o token é de outra conta.

### "O token pertence a X, mas o Gist é de Y"

Gists só podem ser editados pelo dono. Use o token da conta que criou o Gist.

### A página não carrega os itens

- Abra a URL de `gistUrl` direto no navegador: deve mostrar o JSON bruto
- Confira o Console (F12) — a mensagem de falha aparece também no modal, em "Origem dos dados"
- Verifique se o JSON tem as quatro chaves de zona (`staging-pool`, `tier-ensino`, `tier-pesquisa`, `tier-extensao`)

### Publiquei, mas os visitantes veem a versão antiga

- Confirme que `gistUrl` **não** contém o SHA da revisão
- Peça para clicarem em **"🔄 Recarregar"** (a página já busca com `cache: no-store`, mas o CDN do GitHub pode demorar alguns segundos)

## Segurança

- `gist-config.json` é **público**: nunca coloque token nele.
- O token fica em `sessionStorage` (apaga ao fechar a aba) ou `localStorage` (se marcar "Lembrar").
- Em computador compartilhado (sala de aula), **não** marque "Lembrar" e clique em **"🔒 Sair"** ao final.
- Use um token com escopo mínimo (`gist`) e regenere periodicamente.
- O modo leitura não expõe o token: quem não tem token não consegue editar o Gist, mesmo alterando a página no próprio navegador.

---

**Dúvidas?** Consulte a documentação oficial do GitHub sobre [Gists](https://docs.github.com/en/get-started/writing-on-github/editing-and-sharing-content-with-gists).
