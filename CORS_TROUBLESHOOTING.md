# Guia Rápido de Troubleshooting - Gist & CORS

## Erro: "Erro de CORS" ou "NetworkError when attempting to fetch resource"

### ✅ Solução Rápida
1. Abra **"⚙️ Administração Gist"**
2. Ative: **☑ "Usar API do GitHub (melhor suporte CORS)"**
3. Clique **"🔍 Validar & Carregar"**

Se ainda não funcionar, continue abaixo.

---

## Checklist de Diagnóstico

- [ ] **URL está correta?**
  - Deve ser a URL **RAW** do Gist (contém `/raw/`)
  - Teste: Cole a URL no navegador, deve mostrar JSON bruto
  - Não use a URL da página do Gist, use o botão "Raw"

- [ ] **Gist é público ou privado?**
  - **Público**: Funciona sem token
  - **Privado/Secret**: Precisa de token válido

- [ ] **Token está válido?**
  - Deve começar com `ghp_`
  - Pode ter expirado → Regenere em [github.com/settings/tokens](https://github.com/settings/tokens)
  - Verifique se tem escopo `gist` ativado

- [ ] **JSON está no formato correto?**
  ```json
  {
    "staging-pool": [],
    "tier-ensino": [],
    "tier-pesquisa": [],
    "tier-extensao": []
  }
  ```
  - Cada zona é um array
  - Está no Gist como arquivo `.json`?

---

## Erros Específicos e Soluções

| Erro | Solução |
|------|---------|
| **HTTP 401 - Token inválido** | Regenere token em GitHub settings |
| **HTTP 403 - Acesso negado** | Verifique se token pertence à conta que criou o Gist |
| **HTTP 404 - Não encontrado** | Gist foi deletado ou URL está errada |
| **CORS error** | Ative "Usar API do GitHub" ou tente Gist público |
| **JSON inválido** | Exporte seu estado atual para ver formato correto |

---

## Testes Progressivos

### Teste 1: Verificar Conectividade
```
1. Abra DevTools (F12)
2. Vá para aba "Console"
3. Cole: fetch('https://gist.githubusercontent.com/seu-usuario/seu-id/raw/seu-arquivo.json').then(r => r.json()).then(d => console.log(d))
4. Se funcionar, verá o JSON no console
5. Se falhar, verá o erro específico
```

### Teste 2: Com Token (se Gist é secreto)
```
fetch('https://api.github.com/gists/seu-id', {
  headers: {
    'Authorization': 'token seu-token-aqui'
  }
}).then(r => r.json()).then(d => console.log(d))
```

### Teste 3: Gist Público
1. Crie um Gist PÚBLICO para testes
2. Se funcionar com público mas não com secreto, é problema de acesso
3. Se não funcionar nem com público, pode ser firewall

---

## Cenários Comuns

### Cenário 1: Está em Rede Corporativa
- Firewall pode bloquear GitHub
- Teste: Use dados móvel
- Solução: Contate seu admin de rede

### Cenário 2: Gist Secreto Não Funciona
- Token pode estar expirado
- Token pode ter escopo errado (sem `gist`)
- Solução: Regenere token com escopo `gist`

### Cenário 3: Funciona Localmente Mas Não no Servidor
- Domínio diferente pode ter CORS restrito
- Solução: Use "Usar API do GitHub" que tem CORS melhor

---

## Console Developer (Debugging)

Para ver erros detalhados:
1. Clique com botão direito → **Inspecionar**
2. Abra aba **Console** (F12)
3. Tente validar o Gist
4. Procure por mensagens de erro em vermelho
5. Copie o erro completo para reportar

---

## Quando Tudo Falha: Importar Manualmente

Se nada funcionar, você pode:
1. Exportar seu JSON: **"📤 Exportar"**
2. Baixar o arquivo JSON do Gist manualmente
3. Importar aqui: **"📥 Importar"** (selecione o arquivo)

---

## Dúvidas?

- **Gist Setup**: Veja [GIST_SETUP.md](GIST_SETUP.md)
- **Estrutura JSON**: Exporte um estado atual para usar como template
- **Token GitHub**: [github.com/settings/tokens](https://github.com/settings/tokens)
