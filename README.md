# state-registration
Este repositório contém uma classe responsável por validar uma incrição estadual nos padrões de cada estado do Brasil. Esta classe foi baseada nas informações obtidas no site do Sintegra (http://www.sintegra.gov.br).

## Instalar

Usando Composer

``` bash
$ composer require emersoncavalcanti/state-registration
```

## Uso

Para validar um número de inscrição, basta você realizar uma chamada à classe passando como parâmetros o número da inscrição e a UF a qual pertence este número.

```php
// Aqui definimos valores fictícios, porém válidos
$number = '01.140.812/690-10';
$stateOfRegistration = 'AC';

// Verificar se uma inscrição estadual é válida
if StateRegistration::isValid($number, $stateOfRegistration) {
	echo "A inscrição estadual {$number} / {$stateOfRegistration} é válida";
} else {
	echo "A inscrição estadual {$number} / {$stateOfRegistration} não é válida. Verifique e tente novamente.";
}
```

### UF inválida

Caso o estado informado seja inválido (UF inexistente), a classe irá gerar uma exceção do tipo 'InvalidArgumentException'. Você pode tratar este comportamento facilmente.

```php
// Aqui definimos valores fictícios, porém inválidos
$number = '01.140.812/690-10';
$stateOfRegistration = 'XX';

try {
	// Verificar se uma inscrição estadual é válida
	if StateRegistration::isValid($number, $stateOfRegistration) {
		echo "A inscrição estadual {$number} / {$stateOfRegistration} é válida";
	} else {
		echo "A inscrição estadual {$number} / {$stateOfRegistration} não é válida. Verifique e tente novamente.";
	}
} catch (\InvalidArgumentException $e) {
	// Aqui tratamos os casos de UF inválida
	echo 'Exceção capturada: ',  $e->getMessage(), "\n";
}

```


### Lidando com isenções de inscrição estadual

Nos casos em que ocorrer a isenção da inscrição estadual, você pode simplesmente fornecer no número o valor 'ISENTO' ou 'ISENTA'. A classe lida com letras maiúsculas e minúsculas normalmente, tanto no número da inscrição estadual quanto na UF.

Por exemplo são válidos:
- 'SP', 'Sp' ou 'sp' para o estado de São Paulo
- 'ISENTO', 'Isento' ou 'isento' para isentos

### Lidando com números abreviados (sem zeros à esquerda) e formatação

Você precisa lidar antecipadamente com relação à quantidade de algarismos exigidos na inscrição estadual. Algumas UFs não usam dígitos identificadores da UF (os dois primeiros dígitos) e os usuários tendem à representar o número sem os zeros à esquerda. Nestes casos esta classe não saberá lidar com estes valores e poderá indicar um 'falso' resultado.

Também são usados sinais de pontuação na inscrição estadual e até letras. Nestes casos a classe irá lidar corretamente com estes valores, ignorando as pontuações e tratando os demais caracteres (letras e números).

Por exemplo são valores válidos para inscrições de São Paulo (SP):
- 'P-01100424.3/002' ou 'P011004243002'

Em caso de dúvidas, consulte a documentação de cada estado ou o site do Sintegra (http://www.sintegra.gov.br) para esclarecer suas dúvidas.


## Testes

Incluí uma rotina de testes com valores válidos de todas as unidades da federação.

``` bash
$ phpunit --verbose --testdox tests/StateRegistrationTest.php
```

## Créditos

- [Sintegra](http://www.sintegra.gov.br)

## Licença

A Licença MIT (MIT). Por favor veja [License File](LICENSE) para maiores informações.
