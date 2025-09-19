# Básicos da Linguagem

## `Go CLI`

-   `go mod init NomePasta` - Criando go.mod
-   `go run main.go` - Compila e executa o projeto
-   `go build main.go` - Compila o projeto
-   `go build -o "app" main.go` - Compila o projeto e nomeia como app.exe
-   `go get` - Instala algum package
-   `go install` - Compila e instala um package
-   `go test` - Roda algum teste relacionado ao projeto

## Tipos de Dados e Variáveis

## `Variavéis`

Em Go, nomes que começam com letra maiúscula são exportados (públicos) e podem ser acessados fora do pacote.

```go
const name string = "Marcus" // Privada
const Name string = "Marcus" // Pública

// Variáveis de escopo de pacote; Não precisam ser inicializadas nem usadas imediatamente
var global string

func main() {
    // Variáveis declaradas dentro de funções devem ser usadas depois de inicializadas;
	// caso contrário, ocorre erro.
    var name2 string = "Marcus" // Tipo Explícito
    name3 := "Marcus"          // Inferência de Tipo (só em escopo de função)

    // Declaração múltipla de variáveis de tipos diferentes
    foo, bar := true, 50
}
```

Em Go não existe undefined como em outras linguagens. Se você não inicializar a variável, ela virá com um **zero value** dependendo do tipo: `string: ""` `int: 0` `float: 0` `boolean: false`

Tipos em Go:

-   String: `string`
-   Boolean: `bool`
-   Inteiros: `int`, `int8`, `int16`, `int32`, `int64` e `uint`, `uint8`, `uint16`, `uint32`, `uint64`
-   Aliases: `byte` (equivalente a `uint8`), `rune` (equivalente a `int32`)
-   Ponto flutuante: `float32`, `float64`
-   Números complexos: `complex64`, `complex128`

Usando esses mesmos tipos podemos mudar o tipo de uma variável:

```go
var x int = 10
var y float64 = float64(x)
```

# Funções

## `Funções Básicas`

```go
//Criando uma função que retorna string:
func myName() string { return "Marcus" }

// Quando não colocamos o tipo na primeira variável,
// o Go entende que todas são do mesmo tipo.
func abc(a, b int) {}

// Uma função pode retornar 2 valores
// Podemos dar nome às variáveis de retorno. Isso ajuda na leitura e documentação.
func dividir(a, b int) (res, resto int) {
	res = a / b
	resto = a % b
	return res, resto
}

// Naked return - Não é encorajado usar!
// Como já nomeamos as variáveis de retorno, não precisamos especificá-las no return.
func dividirNaked(a, b int) (res, resto int) {
	res = a / b
	resto = a % b
	return
}

func main() {
	// Funções Anônimas
	foo := func() { fmt.Println("Hello") }

	foo()
}
```

## `Função Higher Order`

```go
func main() {
	x := somar(2)(1)
}

// Função de higher order: retorna outra função
// A função retornada é chamada de Closure
func somar(a int) func(int) int {
	return func(b int) int {
		return a + b
	}
}
```

## `Função Variática`

```go
// Função variática: aceita 1 ou mais argumentos
func somarVariatica(nums ...int) int {
	var out int
	for _, n := range nums {
		out += n
	}
	return out
}
// somarVariatica(10, 10, 10) => 30
```


# Controle de Fluxo

## `Condicionais`

As condicionais em Go são feitas com `if`, `else if`, `else` e `switch`.
Não tem diferencia entre else if e switch em termos de performance, então use o que fizer mais sentido pro seu caso.

```go
func main() {
	a := 10

	// Condicional if / else if / else
	if a > 5 {
		fmt.Println("maior que 5")
	} else if a < 5 {
		fmt.Println("menor que 5")
	} else {
		fmt.Println("igual a 5")
	}

	// Variável declarada no if
	// e só existe dentro do escopo do if
	if b := 5; b < 10 {
		fmt.Println("b menor que 10")
	}

	// Switch case
	switch a {
	case 1:
		fmt.Println("a é 1")
		fallthrough // executa o próximo case mesmo que a condição não seja verdadeira
	case 2, 3, 4:
		fmt.Println("a é 2, 3 ou 4")
	case 6:
		fmt.Println("a é 6")
	default:
		fmt.Println("a é maior que 5")
	}

	// Switch sem expressão: útil para substituir longos if else if else
	switch {
	case a < 5:
		fmt.Println("a é menor que 5")
	case "abc" == "foo": // podemos usar expressões
		fmt.Println("a é igual a foo")
	case a > 5:
		fmt.Println("a é maior que 5")
	}

	// Variável declarada no switch só existe dentro do escopo do switch
	switch b := 5; {
	case b < 10:
		fmt.Println("b menor que 10")
	}
}

```

## `Loops`

Os Loops em Go são feitos com a palavra-chave `for`. Não existe while ou do while, mas podemos usar o for de forma que ele funcione como esses outros tipos de loop.

```go
func main() {
    // Loop for tradicional: inicialização, condição e incremento
	for i := 0; i < 10; i++ {
		fmt.Println(i)
	}

    // Loop for usado como while: com apenas condição
	i := 0
	for i < 10 {
		i++
		fmt.Println(i)
	}

    // Loop for com range em array: pega index e valor
	arr := [5]int{1, 2, 3, 4, 5}
	for index, value := range arr {
		fmt.Println(index, value)
	}

    // Loop for com range ignorando o index (_), pegando só o valor
	for _, value := range arr {
		fmt.Println(value)
	}

    // Loop for com range em número (0 até 9)
	for range 10 {
		fmt.Println("loop")
	}

    // Loop for com range em número (0 até 9) e pegando o index
	for index := range 10 {
		fmt.Println(index, "loop")
	}
}
```

---

## 📚 Navegação

| **Atual** | **Próximo →** |
|---|---|
| **Go Básico** | [Estruturas de Dados](./1-go-estrutura-dados.md) |
