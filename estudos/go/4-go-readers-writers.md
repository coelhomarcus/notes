## `Readers & Writers`

O padrão Reader/Writer é muito comum em Go.

- Qualquer struct que implemente `Write([]byte) (int, error)` é um Writer.
- Qualquer struct que implemente `Read([]byte) (int, error)` é um Reader.

Existem funções utilitárias do pacote io, como: `io.ReadAll(reader)` lê tudo de uma vez só e `io.ReadFull(reader, buffer)` lê o buffer até estar cheio ou EOF Usar essas interfaces permite criar código genérico para leitura e escrita de dados.

```go
package main

import (
	"errors"
	"fmt"
	"io"
	"strings"
)

// Exemplo básico de Readers & Writers em Go

func main() {
	str := "hello, world!"
	reader := strings.NewReader(str) // Cria um Reader baseado em string
	writer := MyWriter{}             // Nosso Writer customizado

	buffer := make([]byte, 2) // Buffer de 2 bytes

	for {
		n, err := reader.Read(buffer)
		if err != nil {
			if errors.Is(err, io.EOF) {
				break // Fim da leitura
			}
			panic(err) // Outro erro
		}
		// Em vez de Println(string(buffer[:n])), usamos um Writer customizado
		_, _ = writer.Write(buffer[:n])
	}
}

// Implementando a interface io.Writer
type MyWriter struct{}

// O método Write recebe um slice de bytes,
// imprime como string e retorna quantidade de bytes lidos
func (MyWriter) Write(b []byte) (int, error) {
	fmt.Print(string(b))
	return len(b), nil
}
```

---

## 📚 Navegação

| **← Anterior** | **Atual** |
|---|---|
| [Tratamento de Erros](./3-go-errors.md) | **Readers & Writers** |
