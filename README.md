# Lúcida JavaScript code style (Google Tag Manager - GTM)

## Escopo das tags
Para garantir que as variáveis da tag não vazem para o escopo da aplicação, utilize o pattern `closure` sempre que possível,
salvo em casos como seletores de jQuery simples ou em tags que são apenas atribuição de eventos.

```javascript
// Ruim
var names = ['WROI', 'Lúcida'],
    name  = names.join(' + ')

console.log(name) // WROI + Lúcida
```

```javascript
// Bom
;(function() {
    var names = ['WROI', 'Lúcida'],
        name  = names.join(' + ')
    
    console.log(name) // WROI + Lúcida
}());
```

## Funções aninhadas como variáveis
Quando for necessário declarar uma função dentro de uma tag, ela deve ser atribuída a uma variável. O próprio GTM não permite
a declaração de funções de outra forma dentro de uma tag. Para facilitar o debugging, evite o uso de funções anônimas.

```javascript
// Ruim
;(function() {
    var num1 = 1,
        num2 = 3

    function subtract(val1, val2) {
        return val1 - val2
    }

    subtract(num1, num2) // -2
}());
```

```javascript
// Bom
;(function() {
    var num1 = 1,
        num2 = 3

    var subtract = function subtract(val1, val2) {
            return val1 - val2
        }

    subtract(num1, num2) // -2
}());
```

## Identação com *tab size* 4
A identação do código deve ser feita com `tab` de tamanho 4. Como as tags do GTM possuem limite de caracteres, essa prática
ajuda a evitar que o código atinja tal limite.

```javascript
// Ruim
;(function() {
  var bar = gtm.bar || ''
  var foo = function() {
      // Stuff
    }
}());
```

```javascript
// Bom
;(function() {
    var bar = gtm.bar || ''
    var foo = function() {
            // Stuff
        }
}());
```

## Ponto-e-vírgula
Não finalize os comandos com ponto-e-vírgula. Como não são obrigatórios no JavaScript, eles acabam sendo apenas uma poluição
visual no código, além de evitar que se atinja o limite máximo de caracteres por tag dentro do GTM.

```javascript
// Ruim
;(function() {
    var foo = '';
    var bar = function() {
            // Stuff..
        };
}());
```

```javascript
// Bom
;(function() {
    var foo = ''
    var bar = function() {
            // Stuff..
        }
}());
```

## Declaração de variáveis
A declaração de variáveis deve ser feita no início do bloco de código e, de preferência, elas já devem ser inicializadas.
Caso o script demande mais de uma variável, separe suas declarações com vírgula.

```javascript
// Ruim
;(function() {
    if (name.indexOf('Lúcida') > 0) {
        status = true
    }

    var name
    var status

    name = "WROI + Lúcida"
}());
```

```javascript
// Bom
;(function(){
    var name   = 'WROI + Lúcida',
        status = false
    
    if (name.indexOf('Lúcida') > 0)
        status = true
}());
```

## Aspas simples em *strings*
Sempre declare strings utilizando aspa simples `('')`. Como no JavaScript não há diferenciação dela para a aspa dupla `("")`, 
estas acabam causando um peso maior e desnecessário na leitura (por seres humanos) do código.

```javascript
// Ruim
;(function() {
    var part1 = "WROI",
        part2 = "Lúcida",
        name  = part1 +  " + " + part2
    
    console.log("Nome:", name) // Nome: WROI + Lúcida
}());
```

```javascript
// Bom
;(function() {
    var part1 = 'WROI',
        part2 = 'Lúcida',
        name  = part1 + ' + ' + part2
    
    console.log('Nome:', name) // Nome: WROI + Lúcida
}());
```

## `Else` e `default` case
Evite o uso do statement `else`, quando num bloco de `if`, e o case `default`, quando num `switch...case`. Procure sempre 
tratar as possíveis exceções antes de começar a executar o bloco condicional.

```javascript
// Ruim
;(function() {
    var getDayWeekNameOf = function getDayWeekNameOf(num) {
            if (num == 1)
                return 'domingo'
            else if (num == 2)
                return 'segunda'
            // (...)
            else
                return 'número inválido'
        }
}());
```

```javascript
// Bom
;(function() {
    var getDayWeekNameOf = function getDayWeekNameOf(num) {
            if (num < 1 || num > 7)
                return 'número inválido'

            if (num == 1)
                return 'domingo'
            else if (num == 2)
                return 'segunda'
            // (...)
        }
}());
```

