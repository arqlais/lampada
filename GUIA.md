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
3. Aba **Regras**: apague tudo, cole o texto abaixo e toque em **Publicar**. Assim cada conta só enxerga os próprios dados. A parte `pub` é do **ler junto**: lá fica só o nome, o código, o plano e se a pessoa já leu hoje.

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
    match /pub/{uid} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.auth.uid == uid;
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

### 2.1 Pegar uma chave 100% grátis
O segredo é criar a chave num **projeto sem faturamento**. Sem conta de faturamento ligada, o Google **não tem como cobrar nada**: no máximo chega no limite do dia, e no dia seguinte volta sozinho.

1. Entre em **aistudio.google.com/apikey** com a sua conta Google.
2. Clique em **Criar chave de API** e escolha **criar um projeto novo** (nome: `lampada`).
3. **Não** ligue faturamento nesse projeto. Na lista de chaves, ao lado dela **não** pode aparecer "Minha conta de faturamento".
4. Copie a chave.
5. No app: **seu perfil › inteligência artificial** › cole em **sua chave** (salva sozinha) › **testar conexão**.

Atenção: chaves de projetos que já têm faturamento (por exemplo o "Default Gemini Project") **podem cobrar**. Não use essas no Lâmpada.

### 2.2 Modelo
Deixe no **Gemini 3 Flash** (padrão, grátis). O app só oferece modelos com uso grátis.

### 2.3 Se aparecer "limite grátis atingido"
É só o limite do dia da conta grátis. Espere um pouco ou volte amanhã. Nada é cobrado.

### 2.4 Chat com o Claude
No chat (botão redondo no canto), troque para **Claude**: a pergunta abre no **claude.ai** numa aba nova, já com o contexto da leitura. As explicações do dia continuam com o Gemini.

---

## 3. Lembrete da leitura (app Lembretes do iPhone)

O app não coloca nada no Calendário, pra não encher de eventos. O lembrete é feito pelo app **Atalhos**, que cria **um lembrete por dia, só no próprio dia**:

1. Abra o **Atalhos** › aba **Automação** › **+**.
2. **Hora do Dia** › escolha o horário da leitura › **Diariamente** › marque **Executar Imediatamente**.
3. **Nova Automação em Branco** › **Adicionar Ação** › procure **Adicionar Novo Lembrete**.
4. No texto, escreva **Leitura da Bíblia no lâmpada** › **Concluído**.

Esse passo a passo também está no app, em **caminhada › planos › lembrete no iPhone**.

**Limpar o que ficou de antes:**
- **Lembrete que se repete** no app Lembretes: abra o lembrete › **(i)** › **Repetir** › **Nunca**, ou apague o lembrete.
- **Eventos "Lâmpada · …" no Calendário:** toque num deles › **Apagar evento**. Se perguntar, escolha **Apagar todos os eventos futuros**. Se forem eventos separados, um por leitura, apague cada um; num Mac dá pra selecionar vários de uma vez no app Calendário.

## 4. Perfil, cores e várias pessoas

- Toque na **sua foto** (canto de cima) para abrir **seu perfil**: nome, foto (trocar, **ajustar** arrastando e com zoom, ou tirar), **claro / escuro / automático** e a **paleta** (Dusty & baunilha, que é a padrão, Azul, Lilás, Verde sálvia, Rosa antigo ou Laranja). O botão de lua/sol no topo troca claro e escuro na hora.
- **Outras pessoas (ex.: seu namorado):** mande o endereço **arqlais.github.io/lampada**. Cada um entra com a **própria conta Google** (depois que o login do passo 1 estiver ligado) e tem progresso, diário, palavras, chave do Gemini, foto e paleta só seus. As regras do passo 1.4 garantem que ninguém vê os dados do outro.
- **Como você lê a Bíblia** (em **seu perfil › você**): **iniciante**, **intermediário** ou **avançado**. Muda o jeito das explicações, das palavras e das conversas com a IA (no avançado entram hebraico, grego e interpretações, e some a trilha do básico do "hoje"). Cada conta escolhe o seu.
- **Bíblia inteira:** em **caminhada › planos**, escolha pra quem (iniciante, intermediário ou avançado), em quanto tempo e quando começar. O iniciante começa por Jesus e deixa os livros difíceis pro fim; o intermediário segue a ordem dos acontecimentos; o avançado lê três trilhas por dia (Antigo Testamento, sabedoria e Novo Testamento).
- **Ler junto:** em **caminhada › planos › ler junto** aparece o **seu código**. Toque em **enviar** e mande pra pessoa; ela coloca o seu código no campo "código da outra pessoa" (e você o dela). Daí aparece embaixo da leitura de hoje "Lucas já leu hoje". Ninguém vê suas reflexões, conversas ou palavras. Pra parar, toque no **x** ao lado do nome.
- Sem login, o perfil fica salvo só naquele aparelho.
- **Sair da conta:** em **seu perfil**, no fim da página.

## 5. Ícone na tela de início

Para pegar o **símbolo novo** (o arco com a chama acesa):
1. Apague o atalho antigo da tela de início.
2. Abra **arqlais.github.io/lampada** no Safari › **Compartilhar** › **Adicionar à Tela de Início**.
