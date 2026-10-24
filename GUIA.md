# Guia do Lâmpada

Passo a passo de tudo que você pode ligar no app. Os nomes dos botões do Google mudam de vez em quando: se algum não estiver igual, procure o mais parecido.

---

## 1. Entrar com a conta Google (Firebase)

O login já está pronto no app, usando o **Firebase** (do Google, gratuito no plano Spark). Não precisa de Supabase. Com o login, o progresso, o diário, as palavras e a chave do Gemini ficam salvos na nuvem e aparecem no celular, no iPad e no computador.

### 1.1 Criar o projeto
1. Entre em **console.firebase.google.com** com a sua conta Google.
2. Toque em **Criar um projeto** (ou "Adicionar projeto"). Nome: `lampada`.
3. O Google Analytics pode ficar **desligado**. Toque em **Criar projeto** e espere.

### 1.2 Registrar o app da web
1. Na página do projeto, toque no ícone **`</>`** (Web).
2. Apelido: `Lâmpada`. **Não** marque o Firebase Hosting.
3. Toque em **Registrar app**. Aparece um bloco `const firebaseConfig = { ... }`. **Copie só o que está entre as chaves `{ }`** (apiKey, authDomain, projectId etc.).

### 1.3 Ligar o login com Google
1. No menu da esquerda: **Criação** (Build) › **Authentication** › **Vamos começar**.
2. Aba **Método de login** › **Google** › ative › escolha seu e-mail de suporte › **Salvar**.
3. Aba **Configurações** › **Domínios autorizados** › **Adicionar domínio** › `arqlais.github.io` › **Adicionar**.

### 1.4 Criar o banco (Firestore)
1. Menu: **Criação** › **Firestore Database** › **Criar banco de dados**.
2. Local: **southamerica-east1 (São Paulo)**. Modo: **produção**.
3. Aba **Regras**: apague tudo, cole o texto abaixo e toque em **Publicar**. Assim cada conta só enxerga os próprios dados.

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```

### 1.5 Colar a configuração no app
1. No GitHub, abra o repositório **arqlais/lampada** › arquivo **config.js** › lápis (editar).
2. Troque `null` pelo bloco que você copiou, assim:

```js
window.LAMPADA_FIREBASE = {
  apiKey: "...",
  authDomain: "lampada-xxxx.firebaseapp.com",
  projectId: "lampada-xxxx",
  storageBucket: "lampada-xxxx.appspot.com",
  messagingSenderId: "...",
  appId: "..."
};
```

3. **Commit changes** no `main`. Em 1 a 3 minutos o site atualiza.
4. No app: **seu perfil › inteligência artificial › entrar com o Google**. O que você já fez naquele aparelho vai junto no primeiro login.

Esses dados do `config.js` podem ficar públicos: quem protege a sua conta são as regras do passo 1.4.

**Se o login não abrir no app instalado na tela de início:** abra o endereço uma vez no Safari, entre por lá e volte para o app.

---

## 2. Gemini (o chat e as explicações)

### 2.1 Pegar a chave
1. Entre em **aistudio.google.com/apikey** com a sua conta Google.
2. **Create API key** › copie.
3. No app: **seu perfil › inteligência artificial** › cole em **sua chave** › **salvar** › **testar conexão**.

### 2.2 Escolher o modelo (em ajustes)
- **Gemini 3 Flash:** rápido, com **uso grátis** (tem limite por dia). Bom para o dia a dia.
- **Gemini 3.1 Pro:** responde mais a fundo, mas **não tem uso grátis na API**. Ele usa o crédito do seu plano (passo 2.3).

### 2.3 Usar o crédito do seu Google AI Pro
A assinatura Google AI Pro (Google One) **não vem com chave de API**. Ela inclui um **crédito mensal no Google Cloud** (Google cita US$ 10 por mês no AI Pro), que paga a API. Para ativar:
1. Entre em **developers.google.com/program** com a conta que tem o AI Pro e toque em **Participar do programa** (crie o perfil).
2. No menu **Benefícios**, ache o benefício do **Google AI Pro** (créditos mensais de Cloud) e toque em **Ativar**.
3. Escolha (ou crie) uma **conta de faturamento** do Google Cloud e confirme. O crédito vale para **uma** conta de faturamento.
4. No **AI Studio**, abra a página das chaves e confira se o **projeto da sua chave** está ligado a essa conta de faturamento.
5. No app, escolha **Gemini 3.1 Pro** e toque em **testar conexão**.

Atenção: há relatos de que o AI Studio às vezes pede uma **recarga paga** mesmo com o crédito ativo. Se aparecer isso, fique no **3 Flash** (grátis) ou fale com o suporte do Google antes de pagar.

### 2.4 Chat com o Claude
No chat (botão redondo no canto), troque para **Claude**: a pergunta abre no **claude.ai** numa aba nova, já com o contexto da leitura. As explicações do dia continuam com o Gemini.

---

## 3. Agenda e lembretes

Em **caminhada › planos › agenda e lembretes**:
1. Escolha o **horário da leitura**.
2. **Plano na agenda:** baixa um arquivo com cada leitura que falta, no dia certo e com aviso no horário.
3. **Lembrete diário:** baixa um aviso que se repete todo dia.

Para abrir o arquivo:
- **iPhone e iPad:** toque no arquivo baixado (setinha de downloads do Safari) › **Adicionar tudo**. Ele entra no app Calendário com aviso.
- **Google Agenda:** no computador, em **calendar.google.com** › engrenagem › **Configurações** › **Importar e exportar** › **Importar** › escolha o arquivo. Se o Google Agenda já estiver no seu iPhone, ele também aparece lá.
- Criou um plano novo ou mudou o horário? Apague os eventos antigos do Lâmpada no Calendário e baixe de novo, para não ficar repetido.

---

## 4. Perfil, cores e várias pessoas

- Toque na **sua foto** (canto de cima) para abrir **seu perfil**: nome, foto (trocar, **ajustar** arrastando e com zoom, ou tirar), **claro / escuro / automático** e a **paleta** (Dusty & baunilha, que é a padrão, Azul, Lilás, Verde sálvia, Rosa antigo ou Laranja). O botão de lua/sol no topo troca claro e escuro na hora.
- **Outras pessoas (ex.: seu namorado):** mande o endereço **arqlais.github.io/lampada**. Cada um entra com a **própria conta Google** (depois que o login do passo 1 estiver ligado) e tem progresso, diário, palavras, chave do Gemini, foto e paleta só seus. As regras do passo 1.4 garantem que ninguém vê os dados do outro.
- Sem login, o perfil fica salvo só naquele aparelho.
- **Sair da conta:** em **seu perfil**, no fim da página.

## 5. Ícone na tela de início

Para pegar o **símbolo novo** (o arco com a chama acesa):
1. Apague o atalho antigo da tela de início.
2. Abra **arqlais.github.io/lampada** no Safari › **Compartilhar** › **Adicionar à Tela de Início**.
