# Mapa de Atividades Acadêmicas
Mapa de Atividades, estilo kanban com tier list, com colunas de Ensino, Pesquisa e Extensão, para gameficação do ensino da tríade da universidade.

O objetivo é realizar explicações do que pode ser feito em cada uma destas bases da universidade. Este Mapa de Atividades pode ser empregado em complemento a uma atividade de world café, trazendo como post-it as sugestões escritas pelos estudantes nos cartazes.

Mapa gerado com auxílio do Large Language Model (LLM) Anthropic Claude.

## Como funciona

A página é estática (GitHub Pages) e tem **dois papéis**:

| Ação | Usuário comum (leitura) | Administrador (token) |
| --- | --- | --- |
| Ver os itens vindos do Gist | ✅ | ✅ |
| Arrastar itens entre categorias | ✅ (salvo só no navegador dele) | ✅ |
| Criar post-its e imagens | ❌ | ✅ (modal de Administração) |
| Remover itens / Limpar tudo / Importar | ❌ | ✅ |
| Publicar o estado no Gist | ❌ | ✅ |

- A **URL do Gist** vem do arquivo [`gist-config.json`](gist-config.json), publicado junto com a página. Nenhum visitante precisa configurar nada: ao abrir o site, o estado é lido desse Gist.
- O **administrador** entra com um Personal Access Token do GitHub (escopo `gist`) no modal **"⚙️ Administração"**. Só então a criação de cards e o botão de publicar aparecem.
- O token **nunca** é gravado no repositório — fica apenas no navegador do administrador (`sessionStorage`, ou `localStorage` se marcar "Lembrar token neste navegador").

## Funcionalidades

- 🎒 **Banco de Itens**: post-its coloridos e imagens, criados pelo administrador
- 📂 **Três Categorias**: Ensino, Pesquisa e Extensão
- 🖱️ **Drag & Drop**: qualquer visitante arrasta os itens durante a aula
- 💾 **Salvamento local**: a organização feita em aula fica no localStorage do navegador
- 🔄 **Recarregar**: volta ao estado publicado no Gist a qualquer momento
- ☁️ **Publicação no Gist**: o administrador grava o estado atual para toda a turma
- 📤 **Exportar/Importar**: backup em JSON (importar é restrito ao administrador)

## Como Usar

### Usuário comum (aula)

1. Abra a página — os itens já vêm do Gist publicado
2. Arraste os itens do banco para Ensino, Pesquisa ou Extensão
3. A organização é salva no navegador; **🔄 Recarregar** volta ao estado do Gist

### Administrador

1. Clique em **"⚙️ Administração"**
2. Cole o Personal Access Token (escopo `gist`) e clique em **"🔓 Entrar como administrador"**
3. Use a seção **"Criar itens"** do modal para adicionar post-its e imagens
4. Organize com drag & drop na página
5. Clique em **"☁️ Publicar estado no Gist"** — é o que todos passarão a ver

Para instruções detalhadas (criar o Gist, gerar o token, formato do JSON), veja [GIST_SETUP.md](GIST_SETUP.md).

## Configuração (`gist-config.json`)

```json
{
  "gistUrl": "https://gist.githubusercontent.com/usuario/ID/raw/arquivo.json",
  "gistId": "ID",
  "gistFile": "arquivo.json",
  "enabled": true
}
```

- `gistUrl`: URL raw **sem o SHA da revisão** — assim a página sempre lê a versão mais recente (se você colar a URL com SHA, a página remove o SHA automaticamente).
- `gistId` e `gistFile`: opcionais; se ausentes, são extraídos de `gistUrl`. São usados para publicar via API do GitHub.
- `enabled`: `false` desliga a leitura do Gist (usa só o armazenamento local).
- ⚠️ **Nunca** coloque o token neste arquivo — ele é público.
