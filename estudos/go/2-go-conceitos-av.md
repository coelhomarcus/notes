# Conceitos Avançados

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

---

## 📚 Navegação

| **← Anterior** | **Atual** | **Próximo →** |
|---|---|---|
| [Estruturas de Dados](./1-go-estrutura-dados.md) | **Conceitos Avançados** | [Tratamento de Erros](./3-go-errors.md) |
