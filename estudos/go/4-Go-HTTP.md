# GO - HTTP

## Requisição `GET` Simples

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

## Criando `Requisições Personalizadas`

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

## Criando `Servidor HTTP`

> Implementação de um servidor HTTP usando apenas a biblioteca padrão do Go. Aqui criamos um servidor completo com roteamento customizado, middleware de logging e configurações de timeout. É uma abordagem mais manual que oferece controle total sobre o comportamento do servidor.

```go
// Criando nosso Middleware de Log
// Passamos o Handler como parametro e Retornamos um Handler tambem
func Log(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		begin := time.Now()
		next.ServeHTTP(w, r)
		fmt.Println(r.URL.String(), r.Method, time.Since(begin))
	})
}

func main() {
	mux := http.NewServeMux()

	// O mux.HandleFunc permite darmos uma o Metodo, Rota e um Funcao do que deve ser feito quando a Rota for acessada
	// Tudo que eu quero mandar de volta pro cliente utilizamos o ResponseWriter
	// E o request, é a requisicao que foi feita pra gente. cliente -> servidor
	mux.HandleFunc("GET /healthcheck/{id}", func(w http.ResponseWriter, r *http.Request) {
		id := r.PathValue("id")
		fmt.Println(id)
		fmt.Println("Hello")
	})

   // Aqui criamos nosso proprio servidor http
	srv := &http.Server{
		Addr:                         ":8080",
		Handler:                      Log(mux), //aqui usamos o middleware
		DisableGeneralOptionsHandler: false,
		ReadTimeout:                  10 * time.Second,
		WriteTimeout:                 10 * time.Second,
		IdleTimeout:                  1 * time.Minute,
	}

	// srv.ListenAndServe()
	// Iniciando o Servidor e ja tratando Erro de quando servidor fechar.
	if err := srv.ListenAndServe(); err != nil {
		if !errors.Is(err, http.ErrServerClosed) {
			panic(err)
		}
	}
}
```

## Criando Servidor com `CHI`

> Framework HTTP leve e compatível com a biblioteca padrão do Go. O CHI oferece um roteador mais poderoso com suporte nativo a middlewares, agrupamento de rotas, parâmetros de URL e padrões RESTful. É a escolha recomendada pela comunidade.

```go
package main

import (
	"fmt"
	"net/http"
	"time"
	"github.com/go-chi/chi/v5"
	"github.com/go-chi/chi/v5/middleware"
)

func main() {
   // Criando o Router
	r := chi.NewMux()

	// Usando middlewares
	r.Use(middleware.Recoverer)
	r.Use(middleware.RequestID)
	r.Use(middleware.Logger)

   // Criando Rota Get Simples
   // Podemos perceber que o CHI é compativel com a standard lib
   // pois passamos a interface de http
   // func(w http.ResponseWriter, r *http.Request)
	r.Get("/horario", func(w http.ResponseWriter, r *http.Request) {
		now := time.Now()
		fmt.Fprintln(w, now)
	})

	// Agrupando Rotas
	r.Route("/api", func(r chi.Router) {
		r.Route("/v1", func(r chi.Router) {
			r.Get("/users/{id}", func(w http.ResponseWriter, r *http.Request) {
				id := chi.URLParam(r, "id")
				fmt.Fprintln(w, "Seu id é", id)
			})
		})
		r.Route("/v2", func(r chi.Router) {
			r.Get("/marcus", func(w http.ResponseWriter, r *http.Request) {
				fmt.Fprintln(w, "Oiii marcus na v2")
			})
		})

		// Aqui o middleware é exclusivo pra rota pois usamos o middleware na criação da rota
		r.With(middleware.RealIP).Get("/middlewareExclusivo", func(w http.ResponseWriter, r *http.Request) {
			fmt.Fprintln(w, "Middleware Exclusivo apenas para essa ROTA")
		})

		// Aqui fazemos um grupo e as rotas declaradas dentro todas usam o middlewares
		r.Group(func(r chi.Router) {
			r.Use(middleware.BasicAuth("", map[string]string{
				"admin": "admin",
			}))

			// O middleware de Basic Auth vai funcionar nessa rota, pois estamos dentro do grupo
			r.Get("/seila", func(w http.ResponseWriter, r *http.Request) {
				fmt.Fprintln(w, "pingou")
			})
		})
	})

   // Todas as rotas:
   // /api/v1/users/{id}
   // /api/v2/marcus
   // /api/middlewareExclusivo
   // /api/seila

   // Não é criar servidores assim p/ produção
   // é melhor usar o &http.Server
   // mas como é so pra teste não tem problema.
	if err := http.ListenAndServe(":8080", r); err != nil {
		panic(err)
	}
}
```


