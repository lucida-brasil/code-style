# Lúcida JavaScript code style (Google Tag Manager - GTM)

## Escopo das tags
Para garantir que as variáveis da tag não vazem para o escopo da aplicação, utilize
o pattern `module` sempre que possível, salvo em casos como seletores de jQuery
simples ou em tags que são apenas atribuição de eventos.

```html
<!-- Ruim -->
<script>
var names = ['WROI', 'Lúcida']
var name  = names.join(' + ')

console.log(name) // WROI + Lúcida
</script>
```

```html
<!-- Bom -->
<script>
;(function() {
    var names = ['WROI', 'Lúcida'],
    var name  = names.join(' + ')

    console.log(name) // WROI + Lúcida
}());
</script>
```

## Funções aninhadas como variáveis
Quando for necessário declarar uma função dentro de uma tag, ela deve ser atribuída
a uma variável. O próprio GTM não permite a declaração de funções de outra forma
dentro de uma tag. Para facilitar o debugging, evite o uso de funções anônimas.

```html
<!-- Ruim -->
<script>
;(function() {
    var num1 = 1
    var num2 = 3

    function subtract(val1, val2) {
        return val1 - val2
    }

    subtract(num1, num2) // -2
}());
</script>
```

```html
<!-- Bom -->
<script>
;(function() {
    var num1 = 1
    var num2 = 3

    var subtract = function subtract(val1, val2) {
            return val1 - val2
        }

    subtract(num1, num2) // -2
}());
</script>
```

## Variáveis privadas
Sempre inicie o nome de variáveis privadas (cujo escopo exista somente dentro de
uma `function`, à exceção de `closures`) com *underscore* (`_`), seguido do nome
em *camel case*.

```html
<!-- Ruim -->
<script>
;(function() {
    var range = function range() {
        var value_ranges = [
                {limit : 100,            range : '0 - 100'},
                {limit : 500,            range : '101 - 500'},
                {limit : 1000,           range : '501 - 1000'},
                {limit : Math.MAX_VALUE, range : '1000+'}
            ]

        return function getRangeOf(value) {
            return value_ranges.filter(function(range) {
              range.limit < value
            })[0]['range']
        }
    }
}());
</script>
```

```html
<!-- Bom -->
<script>
;(function() {
    var range = function range() {
        var _valueRanges = [
                {limit : 100,            range : '0 - 100'},
                {limit : 500,            range : '101 - 500'},
                {limit : 1000,           range : '501 - 1000'},
                {limit : Math.MAX_VALUE, range : '1000+'}
            ]

        return function getRangeOf(value) {
            return _valueRanges.filter(function(range) {
              range.limit < value
            })[0]['range']
        }
    }
}());
</script>
```

## Identação com *tab size* 4
A identação do código deve ser feita com `tab` de tamanho 4. Como as tags do GTM
possuem limite de caracteres, essa prática ajuda a evitar que o código atinja
tal limite.

```html
<!-- Ruim -->
<script>
;(function() {
  var bar = gtm.bar || ''
  var foo = function() {
      // Stuff
    }
}());
</script>
```

```html
<!-- Bom -->
<script>
;(function() {
    var bar = gtm.bar || ''
    var foo = function() {
            // Stuff
        }
}());
</script>
```

## Ponto-e-vírgula
Não finalize os comandos com ponto-e-vírgula.

```html
<!-- Ruim -->
<script>
;(function() {
    var foo = '';
    var bar = function() {
            // Stuff..
        };
}());
</script>
```

```html
<!-- Bom -->
<script>
;(function() {
    var foo = ''
    var bar = function() {
            // Stuff..
        }
}());
</script>
```

## Declaração de variáveis
A declaração de variáveis deve ser feita no início do bloco de código e, de
preferência, elas já devem ser inicializadas.

```html
<!-- Ruim -->
<script>
;(function() {
    if (name.indexOf('Lúcida') > 0) {
        status = true
    }

    var name
    var status

    name = "WROI + Lúcida"
}());
</script>
```

```html
<!-- Bom -->
<script>
;(function() {
    var name   = 'WROI + Lúcida'
    var status = false

    if (name.indexOf('Lúcida') > 0) {
        status = true
    }
}());
</script>
```

## Aspas simples em *strings*
Sempre declare strings utilizando aspa simples `('')`.

```html
<!-- Ruim -->
<script>
;(function() {
    var part1 = "WROI"
    var part2 = "Lúcida"
    var name  = part1 +  " + " + part2

    console.log("Nome:", name) // Nome: WROI + Lúcida
}());
</script>
```

```html
<!-- Bom -->
<script>
;(function() {
    var part1 = 'WROI',
    var part2 = 'Lúcida',
    var name  = part1 + ' + ' + part2

    console.log('Nome:', name) // Nome: WROI + Lúcida
}());
</script>
```

## `Else` e `default` case
Evite o uso do statement `else`, quando num bloco de `if`, e o case `default`,
quando num `switch...case`. Procure sempre tratar as possíveis exceções antes de
começar a executar o bloco condicional.

```html
<!-- Ruim -->
<script>
;(function() {
    var getDayWeekNameOf = function getDayWeekNameOf(num) {
            if (num == 1) {
                return 'domingo'
            }
            else if (num == 2) {
                return 'segunda'
            }
            // (...)
            else {
                return 'número inválido'
            }
        }
}());
</script>
```

