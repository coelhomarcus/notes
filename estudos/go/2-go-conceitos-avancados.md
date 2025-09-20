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

## `Goroutines` & `Context`

### Conceitos Fundamentais

**Paralelismo**: Executar múltiplas tarefas simultaneamente em núcleos diferentes do processador.

**Concorrência**: Gerenciar múltiplas tarefas ao mesmo tempo, mas não necessariamente executando simultaneamente. É como o exemplo do caixa do mercado: enquanto uma pessoa tem problemas no atendimento, outras pessoas podem ser atendidas em outros caixas, evitando que todos esperem.

### Execução Sequencial vs Concorrente

#### Exemplo Sequencial (sem concorrência)

Neste código fazemos 10 requisições HTTP uma após a outra. Cada requisição precisa terminar completamente antes da próxima começar, tornando o processo lento.

```go
package main

import (
	"fmt"
	"net/http"
	"time"
)

func main() {
	start := time.Now()
	for range 10 {
		resp, err := http.Get("https://google.com")
		if err != nil {
			panic(err)
		}
		defer resp.Body.Close()
		fmt.Println("ok")
	}
	fmt.Println(time.Since(start))
}
```

#### Exemplo Concorrente com `WaitGroup`

Com concorrência podemos fazer várias requisições sem esperar que a anterior termine. Usamos `WaitGroup` para sincronizar goroutines assíncronas.

```go
func main() {
	start := time.Now()
	const n = 10

	var wg sync.WaitGroup
	wg.Add(n) // Dizemos que o WaitGroup vai esperar 10 sinais

	// Dentro do loop, cada defer wg.Done() será executado ao final
	// da goroutine, enviando um sinal de conclusão
	// O wg.Wait() no final trava o código até que todos os sinais sejam recebidos

	for range n {
		go func() {
			defer wg.Done()
			resp, err := http.Get("https://google.com")
			if err != nil {
				panic(err)
			}
			defer resp.Body.Close()
			fmt.Println("ok")
		}()
	}
	wg.Wait()
	fmt.Println(time.Since(start))
}
```

## Quando Usar Concorrência?

**Pergunta-chave**: "Este código tem muita espera?" Se sim, provavelmente se beneficiará de concorrência.

Exemplos de operações com espera:
- Requisições HTTP
- Operações de I/O (leitura/escrita de arquivos)
- Consultas ao banco de dados
- Operações de rede

**Dica importante**: Use benchmarks para verificar se a concorrência realmente melhora a performance. Nem sempre vale a pena a complexidade adicional.

## O que é Context?

O `Context` é um mecanismo do Go para controlar o ciclo de vida de operações, permitindo:
- **Cancelamento**: Interromper operações em andamento
- **Timeout**: Definir tempo limite para operações
- **Deadlines**: Estabelecer quando uma operação deve terminar
- **Valores**: Passar dados entre funções (menos comum)

### Context para Timeout

```go
package main

import (
	"context"
	"errors"
	"fmt"
	"net/http"
	"net/http/httptest"
	"sync"
	"time"
)

func main() {
	start := time.Now()
	const n = 10

	var wg sync.WaitGroup
	wg.Add(n)

	// 1. CRIANDO O CONTEXT COM TIMEOUT
	ctx := context.Background()              // Context raiz (vazio)
	ctx, cancel := context.WithTimeout(ctx, 5*time.Second) // Timeout de 5 segundos
	defer cancel() // Importante: sempre chamar cancel() para liberar recursos

	// 2. SERVIDOR DE TESTE (simula servidor lento)
	// httptest.NewServer cria um servidor HTTP temporário para testes
	server := httptest.NewServer(
		http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
			// Simula processamento lento (10 segundos)
			// Este delay é maior que nosso timeout (5 segundos)
			time.Sleep(10 * time.Second)
			fmt.Fprintln(w, "hello, world")
		}),
	)

	for range n {
		go func(ctx context.Context) {
			defer wg.Done()

			// 3. CRIANDO REQUISIÇÃO COM CONTEXT
			// NewRequestWithContext associa o context à requisição
			// Se o context expirar, a requisição será cancelada automaticamente
			req, err := http.NewRequestWithContext(
				ctx,        // Context com timeout
				"GET",      // Método HTTP
				server.URL, // URL do servidor de teste
				nil,        // Body (nil para GET)
			)
			if err != nil {
				panic(err)
			}

			// 4. EXECUTANDO A REQUISIÇÃO
			// DefaultClient.Do() executa a requisição respeitando o context
			resp, err := http.DefaultClient.Do(req)
			if err != nil {
				// 5. TRATAMENTO ESPECÍFICO DE TIMEOUT
				if errors.Is(err, context.DeadlineExceeded) {
					fmt.Println("timeout - requisição cancelada após 5s")
					return
				}
				panic(err)
			}
			defer resp.Body.Close()
			fmt.Println("ok - requisição completada")
		}(ctx) // Passando o context para a goroutine
	}
	wg.Wait()
	fmt.Println("Tempo total:", time.Since(start))
	// Saída esperada: ~5 segundos (tempo do timeout)
	// Sem timeout seria: ~10 segundos (tempo do servidor lento)
}
```

#### Por que usar Context em HTTP?

Requisições HTTP podem ser lentas ou travar completamente. Sem timeout, sua aplicação pode:
- Esperar indefinidamente por uma resposta
- Consumir recursos desnecessariamente
- Criar uma má experiência do usuário
- Sobrecarregar o sistema com conexões pendentes

---
