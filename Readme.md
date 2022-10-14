# Estudos sobre o sigle-span para o desenvolvimento de micro frontends

## O que é o single spa

single-spa é uma estrutura para reunir vários microfrontends JavaScript em um aplicativo frontend. Arquitetar seu frontend usando um spa único permite muitos benefícios, como:

Use vários frameworks na mesma página sem atualizar a página ( React , AngularJS , Angular , Ember ou o que você estiver usando)
Implante seus microfrontends de forma independente
Escreva código usando uma nova estrutura, sem reescrever seu aplicativo existente
Código de carregamento lento para melhorar o tempo de carregamento inicial

Link para a [documentação](https://single-spa.js.org/docs/getting-started-overview)

### Instalação da CLI

```shell
npm install --global create-single-spa

# or
yarn global add create-single-spa
```

### iniciando um app

```shell
create-single-spa
```

## Exemplo de uso

### root

```shell
http://localhost:9000/
```

### react app navbar

```shell
http://localhost:8080/
```

para acessar via a root

```shell
http://localhost:9000/navbar
```

### Como foi configurado o app em react

Adiciona o react e react DOM dentro da configuração do single spa

```javascript
<script type="systemjs-importmap">
      {
        "imports": {
          "react": "https://cdn.jsdelivr.net/npm/react@16.13.1/umd/react.production.min.js",
          "react-dom": "https://cdn.jsdelivr.net/npm/react-dom@16.13.1/umd/react-dom.production.min.js"
        }
      }
    </script>
```

Adiciona a aplicação

```javascript
   <% if (isLocal) { %>
    <script type="systemjs-importmap">
      {
        "imports": {
          "@single-spa/welcome": "https://unpkg.com/single-spa-welcome/dist/single-spa-welcome.js",
          "@mfe/root-config": "//localhost:9000/mfe-root-config.js",
          "@mfe/navbar": "http://localhost:8080/mfe-navbar.js"
        }
      }
    </script>
    <% } %>
```

Configura a rota para chamar a aplicação em react

```html
<main>
  <route default>
    <application name="@single-spa/welcome"></application>
  </route>
  <route path="/navbar">
    <application name="@mfe/navbar"></application>
  </route>
</main>
```

### App1

### react app1

```shell
http://localhost:8081/
```

para acessar via a root

```shell
http://localhost:9000/app1
```

## Log dos ciclos de vida das applicações dentro do single spa

```javascript
export const {
  bootstrap: _bootstrap,
  mount: _mount,
  unmount: _unmount,
} = lifecycles;

// log life cycle bootstrap
export function bootstrap(props) {
  return Promise.resolve().then(() => {
    console.log(props.name, 'bootstrap');
    _bootstrap(props);
  });
}
// log life cycle mount
export function mount(props) {
  return Promise.resolve().then(() => {
    console.log(props.name, 'mount');
    _mount(props);
  });
}
// log life cycle unmount
export function unmount(props) {
  return Promise.resolve().then(() => {
    console.log(props.name, 'unmount');
    _unmount(props);
  });
}
```
