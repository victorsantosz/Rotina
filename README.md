# RotinAI — automação WhatsApp

Backend Firebase Functions para:

- gerar a próxima semana a partir das tarefas marcadas como `🔁 Repetir toda semana` (o sistema usa a ocorrência semanal mais recente como modelo e evita duplicar a semana-alvo);
- enviar o resumo semanal no domingo às 08:00 (America/Bahia);
- enviar lembretes de tarefas a cada 5 minutos, no intervalo configurado pelo usuário;
- enviar uma mensagem de teste pelo botão do RotinAI.

## 1. Instalar

```bash
npm install -g firebase-tools
firebase login
cd rotinai-functions
npm install
```

## 2. Associar ao projeto

Na pasta do projeto Firebase:

```bash
firebase use rotina-bc181
```

Se ainda não existir `firebase.json`, rode `firebase init functions` e escolha o projeto `rotina-bc181`.

## 3. Configurar WhatsApp Cloud API

Você precisa de uma conta/app no Meta for Developers com WhatsApp Cloud API e:

- `META_ACCESS_TOKEN`: token da API;
- `META_PHONE_NUMBER_ID`: ID do número usado pela API;
- `META_GRAPH_VERSION`: versão do Graph API usada no seu app.

Configure o token como Secret:

```bash
firebase functions:secrets:set META_ACCESS_TOKEN
```

Configure os parâmetros/string no deploy ou via console conforme o Firebase CLI solicitar:

```bash
firebase functions:config:set placeholder.value="deprecated"
```

Para as Functions 2nd gen deste projeto, prefira configurar `META_PHONE_NUMBER_ID` e `META_GRAPH_VERSION` quando o CLI solicitar os parâmetros `defineString`.

## 4. Deploy

```bash
firebase deploy --only functions
```

O Firebase Cloud Scheduler cria os agendamentos das funções automaticamente. A documentação oficial confirma que `onSchedule` usa Cloud Scheduler e que as funções agendadas são criadas no deploy.

## 5. Como usar no RotinAI

No cadastro de uma rotina:

1. informe data e horário;
2. marque `🔁 Repetir toda semana`;
3. informe concurso/matéria/assunto, quando houver;
4. informe a fonte de questões e o cursinho/material;
5. coloque o link da reunião/aula/material quando existir.

Em `📱 WhatsApp automático`:

- informe o número em formato internacional, por exemplo `5575999999999`;
- ative o WhatsApp;
- ative o resumo semanal;
- escolha o tempo do lembrete;
- salve;
- clique em `📤 Enviar teste`.

## Segurança

O token da Meta fica no backend como Secret e não deve ser colocado no HTML/GitHub Pages. A lógica de envio e o agendamento ficam nas Cloud Functions.
