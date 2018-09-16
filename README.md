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
...
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
	
Após clicar no botão, o input é limpo.

# Directives:

### ngIf
Para mostrar ou não conteúdos dentro de uma tad html.
```html
<p *ngIf="true_false_conditional">Texto que só aparece quando é true</p>
```
Também é possivel criar uma espécie de else:
```html
<p *ngIf="serverCreated; else casoContrario">Server was created, server name is {{ serverName }}</p>
<ng-template #casoContrario>
    <p>Texto que aparece caso contrario.</p>
</ng-template>
```
Exemplo de aplicação: toggle simples.
```html
<button class="btn btn-primary" (click)="variavel = !variavel">Add Server</button>
<p *ngIf="variavel">Senha secreta = luna</p>
```
Fica alternando entre true e false, fazendo com que o paragrafo desapareça se estiver na tela, ou apareça caso contrário.


### ngStyle
Podemos usar o ngStyle para mudar o css de um elemento dinamicamente.
```html
<p [ngStyle]="{'background-color': getColor()}">Some text</p>
```
No arquivo typescript:
```typescript
getColor() {
    return this.serverStatus === 'online' ? 'green' : 'red';
}
```

### ngClass
Adiciona uma classe css se uma determinada condição for satisfeita.
```html
<p [ngClass]="{'online': serverStatus === 'online'}">Server with ID {{ serverId }} is {{ getServerStatus() }}</p>
```
Adicionará a classe .online apenas se o server estiver online (condição de true or false). No arquivo typescript:
```typescript
@Component({
    selector: 'app-server',
    templateUrl: './server.component.html',
    styles: [`
        .online {
            color: white;
        }
    `]
})
```

### ngFor:
Para adicionar uma lista de elementos no DOM utilizamos o ngFor.
```html
<app-server *ngFor="let nome_de_variavel_qualquer of array_definido_app.component.ts"></app-server>
```
Ele tem um funcionamento semelhante ao forEach. Exemplo de aplicação:
```html
<app-server *ngFor="let server of servers"></app-server>
```
```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-servers',
  templateUrl: './servers.component.html',
  styleUrls: ['./servers.component.css']
})
export class ServersComponent implements OnInit {
  serverName = '';
  servers = ['Testserver', 'Testserver 2'];
  onCreateServer() {
    this.serverCreated = true	// Obtido atraves do ngModel
    this.servers.push(this.serverName); // Adicionando mais um elemento no array de listas
  }
}
```
Exemplo de aplicação simples:
```html
<div 
  *ngFor="let logItem of log; let i = index"  // a posição de cada elemento
  [ngStyle]="{backgroudColor: i >= 4 ? 'blue' : 'transparent'}">{{ logItem }} // se a posição no vetor for >= 4 escolhe azul
  [ngClass]="{'white-text': i >= 4}"  // se a posição for >= 4 a classe white-text, definida previamente, é adicionada
</div>
```
