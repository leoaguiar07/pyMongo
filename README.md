![capa](https://github.com/leoaguiar07/pyMongo/blob/main/mongo_python.fw.png)


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
print(l.inserted_ids) 
```
`  [1, 2, 3, 4, 5]  `<br><br>
Observe que dessa vez inserimos manualmente os _ids, porém você pode omití-los se quiser!


## Buscando Dados no MongoDB
Para selecionarmos dados de uma coleção no MongoDB podemos usar o método find_one() que nos trará a primeira ocorrência na seleção.

```python
import pymongo
cliente = pymongo.MongoClient("mongodb://localhost:27017/")
meu_banco = cliente['banco_de_dados']
colecao = meu_banco['cientistas']
busca = colecao.find_one()
print(busca)
```
` {'_id': ObjectId('5fb2500b16fa7564711462fb'), 'nome': 'Donald Knuth', 'país': 'USA'}`<br><br>

### 1. Buscando Todos os Dados
O método find() nos permite buscarmos por todas as ocorrências, o primeiro parâmetro do método find() é um objeto de query, nesse caso específico vamos deixá-lo vazio para retornarmos todos os documentos da coleção.

```python
import pymongo
cliente = pymongo.MongoClient("mongodb://localhost:27017/")
meu_banco = cliente['banco_de_dados']
colecao = meu_banco['cientistas']
for itens in colecao.find():
    print(itens)
```
`{'_id': ObjectId('5fb252ddbbf4a01d238808d8'), 'nome': 'Donald Knuth', 'país': 'USA'}
{'_id': ObjectId('5fb252e3bbf4a01d238808d9'), 'nome': 'Charles Babbage', 'país': 'England'}
{'_id': 1, 'nome': 'Marvin Minsky', 'país': 'USA'}
{'_id': 2, 'nome': 'Dennis Ritchie', 'país': 'USA'}
{'_id': 3, 'nome': 'Edsger Dijkstra', 'país': 'Netherlands'}
{'_id': 4, 'nome': 'Grace Hopper', 'país': 'USA'}
{'_id': 5, 'nome': 'John McCarthy', 'país': 'USA'}`<br><br>

### 2. Buscando Dados Específicos
Se desejarmos que a busca nos retorne apenas campos específicos, o segundo parâmetro do método find() é um objeto descrevendo qual campo incluir no resultado.

O parâmetro é opcional, e se for omitido, todos os campos serão incluídos no resultado.

```python
import pymongo
cliente = pymongo.MongoClient("mongodb://localhost:27017/")
meu_banco = cliente['banco_de_dados']
colecao = meu_banco['cientistas']
for item in colecao.find({'_id': 1}):
    print(item)
```

`{'_id': 1, 'nome': 'Marvin Minsky', 'país': 'USA'}`<br><br>

## 3. Selecionando apenas cientistas de um país específico:

```python
for item in colecao.find({'país': 'USA'}):
    print(item)
```

`{'_id': ObjectId('5fb252ddbbf4a01d238808d8'), 'nome': 'Donald Knuth', 'país': 'USA'}
 {'_id': 1, 'nome': 'Marvin Minsky', 'país': 'USA'}
 {'_id': 2, 'nome': 'Dennis Ritchie', 'país': 'USA'}
 {'_id': 4, 'nome': 'Grace Hopper', 'país': 'USA'}
 {'_id': 5, 'nome': 'John McCarthy', 'país': 'USA'}`<br><br>

Podemos também usar expressões regulares como filtros de nossas consultas. Vejamos um exemplo de como podemos consultar apenas os cientistas que começam com a letra D:

```python
query = { "nome": { "$regex": "^D" } }
for item in colecao.find(query):
    print(item)
```

`{'_id': ObjectId('5fb252ddbbf4a01d238808d8'), 'nome': 'Donald Knuth', 'país': 'USA'}
 {'_id': 2, 'nome': 'Dennis Ritchie', 'país': 'USA'}`<br><br>



## Ordenando o Resultado
O método sort() pode ser usado para ordenar os resultados em ordem ascendente ou descendente.

O método sort() recebe um parâmetro para "fieldname" (campo) e um parâmetro para "direção" (ascendente é a direção padrão).

```python
doc = colecao.find().sort('nome')
for d in doc:
    print(d)
```

`{'_id': ObjectId('5fb252e3bbf4a01d238808d9'), 'nome': 'Charles Babbage', 'país': 'England'}
 {'_id': 2, 'nome': 'Dennis Ritchie', 'país': 'USA'}
 {'_id': ObjectId('5fb252ddbbf4a01d238808d8'), 'nome': 'Donald Knuth', 'país': 'USA'}
 {'_id': 3, 'nome': 'Edsger Dijkstra', 'país': 'Netherlands'}
 {'_id': 4, 'nome': 'Grace Hopper', 'país': 'USA'}
 {'_id': 5, 'nome': 'John McCarthy', 'país': 'USA'}
 {'_id': 1, 'nome': 'Marvin Minsky', 'país': 'USA'}`<br><br>
Para ordenarmos na ordem inversa, usamos -1 como segundo parâmetro, nossa lista será então ordenada de forma descendente.

```python
doc = colecao.find().sort('nome', -1)
for d in doc:
    print(d)
```
`{'_id': 1, 'nome': 'Marvin Minsky', 'país': 'USA'}
{'_id': 5, 'nome': 'John McCarthy', 'país': 'USA'}
{'_id': 4, 'nome': 'Grace Hopper', 'país': 'USA'}
{'_id': 3, 'nome': 'Edsger Dijkstra', 'país': 'Netherlands'}
{'_id': ObjectId('5fb252ddbbf4a01d238808d8'), 'nome': 'Donald Knuth', 'país': 'USA'}
{'_id': 2, 'nome': 'Dennis Ritchie', 'país': 'USA'}
{'_id': ObjectId('5fb252e3bbf4a01d238808d9'), 'nome': 'Charles Babbage', 'país': 'England'}`<br><br>



## Deletando Documentos
Para deletarmos documentos utilizamos o método delete_one(), o primeiro parâmetro do método delete_one() é um objeto query definindo o documento a ser deletado.

Importante: Se o query (consulta) encontrar mais de um documento, somente a primeira ocorrência será deletada.
```python
import pymongo
cliente = pymongo.MongoClient("mongodb://localhost:27017/")
meu_banco = cliente['banco_de_dados']
colecao = meu_banco['cientistas']
query = {"nome": "Marvin Minsky"}
colecao.delete_one(query)
```
Observe que ao executarmos a remoção, nos será retornado um objeto DeleteResult.

## Deletando vários Documentos
Caso queiramos deletar mais de um documento, temos o método delete_many() para nos auxiliar, que recebe como primeiro parâmetro um objeto query que definirá quais documentos deletar.

```python
import pymongo
cliente = pymongo.MongoClient("mongodb://localhost:27017/")
meu_banco = cliente['banco_de_dados']
colecao = meu_banco['cientistas']
minha_query = { "nome" : {"$regex": "^J"} }
d = colecao.delete_many(minha_query)
```

Veja que utilizamos uma expressão regular, nesse caso todos os nomes que começarem com J vão ser deletados. Caso seja necessário deletar todos os documentos, utilizamos o método delete_many() passando um objeto vazio, veja:

```python
d = colecao.delete_many({})
```
Para vermos quantos documentos foram removidos, podemos acessar o atributo deleted_count:

```python
print(f'Documentos removidos: {d.deleted_count}')
```

`Documentos removidos: 5`

## Deletando uma Coleção
Podemos deletar uma coleção de forma muito simples com o método drop(), nesse caso vamos deletar nossa coleção de cientistas:

```python
import pymongo
cliente = pymongo.MongoClient("mongodb://localhost:27017/")
meu_banco = cliente['banco_de_dados']
colecao = meu_banco['cientistas']
colecao.drop()
```
O método drop() irá retornar True se a coleção for deletada com sucesso, caso contrário retornará False (se ela não existir).

## Atualizando Coleções
Podemos atualizar nossos documentos com o método update_one(), que recebe como primeiro parâmetro um objeto query definindo o documento a ser atualizado, se o query encontrar mais de um elemento, somente a primeira ocorrência será atualizada, o segundo parâmetro é um objeto definindo os novos valores do documento.

```python
import pymongo
cliente = pymongo.MongoClient("mongodb://localhost:27017/")
meu_banco = cliente['banco_de_dados']
colecao = meu_banco['cientistas']
query = { "nome": "Marvin Minsky" }
novos_valores = { "$set": { "país": "China"} }
colecao.update_one(query, novos_valores)
for x in colecao.find():
    print(x)
```
` {'_id': 1, 'nome': 'Marvin Minsky', 'país': 'China'}
 {'_id': 2, 'nome': 'Dennis Ritchie', 'país': 'USA'}
 {'_id': 3, 'nome': 'Edsger Dijkstra', 'país': 'Netherlands'}
 {'_id': 4, 'nome': 'Grace Hopper', 'país': 'USA'}
 {'_id': 5, 'nome': 'John McCarthy', 'país': 'USA'}`

## Atualizando Diversas Coleções
Para atualizarmos todos os documentos que atendam ao critério de nosso query, utilizamos o método update_many(). Nesse caso vamos atualizar todos os endereços que começam com a letra R.
```python
import pymongo
cliente = pymongo.MongoClient("mongodb://localhost:27017/")
meu_banco = cliente['banco_de_dados']
colecao = meu_banco['cientistas']
minha_query = { "país": { "$regex": "^U"} }
valores_novos = { "$set": { "país": "Índia" } }
x = colecao.update_many(minha_query, valores_novos)
for itens in colecao.find():
    print(itens)
```
` {'_id': 1, 'nome': 'Marvin Minsky', 'país': 'China'}
 {'_id': 2, 'nome': 'Dennis Ritchie', 'país': 'Índia'}
 {'_id': 3, 'nome': 'Edsger Dijkstra', 'país': 'Netherlands'}
 {'_id': 4, 'nome': 'Grace Hopper', 'país': 'Índia'}
 {'_id': 5, 'nome': 'John McCarthy', 'país': 'Índia'}`
 <br>
 
O atributo modified_count nos informa quantos documentos foram atualizados:

```python
print(f'Documentos atualizados: {x.modified_count}')
```
`3`
##  Limitando o Resultado
Se desejarmos limitar nosso resultado apenas a uma certa quantidade, podemos usar o método limit(). O método limit() recebe um parâmetro: um número que define quantos documentos retornar.

Para usá-lo é muito simples:
```python
limite = colecao.find().limit(3)
for l in limite:
    print(l)
```
` {'_id': 1, 'nome': 'Marvin Minsky', 'país': 'China'}
 {'_id': 2, 'nome': 'Dennis Ritchie', 'país': 'Índia'}
 {'_id': 3, 'nome': 'Edsger Dijkstra', 'país': 'Netherlands'}`

##### Original: https://pythoniluminado.netlify.app/mongodb#limitando-o-resultado
