# Discord Webhook sending
![version](https://img.shields.io/npm/v/discord-webhook-node "Version")
![npm](https://img.shields.io/npm/dt/discord-webhook-node.svg "Total Downloads")
[![Total alerts](https://img.shields.io/lgtm/alerts/g/matthew1232/discord-webhook-node.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/matthew1232/discord-webhook-node/alerts/)
[![Language grade: JavaScript](https://img.shields.io/lgtm/grade/javascript/g/matthew1232/discord-webhook-node.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/matthew1232/discord-webhook-node/context:javascript)

- [Installation](#installation)
- [Examples](#examples)
    - [Basic use](#basic-use)
    - [Custom embeds](#custom-embeds)
    - [Sending files](#sending-files)
    - [Preset messages](#preset-messages)
    - [Custom settings](#custom-settings)
- [Notes](#notes)
- [API](#api)
    - [Webhook](#webhook---class)
    - [MessageBuilder](#messagebuilder---class)
- [License](#license)

# Instalação
```npm install rafaelsupport/discord-webhook-node``` or ```yarn add rafaelsupport/discord-webhook-node```

# Exemplos

## Uso básico
```js
const { Webhook } = require('discord-webhook-node');
const hook = new Webhook("YOUR WEBHOOK URL");

const IMAGE_URL = 'https://homepages.cae.wisc.edu/~ece533/images/airplane.png';
hook.setUsername('Discord Webhook Node Name');
hook.setAvatar(IMAGE_URL);

hook.send("Hello there!");
```

## Embeds customizados
```js
const { Webhook, MessageBuilder } = require('discord-webhook-node');
const hook = new Webhook("YOUR WEBHOOK URL");

const embed = new MessageBuilder()
.setTitle('My title here')
.setAuthor('Author here', 'https://cdn.discordapp.com/embed/avatars/0.png', 'https://www.google.com')
.setURL('https://www.google.com')
.addField('First field', 'this is inline', true)
.addField('Second field', 'this is not inline')
.setColor('#00b0f4')
.setThumbnail('https://cdn.discordapp.com/embed/avatars/0.png')
.setDescription('Oh look a description :)')
.setImage('https://cdn.discordapp.com/embed/avatars/0.png')
.setFooter('Hey its a footer', 'https://cdn.discordapp.com/embed/avatars/0.png')
.setTimestamp();

hook.send(embed);
```

Tenha em mente que o método custom embed `setColor` usa uma cor decimal/uma cor hexadecimal (as cores hexadecimais puras em preto e branco serão imprecisas). Você pode converter cores hexadecimais em decimais usando este site aqui: [https://convertingcolors.com](https://convertingcolors.com)

## Envio de arquivos
```js
const { Webhook } = require('discord-webhook-node');
const hook = new Webhook('YOUR WEBHOOK URL');

hook.sendFile('../yourfilename.png');
```

## Mensagens predefinidas
```js
const { Webhook } = require('discord-webhook-node');
const hook = new Webhook('YOUR WEBHOOK URL');

//Sends an information message
hook.info('**Information hook**', 'Information field title here', 'Information field value here');

//Sends a success message
hook.success('**Success hook**', 'Success field title here', 'Success field value here');

//Sends an warning message
hook.warning('**Warning hook**', 'Warning field title here', 'Warning field value here');

//Sends an error message
hook.error('**Error hook**', 'Error field title here', 'Error field value here');
```

## Configurações customizadas
```js
const { Webhook } = require('discord-webhook-node');
const hook = new Webhook({
    url: "YOUR WEBHOOK URL",
    //If throwErrors is set to false, no errors will be thrown if there is an error sending
    throwErrors: false,
    //retryOnLimit gives you the option to not attempt to send the message again if rate limited
    retryOnLimit: false
});

hook.setUsername('Username'); //Overrides the default webhook username
hook.setAvatar('YOUR_AVATAR_URL'); //Overrides the default webhook avatar
```

# Notas
discord-webhook-node é uma biblioteca baseada em promise, o que significa que você pode usar `.catch`, `.then` e `await`, embora se for bem-sucedido não retornará nenhum valor. Por exemplo:

```js
const { Webhook } = require('discord-webhook-node');
const hook = new Webhook("YOUR WEBHOOK URL");

hook.send("Hello there!")
.then(() => console.log('Sent webhook successfully!'))
.catch(err => console.log(err.message));
```

ou usando async:
```js
const { Webhook } = require('discord-webhook-node');
const hook = new Webhook("YOUR WEBHOOK URL");

(async () => {
    try {
        await hook.send('Hello there!');
        console.log('Successfully sent webhook!');
    }
    catch(e){
        console.log(e.message);
    };
})();
```

Por padrão, ele lidará com a limitação de taxa do Discord e, se houver um erro ao enviar a mensagem (diferente da limitação de taxa), um erro será gerado. Você pode alterar essas opções com as opções de configurações personalizadas abaixo.

# API
## Webhook - class
Constructor
- options (optional) : object
    - throwErrors (optional) : boolean
    - retryOnLimit (optional) : boolean

Methods
- setUsername(username : string) returns this
- setAvatar(avatarURL : string (image url)) returns this
- async sendFile(filePath : string)
- async send(payload : string/MessageBuilder)
- async info(title : string, fieldName (optional) : string, fieldValue (optional) : string, inline (optional) : boolean)
- async success(title : string, fieldName (optional) : string, fieldValue (optional) : string, inline (optional) : boolean)
- async warning(title : string, fieldName (optional) : string, fieldValue (optional) : string, inline (optional) : boolean)
- async error(title : string, fieldName (optional) : string, fieldValue (optional) : string, inline (optional) : boolean)
- 
## MessageBuilder - class
Methods
- setText(text: string)
- setAuthor(author: string (text), authorImage (optional) : string (image url), authorUrl (optional) : string (link))
- setTitle(title: string)
- setURL(url: string)
- setThumbnail(thumbnail : string (image url))
- setImage(image : string (image url))
- setTimestamp(date (optional) number/date object)
- setColor(color : string/number (hex or decimal color))
- setDescription(description : string)
- addField(fieldName : string, fieldValue: string, inline (optional) : boolean)
- setFooter(footer : string, footerImage (optional) : string (image url))

# License

MIT
