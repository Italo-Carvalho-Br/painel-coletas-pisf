# Painel de Coletas — PISF

Dashboard web para **coordenadores** acompanharem, em tempo real, as coletas de
campo enviadas pelo app [Portal do Técnico](https://github.com/Italo-Carvalho-Br/portal_do_tecnico)
(Projeto de Integração do São Francisco — PISF).

🔗 **Em produção:** https://painel.ambientalhub.com.br

## Recursos
- Login de coordenador (Firebase Authentication)
- KPIs: total de coletas, hoje, últimos 7 dias, pontos distintos, técnicos
- Filtros: ponto, técnico, período, status de sincronização e busca
- Gráfico de coletas por dia (últimos 14 dias)
- Tabela com os 8 parâmetros medidos
- Exportação para **Excel (.xlsx)**
- Atualização **ao vivo** (Firestore `onSnapshot`)

## Arquitetura
- Página estática única (`index.html`), sem build. Bibliotecas via CDN
  (Firebase Web SDK, Chart.js, SheetJS).
- Lê os mesmos dados do Firestore que o app dos técnicos.
- **Sem chave de administrador no cliente**: o acesso usa login de coordenador
  + regras de segurança do Firestore (coleção `admins`). A `apiKey`/`appId` do
  Firebase Web **não são segredos** (identificadores públicos de cliente).

## Configuração
1. **App Web no Firebase** (mesmo projeto do app): Configurações do projeto →
   Seus apps → adicionar Web `</>` → copiar `firebaseConfig` para o bloco
   `firebaseConfig` em `index.html`.
2. **Admin**: Authentication → Users (copiar UID do coordenador) → Firestore →
   coleção `admins` → documento com ID = esse UID.
3. **Regras**: publicar o `firestore.rules` do repositório do app.
4. **Domínio autorizado**: Authentication → Settings → Authorized domains →
   adicionar o domínio do painel.

Veja o passo a passo detalhado em [`LEIA-ME.txt`](LEIA-ME.txt).

## Deploy
Hospedado na Hostinger (arquivos estáticos). Basta enviar o `index.html` para a
raiz do site/subdomínio — não há etapa de build.
