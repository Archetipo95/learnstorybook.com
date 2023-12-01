---
title: 'Tutorial de Storybook para React Native'
tocTitle: 'Introdução'
description: 'Configurando o Storybook em seu ambiente de desenvolvimento'
---

O Storybook ajuda você a criar componentes de interface do usuário isolados da lógica de negócios e do contexto do seu aplicativo. Esta edição do tutorial Introdução ao Storybook é para React Native; existem outras edições para [React](https://storybook.js.org/tutorials/intro-to-storybook/react/pt/get-started/), [Vue](https://storybook.js.org/tutorials/intro-to-storybook/vue/pt/get-started/), [Angular](https://storybook.js.org/tutorials/intro-to-storybook/angular/pt/get-started/), [Svelte](https://storybook.js.org/tutorials/intro-to-storybook/svelte/en/get-started) e [Ember](https://storybook.js.org/tutorials/intro-to-storybook/ember/en/get-started).

![Storybook e seu aplicativo](/intro-to-storybook/storybook-relationship.jpg)

## Configurar o Storybook no React Native

Precisamos seguir alguns passos para começar. Neste tutorial, usaremos este [template](https://github.com/chromaui/intro-storybook-react-native-template) onde já configuramos um aplicativo React Native usando o [Expo](https://expo.io/tools) e adicionamos o [Storybook](https://storybook.js.org/) ao projeto.

Antes de prosseguir-mos, alguns tópicos importantes que temos que ter em mente:
- Para ajudá-lo ao longo do tutorial, você precisará de um telefone ou simulador previamente configurado para permitir a execução do aplicativo. Para obter mais informações, consulte a documentação da Expo sobre execução em [rodando no IOS](https://docs.expo.dev/workflow/ios-simulator/) e [Android](https://docs.expo.dev/workflow/android-studio-emulator/).
- Este tutorial será focado em IOS/Android. O React Native pode ter como alvo outras plataformas que este tutorial não cobrirá.
- Você  precisará do [nodejs](https://nodejs.org/en/download/) configurado no seu ambiente de desenvolvimento.

Comece por fazer o download do template que criámos para este tutorial:

```shell
npx degit chromaui/intro-storybook-react-native-template#main taskbox
```

Em seguida, vamos instalar as dependências e executar o aplicativo para garantir que tudo esteja funcionando conforme o esperado.

```shell
cd taskbox
yarn install
```

Agora que você tem o aplicativo, vamos executá-lo para garantir que tudo esteja funcionando conforme o esperado.

Você pode escolher ios ou android e executar qualquer um deles e verificar se o aplicativo está funcionando.

```shell:clipboard=false

# Run the application on IOS
yarn ios

# Run the application on Android
yarn android

# Run Storybook on ios
yarn storybook:ios

# Run Storybook on android
yarn storybook:android
```

<div class="aside">
💡 Ao longo desta versão do tutorial, vai ser usado o Yarn para executar a maioria dos comandos. Se estiveres a seguir este tutorial e não tiveres o Yarn configurado, podes substituir os comandos para o teu gestor de pacotes preferido (por exemplo, [npm](https://www.npmjs.com/), [pnpm](https://pnpm.io/)). 
</div>

Ao executar o aplicativo com `yarn ios`, você deve ver isso renderizado no dispositivo:

<img src="/intro-to-storybook/react-native-expo-getting-started.png" alt="expo starter screen" height="600">

Ao executar o Storybook com `yarn storybook:ios`, você deve ver isto:

<img src="/intro-to-storybook/react-native-hello-world.png" alt="Storybook UI" height="600">

## Como funciona

Á partida, quando inicializado, o template fornece toda a configuração necessária para que possamos começar a desenvolver a nossa aplicação com React Native. No entanto, antes de começarmos a desenvolver o nosso IU do zero, vamos ver como funciona o Storybook dentro de um aplicativo React Native e o que há de diferente.

Com o React Native, o Storybook é um componente que você pode renderizar no seu aplicativo, ao contrário de outras versoões do Storybook, onde você pode executar o Storybook como um aplicativo separado de forma independente.

Por causa dessa distinção, precisamos de uma maneira de alternar entre o aplicativo e o Storybook. Para fazer isso, usamos variáveis ​​de ambiente e veremos isso rapidamente agora.

<div class="aside">
💡 Consulte a <a href="https://docs.expo.dev/guides/environment-variables/">documentação do expo</a> para obter mais detalhes sobre como usar variáveis ​​de ambiente.
</div>

Em nosso projeto existe um arquivo de configuração para expo chamado `app.config.js` este arquivo é onde configuramos coisas como o nome do nosso aplicativo e constantes que podemos usar em todo o aplicativo.

Isso nos permite acessar a variável `storybookEnabled` para o valor da variável de ambiente `STORYBOOK_ENABLED`, que veremos em breve.

```js:title=app.config.js
export default ({ config }) => ({
  ...config,
  name: "Storybook Tutorial Template",
  slug: "storybook-tutorial-template",
  extra: {
    storybookEnabled: process.env.STORYBOOK_ENABLED,
  },
});
```

Isso nos permite acessar a variável `storybookEnabled` em nosso aplicativo usando o pacote `expo-constants` e usamos isso para determinar se renderizamos o Storybook ou seu aplicativo.

```jsx:title=App.js
import Constants from 'expo-constants';

function App() {
  // ... removed for brevity
}

// Default to rendering your app
let AppEntryPoint = App;

// Render Storybook if storybookEnabled is true
if (Constants.expoConfig.extra.storybookEnabled === 'true') {
  AppEntryPoint = require('./.storybook').default;
}

export default AppEntryPoint;
```

Em package.json, vemos alguns scripts do Storybook. Nós os usamos para passar essa variável de ambiente para nosso aplicativo e executar algumas configurações.

```json:title=package.json
"scripts": {
  "storybook": "sb-rn-get-stories && STORYBOOK_ENABLED='true' expo start",
  "storybook:ios": "sb-rn-get-stories && STORYBOOK_ENABLED='true' expo ios",
  "storybook:android": "sb-rn-get-stories && STORYBOOK_ENABLED='true' expo android"
}
```

É aqui que nossa variável de ambiente `STORYBOOK_ENABLED` é definida como true, o que informa nosso aplicativo para renderizar o Storybook em vez de nosso aplicativo.

<div class="aside">
💡 Existem outras maneiras de configurar o Storybook, esta é apenas a maneira mais simples de começar.
</div>

## Confirmar alterações

Nesta fase, é seguro adicionar nossos arquivos a um repositório local. Execute os seguintes comandos para inicializar um repositório local, adicionar e confirmar as alterações que fizemos até agora.

```shell
git init
```

Seguido pela:

```shell
git add .
```

Então:

```shell
git commit -m "first commit"
```

E finalmente:

```shell
git branch -M main
```

Vamos começar a construir nosso primeiro componente!
