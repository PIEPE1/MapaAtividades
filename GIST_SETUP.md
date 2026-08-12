# Configuração de Sincronização com Gist

Este guia explica como configurar a sincronização de estados de cards com um Gist do GitHub.

## O que é?

O sistema de Gist permite:
- **Salvar estados de cards** em um Gist do GitHub de forma centralizada
- **Compartilhar configurações** entre múltiplos usuários
- **Versionar históricos** de estados
- **Sincronizar automaticamente** ao carregar a página

## Pré-requisitos

1. Conta no GitHub
2. Um Gist existente (que você criará)
3. Um Token de Acesso Pessoal do GitHub (Personal Access Token)

## Como Configurar

### 1. Criar um Gist

1. Acesse [gist.github.com](https://gist.github.com)
2. Clique em "Create a new gist"
3. Crie um arquivo com a extensão `.json` (exemplo: `tierlist-state.json`)
4. Adicione o conteúdo JSON inicial:

```json
{
  "staging-pool": [],
  "tier-ensino": [],
  "tier-pesquisa": [],
  "tier-extensao": []
}
```

5. Clique em "Create public gist" ou "Create secret gist" (recomendado secret para dados sensíveis)
6. Copie a **URL RAW** do arquivo. Exemplo:
   ```
   https://gist.githubusercontent.com/seu-usuario/abc123def456/raw/nome-do-arquivo.json
   ```

### 2. Gerar Token de Acesso Pessoal

1. Acesse [github.com/settings/tokens](https://github.com/settings/tokens)
2. Clique em "Generate new token"
3. Escolha o escopo `gist` (para leitura/escrita de gists)
4. Copie o token (você não verá novamente!)
5. Guarde este token em um local seguro

### 3. Configurar na Aplicação

1. Abra o site da Mapa de Atividades Acadêmicas
2. Clique no botão **"⚙️ Administração Gist"** no topo
3. Preencha os campos:
   - **URL do Gist**: Cole a URL RAW que você copiou
   - **Token de Acesso**: Cole o GitHub Personal Access Token
4. Clique em **"🔍 Validar & Carregar"**
5. Se tudo funcionar, clique em **"💾 Salvar Configuração"**

## Usando o Sistema

### Carregar Estado do Gist

**Na primeira visita ou após configuração:**
- Acesse o modal de administração
- Configure a URL do Gist e o token (se necessário)
- Clique em "🔍 Validar & Carregar"

**Em recarregamentos da página:**
- Se o Gist é **público**: O sistema carrega automaticamente
- Se o Gist é **privado**: Você precisará fornecer o token novamente
  - Abra o modal de administração
  - Preencha o token (URL já estará preenchida)
  - Clique em "🔍 Validar & Carregar"

⚠️ **Nota sobre token**: O token é mantido apenas em memória durante a sessão. Para sua segurança, ele **não é armazenado** e será esquecido ao recarregar a página.

### Salvar Alterações no Gist

Para salvar seu estado atual no Gist:
1. Abra o Gist diretamente no GitHub
2. Use o botão **"📤 Exportar"** da aplicação
3. Copie o conteúdo JSON e atualize seu Gist manualmente

### Criar Múltiplos Estados

Você pode ter múltiplos Gists para diferentes cenários:
- `aula-teoria.json` - Para aulas teóricas
- `aula-prática.json` - Para aulas práticas
- `semestre-novo.json` - Template para novo semestre

Basta alterar a URL do Gist na configuração para trocar entre eles.

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

### Tipos de Items

- **postit**: Post-it com texto
  - `text`: Conteúdo do post-it (máx 25 caracteres)
  - `color`: Cor em formato hex
  - `rotation`: Rotação em graus (ex: "2.5deg")

- **image**: Imagem
  - `src`: URL da imagem ou Data URL (base64)

## Troubleshooting

### "Erro: URL do Gist inválida"
- Verifique se a URL é a **RAW** (contém `/raw/`)
- Confirme que a URL termina com `.json`

### "Erro: HTTP 404"
- O Gist não existe ou foi deletado
- Verifique a URL e se o Gist é público ou se o token está correto

### "Erro: HTTP 401"
- Token expirou ou é inválido
- Gere um novo token no GitHub

### Configuração não carrega ao recarregar a página
- Verifique se você clicou em **"💾 Salvar Configuração"**
- Verifique o console do navegador (F12) para mensagens de erro
- Se a URL/token estiverem incorretos, o sistema volta ao localStorage

### CORS error
- Alguns Gists privados podem ter problemas de CORS
- Tente usar um Gist público, ou
- Verifique se o navegador permite requisições cross-origin

## Segurança

⚠️ **Importante**: 
- **O token NUNCA é armazenado** no navegador - apenas em memória durante a sessão atual
- Ao recarregar a página, você precisará fornecer o token novamente
- Isso garante que o token sensível não fica vulnerável no localStorage
- Gists públicos funcionarão após reload sem necessidade de token
- Gists privados precisam ser configurados novamente após reload da página
- Use "Secret gist" se precisar fazer login com token

## Dicas Avançadas

### Versioning
Use a data/hora no nome do arquivo para versionamento:
- `tierlist-2024-08-12-v1.json`
- `tierlist-2024-08-12-v2.json`

### Backup
Periodicamente, exporte seu estado atual e salve como backup local.

### Colaboração
Múltiplos usuários podem usar a mesma URL de Gist, mas **precisam ter acesso** ao repositório.

## Limpar Configuração

Para remover a configuração:
1. Abra **"⚙️ Administração Gist"**
2. Clique em **"🗑️ Limpar Config"**
3. Confirme na caixa de diálogo

Isso não deleta o Gist no GitHub, apenas remove a configuração local.

---

**Dúvidas?** Consulte a documentação oficial do GitHub sobre [Gists](https://docs.github.com/en/get-started/writing-on-github/editing-and-sharing-content-with-gists).