## Chave de abertura de bloco na mesma linha da declaração
Quando declarar um bloco (`if...else if`, `switch...case`, `for`, `while`), sempre o inicie abrindo a chave na mesma linha da
declaração dele. Quando o bloco tiver apenas uma linha, omita as chaves

```javascript
// Ruim
;(function() {
    if (num >= 4)
    {
        return 1
    }
    else if (num <= 1)
    {
        return -1
    }

    return 0
}());
```

```javascript
// Bom
;(function() {
    if (num >= 4)
        return 1
    else if (num <= 1)
        return -1
    
    return 0
}());
```

## `do...while()` e `while()`
**Jamais** utilize o laço `do...while()` e evite ao máximo o uso do `while`. Priorize sempre o uso de *high order functions*
(`forEach()`, `map()`, `filter()`, `reduce()`) ao invés das estruturas de repetição tradicionais. Como cada functor tem um uso 
bem definido, elas facilitam o entendimento do código por seres humanos. Sempre que a compatibilidade com browsers muito antigos 
for um requisito, utilize *polyfills* que permitam o uso das funções.

```javascript
// Péssimo
;(function() {
    var list  = ['WROI', 'Lúcida'],
        index = 0

    do {
        console.log(list[index])
        index++
    } while(index < list.length)
}());
```

```javascript
// Ruim
;(function {
    var list  = ['WROI', 'Lúcida'],
        index = 0

    while(i < list.length) {
        console.log(list[index])
        index++
    }
}());
```

```javascript
// Ruim
;(function() {
    var list = ['WROI', 'Lúcida']

    for (var index = 0; index < list.length; index++) {
        console.log(list[index])
    }
}());
```

```javascript
// Bom
;(function() {
    var list = ['WROI', 'Lúcida']

    list.forEach(function(item) {
        console.log(item)
    })
}());
```

## lowerCamelCase
Sempre declare métodos e variáveis utilizando o padrão lower camel case, à exceção de funções construtoras, que devem serguir
o padrão upper camel case (UpperCamelCase).

```javascript
// Ruim
;(function() {
    var GetMultiplicationOf = function(num1, num2) {
            return num1 * num2
        }
    var result = GetMultiplicationOf(2,3)

    console.log(result) // 6
}());
```

```javascript
// Bom
;(function() {
    var getMultiplicationOf = function(num1, num2) {
            return num1 * num2
        }
    var result = getMultiplicationOf(2,3)
    
    console.log(result) // 6
}());
```

## *English only*
Sempre declare variáveis, métodos e atributos em inglês, tomando cuidado para não fazer uso de keywords reservadas pelo
javascript (atenção maior quando em strict mode).

```javascript
// Ruim
;(function() {
    var primeiroNome = 'WROI',
        segundoNome  = 'Lúcida'

    console.log(primeiroNome + ' + ' + segundoNome) // WROI + Lúcida
}());
```

```javascript
// Bom
;(function() {
    var firstName  = 'WROI',
        secondName = 'Lúcida'
    
    console.log(firstName + ' + ' + secondName) // WROI + Lúcida
}());
```

## `$()` ao invés de `jQuery()`
Quando fizer uso do jQuery em tags, utilize sempre a variável `$` para referenciá-lo, passando `jQuery` como parâmetro 
da closure.

```javascript
// Ruim
;(function() {
    jQuery('#elementId').on('click', function(event) {
        // Stuff...
    })
}());
```

```javascript
// Bom
;(function($) {
    $('#elementId').on('click', function(event) {
        // Stuff...
    })
}(jQuery));
```

## Linhas com 80 caracteres
Procure sempre não ultrapassar muito o limite de 80 caracteres por linha de código.

```javascript
// Lamentável
;(function() {
    var variableNameVeryVeryBigBecauseWeNeedIt = [100, 400, 300, 600, 200, 900, 1000, 700, {isTrue: true}]

    variableNameVeryVeryBigBecauseWeNeedIt.forEach(function(variableNameStillBigEnoughToDemonstrate) {
        console.log(variableNameStillBigEnoughToDemonstration)
    })
}());
```

```javascript
// Bom
;(function() {
    var values = [100, 400, 300, 600]

    values.forEach(function(value) {
        console.log(value)
    })
}());
```