```html
<!-- Bom -->
<script>
;(function() {
    var getDayWeekNameOf = function getDayWeekNameOf(num) {
            if (num < 1 || num > 7) {
                return 'número inválido'
            }

            if (num == 1) {
                return 'domingo'
            }
            else if (num == 2) {
                return 'segunda'
            }
            // (...)
        }
}());
</script>
```

## Chave de abertura de bloco na mesma linha da declaração
Quando declarar um bloco (`if...else if`, `switch...case`, `for`, `while`),
sempre o inicie abrindo as chaves `{}` na mesma linha da declaração dele.
Não inicie outro comando na mesma linha do fechamento da chave.

```html
<!-- Ruim -->
<script>
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
</script>
```

```html
<!-- Bom -->
<script>
;(function() {
    if (num >= 4) {
        return 1
    }
    else if (num <= 1) {
        return -1
    }

    return 0
}());
</script>
```

## `do...while()` e `while()`
**Jamais** utilize o laço `do...while()` e evite ao máximo o uso do `while`.
Priorize sempre o uso de métodos nativos de coleções (`forEach()`, `map()`,
`filter()`, `reduce()`) ao invés das estruturas de repetição tradicionais.
Como cada functor tem um uso bem definido, elas facilitam o entendimento do
código por seres humanos. Sempre que a compatibilidade com browsers muito antigos
for um requisito, utilize *polyfills* que permitam o uso das funções.

```html
<!-- Péssimo -->
<script>
;(function() {
    var list  = ['WROI', 'Lúcida']
    var i     = 0

    do {
        console.log(list[i])
        i++
    }
    while(i < list.length)
}());
</script>
```

```html
<!-- Ruim -->
<script>
;(function() {
    var list  = ['WROI', 'Lúcida']
    var i     = 0

    while(i < list.length) {
        console.log(list[i])
        i++
    }
}());
</script>
```

```html
<!-- Ok... -->
<script>
;(function() {
    var list = ['WROI', 'Lúcida']

    for (var i = 0; i < list.length; i++) {
        console.log(list[i])
    }
}());
</script>
```

```html
<!-- Bom -->
<script>
;(function() {
    var list = ['WROI', 'Lúcida']

    list.forEach(function(item) {
        console.log(item)
    })
}());
</script>
```

## lowerCamelCase
Sempre declare métodos e variáveis utilizando o padrão lower camel case, à
exceção de funções construtoras, que devem serguir o padrão upper
camel case (UpperCamelCase).

```html
<!-- Ruim -->
<script>
;(function() {
    var GetMultiplicationOf = function(num1, num2) {
            return num1 * num2
        }
    var result = GetMultiplicationOf(2,3)

    console.log(result) // 6
}());
</script>
```

```html
<!-- Bom -->
<script>
;(function() {
    var getMultiplicationOf = function(num1, num2) {
            return num1 * num2
        }
    var result = getMultiplicationOf(2,3)

    console.log(result) // 6
}());
</script>
```

## Nomenclatura em inglês, comentários em português
Sempre declare variáveis, métodos e atributos em inglês, tomando cuidado para
não fazer uso de keywords reservadas pelo javascript (atenção maior quando em
strict mode). Os comentários, por sua vez, devem ser em português, visando um
entendimento imediato.

```html
<!-- Ruim -->
<script>
;(function() {
    var primeiroNome = 'WROI'
    var segundoNome  = 'Lúcida'

    // Concat both variables and print the result
    console.log(primeiroNome + ' + ' + segundoNome) // WROI + Lúcida
}())
</script>
```

```html
<!-- Bom -->
<script>
;(function() {
    var firstName  = 'WROI'
    var secondName = 'Lúcida'

    // Concatena as variáveis e imprime o resultado
    console.log(firstName + ' + ' + secondName) // WROI + Lúcida
}());
</script>
```

## Injeção de dependências
Evite ao máximo acessar recursos do escopo global. Quando necessitar de algum
externo, injete-o como dependência do módulo.

```html
<!-- Ruim -->
<script>
;(function() {
    jQuery('#elementId').on('click', function(event) {
        // Stuff...
    })
}());
</script>
```

```html
<!-- Bom -->
<script>
;(function($) {
    $('#elementId').on('click', function(event) {
        // Stuff...
    })
}(jQuery));
</script>
```

## Linhas com 80 caracteres
Procure sempre não ultrapassar muito o limite de 80 caracteres por linha de código.

```html
<!-- Lamentável -->
<script>
;(function() {
    var variableNameVeryVeryBigBecauseWeNeedIt = [100, 400, 300, 600, 200, 900, 1000, 700, {isTrue: true}]

    variableNameVeryVeryBigBecauseWeNeedIt.forEach(function(variableNameStillBigEnoughToDemonstrate) {
        console.log(variableNameStillBigEnoughToDemonstration)
    })
}());
</script>
```

```html
<!-- Bom -->
<script>
;(function() {
    var values = [100, 400, 300, 600]

    values.forEach(function(value) {
        console.log(value)
    })
}());
</script>
```
