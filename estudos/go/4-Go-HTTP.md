# GO - HTTP

## GET Simples

> Forma mais direta de fazer uma requisição HTTP em Go. Ideal para casos simples onde não precisamos configurar headers ou outros parâmetros.

```go
func main() {
	resp, err := http.Get("https://coelhomarcus.com")

	if err != nil {
		panic(err)
	}

   // Sempre fechar a conexão, se não a conexão TCP ficará aberta consumindo recurso, e isso é conhecido como vazamento de memória (memory leaks).
	defer resp.Body.Close()

	data, err := io.ReadAll(resp.Body)

	if err != nil {
		panic(err)
	}

	fmt.Println(string(data))
}
```

## Criando Requisições Personalizadas

> Método mais flexível que permite configurar headers, métodos HTTP, body e outras opções. Essencial para APIs que exigem autenticação ou configurações específicas.

```go
func main() {

   // "GET" e http.MethodGet são iguais, mas é uma boa pratica usar http.MethodGet
   // req, err := http.NewRequest("GET", "https://coelhomarcus.com", nil)
	req, err := http.NewRequest(http.MethodGet, "https://coelhomarcus.com", nil)

	if err != nil {
		panic(err)
	}

	//Depois de criada nossa requisição, podemos manipula-la
	// Por exemplo vamos alterar o Header

    //.Set sobrescreve ou adiciona, .Add adiciona
	req.Header.Add("authorization", "123")
	req.Header.Set("accept", "application/json")

   // Aqui fazemos a requisição, com um Client HTTP
	resp, err := http.DefaultClient.Do(req)

	if resp != nil {
		panic(err)
	}

	data, err := io.ReadAll(resp.Body)

	if err != nil {
		panic(err)
	}

	fmt.Println(string(data))
}
```
