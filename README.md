# Chá de bebê — HTML, CSS, JavaScript e Firebase

Projeto estático sem React, sem bundler e sem etapa de compilação. O convite público e o painel administrativo usam HTML/CSS/JavaScript puro e módulos do Firebase carregados por CDN.

## Arquivos
- `index.html`, `styles.css`: convite público responsivo, presentes e formulário RSVP.
- `admin.html`, `admin.css`: painel administrativo com login, CRUD de presentes, upload de fotos, controle de participantes, reservas e dados do evento.
- `js/firebase-config.js`: configuração pública do app Firebase.
- `js/app.js`, `js/admin.js`: integração Firebase.
- `firestore.rules`, `storage.rules`: regras de segurança.
- `.github/workflows/deploy.yml`: publicação no GitHub Pages.

## 1. Configure o Firebase
1. Crie um projeto no [Firebase Console](https://console.firebase.google.com/).
2. Adicione um app Web e copie a configuração para `js/firebase-config.js`.
3. Ative **Authentication > Sign-in method > Email/Password**.
4. Crie um usuário administrador em Authentication.
5. Crie um banco Cloud Firestore em modo de produção.
6. Ative Firebase Storage.
7. Publique as regras deste repositório nas seções Firestore Rules e Storage Rules.

A configuração web do Firebase é destinada ao cliente. Não coloque senhas privadas, service account ou credenciais do Admin SDK em nenhum arquivo publicado.

## 2. Acesso administrativo
Neste pacote, o e-mail autorizado nas regras e no painel é `vinicius.bandeira.goncalves@gmail.com`. Para usar outro e-mail, altere o endereço em `firestore.rules`, `storage.rules` e `js/admin.js` antes de publicar. As regras do Firebase também verificam a identidade autenticada, então não basta alterar o texto do e-mail no navegador.

Não compartilhe a senha da conta. Use uma senha forte e mantenha o acesso à caixa de e-mail. As regras do Firestore e do Storage devem ser publicadas no Firebase Console para que a autorização funcione no servidor.

## 3. Execute localmente
Abra um terminal na pasta do projeto e execute:

```bash
python -m http.server 8000
```

Acesse `http://localhost:8000`. Não abra com `file://`, pois os módulos JavaScript e o Firebase precisam de HTTP/HTTPS.

## 4. Publique no GitHub Pages
1. Envie os arquivos para um repositório GitHub na branch `main`.
2. Em **Settings > Pages**, selecione **GitHub Actions** como fonte.
3. O workflow `.github/workflows/deploy.yml` publicará o site quando houver push na branch `main`.
4. Cadastre o domínio publicado nas configurações autorizadas do Firebase Authentication, se necessário.

## 5. Como funciona
- O visitante pode ver presentes e enviar uma confirmação sem criar conta.
- A reserva de presente pede o nome do convidado e usa uma transação do Firestore para evitar duas reservas simultâneas do mesmo item.
- A lista de reservas e as confirmações completas são legíveis apenas por administradores.
- A administradora pode cadastrar, editar, ocultar e excluir presentes, carregar imagem JPG/PNG de até 5 MB, liberar uma reserva e editar os dados do evento.
- As informações de exemplo no HTML são apenas placeholders. Os dados reais aparecem depois de configurar o Firebase.

## Segurança e limitações
- Regras do Firebase são parte obrigatória da segurança; ocultar a tela não basta.
- O convite permite RSVP público e reserva pública por nome. Como não há conta do convidado, não existe verificação forte de identidade; alguém pode informar o nome de outra pessoa.
- Para produção pública, configure orçamento/alertas, monitoramento e medidas contra abuso (por exemplo, App Check e/ou um fluxo com backend). Não deixe regras em modo de teste.
- A URL de download da imagem é pública para que os convidados possam ver a foto; não carregue imagens privadas.
- O código inicial não usa CAPTCHA e não implementa limite de taxa no cliente. Avalie essas proteções antes de divulgar amplamente.
