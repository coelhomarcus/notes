# Conceitos Avançados

## `Generics`

Generics permitem escrever funções e tipos que trabalham com diferentes tipos de dados, mantendo a segurança de tipos.

### Diferença entre `interface{}` (any) e Generics

- **`interface{}`**: O tipo é verificado em tempo de execução (runtime)
- **Generics**: O tipo é verificado em tempo de compilação, oferecendo mais segurança

### `Função Genérica` Básica

```go
// T pode ser qualquer tipo
func foo[T any](arg T) {
	fmt.Println(arg)
}

func main() {
	foo("Hello")      // string
	foo(42)           // int
	foo([]int{1,2,3}) // slice
}
```

### `Constraints` (Restrições)

As constraints definem limitações sobre quais tipos podem ser usados com generics, garantindo que o tipo tenha certas características ou métodos necessários.

#### Constraint `comparable`

Permite apenas tipos que suportam operadores de comparação (`==`, `!=`), como números, strings e structs comparáveis.

```go
// T deve ser um tipo comparável (permite == e !=)
func equal[T comparable](a, b T) bool {
	return a == b
}

func main() {
	fmt.Println(equal(1, 1))         // true
	fmt.Println(equal("a", "b"))     // false
	// equal([]int{}, []int{}) // ERRO: slice não é comparável
}
```

#### `Constraint Personalizada` com `Interface`

Cria constraints baseadas em interfaces, exigindo que o tipo implemente métodos específicos.

```go
// Definindo uma constraint que exige um método
type MyConstraint interface {
	String() string
}

func show[T MyConstraint](item T) {
	fmt.Println(item.String())
}

// Implementando a interface
type Person struct {
	Name string
}

func (p Person) String() string {
	return p.Name
}

func main() {
	p := Person{Name: "João"}
	show(p) // João
}
```

#### `Constraint` com `Union Types`

Permite especificar uma lista de tipos aceitos usando o operador `|`, restringindo a apenas esses tipos específicos.

```go
// Aceita apenas int ou string
type MyConstraint2 interface {
	int | string
}

func process[T MyConstraint2](value T) T {
	return value
}

func main() {
	process(42)    // OK
	process("hi")  // OK
	// process(true)  // ERRO: bool não permitido
}
```

#### Usando `~` para Tipos Derivados

O operador `~` permite aceitar tanto o tipo base quanto tipos derivados dele (ex: `type MyString string`).

```go
// ~ permite tipos baseados em string (ex: type MyText string)
type MyConstraint3 interface {
	~string
}

type MyText string

func length[T MyConstraint3](text T) int {
	return len(text)
}

func main() {
	length("normal")           // string comum
	length(MyText("custom"))   // tipo personalizado
}
```

### `Generics` em `Structs`

```go
// Struct genérica
type MyStruct[T any] struct {
	value T
}

// Método em struct genérica
func (s MyStruct[T]) getValue() T {
	return s.value
}

func (s *MyStruct[T]) setValue(val T) {
	s.value = val
}

func main() {
	intStruct := MyStruct[int]{value: 42}
	fmt.Println(intStruct.getValue()) // 42

	stringStruct := MyStruct[string]{value: "Hello"}
	stringStruct.setValue("World")
	fmt.Println(stringStruct.getValue()) // World
}
```

## `Ponteiros`

Os ponteiros são uma maneira de referenciar a localização de uma variável na memória, ao invés de seu valor. Isso é útil quando queremos modificar o valor original de uma variável dentro de uma função.

Quando temos `*` na frente de um tipo, estamos dizendo que é um ponteiro. `ex: x *int,`

Quando temos `*` na frente de uma variável, estamos fazendo um dereference. `ex: *a`

```go
func main() {
	x := 10
	pointer := &x // &x é o endereço de memória da variável x
	fmt.Println(pointer, *pointer)
	//*pointer estou fazendo um dereference
	//ou seja, estou pegando o valor que está no endereço de memória
	//que a variável pointer (&x) está apontando

	take(&x) //passando o endereço de memória da variável x
	//dentro da função take, o valor que está no endereço de memória
	//da variável x será alterado para 20
	//pois estamos passando o endereço de memória da variável x
	//e dentro da função take, estamos alterando o valor que está nesse endereço de memória
	//se não fizéssemos isso, o valor de x continuaria 10
	//pois estaríamos passando uma cópia do valor de x
	//e não o endereço de memória de x
	fmt.Println(x)
}

// x *int é um ponteiro para um inteiro
func take(x *int) {
	*x = 20
}
```

## `Defer`

A palavra-chave `defer` adia a execução de uma função até que a função que a contém retorne. Ou seja, o código marcado com `defer` só será executado no final, mesmo que esteja no meio da função.

```go
func defer() {
	// Adia a execução de uma função até o fim da função que a chamou
	defer fmt.Println("defer 1")
	defer fmt.Println("defer 2")
	fmt.Println("funcão principal")
	// saída:
	// função principal
	// defer 2
	// defer 1
}
```
