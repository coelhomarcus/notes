
## Sobre mim e minhas experiências

##### Pode nos contar um pouco sobre você e sua jornada na programação?

Desde criança, sempre fui fascinado por tecnologia, começando pelos video games, com o PS2. Meu contato mais profundo com computadores veio quando minha irmã ganhou um, e foi aí que comecei a me envolver mais, principalmente jogando. À medida que fui crescendo, percebi que o trabalho no computador era o que eu realmente queria, então comecei a pesquisar mais sobre a área. Uma prima minha, que fazia Sistemas de Informação, me recomendou um curso de front-end chamado Origamid. Com isso, comecei a estudar HTML e CSS aos 16 anos. Após algum tempo, dei uma pausa porque senti que faltava uma base mais sólida, mas continuei me envolvendo com tecnologia. Quando entrei na faculdade, aprendi a base da programação e tudo foi ficando mais familiar, o que me motivou a estudar ainda mais, sem um foco específico. Experimentei o desenvolvimento de jogos, fiz vários protótipos, mas ainda não sentia que havia encontrado meu verdadeiro caminho.

Foi no **HackaTruck**, um projeto itinerante focado em desenvolvimento de software, que uma nova possibilidade se acendeu. Durante o projeto, aprendi Swift, MapKit, Fetch em API, além do uso de IoT para simular a temperatura ambiente. Foi nesse contexto que desenvolvemos o app **SOS Queimadas**, um aplicativo de prevenção e controle de queimadas para a região de Marabá.

Após essa experiência, entrei para a **Exception JR**, uma empresa júnior da UNIFESSPA, com foco no desenvolvimento web, e ali descobri que era o que eu realmente gostava. Comecei a estudar **React**, **Javascript**, **Tailwind** e, mais recentemente, **TypeScript**. Na Exception JR, tive a oportunidade de trabalhar em projetos reais, como o **Sistema de Controle de Empresas Juniores** e o **Conecta Canaã**, um sistema de modernização da gestão pública, desenvolvido em parceria com a Prefeitura de Canaã, onde cidadãos podem solicitar serviços municipais, fazer denúncias e mais.
   
##### Você já trabalhou em algum projeto real ou acadêmico usando React, JavaScript ou TypeScript? Pode nos contar sobre ele?

Sim, projetos como meu Portfolio, Conecta Canaã, Sistema Exception.
além de outros projetos pessoais, como BunnyBash, Catype, GetMangaCharacters.
   
##### Como você costuma aprender novas tecnologias e se manter atualizado no desenvolvimento Front-End?

Para aprender coisas novas costumo utilizar cursos, e videos no youtube, alem de documentações.

Para se manter atualizado uma boa ferramente é o YouTube, Linkedin, Twitter e etc.
   
##### Você tem um portfólio ou algum projeto no GitHub que possa nos mostrar?

Sim, tenho um portfolio e alguns projetos.
Portfolio
BunnyBash
Catype
Get Manga Characters

## Sobre REACT

##### O que é o React e quais são seus principais benefícios?

React é uma biblioteca JavaScript focado na construção de interfaces de usuario, onde temos componentização, facilitando a criação, organização e manutenção de um projeto.

##### Como funciona o Virtual DOM no React?

A Virtual DOM no React é basicamente um clone do DOM, onde é utilizado para melhorar a velocidade, e também nas alterações é mais fácil alterar algo na virtual dom que esta salva em memoria, do que no próprio DOM onde é mais burocrático.
Além do REACT fazer a comparação da Virtual DOM novo com a anterior para que as alterações sejam mínima e não o código inteiro.

##### O que são hooks no React? Pode nos explicar e dar exemplos de alguns?

Hooks no React são basicamente funções onde podemos usar estados e outros recursos do React.

por exemplo useState, useEffect, useContext...

##### Qual a diferença entre `useState`, `useEffect` e `useContext`?

useState: é para a alteração de estado de uma variável, onde é agendado uma re-renderização de acordo com a alteração do valor.

useEffect: é uma função que sempre ira rodar de acordo com seu segundo parâmetro, por exemplo um Array, sempre que o Array se alterar seja aumentando ou diminuindo ele rodara a função novamente.

