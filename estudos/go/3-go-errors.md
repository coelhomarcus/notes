# Errors

## Tratamento de `Erros` Básico

Em Go, erros são representados por interfaces do tipo error. O tratamento de erros é explícito e faz parte do fluxo do programa.

Funções que podem falhar geralmente retornam dois valores: o resultado e um error.

```go
import (
    "errors"
    "fmt"
)

func main() {
    res, err := dividir(3, 0)

    if err != nil {
        fmt.Println("Ocorreu um erro:", err)
        // Pode tratar o erro de acordo com a situação
        return
    }

    fmt.Println("Resultado da divisão:", res)
}

// dividir retorna o resultado da divisão de a por b, ou um erro caso b seja zero.
func dividir(a, b int) (int, error) {
    if b == 0 {
        // Criando um novo erro com errors.New
        return 0, errors.New("não pode dividir por zero")
    }

    return a / b, nil
}
```

## `Errors Is` e `Errors As`

```go
import (
	"errors"
	"fmt"
)

// Exemplo de uso de errors.Is e errors.As

var ErrValorInvalido = errors.New("valor inválido")

// Tipo de erro customizado
type MeuErro struct {
	Msg string
}

func (e *MeuErro) Error() string {
	return e.Msg
}

// Exemplo errors.Is
func main() {
	err := ErrValorInvalido
	if errors.Is(err, ErrValorInvalido) {
		fmt.Println("errors.Is: O erro é ErrValorInvalido")
	}
}

// Exemplo errors.As
func main() {
   var err error = &MeuErro{"erro personalizado"}
	var meuErr *MeuErro
	if errors.As(err, &meuErr) {
		fmt.Println("errors.As: O erro é do tipo MeuErro:", meuErr.Msg)
	}
}
```

## `Errors Join`

```go
func main() {
	err1 := errors.New("erro 1")
	err2 := errors.New("erro 2")

	// Agrupando múltiplos erros em um só
	combined := errors.Join(err1, err2)

	fmt.Println("Erros agrupados:", combined)
	// Saída: Erros agrupados: erro 1; erro 2

	// Checando se um dos erros está presente no agrupado
	if errors.Is(combined, err1) {
		fmt.Println("O erro 1 está presente dentro do erro combinado")
	}
}
```

## `Errors Wrappers`

```go
func dividir(a, b int) (int, error) {
	if b == 0 {
		// Erro original
		erroOriginal := errors.New("divisão por zero")
		// Adiciona contexto ao erro original
		return 0, fmt.Errorf("erro ao dividir %d por %d: %w", a, b, erroOriginal)
	}
	return a / b, nil
}

func main() {
	_, err := dividir(10, 0)
	if err != nil {
		fmt.Println("Error wrapping:", err)
		// Checando se o erro original está presente
		erroOriginal := errors.New("divisão por zero")
		if errors.Is(err, erroOriginal) {
			fmt.Println("Error wrapping: O erro original está presente na cadeia de erros!")
		}
	}
}
```
