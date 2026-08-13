# Guia Rápido de Troubleshooting — Gist & CORS

> A URL do Gist vem de `gist-config.json` (não há mais campo de URL na interface).
> Leitura: qualquer visitante, pela URL raw. Escrita: só o administrador, com token.

## A página abre mas não mostra os itens

### Checklist

- [ ] **`gist-config.json` está publicado junto com a página?**
  - Abra `https://seu-usuario.github.io/seu-repo/gist-config.json` — deve mostrar o JSON
  - No modal **"⚙️ Administração"**, a seção **"Origem dos dados"** mostra o que foi lido e o erro, se houver

- [ ] **A `gistUrl` está correta?**
  - Deve ser a URL **RAW** (contém `/raw/`) e **sem o SHA da revisão**
  - Teste: cole a URL no navegador, deve mostrar o JSON bruto
  - Não use a URL da página do Gist, use o botão "Raw"

- [ ] **O JSON está no formato correto?**

  ```json
  {
    "staging-pool": [],
    "tier-ensino": [],
    "tier-pesquisa": [],
    "tier-extensao": []
  }
  ```

  - Cada zona é um array
  - O arquivo no Gist é `.json` e o nome bate com `gistFile`

- [ ] **`enabled` está `true`** em `gist-config.json`?

## A página não publica (modo admin)

- [ ] **Token válido?** Regenere em [github.com/settings/tokens](https://github.com/settings/tokens) com escopo `gist`
- [ ] **Token é da conta dona do Gist?** A página recusa tokens de outra conta no login
- [ ] **`gistId` correto?** Confira em `gist-config.json`

## Erros específicos

| Erro | Solução |
|------|---------|
| **HTTP 401 — Token inválido** | Regenere o token nas configurações do GitHub |
| **HTTP 403 — Sem permissão** | Token sem escopo `gist`, ou rate limit da API atingido |
| **HTTP 404 — Não encontrado** | Gist deletado, `gistId` errado ou token de outra conta |
| **"O token pertence a X, mas o Gist é de Y"** | Use o token da conta que criou o Gist |
| **CORS / NetworkError na URL raw** | A página tenta a API do GitHub como fallback; verifique firewall/proxy |
| **JSON inválido** | Use **"📤 Exportar"** para ver o formato correto |

## Testes no Console (F12)

### Teste 1 — leitura pública (sem token)

```js
fetch('https://gist.githubusercontent.com/USUARIO/ID/raw/ARQUIVO.json')
  .then(r => r.json()).then(console.log)
```

### Teste 2 — leitura via API com token

```js
fetch('https://api.github.com/gists/ID', {
  headers: { Authorization: 'Bearer SEU_TOKEN' }
}).then(r => r.json()).then(console.log)
```

### Teste 3 — a quem pertence o token

```js
fetch('https://api.github.com/user', {
  headers: { Authorization: 'Bearer SEU_TOKEN' }
}).then(r => r.json()).then(u => console.log(u.login))
```

## Cenários comuns

### Rede corporativa / da instituição

- O firewall pode bloquear `gist.githubusercontent.com` ou `api.github.com`
- Teste com dados móveis; se funcionar, é bloqueio de rede

### Publiquei, mas a turma vê a versão antiga

- Confirme que `gistUrl` **não** contém o SHA da revisão (senão a URL fica congelada naquela versão)
- Peça para clicarem em **"🔄 Recarregar"**

### Alunos perdem a organização feita em aula

- A organização local é mantida enquanto o Gist não mudar. Se você **publicar** durante a aula, o novo estado substitui o dos alunos no próximo carregamento — publique antes ou depois da atividade.

## Quando tudo falha: importação manual (admin)

1. Baixe o JSON do Gist manualmente
2. Entre como administrador
3. Use **"📥 Importar"** e selecione o arquivo
4. Publique com **"☁️ Publicar estado no Gist"**

## Dúvidas?

- **Setup do Gist**: veja [GIST_SETUP.md](GIST_SETUP.md)
- **Estrutura JSON**: exporte um estado atual para usar como template
- **Token GitHub**: [github.com/settings/tokens](https://github.com/settings/tokens)
