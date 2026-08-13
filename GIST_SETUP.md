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

5. Clique em "Create public gist" ou "Create secret gist" 
   - **Public**: Qualquer um com o link pode acessar
   - **Secret**: Apenas com o link + token (recomendado para dados sensíveis)
   
6. **Copiar a URL RAW corretamente:**
   - Na página do Gist, clique no botão "Raw" 
   - A URL será algo como: `https://gist.githubusercontent.com/seu-usuario/abc123def456/raw/nome-do-arquivo.json`
   - Copie esta URL completa

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

**Ao recarregar a página:**
- O sistema tenta carregar automaticamente usando URL e token salvos
- Se for Gist público, funcionará automaticamente
- Se for Gist privado, usará o token salvo em localStorage
- Se falhar, volta ao localStorage local

**Para reconfigurar:**
1. Clique em **"⚙️ Administração Gist"**
2. Modifique URL ou token conforme necessário
3. Clique em **"🔍 Validar & Carregar"** para testar
4. Clique em **"💾 Salvar Configuração"** para persistir

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

### "Erro de rede - Verifique: 1) URL do Gist..."
- Certifique-se de usar a **URL RAW** do Gist (contém `/raw/`)
- Verifique sua **conexão com internet**
- Tente copiar e colar a URL do navegador (deve abrir o JSON)
- Se usar Gist secreto, **o token é obrigatório**

### "Token inválido ou expirado (HTTP 401)"
- Seu token expirou ou foi revogado
- Regenere um novo token em [github.com/settings/tokens](https://github.com/settings/tokens)
- Verifique se tem escopo `gist` ativado

### "Acesso negado (HTTP 403)"
- Você não tem permissão para acessar este Gist
- Se for Gist secreto, precisa do token correto
- Verifique se o token pertence à conta que criou o Gist

### "Gist não encontrado (HTTP 404)"
- O Gist foi deletado
- A URL está incorreta
- Copie a URL diretamente do Gist no navegador

### Formato JSON inválido
- Gist deve ter a estrutura correta:
```json
{
  "staging-pool": [],
  "tier-ensino": [],
  "tier-pesquisa": [],
  "tier-extensao": []
}
```
- Cada zona pode conter post-its ou imagens
- Use o botão **"📤 Exportar"** para ver o formato correto

## Segurança

⚠️ **Importante**: 
- **URL e token são salvos localmente** no localStorage do navegador para conveniência
- Dados são armazenados no seu computador/navegador, não em servidores terceirizados
- Para máxima segurança com Gists privados, considere:
  - Usar "Secret gist" (não publicamente visível)
  - Usar navegador privado/incógnito se em computador compartilhado
  - Regenerar o token periodicamente no GitHub
  - Usar um token com escopo limitado (apenas `gist` se possível)
- **Nunca compartilhe seu token pessoal**
- Para remover os dados salvos, use **"🗑️ Limpar Config"**

## Dicas Avançadas

### Testando a URL
Antes de salvar no aplicativo, copie a URL RAW do Gist e abra em uma abinha do navegador. Deve mostrar o JSON bruto.

### Regenerar Token Expirado
1. Vá para [github.com/settings/tokens](https://github.com/settings/tokens)
2. Clique na token antiga e delete (🗑)
3. Clique "Generate new token"
4. Selecione escopo `gist`
5. Copie o novo token e atualize no modal de administração

### Versioning
Use a data/hora no nome do arquivo para versionamento:
- `tierlist-2024-08-12-v1.json`
- `tierlist-2024-08-12-v2.json`
- Mantenha múltiplos Gists para diferentes cenários

### Backup
Periodicamente, exporte seu estado atual com **"📤 Exportar"** e salve como backup local.

### Colaboração
Se múltiplos usuários precisam acessar o mesmo Gist:
- Compartilhem a mesma URL
- Use um token de repositório (não pessoal) se disponível
- Considere usar um repositório Git compartilhado em vez de Gist

### CORS e Firewalls
Se receber erro de rede:
- Verifique se seu firewall bloqueia fetch requests
- Tente de outro navegador ou máquina
- Gists públicos têm melhor suporte a CORS

## Limpar Configuração

Para remover a configuração:
1. Abra **"⚙️ Administração Gist"**
2. Clique em **"🗑️ Limpar Config"**
3. Confirme na caixa de diálogo

Isso não deleta o Gist no GitHub, apenas remove a configuração local.

---

**Dúvidas?** Consulte a documentação oficial do GitHub sobre [Gists](https://docs.github.com/en/get-started/writing-on-github/editing-and-sharing-content-with-gists).
