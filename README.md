# AngularTutorial
Um tutorial de Angular!

## Instalando e iniacializando o Angular

```shell
$ npm install -g @angular/cli
$ ng new app-name
$ cd app-name
$ ng serve
```

# Conceitos Iniciais:

Vamos iniciar usando o ngModel. Primeiro é necessário importaro FormsModule do @angular/forms:
```typescript
import { FormsModule } from '@angular/forms';
...
imports {
	FormsModule
},
..
```
No arquivo app.component.html:
```html
<input type="text" [(ngModel)] = "name">
<p>{{ name }}</p>
```
No arquivo app.component.ts:
```typescript
export class AppComponent {
	name = 'Max';
}
```
O codigo acima faz com que o texto colocado no input seja automaticamente colocado na pagina sem precisar de recarregar.

### Adicionando o BootStrap:
No terminal na pasta do projeto:
```shell
npm install --save bootstrap@3
```
Também é necessário adicionar a dependencia do bootstrap no arquivo angular.json
```json
"styles": [
    "node_modules/bootstrap/dist/css/bootstrap.min.css",
    "src/styles.css"
],
```

### Criando Componentes:
```shell
ng generate component component-name
ng g c component-name                  // abreviando os comandos
```

### Selectors
Selectors dão o nome a tag que colocaremos no arquivo .html para chamar o componente.
```typescript
@Component({
  selector: 'app-servers',
  ...
})
```
No arquivo .html:
```html
<app-servers></app-servers>
```

### String Interpolation:
Forma de fazer com que o codigo typescript transmita informações para o html.
```typescript
export class ServerComponent {
    serverId:number = 10;
    serverStatus: string = 'offline';
    
    getServerStatus() {
        return this.serverStatus;
    }
}
```
No arquivo .html:
```html
<p>Server with ID {{ serverId }} is {{ getServerStatus() }}</p>
```

### Property Binding:
Outra forma de realizar as mesmas coisas que a string interpolation.
```html
<button class="btn btn-primary" [disabled] = "!allowNewServer">Add Server</button>
```
No aquivo typescript:
```typescript
export class ServersComponent implements OnInit {
  allowNewServer:boolean = false;
  constructor() { 
    setTimeout(() => {
      this.allowNewServer = true;
    }, 2000);
  }
}
```
Após 2 segundos o botão se tornará utilizável.

As duas formas abaixo são equivalentes, usando string interpolation e property binding:
```html
<p> {{ allowNewServer }} </p>
<p [innerText] = "allowNewServer"></p>
```

### $event:
Utilizamos uma função para alterar elementos no arquivo .html dinamicamente
```html
<label>Server name</label>
<input type="text" class="form-control" (input)="onUpdateServerName($event)">
<p>{{ serverName }}</p>
```
A variavel $event passa tudo aquilo que está acontecendo com o input. No arquivo app.component.ts usamos (<HTMLInputElement>event.target).value para acessar o que o usuario digitou no input:
```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-servers',
  templateUrl: './servers.component.html',
  styleUrls: ['./servers.component.css']
})
export class ServersComponent implements OnInit {
  serverName = '';
  constructor() { }
  ngOnInit() {
  }
  onUpdateServerName(event: Event) {
    this.serverName = (<HTMLInputElement>event.target).value;
  }
}
```
Uma maneira mais faciel é usar o [(ngModel)].
É possível deixar o botão desabilitado enquanto o input não estiver preenchido 
```html
<input type="text" class="form-control" [(ngModel)] = serverName>
<p>{{ serverName }}</p>

<button class="btn btn-primary" [disabled] = "serverName === ''" (click)="serverName = ''">Add Server</button>
```