useContext: basicamente é uma forma de passar informações entre componentes, onde os componentes serão filhos do provider e poderão acessar essas informações.

##### Como funciona o conceito de lifting state up no React?

Basicamente é a ideia do useContext so que passado com props.
entao vai ter o componente pai e tera filhos, a variavel do pai sera passado como props para os filhos, assim dando acesso e evitando duplicação de variaveis e estados.

##### Você já trabalhou com gerenciamento de estado no React? Pode nos contar sobre Redux ou Context API?

Sim.

Redux e Context API (useContext) tem a mesma função de gerenciamento de estado no React.

a diferença é que o redux é mais flexivel e escalavel, e tudo fica salvo em um objeto chamado store.

Redux para aplicações que tendem a crescer.
Context API para aplicações leves / simples.

## JavaScript e TypeScript

##### Qual a diferença entre `let`, `const` e `var` em JavaScript?

**var** : variável que pode ser criada sem valor, e pode ser alterada, não respeita o escopo, e pode ser acessada fora de funções por exemplo.

**let** : variavel que pode ser criada sem valor, e pode ser alterada.

**const** : variavel que precisa ser inicializada e nao pode ser alterada.

##### Como funciona o conceito de async/await em JavaScript?

async é para criar uma função assíncrona onde usaremos o await para fazer por exemplo um fetch, onde so passara para próxima linha quando o resultado da promise estiver liberado.

é bom lembrar que se usado o async essa função sempre retornara uma promise.
e o await só pode ser usado dentro de uma função async.

##### O que são closures e como elas funcionam?

basicamente vamos imaginar que temos 3 funções, a função 1, 2 e 3

uma dentro da outra respectivamente

1 dentro da 2 e a 2 dentro da 3.

a função 3 tem acesso as informações fora da função.
a função 2 tem acesso as informações fora, e as informações da função 3.
a função 1 tem acesso as informações fora, as informações da função 2 e 3.


##### O que é destructuring em JavaScript e como usar?

desestruturação em JavaScript é feito de diversas maneiras, como importar alguma biblioteca, arquivo e etc.

basicamente no React por exemplo quando estamos importando o useState, não precisamos de todas as funções do React só precisamos do useState, então desestruturamos a pegamos apenas a informação que precisamos.

podemos fazer também com Array, mas agora a desestruturação será de acordo com o index, mas podemos ignorar valores que não precisamos apenas com uma virgula.

##### Qual a diferença entre `null` e `undefined`?

null -> ausência de valor.

undefined -> quando uma variável não é inicializada.

##### Quais as principais diferenças entre JavaScript e TypeScript?

A principal diferença entre typescript e javascript é a tipagem, onde podemos tipar nosso código através das interfaces e types.

diferente do javascript que não temos essa restrição.

##### O que são interfaces e tipos em TypeScript?

Interface é o modelo que utilizamos para tipar um objeto.

Type é utilizado para tipar variáveis com tipos primitivos.

## Integração com APIs

##### Como você faz requisições para uma API no Front-End?

podemos usar fetch com

fetch(url).then()
ou
async await

ou utilizar bibliotecas como axios

##### Qual a diferença entre `fetch` e `axios`?

a diferença do fetch para axios

é a facilidade do uso pra mim o axios é mais facil e com muitas funcionalidades prontas.

o fetch retorna response e é necessario usar .json() por exemplo para converter
ja o axios ja converte diretamente para json

axios lança erros automaticamente no status, ja o fetch nao vc tem que checar manualmente.

##### Como você lida com erros ao fazer chamadas para uma API?

primeiramente eu checo o status/erro que esta retornando da requisição.
vejo se não é algum problema no meu codigo, como tentando acessar alguma informação invalida ou inexistente.
ou esquecendo de utiliar a key da api.

##### O que são CORS e como eles podem afetar a integração com APIs?

CORS é uma politica de requisições em APIs, onde controlam/bloqueiam requisições de dominos diferentes ou ate mesmo vindas de localhost e etc

##### Já teve que lidar com autenticação em uma API? Como foi a experiência?

Sim, em geral foi tranquilo, onde só autorizamos a requisição com um token valido.