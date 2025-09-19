# Estruturas de Dados

## `Arrays`

Arrays tem tamanho fixo, nunca podem crescer ou diminuir

```go
var arr1 [5]int
var arr2 [5]int = [5]int{1, 2, 3, 4, 5}
arr3 := [5]int{3: 10} // inicializa o indice 3 com valor 10
arr4 := [10]int{3: 10, 6: 20} // inicializa os indices 3 e 6 com valores 10 e 20
```

## `Slices`

Uma Slice é uma versão dinâmica de um Array.

Use `Array` quando o tamanho da lista for fixo e use `Slice` quando o tamanho for variável.

```go
// Criando uma Slice a partir de um Array
arr := [5]int{1, 2, 3, 4, 5}

// Criando o Slice pegando do index 1 a 3 do Array
// colocamos 4 pois ele descarta o ultimo (Slice Range Syntax)
sliceArr := arr[1:4] // [2,3,4]
// mesmo alterando depois de criar o Slice, a Array tbm é modificada
// pois apontam pro mesmo endereço de memoria
arr[2] = 65
fmt.Println(sliceArr) // [2,65,4]

// Criando uma Slice
// Basta apenas criar um array mas sem limitar os espaços
slice := []int{1, 2, 3}
fmt.Println(slice)

// todo Slice tem Length e Capacity
// Length -> quantidade de elementos salvos no presente
// Capacity -> quantidade que o Slice pode armazenar sem fazer outra alocação
fmt.Println(slice, len(slice), cap(slice))
//Para adicionar um elemento no Slice criamos outro Slice e Reatribuimos
slice = append(slice, 2)
fmt.Println(slice, len(slice), cap(slice))

// Ao usar append em slices sem capacidade pré-alocada,
// cada vez que for necessário aumentar o espaço
// o Go realocará a memória e dobrará a capacidade do slice
// Em loops isso pode gerar muitas realocações, impactando negativamente a performance

// Uma forma mais eficiente de criar slices é com make
// assim podemos definir o tipo, length e capacity
// garantindo que o espaço já esteja pré-alocado na memória
// Essa abordagem é útil principalmente quando sabemos previamente o tamanho do Slice
slicePerformatico := make([]string, 0, 10)
fmt.Println(slicePerformatico, len(slicePerformatico), cap(slicePerformatico))
```

## `Map`

O Map em GO basicamente é o HashMap Padrão - Chave e Valor.

```go
func main() {
   //criando um Map
	m := make(map[string]string)
	mapWithSize := make(map[string]string, 100)

	//outro modo de criar um Map
	mLiteral := map[string]string{}
	mLiteralWithValues := map[string][]int{
		"Marcus": {1, 2, 3},
		// "Marcus": []int{1, 2, 3}, //mesma coisa
	}

   //acessando valores
   fmt.Println(mLiteralWithValues["Marcus"])
   fmt.Println(mLiteralWithValues["Marcus"][0])

   //adicionando mais valores
   mLiteralWithValues["Coelho"] = []int{4, 5, 6}
   fmt.Println(mLiteralWithValues["Coelho"])

  //podemos verificar a existencia da chave
   valor, seExiste := mLiteralWithValues["NaoExiste"]
   fmt.Println(seExiste) //true or false
   if seExiste { fmt.Println(valor)	}

   //apagar Chave do Map
   delete(mLiteralWithValues, "Coelho")

   //limpa todas as chaves mas mantem a Capacity
   clear(mLiteralWithValues)
}
```

## `Structs`

Em Go, `structs` são semelhantes a classes e são declaradas com a keyword `type`. Cada campo é definido em uma nova linha. Métodos são funções que recebem um receiver parameter, permitindo que sejam associados a um tipo específico. Também é possível usar embed para incorporar um tipo dentro de outro. Struct tags permitem controlar o comportamento de serialização, por exemplo, para JSON.

```go
type User struct {
	Name string
	ID   int
}

// Criando um método para o tipo User
func (u User) hello() {
	fmt.Println("Hello", u.Name)
}

func main() {
	// Inicializando uma struct User. Se não inicializada, os valores virão zerados
	user1 := User{"Marcus", 1}
	// Alternativa usando nomes de campos -> user1 := User{Name: "Marcus", ID: 1}

	fmt.Println(user1, user1.Name, user1.ID)
	user1.hello() // Output: Hello Marcus
}
```

Usando `Struct tags`

```go
// Struct tags indicam como os campos devem ser nomeados ao serializar para JSON
type Pet struct {
	Name string `json:"name"`
	ID   int    `json:"id"`
}

func main() {
   // Criando e serializando um Pet para JSON
	pet1 := Pet{"Cafundongo", 1}
	petJson, err := json.Marshal(pet1)

	if err != nil { panic(err)	}

	fmt.Println(string(petJson))
   // Output: {"name":"Cafundongo","id":1}
}
```

## `Interfaces`

`Interfaces` em Go permitem definir comportamentos que diferentes tipos podem implementar.

Um tipo "satisfaz" uma interface automaticamente se possuir todos os `métodos` que ela exige.

No exemplo abaixo, `Dog` implementa a `interface Animal` porque define o `método Sound`.

Assim, podemos passar um `Dog` para qualquer função que receba um Animal.

```go
type Animal interface {
	Sound() string
}

type Dog struct{}

func (Dog) Sound() string {
	return "Au! au!"
}

func whatDoesThisAnimalSay(a Animal) {
	fmt.Println(a.Sound())
}

func main() {
	dog := Dog{}
	whatDoesThisAnimalSay(dog)
}
```
## `Type Assertion` e `Type Switch`

Interfaces permitem que diferentes tipos sejam tratados de forma genérica.

Mas às vezes precisamos descobrir qual é o tipo concreto por trás da interface.

Para isso, podemos usar:
- Type Assertion: `a.(Tipo)` ex: `a.(string)` que retorna valor e ok
- Type Switch: `switch a.(type)`

```go
type Animal interface {
	Sound() string
}

type Dog struct{}

func (Dog) Sound() string {
	return "Au! au!"
}

type Cat struct{}

func (Cat) Sound() string {
	return "Miau!"
}

// Exemplo de Type Switch para identificar o tipo concreto
func takeAnimal(a Animal) string {
	switch t := a.(type) {
	case Dog:
		return "É um cachorro: " + t.Sound()
	case Cat:
		return "É um gato: " + t.Sound()
	default:
		return "Animal desconhecido"
	}
}

func main() {
	cat := Cat{}
	dog := Dog{}

	fmt.Println(takeAnimal(cat)) // É um gato: Miau!
	fmt.Println(takeAnimal(dog)) // É um cachorro: Au! au!
}
```



## `Any`

O tipo `any` é apenas um alias (apelido) para `interface{}`. Ou seja, type `any = interface{}` está definido no pacote padrão.

Mas usar any como tipo é altamente desencorajado!

```go
func foo(a any) { fmt.Println(a) }
// mas duas sao a mesma coisa
func foo(a interface{}) { fmt.Println(a) }

func main() {
   foo("Marcus")
   foo(19)
   foo([]int{1,2,3})
}
```
