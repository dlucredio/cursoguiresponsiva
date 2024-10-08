O conteúdo desta parte é baseado no tutorial [W3Schools HTML Forms](https://www.w3schools.com/html/html_forms.asp)

A primeira coisa que precisa ser discutida é a relação entre HTML e GUI. Falar sobre como HTML nasceu como uma linguagem para conteúdo, mas que hoje evoluiu para incluir a criação de interfaces também.

1. Mostrar exemplo de HTML como conteúdo. Mostrar parágrafo, seção, heading, formatação...
2. Discutir que o navegador evoluiu para um ambiente "sandbox" para criação de aplicativos. Mesmo antigamente, já existiam elementos de UI básicos, como formulários e botões.
3. Falar que, para a criação de aplicativos, atualmente, é possível fazer tudo o que se faz com outras ferramentas (Android/Windows/Linux)

Para testar o envio dos dados dos formulários, vamos criar um servidor para receber as requisições enviadas pelo navegador. Você vai precisar do [node.js](nodejs.org) instalado.

1. Criar uma pasta chamada `htmlforms` no seu computador
2. Acesse a pasta e execute o comando `npm init` (siga todas as opções padrão)
3. Execute o comando `npm install express`
4. Modifique o arquivo `package.json` para incluir o tipo correto de projeto node:

```diff
{
  "name": "htmlforms",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
+  "type": "module",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```
5. Crie um arquivo chamado `index.js` com o seguinte conteúdo:

```js
import express from 'express';
const app = express();
const port = 3000;

app.use('/dosomething', express.urlencoded({
    extended: true
}));

app.use('/dosomethingbinary', express.raw({
    inflate: true,
    limit: '100kb',
    type: '*/*'
}));

app.get('/dosomething', (req, res) => {
    console.log(req.query);
    res.send(`Params GET: ${JSON.stringify(req.query)}`)
});

app.get('/dosomethingasadmin', (req, res) => {
    console.log(req.query);
    res.send(`(ADMIN) Params GET: ${JSON.stringify(req.query)}`)
});

app.post('/dosomething', (req, res) => {
    console.log(req.body);
    res.send(`Params POST: ${JSON.stringify(req.body)}`)
});

app.post('/dosomethingbinary', (req, res) => {
    console.log(req.body);
    res.send(`Params POST: ${JSON.stringify(req.body)}`)
});

app.listen(port, () => {
    console.log(`Server listening: http://localhost:${port}/dosomething`)
    console.log(`Server listening: http://localhost:${port}/dosomethingasadmin`)
    console.log(`Server listening: http://localhost:${port}/dosomethingbinary`)
});
```

6. Execute o seguinte comando `node index.js`
7. A partir deste momento, siga os exemplos do tutorial, e sempre que for especificado um formulário, no atributo `action` substitua o endereço pelo correspondente aqui criado.
8. Testar também métodos `GET` e `POST`, além dos botões para envio diferentes:

```
        <input type="submit" value="Submit">
        <input type="submit" formaction="http://localhost:3000/dosomethingasadmin" value="Submit as Admin">
        <input type="submit" formenctype="multipart/form-data" formaction="http://localhost:3000/dosomethingbinary" formmethod="POST" value="Submit as Multipart/form-data">
```
