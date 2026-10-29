# Drive Pro

Gerenciador de Google Drive que roda 100% no navegador (HTML + JS puro). Sem servidor, sem Colab:
o navegador conversa direto com a API do Google Drive usando o login Google (OAuth) da própria pessoa.

Recursos: navegar (Meu Drive, recentes, estrela, compartilhados, lixeira, drives compartilhados), pesquisar,
filtrar/ordenar, seleção múltipla, renomear, mover, copiar (inclusive pastas), favoritar, restaurar, excluir,
upload resumável (arquivos, pastas, arrastar e soltar, pausa/cancelamento), download com streaming
(Chrome/Edge gravam direto no disco), ZIP de pastas/seleções (exporta Docs/Sheets/Slides) e prévia de arquivos.

## 1. Criar o Client ID (uma vez)

1. https://console.cloud.google.com → crie um projeto.
2. **APIs e serviços → Biblioteca** → ative **Google Drive API**.
3. **Tela de permissão OAuth** → tipo *Externo* → deixe em **Testando** e adicione seu e-mail em **Usuários de teste**
   (só quem estiver na lista consegue entrar; isso é o que torna o acesso privado).
4. **Credenciais → Criar credenciais → ID do cliente OAuth → Aplicativo da Web**.
   Em **Origens JavaScript autorizadas** coloque `https://SEU-USUARIO.github.io` (e `http://localhost:8000` para testes).
5. Copie o Client ID (`xxxx.apps.googleusercontent.com`). Ele não é segredo.

O Google mostrará "app não verificado" porque o escopo `drive` é restrito; em modo Testando basta clicar em
*Avançado → Continuar*. Os testadores precisam reautorizar a cada 7 dias.

## 2. Publicar no GitHub Pages

```bash
git remote add origin https://github.com/SEU-USUARIO/DRIVE-GIT.git
git push -u origin main
```
No repositório: **Settings → Pages → Deploy from a branch → main / (root)**.
Acesse `https://SEU-USUARIO.github.io/DRIVE-GIT/`, cole o Client ID e entre.
Para não digitar o Client ID, preencha `CONFIG.clientId` no `index.html`.

## Sobre privacidade

- O **código** é público no GitHub Pages (plano gratuito). Os **seus dados não**: o token do Google fica só na memória da aba,
  nada é gravado em servidor, e quem abrir o site só enxerga o próprio Drive depois de logar.
- Com o app em modo *Testando*, apenas os e-mails que você listou conseguem autorizar.
- Quer o **site** em si restrito? Pages com controle de acesso exige GitHub Enterprise Cloud. Alternativas gratuitas:
  Cloudflare Pages + Cloudflare Access, ou Netlify com senha.
- A página usa `noindex`, `no-referrer` e uma Content-Security-Policy que só permite falar com `googleapis.com` e o login do Google.

## Limites

- ZIP: sem compressão e até ~4 GB. Fora do Chrome/Edge (sem File System Access API) o ZIP/download é montado na memória (limite 1,5 GB).
- Exportação de Docs/Sheets/Slides: máximo de 10 MB por arquivo (limite da API). Forms/Sites não são exportáveis.
- Token de acesso dura 1 h; o app renova sozinho (pode abrir um pop-up do Google).

## Gerenciar armazenamento

Botão **Gerenciar armazenamento**, logo abaixo da barra de capacidade. Abre uma tela que analisa os arquivos que são seus e permite:

- ver o espaço por tipo e as **sugestões de limpeza** (temporários, formato desconhecido, duplicados, sem título, grandes e antigos);
- filtrar por **maiores**, **antigos** (data limite por modificação, criação ou último acesso), **sem título**, **nomes genéricos**, **duplicados**, **soltos na raiz** e **compartilhados**;
- filtrar por **tipo** (imagem, áudio, vídeo, documento, planilha, apresentação, PDF, compactado) e depois por **formato** (jpg, png, mp3, wav, xls…), além de tamanho mínimo e nome;
- selecionar e **mover para a lixeira** (com “Desfazer”), baixar, restaurar ou excluir definitivamente, e exportar a lista em **CSV**.

## Pesquisa avançada

O ícone de filtros dentro da barra de pesquisa permite buscar por texto dentro do arquivo, tipo, extensão, proprietário, período de modificação e de criação, tamanho mínimo/máximo, pasta atual, com estrela, compartilhados e lixeira.

