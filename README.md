![capa](https://webimages.mongodb.com/_com_assets/cms/kuzt9r42or1fxvlq2-Meta_Generic.png)


## MongoDB
Como já sabemos, a linguagem Python pode ser usada em conjunto com banco de dados, anteriormente utilizamos os bancos de dados relacionais SQL, agora vamos dar um passo a frente e trabalhar com NoSQL.

Um banco de dados NoSQL (originalmente referindo-se a "não SQL" ou "não relacional") fornece um mecanismo para armazenamento e recuperação de dados que são modelados em meios diferentes das relações tabulares usadas em bancos de dados relacionais.


MongoGB é um dos mais populares banco de dados NoSQL, ele permite guardarmos nossos dados em um modelo semelhante ao JSON, o que torna o nosso banco de dados flexível e escalável.

Um registro no MongoDB é um documento, que é uma estrutura de dados composta de pares de campo e valor. Os valores dos campos podem incluir outros documentos, arrays e arrays de documentos.

O MongoDB armazena documentos em coleções. Coleções são análogas às tabelas em bancos de dados relacionais e documentos às linhas.<br><br>


## Vamos então ao Python!

Para que possamos trabalhar com o MongoDB em Python precisamos instalar o driver PyMongo, para isso utilizaremos o PIP. Vamos então executar o seguinte comando:

```python
pip install pymongo
```



Aguarde o download e instalação. Para confirmarmos se está tudo certo, vamos digitar no nosso interpretador

```python
import pymongo
```

Caso nenhum erro tenha sido disparado, signifca que estamos com o pymongo instalado e pronto para ser usado. <br><br>

## Banco de Dados 
### 1.  Criando um Banco de Dados
Para criarmos um banco de dados no MongoDB é necessário criarmos um objeto MongoClient, em seguida devemos especificar uma URL de conexão com o endereço IP correto e o nome do banco de dados, caso o banco de dados não exista, MongoDB criará para nós e fará uma conexão.

Vejamos então como podemos instanciar o cliente e nos conectar ao MongoDB, criaremos um banco de dados chamado de banco_de_dados:


```python
import pymongo
cliente = pymongo.MongoClient("mongodb://localhost:27017/")
meu_banco = cliente['banco_de_dados']
print(meu_banco)
```


`Database(MongoClient(host=['localhost:27017'], document_class=dict, tz_aware=False, connect=True), 'base_de_dados')`


 <aside>
    ⚠️ Importante lembrarmos que no MongoDB um banco de dados não é criado até que ele receba conteúdo. MongoDB esperará até que criemos a coleção (similar a tabela SQL), 
   com pelo menos um documento (similar ao registro SQL) antes de criar o banco de dados.<br><br>


### 2. Verificando se um Banco de Dados existe
Podemos checar se um determinado banco de dados existe executando o método list_database_names()

```python
print(cliente.list_database_names()) # ['local', 'admin']
```
Observe que nosso banco anterior ainda não está presente, precisamos populá-lo!<br><br>

## Coleções
### 1. Criando uma Coleção
No MongoDB chamamos de coleção o que no SQL seriam as tabelas, são os compartimentos que guardamos nossos dados. Para criarmos uma coleção no MongoDB vamos usar o objeto do banco de dados que definimos anteriormente e especificar o nome da coleção que queremos criar.


```python
 import pymongo
 cliente = pymongo.MongoClient("mongodb://localhost:27017/")
 meu_banco = cliente['banco_de_dados']
 colecao = meu_banco['cientistas']
```
 ⚠️ Lembrando novamente que a coleção não será criada até que receba conteúdo!


### 2. Checando se uma coleção existe


```python
print(meu_banco.list_collection_names())
```

Veja que nos retornou uma lista vazia, isso porque nossa coleção só será criada de fato quando inserirmos dados nela, aguarde que logo teremos nossa tão esperada coleção!

## Documentos

### 1. Inserindo Documentos no MongoDB
Um documento no MongoDB é como um registro nos bancos de dados SQL. Para que possamos inserir um documento numa coleção do MongoDB usaremos o método insert_one().

O primeiro parâmetro do método insert_one() é um dicionário contendo nomes e valores de cada campo do documento que queremos inserir. Vejamos um exemplo:



```python
 import pymongo
 cliente = pymongo.MongoClient("mongodb://localhost:27017/")
 meu_banco = cliente['banco_de_dados']
 colecao = meu_banco['cientistas']
 cientista = {"nome": "Donald Knuth", "país": "USA"}
 c = colecao.insert_one(cientista)
 print(c) 
```

`<pymongo.results.InsertOneResult object at 0x7fce96f8f320>` <br><br>
Veja que nos foi retornado um objeto InsertOneResult, que tem a propriedade inserted_id que guarda o id do documento inserido. 
Agora vamos inserir outro documento na coleção usuarios e retornaremos o valor do campo _id.

```python
novo_cientista = {"nome": "Charles Babbage", "país": "England"}
nc = colecao.insert_one(novo_cientista)
print(nc.inserted_id) # 5fb2505216fa7564711462fc
```
Observe que em nenhum momento especificamos um _id, MongoDB o fez automaticamente para nós e ele o fará sempre que não o especificarmos.


### 2. Inserindo Múltiplos Documentos
PyMongo nos permite inserir diversos documentos em uma coleção, para isso utilizamos o método insert_many(). O primeiro parâmetro de insert_many() é uma lista contendo dicionários dos dados que queremos inserir.

```python
import pymongo
cliente = pymongo.MongoClient("mongodb://localhost:27017/")
meu_banco = cliente['banco_de_dados']
colecao = meu_banco['cientistas']
lista = [
  { "_id": 1, "nome": "Marvin Minsky", "país": "USA"},
  { "_id": 2, "nome": "Dennis Ritchie", "país": "USA"},
  { "_id": 3, "nome": "Edsger Dijkstra", "país": "Netherlands"},
  { "_id": 4, "nome": "Grace Hopper", "país": "USA"},
  { "_id": 5, "nome": "John McCarthy", "país": "USA"}
]
l = colecao.insert_many(lista)
print(l.inserted_ids) # [1, 2, 3, 4, 5]
```
Observe que dessa vez inserimos manualmente os _ids, porém você pode omití-los se quiser!



















## Controle de Versão e Git

**Objetivo:**

- Compreender a importância do controle de versão.
- Aprender os conceitos básicos do Git, incluindo forks e resolução de conflitos.

### 1. O que é Controle de Versão?

![capa](./image/cd5ikg9p.bmp)

O Controle de Versão é um sistema que registra as mudanças feitas em um conjunto de arquivos ao longo do tempo. Ele é essencial para colaboração em projetos de software e para o acompanhamento das alterações realizadas.

### 2. Por que usar Controle de Versão?

- 🤝🏻**Colaboração eficiente:** Permite que várias pessoas trabalhem simultaneamente no mesmo projeto, mantendo um histórico claro de quem fez quais alterações.
- 🔁 **Reversão de mudanças:** É possível voltar a versões anteriores do código caso algo dê errado.
- 🔎 **Rastreamento de alterações:** Ajuda a entender como o código evoluiu ao longo do tempo e por quais mãos passou.

### 3. Conceitos Básicos do Git

- **`Repositório`:** É o espaço onde o Git armazena as informações do seu projeto, incluindo o histórico de alterações.
- **`Commit`:** É uma "foto" instantânea de todos os arquivos do projeto em um determinado momento. Cada commit possui uma mensagem descritiva.
- **`Branch (Ramificação)`:** É uma linha de desenvolvimento independente que permite trabalhar em novas funcionalidades ou correções sem afetar o código principal. O branch principal é geralmente chamado de "master" ou "main".
- **`Merge (Mesclagem)`:** É a combinação de alterações de um branch para outro, como incorporar uma funcionalidade desenvolvida em um branch de desenvolvimento de volta ao branch principal.
- **`Fork`:** Um fork é uma cópia independente de um repositório. É frequentemente usado para contribuir para projetos de código aberto sem afetar diretamente o repositório original.

### 4. Trabalhando com commit

1. **Boas práticas:** Criar mensagens de commit claras e informativas é uma prática importante ao trabalhar com o Git. Mensagens de commit bem escritas ajudam a comunicar as alterações feitas no código, facilitam a colaboração com outros desenvolvedores e tornam o histórico do projeto mais compreensível.
    1. **Separe o título do corpo**: Uma mensagem de commit geralmente consiste em um título curto (linha única) seguido de um corpo opcional.
    2. **Título conciso e descritivo**: O título deve ser curto,
    mas ainda assim informativo. Ele deve resumir o que a alteração fez, em
    termos claros e descritivos. Use verbos no imperativo, como "Adicionar",
     "Corrigir" ou "Atualizar", para indicar a ação realizada.

        ```bash
        git commit -a -m "Inserindo um commit da forma certa"
        ```

    3. **Capitalização e pontuação**: Use letras maiúsculas no
