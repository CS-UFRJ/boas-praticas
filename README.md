# Manual de Boas Práticas de Desenvolvimento - Ciência Sustentável UFRJ

## Sumário

- [Introdução](#introdução)
- [Codigo Limpo](#codigo-limpo)
  - [Tipos primitivos e tipos complexos](#tipos-primitivos-e-tipos-complexos)
  - [Variáveis](#variaveis)
  - [Classes métodos e funções](#classes-métodos-e-funções)
  - [Arrays](#arrays)
  - [Strings](#strings)
  - [Promises e callbacks](#promises-e-callbacks)
  - [Erros](#erros)
  - [Codigo Morto](#código-morto)
  - [Definiçao de classes e ES6](#definição-de-classes-e-es6)
  - [Arquivo Readme](#arquivo-readme)
  - [Estrutura de diretórios](#estrutura-de-diretorios)
  - [Scripts NPM](#scripts-npm)
- [Fluxo de desenvolvimento](#fluxo)
  - [Fluxo de desenvolvimento](#fluxo-desenvolvimento)
  - [Controle de versão](#versao)
- [Referências](#referencias)

  ## Introdução

Esse guia tem o objetivo de formular boas práticas de programação que deverá ser utilizado pelos grupos de desenvolvimento do trabalho Ciência Sustentável UFRJ. A utilização deste guia durante o desenvolvimento facilitará a padronização garantindo assim uma melhor qualidade de código criado e um melhor intercâmbio entre as equipes.

Este compilado de boas práticas foi baseado em guias já existentes e largamente adotados (referências ao final). A linguagem primária é Javascript, mas o mesmo pode ser adaptado para qualquer linguagem tendo em vista a similaridade de conceitos.

  ## Codigo

## Tipos primitivos e tipos complexos

É importante entendermos que o javascript divide seus tipos em 2 grupos ( primitivos e complexos ) e que a manipulação dos dados é um pouco diferente em cada um desses grupos.

Os tipos primitivos são:

- string
- number
- boolean
- null
- undefined
- symbol

Quando você acessa um tipo primitivo, sempre estará trabalhando direto em seu valor.

```javascript
var nome = 'Pedro';
var outroNome = nome;

outroNome = 'João';

console.log(nome); // Pedro
console.log(outroNome); // João
```

Os tipos complexos são:

- object
- array
- function

Nos tipos complexos você sempre estará trabalhando com a referência, isso significa que se replicarmos o exemplo anterior utilizando objetos, após trocar o nome, o objeto original também será alterado.

```javascript
const pessoa = { nome: 'Pedro' };
const outraPessoa = pessoa;

outraPessoa.nome = 'João'

console.log(pessoa); // { nome: 'João' }
console.log(outraPessoa); // { nome: 'João' }
```

Devemos tomar cuidado com isso, pois essa caracteristica pode gerar [comportamentos indesejados](#efeitos-colaterais-\--clone-objetos-como-argumentos).

## Variaveis

### Para variáveis prefira nomes completos, pronunciáveis e representativos

Escolha bons nomes e não tenha medo de escrever um nome longo. Os nomes devem ser produnciáveis e representar bem o valor que estão recebendo.

Ruim:

```javascript
    let n = 'Pedro Sotero';
    let term = '2199999999';
    let ultimaFat  = '09890809898908';
    let datAtualiz = new Date();
    let req;
    let res;
```

```javascript
    produtos.map(p => {
        fazIsso(p);
        fazAquilo(p);
    });
```

Bom:

```javascript
    let nome = 'Pedro Sotero';
    let terminal = '2199999999';
    let ultimaFatura  = '09890809898908';
    let dataDeAtualizacao = new Date();
    let request;
    let response;
```

```javascript
    produtos.map(produto => {
        fazIsso(produto);
        fazAquilo(produto);
    });
```

### Crie apelidos

Sempre que possível atribua valores a variáveis bem nomeadas. Isso facilita a legibilidade do código.

Ruim:

```javascript
    setTimeout(faca_qualquer_coisa, 600000);
```

Bom:

```javascript
    const DEZ_MINUTOS_EM_MILISEGUNDOS = 600000;
    setTimeout(faca_qualquer_coisa, DEZ_MINUTOS_EM_MILISEGUNDOS);
```

Ruim:

```javascript
    if(cliente.tipoPlano = 'PRE' && cliente.produtos.length > 0)
        migrar();
```

Bom:

```javascript
    let jaPodeMigrar = cliente.tipoPlano = 'PRE' && cliente.produtos.length > 0;

    if(jaPodeMigrar)
        migrar();

    // ou você pode abstrair ainda mais...

    let clientePrepago = cliente.tipoPlano = 'PRE';
    let possuiProdutos = cliente.produtos.length > 0;
    let jaPodeMigrar = clientePrepago && possuiProdutos;

    if(jaPodeMigrar)
        migrar();
```

### Nunca coloque contextos desnecessários

Escreva os objetos de forma clara, sem incluir contextos desnecessários para o entendimento.

Ruim:

```javascript
    let cliente = {
        clienteNome: 'Pedro',
        clienteTerminal: '21999999999',
        clienteAtivo: false
    };

    function ativarCliente(cliente) {
        cliente.clienteAtivo = true;
    };
```

Bom:

```javascript
    let cliente = {
        nome: 'Pedro',
        terminal: '21999999999',
        ativo: false
    };

    function ativarCliente(cliente) {
        cliente.ativo = true;
    };
```

### Let e Const

Sempre use [const](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/const) ou [let](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/let), evite usar var.

Ruim

```javascript
conta = new Conta();

var status_criado = 3;
```

```javascript
let conta = new Conta();

const STATUS_CRIADO = 3;
```

### Evite agrupar declarações com let e const

É mais facil incluir outras variáveis quando você as mantem separadas.

Ruim:

```javascript
const A = 1,
    B = 2,
    C = 3;
```

Bom:

```javascript
const A = 1;
const B = 2;
const C = 3;
```

### Não encadeie atribuições de variáveis

Isso pode criar variaveis globais acidentalmente.

Ruim:

```javascript
let a = b = c = 1;
```

Bom:

```javascript
let a = 1;
let b = a;
let c = a;
```

### Prefira === e !== a == e !=

Os [operadores === e !== são type-safe](https://eslint.org/docs/rules/eqeqeq.html) e podem evitar comportamentos inesperados.

Ruim

```javascript
a == b
foo == true
bananas != 1
value == undefined
typeof foo == 'undefined'
'hello' != 'world'
0 == 0
true == true
foo == null
```

Bom:

```javascript
a === b
foo === true
bananas !== 1
value === undefined
typeof foo === 'undefined'
'hello' !== 'world'
0 === 0
true === true
foo === null
```

## Classes métodos e funções

### Classes, metodos e funcões devem fazer apenas uma coisa (Single Responsability Principle)

O [princípio da responsabilidade única ou SRP](https://drive.google.com/file/d/0ByOwmqah_nuGNHEtcU5OekdDMkk/view) é a regra mais importante desse guia, se você fosse escolher apenas uma regra para aplicar, essa seria ela. Não é a toa que é o primeiro dos [principles os odd]((http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)).

Esse é um conceito primordial da engenharia de software, quando funções fazem muitas coisas é mais muito difícil, entender, refatorar e testar, por isso manter os métodos enxutos e com apenas um propósito tem que ser o seu maior objetivo.

Uma função pode chamar outras funções mas essa só pode ter uma responsabilidade e um motivo para mudar.

[The Principles of OOD](http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)

Ruim:

O método enviar conta está enviando a conta e marcando ela como enviada.

```javascript
function enviarConta(cliente, contas) {
    contas.forEach(conta => {

        emailSender('sistema@email', cliente.email, 'Nova Conta', `Valor ${conta.valor}`).then(() => {
            conta.enviada = true;
            conta.dataDoEnvio = new Date();
            atualizarConta(conta);
        });
    });
};
```

Bom:

Agora a funcionalidade continua igual mas o método de envio de conta não tem mais a responsabilidade de saber como a data de envio de conta tem que ser atualizada.

Repare que cada método tem a sua responsabilidade e pode ser refatorado e testado individualmente.

Caso a forma com que a data do envio é definida mude, não teremos mais que alterar o método de envio de conta. A alteração ocorrerá apenas no método de definição de data de envio.

```javascript
function enviarConta(cliente, contas) {
    contas.forEach(conta => {
        enviarEmailDeConta(cliente.email, conta.valor).then(definirDataDeEnvio);
    });
};

function enviarEmailDeConta(email, valor) {
    return  emailSender('sistema@email', email, 'Nova Conta', `Valor ${valor}`);
};

function definirDataDeEnvio(conta) {
    conta.enviada = true;
    conta.dataDoEnvio = new Date();
    atualizarConta(conta);
};

```

### Evite condicionais

De acordo com o principio da responsabilidade unica, sempre que necessario devemos seprar as coisas para que cada classe, método ou função tenha apenas uma responsabilidade.

Ruim:

```javascript
class Fatura {
    // Fatura do prepago, do pospago, e do cliente misto.
    gerar(cliente) {
        switch(cliente.tipo) {
            case 'pre':
                return this.valor + this.taxa;
            case 'pos':
                return this.valor - this.desconto;
            default:
                return this.valor;
        }
    };
};
```

Bom:

```javascript
// Fatura do cliente misto.
class Fatura {
    gerar(cliente) {
        return this.valor;
    };
};

// Fatura do cliente prepago.
class FaturaPre {
    gerar(cliente) {
        return this.valor + this.taxa;
    };
};

// Fatura do cliente pospago.
class FaturaPos {
    gerar(cliente) {
        return this.valor - this.desconto;
    };
};
```

### Prefira nomes completos, pronunciáveis e representativos

Ruim:

```javascript
function adiar(numero) {
    pagamento.vencimento.add(numero, 'week');
};
```

Bom:

```javascript
function adiarPagamento(numeroDeSemanas) {
    pagamento.vencimento.add(numeroDeSemanas, 'week');
};
```

Ruim:

```javascript
function adiar() {
    pagamento.vencimento.add(1, 'week');
};
```

Bom:

```javascript
function adiarPagamentoEmUmaSemana() {
    pagamento.vencimento.add(1, 'week');
};
```

### Poucos argumentos

É uma boa prática limitar o número de argumentos de uma função. O ideal é que ela tenha apenas 2 ou menos argumentos.

Isso é importante pois torna os testes e a legibilidade mais fáceis. Muitas vezes funções com muitos argumentos estão fazendo mais do que deveriam fazer e quando não estão, usar um objeto como argumento simplifica as coisas.

As versões modernas do Javascript já suportam [*destructuring*](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/Atribuicao_via_desestruturacao) e essa sintaxe pode nos ajudar na definição dos argumento de uma função.

Ruim:

```javascript
function enviarEmail(de, para, copia, assunto, texto) {
    // envia o e-mail
};

enviarEmail('pedro@email', 'fulano@email', null, 'Boas praticas', 'um texto');
```

Ruim:

```javascript
function enviarEmail(email) {
    // envia o e-mail
};

let email = {
    de: 'pedro@email',
    para: 'fulano@email',
    assunto: 'Boas praticas',
    texto: 'um texto'
};


enviarEmail(email);
```

Bom:

Repare que os argumentos ficam explicitos mas não é necessário definir nem "de" nem "copia".

```javascript
function enviarEmail({ de = 'padrao@email.com', para, copia, assunto, texto }) {
    // envia o e-mail
};

let email = {
    para: 'fulano@email',
    assunto: 'Boas praticas',
    texto: 'um texto'
};

enviarEmail(email);
```

### Caso necessário, utilize valores padrão para os argumentos

Péssimo:

```javascript
function boasVindas(nome) {
    if(!nome)
        nome = 'Visitante';

    console.log(`Olá ${nome}`);
};

boasVindas(); // Olá Visitante
boasVindas('Pedro'); // Olá Pedro
```

Ruim:

```javascript
function boasVindas(nome) {
    nome = nome || 'Visitante';
    console.log(`Olá ${nome}`);
};

boasVindas(); // Olá Visitante
boasVindas('Pedro'); // Olá Pedro
```

Bom:

Só tenha cuidado pois os valores padrão só são definidos para argumentos que receberem *undefined*. Caso você chame *boasVindas(null)* por exemplo, o valor padrão não será aplicado para nome.

```javascript
function boasVindas(nome = 'Visitante') {
    console.log(`Olá ${nome}`);
};

boasVindas(); // Olá Visitante
boasVindas('Pedro'); // Olá Pedro
```

Bom:

Quando o argumento for um objeto, utilize [Object.assign](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) na definição dos atributos padrão.

```javascript
function enviarEmail(opcoes) {
    opcoes = Object.assign({}, {
        de: 'padrao@email.com',
        assunto: 'Assunto padrao'
    }, opcoes);
    console.log(opcoes);
};

enviarEmail({ assunto:'Novo assunto', texto: 'Novo texto' });
```

Melhor:

Ou ainda, utilize o padrão destructuring

```javascript
function enviarEmail({ de = 'padrao@email.com', assunto = 'Assunto Padrao', texto, para }) {
    console.log(de, assunto, texto, para);
};

enviarEmail({ assunto:'Novo assunto', texto: 'Novo texto' });
```

## Remova codigos duplicados

Sempre remova duplicidades desnecessárias.

Ruim:

```javascript
function mascararTerminalMovel(terminal) {
    terminal = String(terminal);
    terminal = terminal.replace('-', '');
    return terminal.replace(terminal.substring(2, 7), '*****');
};

function mascararTerminalFixo(terminal) {
    terminal = String(terminal);
    terminal = terminal.replace('-', '');
    return terminal.replace(terminal.substring(2, 6), '*****');
};
```

Bom:

```javascript
function mascararTerminal(terminal) {
    terminal = String(terminal);
    terminal = terminal.replace('-', '');

    numerosVisiveisNoFinal = 4;
    numerosEscondidos = terminal.length - numerosVisiveisNoFinal - 2;
    posicao  = terminal.length - numerosVisiveisNoFinal;
    valorSubstituido = terminal.substring(2, posicao);

    return terminal.replace(valorSubstituido , '*'.repeat(numerosEscondidos));
};
```

### Evite negar condiçoes

Condições negadas podem passar despercebinas na hora da leitura do código. Sempre que possível organize o código de outra forma.

Ruim

```javascript
if(!temEmail)
    mandarSms();
else
    mandarEmail();
```

Bom:

```javascript
if(temEmail)
    mandarEmail();
else
    mandarSms();
```

### Evites elses desnecessários

Se um bloco If sempre executa um return o bloco else torna-se desnecessário.

Ruim:

```javascript
function foo() {
  if (x) {
    return x;
  } else {
    return y;
  }
}

function cats() {
  if (x) {
    return x;
  } else if (y) {
    return y;
  }
}

function dogs() {
  if (x) {
    return x;
  } else {
    if (y) {
      return y;
    }
  }
}
```

Bom:

```javascript
function foo() {
  if (x) {
    return x;
  }

  return y;
}

function cats() {
  if (x) {
    return x;
  }

  if (y) {
    return y;
  }
}

function dogs(x) {
  if (x) {
    if (z) {
      return y;
    }
  } else {
    return z;
  }
}
```

### Efeitos Colaterais - Evite alterar variáveis de fora do escopo

Evite que uma função altere um valor global, isso pode quebrar seu código.

Ruim:

```javascript
let terminal = '21999999999';

function mascararTerminal() {
    terminal = terminal.replace(terminal.substring(2, 7), '*****');
};

mascararTerminal();

console.log(terminal); // 21*****9999
```

Bom:

```javascript
function mascararTerminal(terminal) {
    return terminal.replace(terminal.substring(2, 7), '*****');
};

const terminal = '21999999999';
const terminalMascarado = mascararTerminal(terminal);

console.log(terminal); // 21999999999
console.log(terminalMascarado); // 21*****9999
```

### Efeitos Colaterais - Clone objetos como argumentos

No Javascript tipos primitivos são passados como valor e objetos/arrays são passados como referência. Isso pode causar efeitos colateráis indesejados caso não se tome cuidado.

No caso de objetos ou arrays, caso a função que os receba, faça alterações, essas mudanças se refletirão no objeto original.

Vamos supor que se passe um objeto cliente para uma função e dentro dela esse objeto seja alterado para inativo. Esse status inativo será refletido no objeto original e essa alteração não existirá apenas no contexto da função.

Uma boa solução para esse cenário é SEMPRE CLONAR os objetos de entrada, assim garantimos que qualquer alteração só exista no contexto da função e não reflita no objeto original.

Ruim:

```javascript
function realizarLogin(cliente) {
    // faz algumas coisas
    cliente.ativo = false;
};

let cliente = { nome: 'Pedro Sotero', login: 'Pedro', senha: '123', ativo: true };

console.log('Antes: ', cliente.ativo); // true

realizarLogin(cliente);

console.log('Depois: ', cliente.ativo); // false, a função altera o objeto original
```

Bom:

```javascript
function realizarLogin(cliente) {
    // clona o objeto
    cliente = Object.assign({}, cliente);

    // faz algumas coisas
    cliente.ativo = false;
};

let cliente = { nome: 'Pedro Sotero', login: 'Pedro', senha: '123', ativo: true };

console.log('Antes: ', cliente.ativo); // true

realizarLogin(cliente);

console.log('Depois: ', cliente.ativo); // true, a função não altera o objeto original
```

### Efeitos Colaterais - Evite alterar funções globais

Alterar funções globalmente não é uma boa pratica pois a alteração que você fez reflete para outras bibliotecas que tambem usam a função e isso pode gerar confusão.

Vamos supor que você queria alterar o método toString da classe string para sempre mostrar uma exclamação no final. Se você fizer isso no prototype essa alteração irá refletir para todas as bibliotecas que usem o método toString e essa provavelmente não é a sua intenção.

A forma correta criar uma nova classe extendendo a classe original para só depois alterar seu comportamento.

Ruim

```javascript
String.prototype.toString = function() {
    return this + "!!!"
};

"Ola".toString();
```

Bom:

```javascript
class StringQueGrita extends String {
    toString() {
        return this + "!!!"
    };
};

new StringQueGrita("Ola").toString();
```

## Arrays

Existem diversas formas de trabalhar com arrays, a idéia aqui é expor algumas delas mas se possível, visite a [documentação](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array) e conheça diversos outros recursos interessantes dos arrays.

### Filter

Caso precise filtrar um array, use [filter](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/filtro) ao invés de ifs dentro de for.

Ruim:

```javascript
let terminais = ['21985665236', '21985652555', '2125706666'];

let numerosMoveis = [];

for (let i = 0; i < terminais.length; i++)
    if (terminais[i].length == 11)
        numerosMoveis.push(terminais[i]);

console.log('Números móveis:', numerosMoveis);

// Números móveis: [ '21985665236', '21985652555' ]
```

Bom:

```javascript
let terminais = ['21985665236', '21985652555', '2125706666'];

let numerosMoveis = terminais.filter(terminal => terminal.length == 11);

console.log('Números móveis:', numerosMoveis);

// Números móveis: [ '21985665236', '21985652555' ]
```

### Find

Use o [find](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/find) para recuperar um determinado item do array.

```javascript
let clientes = [{ nome: 'João' }, { nome: 'Pedro' }, { nome: 'Ana' }];

let Pedro = clientes.find(cliente => cliente.nome === 'Pedro');

console.log(Pedro);

// { nome: 'Pedro' }
```

Você pode obter um resultado semelhante com o filter com a diferença de que o filter sempre retornará um array e o find retorna o próprio item encontrado. Caso não encontre nada, o filter retornará [] e o find undefined.

```javascript
let clientes = [{ nome: 'João' }, { nome: 'Pedro' }, { nome: 'Ana' }];

console.log(clientes.filter(cliente => cliente.nome === 'Pedro'));
// [ { nome: 'Pedro' } ]

console.log(clientes.find(cliente => cliente.nome === 'Pedro'));
// { nome: 'Pedro' }

console.log(clientes.filter(cliente => cliente.nome === 'Fulano'));
// []

console.log(clientes.find(cliente => cliente.nome === 'Fulano'));
// undefined
```

### Sort

Use o [sort](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) caso queira ordenar um array.

```javascript
let nomes = ['Carlos', 'Pedro', 'André', 'Bruna'];
let numeros = [10, 3, 80, 20, 2, 1];
let clientes = [{ nome: 'João' }, { nome: 'Pedro' }, { nome: 'Ana' }];

console.log(numeros.sort((primeiro, segundo) => primeiro - segundo));
// [ 1, 2, 3, 10, 20, 80 ]

console.log(nomes.sort());
// [ 'André', 'Bruna', 'Carlos', 'Pedro' ]

console.log(nomes.sort().reverse());
// [ 'Pedro', 'Carlos', 'Bruna', 'André' ]

console.log(clientes.sort((primeiro, segundo) => primeiro.nome > segundo.nome));
// [ { nome: 'Ana' }, { nome: 'Pedro' }, { nome: 'João' } ]
```

### Map

Utilize o map sempre que precisar aplicar uma função a cada item do array e obter um novo array como resultado.

```javascript
let tamanhosEmKb = [1500, 100, 450, 2048];

// transformando em MegaBytes
let tamanhosEmMb = tamanhosEmKb.map(tamanho => tamanho / 1024);

// novo array
console.log('Tamanhos em MB:', tamanhosEmMb);
// [ 1.46484375, 0.09765625, 0.439453125, 2 ]

// não altera o array original
console.log('Tamanhos em KB:', tamanhosEmKb);
// [ 1500, 100, 450, 2048 ]
```

Só temos que tomar cuidado com arrays de [objeto](https://developer.mozilla.org/pt-BR/docs/Glossario/objeto), pois por não serem tipos [primitivos](https://developer.mozilla.org/pt-BR/docs/Glossario/Primitivo) não são passados por valor para as funções. No caso de objetos o javascript passa por referência e isso faz com  que o objeto original seja alterado.

```javascript
let faturas = [{
    mes: 'janeiro',
    valor: 30.45,
    paga: false
}, {
    mes: 'fevereiro',
    valor: 20.10,
    paga: false
},
{
    mes: 'março',
    valor: 60,
    paga: false
}];

let faturasPagas = faturas.map(fatura => {
    fatura.paga = true;
    return fatura;
});

// novo array
console.log('Faturas Pagas:', faturasPagas);
// [ { mes: 'janeiro', valor: 30.45, paga: true },
// { mes: 'fevereiro', valor: 20.1, paga: true },
// { mes: 'março', valor: 60, paga: true } ]

// MODIFICA o array original
console.log('Faturas:', faturas);
// [ { mes: 'janeiro', valor: 30.45, paga: true },
// { mes: 'fevereiro', valor: 20.1, paga: true },
// { mes: 'março', valor: 60, paga: true } ]
```

Para resolver esse problema, você precisaria clonar o objeto antes de alterar.

```javascript
let faturas = [{
    mes: 'janeiro',
    valor: 30.45,
    paga: false
}, {
    mes: 'fevereiro',
    valor: 20.10,
    paga: false
},
{
    mes: 'março',
    valor: 60,
    paga: false
}];

let faturasPagas = faturas.map(fatura => {
    // clona o objeto antes de alterar para que a alteração não afete o array original
    let copiaFatura = Object.assign({}, fatura);

    copiaFatura.paga = true;
    return copiaFatura;
});

// novo array
console.log('Faturas Pagas:', faturasPagas);
// [ { mes: 'janeiro', valor: 30.45, paga: true },
// { mes: 'fevereiro', valor: 20.1, paga: true },
// { mes: 'março', valor: 60, paga: true } ]

// NAO modifica o array original
console.log('Faturas:', faturas);
// [ { mes: 'janeiro', valor: 30.45, paga: false },
// { mes: 'fevereiro', valor: 20.1, paga: false },
// { mes: 'março', valor: 60, paga: false } ]
```

### ForEach

Use o [foreach](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) para executar uma função para cada item do array.

```javascript
let clientes = [{ nome: 'Pedro', email: 'd@d.com' }, { nome: 'Maria', email: 'm@m.com' }];

function enviarEmailBoasVindas(cliente) {
    console.log(`Enviando email para ${cliente.nome} <${cliente.email}>`);
};

clientes.forEach(enviarEmailBoasVindas);

// Enviando email para Pedro <d@d.com>
// Enviando email para Maria <m@m.com>
```

### Reduce

Para fazer somas ou reduzir um array, utilize o [reduce](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce). Essa função pode fazer coisas interessantes se usada com sabedoria, recomendo a leitura da [documentação](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce).

```javascript
let faturas = [{
    mes: 'janeiro',
    valor: 30.45
}, {
    mes: 'fevereiro',
    valor: 20.10
},
{
    mes: 'março',
    valor: 60
}];

let total = faturas.reduce((resultado, fatura) => resultado + fatura.valor, 0);

console.log('Total acumulado:', total); // 110.55
```
## Strings

### Aspas

Prefira o uso de aspas simples.

Ruim

```javascript
const nome = "Pedro";
```

Bom

```javascript
const nome = 'Pedro';
```

### Multiplas linhas

Evite escrever strings em multiplas linhas, quebrar strings torna o código menos buscavel.

Ruim

```javascript
const observacao = 'Lorem ipsum dolor sit amet, consectetur \ adipiscing elit. Aliquam ullamcorper metus ac tortor mattis, \ eget sagittis diam imperdiet. Morbi ac bibendum libero. In \ at felis augue. Maecenas id euismod augue, at. Vestibulum \ dapibus nunc placerat hendrerit tempus.';

const observacao = 'Lorem ipsum dolor sit amet, consectetur ' +
'adipiscing elit. Aliquam ullamcorper metus ac tortor mattis ' +
'eget sagittis diam imperdiet. Morbi ac bibendum libero. In ' +
'at felis augue. Maecenas id euismod augue, at. Vestibulum ' +
'dapibus nunc placerat hendrerit tempus.';
```

Bom

```javascript
const observacao = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam ullamcorper metus ac tortor mattis, eget sagittis diam imperdiet. Morbi ac bibendum libero. In at felis augue. Maecenas id euismod augue, at sollicitudin diam. Vestibulum dapibus nunc placerat hendrerit tempus.';
```

### Concatenação

Para concatenar prefira usar [template strings](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/template_strings).

Ruim

```javascript
function Ola(nome) {
    console.log('Ola' + nome);
}
```

Bom

```javascript
function Ola(nome) {
    console.log(`Ola ${nome}`);
}
```


## Promises e callbacks

Callbacks não são tão claros de ler, por isso, prefira usar [promises](https://javascript.info/async).

O formato das promisses além de nos afastar do **calback hell**, facilita a leitura e o entendimento do código.

```javascript
boasVindas(nome) {
    return new Promise((resolve, reject) => {
        resolve(`Olá ${nome}`);
    });
};


boasVindas().
    then(console.log);
```

E você pode [encadear](https://javascript.info/promise-chaining) o retorno passando o resultado sempre adiante.

```javascript
function processarContasVencidas(cpf)
{
    carregarCliente(cpf)
        .then(possuiContasVencidas)
        .then(enviarEmailDeCobranca)
        .catch(console.error);
}

function carregarCliente(cpf) {
    return new Promise((resolve, reject) => {
        // ...
        resolve(cliente);
    });
};

function possuiContasVencidas(cliente) {
    return new Promise((resolve, reject) => {
        // ...
        resolve(cliente);
    });
};

function enviarEmailDeCobranca(cliente) {
    // ...
    return servicoDeEnvioDeEmail.enviar(mensagem);
};
```

Se você precisar promissificar uma classe que já exista mas está feita com com callbacks, é possivel usar o método [promisifyAll](http://bluebirdjs.com/docs/api/promise.promisify.html) do [bluebird](http://bluebirdjs.com).

Repare como a legibilidade aumenta quando paramos de usar callbacks e usamos promises.

Ruim:

```javascript
import { get } from 'request';
import { writeFile } from 'fs';

get('http://uma_api_qualquer', (requestErr, response) => {
  if (requestErr)
    console.error(requestErr);
  else {
    writeFile('arquivo.txt', response.body, (writeErr) => {
      if (writeErr)
        console.error(writeErr);
      else
        console.log('Arquivo gravado');
    });
  }
});
```

Bom:

```javascript
import { get } from 'request';
import { writeFile } from 'fs';

get('http://uma_api_qualquer')
  .then((response) => {
    return writeFile('arquivo.txt', response);
  })
  .then(() => {
    console.log('Arquivo gravado');
  })
  .catch(console.error);
```

[Async e Await](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/await) são ainda mais legiveis que as promises, mas não fazem parte do ES6. Caso você já use ES7 poderia escrever o exemplo acima da seguinte forma:

```javascript
import { get } from 'request';
import { writeFile } from 'fs';

async pegarArquivo()
{
    try {
        const response = await get('http://uma_api_qualquer');
        await writeFile('arquivo.txt', response);
        console.log('Arquivo gravado');
    } catch (error) {
        console.log(error);
    }
}
```

## Erros

### Não ignore os erros

Não ignore os erros os impactos podem ser enormes para o negócio.

Péssimo

```javascript
enviarSms()
    .then(() => { ... });
```

Ruim:

```javascript
enviarSms()
    .then(() => { ... });
    .catch(console.log);
```

Bom:

Sempre logue os erros e se possível tome alguma ação para que esse erro seja analisado, resolvido ou contornado.

```javascript
enviarSms()
    .then(() => { ... });
    .catch(erro => {
        console.error(erro);
        logger.logarErro(erro);
        notificarAdministrador(erro);
    });
```

## Código Morto

Uma coisa muito importante que temos que ter em mente é que nosso código é vivo. Estamos em um ambiente onde perseguimos a melhora contínua e onde mudanças são bem vindas, logo, temos que nos preocupar diáriamente com a saúde do nosso projeto e não tem nada mais "morto" que um código antigo comentado, não é mesmo?

Por isso **prefira apagar códigos do que comentar**, lembre que utilizamos ferramentas de controle de versão que estão ai para nos ajudar caso seja necessário dar uma olhada em uma versão anterior do arquivo.

Sempre que perceber que um método, classe, arquivo etc. não está mais sendo utilizado pelo sistema, sinta-se a vontade para apaga-lo! Dessa forma garantimos que o projeto está sempre em sua versão mais enxuta e evitamos que tempo seja perdido analisando coisas que não usamos mais.

Ruim:

```javascript
function exibirAlerta(texto) {
    // mudamos a biblioteca de notificacao
    // jupter.show({
    //     size: 3000,
    //     overflow: 'hidden',
    //     color: 'red',
    //     title: 'Cuidado';
    // }, texto);

    novaClasseDeNotificacao.exibir(texto);
};

// function esconderAlertas() {
//     // escondendo os alertas da jupter
// }
```

Bom:

```javascript
function exibirAlerta(texto) {
    novaClasseDeNotificacao.exibir(texto);
};
```

## Definição de classes e ES6

Dê preferência ao modelo ES6 por ser mais conciso e facil de entender.

- [Medium guia ES6](https://medium.com/@matheusml/o-guia-do-es6-tudo-que-voc%C3%AA-precisa-saber-8c287876325f)
- [https://github.com/lukehoban/es6features](https://github.com/lukehoban/es6features)

Ruim:

```javascript
Fatura = function() {
    this.logger = require('logger');
};

Fatura.prototype.gerar = function(cliente) {
        // ...
};

Fatura.prototype.arquivar = function(fatura) {
        // ...
};
```

Bom:

```javascript
class Fatura {
    constructor() {
        this.logger = require('logger');
    };

    gerar(cliente) {
        // ...
    };

    arquivar(fatura) {
        // ...
    };
};
```

### Aproveite o recurso de herança

Utilize [extends](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Classes/extends) para extender classes.
```javascript
class Cachorro {
    constructor() {
        console.log('Sou um cachorro!');
    }

    latir() {
        console.log('Au au au au');
    }

    abanarRabo() {
        console.log('flap flap flap!');
    }
}

class CachorroRouco extends Cachorro {
    constructor() {
        super();
        console.log('mas sou rouco :(');
    }

    latir() {
        console.log('A.... A... ...');
    }
}

let cachorro = new Cachorro();

cachorro.latir();
cachorro.abanarRabo();

// Sou um cachorro!
// Au au au au
// flap flap flap!

let cachorroRouco = new CachorroRouco();

cachorroRouco.latir();
cachorroRouco.abanarRabo();

// Sou um cachorro!
// mas sou rouco :(
// A.... A... ...
// flap flap flap!
```

## Arquivo Readme


Devido a natureza do projeto é de extrema importância dedicar um esforço ao readme, assim facilitanto o entendimento pelos outros grupos de desenvolvimento e aos próximos passos a serem tomados para a consolidação do projeto. 

Em primeiro lugar procure sempre colocar uma descrição breve e clara dos objetivos do projeto:

```bash
Visão Geral

Esse crawler usa a biblioteca X para navegar pelas páginas da plataforma Y para captura de dados e metadados.
```

Em seguida, você pode colocar o e-mail dos responsáveil pelo projeto assim, caso alguem tenha alguma dúvida saberá quem procurar.

```bash
Equipe X:

- Joao <j@email>
- Maria <m.@email>
```

Depois dessas informaçõe básicas você pode usar o Readme para ensinar a instalação e configuração do projeto, colocar informações de ambiente, exemplos de chamadas, links para documetação etc.

O importante é que seu readme não seja um arquivo sem propósito. Use ele para ajudar as pessoas que podem precisar do seu projeto.

Esse [repositório](https://github.com/matiassingers/awesome-readme) tem alguns exemplos de arquivos de readme bem escritos.

## Fluxo de desenvolvimento

1. Crie um novo branch no seu repositório local.
2. Implemente suas mudanças
3. Faça um push do seu branch para o repositório remoto
4. Submeta um Pull Request (no caso do GitHub, na página do repositório ou utilizando a aplicação desktop)
5. Aguarde a revisão do mesmo por outros desenvolvedores da sua equipe
6. Caso alguma mudança seja solicitada, implemente-a.
7. Seus colegas de equipe integrarão seu código quando ele estiver ok.

A etapa 6 é feita na página de discussão do pull request. Nela é possível fazer comentários sobre trechos de código ou sobre o pull request em geral, tal como um fórum.

Exemplo: https://github.com/coopera/teamtracker/pull/44

Orientações:
- Revisor:
  - Seja cordial com quem está submetendo o pull request. 
  - Assuma que ele não necessariamente possui o mesmo conhecimento que você e seja claro com seus pedidos de mudança.
  - Caso conheça algum tutorial ou link útil para resolver algum problema apontado, compartilhe-o na área de discussão do pull request.
- Desenvolvedor que submeteu o pull request:
  - Responda os questionamentos dos revisores.
  - Caso algum revisor tenha solicitado algo e você não consiga entender a solicitação, não hesite em questioná-lo.

## Controle de versão
- Mensagens de commit devem ser curtas e objetivas. Exemplos:
  - "Fix bug at user controller"
  - "Add sign in feature"
- Commits devem possuir poucas mudanças, em geral. Ou seja: commite constantamente.
- Se o seu código for público, também é preferível que suas mensagens de commit estejam em inglês.
- Caso sua equipe use algum issuetracker (como o do próprio GitHub, por exemplo), lembre-se de referenciar o número da issue em que você está trabalhando no commit. Exemplo:
  - Existe a issue: #10 - Fix password bug.
  - Seu commit que corrige tal bug deve ser algo do tipo: "Change user password algorithm. Fix #10"
- Pulls Request devem ser revisados o quanto antes, para evitar que quem os tenha feito fique "bloqueado" enquanto aguarda feedback

  ## Referências

- [Boas Práticas de programação - Node](https://github.com/pedrosoteroth/boas-praticas/blob/master/README.md#arquivo-readme)

- [Boas Práticas de Desenvolvimento](https://github.com/coopera/boas-praticas/blob/master/README.md)