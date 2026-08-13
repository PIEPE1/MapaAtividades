# Mapa de Atividades Acadêmicas
Mapa de Atividades, estilo kanban com tier list, com colunas de Ensino, Pesquisa e Extensão, para gameficação do ensino da tríade da universidade.

O objetivo é realizar explicações do que pode ser feito em cada uma destas bases da universidade. Este Mapa de Atividades pode ser empregado em complemento a uma atividade de world café, trazendo como post-it as sugestões escritas pelos estudantes nos cartazes.

Mapa gerado com auxílio do Large Language Model (LLM) Anthropic Claude.

## Funcionalidades

- 🎒 **Banco de Itens**: Crie post-its coloridos e adicione imagens
- 📂 **Três Categorias**: Organize itens em Ensino, Pesquisa e Extensão
- 🖱️ **Drag & Drop**: Arraste itens entre categorias facilmente
- 💾 **Salvamento Automático**: Dados são salvos no localStorage do navegador
- 📤 **Exportar/Importar**: Exporte seu estado como JSON e importe posteriormente
- ☁️ **Sincronização com Gist**: Sincronize estados com um Gist do GitHub

## Sincronização com Gist

O sistema agora suporta sincronização com **Gists do GitHub**! Isso permite:

- ✓ Salvar e compartilhar estados de forma centralizada
- ✓ Sincronizar automaticamente ao carregar a página
- ✓ Trabalhar colaborativamente com outros usuários
- ✓ Versionar diferentes configurações de aula
- ✓ Suporte a Gists públicos e secretos
- ✓ Múltiplas formas de acesso (API do GitHub ou URL raw)

### Como Começar

1. Clique em **"⚙️ Administração Gist"** no topo da página
2. Configure a URL de um Gist e seu token (se necessário)
3. Ative **"Usar API do GitHub"** se tiver problemas de CORS
4. Clique em **"🔍 Validar & Carregar"**

Para instruções detalhadas, veja [GIST_SETUP.md](GIST_SETUP.md).

## Como Usar

### Criando Itens

1. No "Banco de Itens", digite o texto do post-it
2. Escolha uma cor
3. Clique em **"+ Post-it"**
4. Ou faça upload de uma imagem com **"+ Imagem"**

### Organizando

1. Arraste itens do banco para as categorias
2. Itens podem ser movidos entre categorias
3. Arraste de volta para o banco para descartar

### Salvando

- Os dados são salvos **automaticamente** no localStorage
- Use **"📤 Exportar"** para fazer backup como arquivo JSON
- Use **"📥 Importar"** para restaurar um estado anterior
- Use **"⚙️ Administração Gist"** para sincronizar com GitHub

