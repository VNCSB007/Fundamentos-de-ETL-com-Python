# Projeto: Extração e Validação 

## Como será feito a extração

Alguns dados se repetem na tabela por conta da logística do banco de dados. É importante identificar essas ocorrências e agrega-las ou reduzi-las

colunas retiradas: codigo_ocorrencia1, 3 e 4; ocorrencia_latitude e ocorrencia_longitude; ocorrencia_pais; investigacao_aeronave_liberada, investigacao_status, divulgacao_relatorio_numero, divulgacao_relatorio_publicado, divulgacao_dia_publicacao; total_aeronaves_envolvidas; ocorrencia_saida_pista 

Ao final de retirar todas as colunas desnecessárias, deverão sobrar 9 colunas para desenvolver o projeto ETL (eles foram excluídos para a didaticidade do projeto, mas o arquivo poderia estar integro) 

## Extração: Pandas 

Precisamos importar a biblioteca pandas com o seguinte código no Jupyter:

"import pandas as pd"

Fazer o carregamento do arquivo csv para o Notebook versão Web e então insira o código:

"

dataframe = pd.read_csv("ocorrencia_2010_2020.csv")

dataframe

"

Mas será percebido que tudo estará em uma coluna, separado de forma bruta por ponto e vírgula; adicione ao código tbm: 

"[...]ocorrencia_2010_2020.csv", sep = ";")"

Uma boa prática na criação de uma estrutura  de dados (_data frames_) é chama-lo de df como variável que conterá o quadro de dados

## Validação Parte I: corrigindo coluna "data"

Para verificar os tipos de dados presentes na estrutura de dados, pode se utilizar a função (levando em conta que o data frame foi alocado à variável df):

"df.dtypes"

Importante ressaltar que no nosso caso, a data foi considerado como um objeto e não um número inteiro (_integer_)

Existem funções específicas para trabalhar com data dentro do Pandas

"

df.ocorrencia_dia

df.ocorrencia_dia.dt.month

"

Entretanto, o último código retornará um erro, afirmando que só há leitura para valores do tipo 'datetimelike'

É preciso converter a coluna do tipo data, que foi carregada como tipo Object, em datetimelike. Na definição do df, coloque:

"[...] sep = ";", parse_dates = ['ocorrencia_dia']"

Caso mais de uma coluna se identifique datetimelike, é possível fazer:

"[...] sep = ";", parse_dates = ['ocorrencia_dia', '...']"

Entretanto, há um **CUIDADO** em relação à função parse_dates: SE UM MÊS VIER COM O VALOR 50, O CÓDIGO NÃO SURTIRÁ EFEITO E CONTINUARÁ OBJECT (o parse não será feito por conta de data não coerente)

## Validação Parte II: corrigindo leitura de "dia"

existe tanto uma função para poder verificar as primeiras linhas de um df quanto as últimas, e inserir um número dentro dos parênteses define a quantidade de linhas que você gostaria de ver :

"df.head()"

"df.tail()"

É possível observar que o Jupyter está lendo a Data como YYYY-MM-DD, mas confundindo o dia na arquivo original pelo mês (03/01 se torna 01/03). Para correção, existe uma condição que, nesse caso, se diz True para que o primeiro valor da coluna especificada tenha o valor "dia":

"[...]= ['ocorrencia-dia'], dayfirst = True)"

Algo de se destacar é que caso uma data (configurado a coluna para datetimelike) tenha uma célula vazia, virá "NaT", e caso um Object tenha uma célula vazia, virá "NaN"

## Validação parte III: Pandera

código de ocorrência já está no tipo "Int", e a data está tipo "datetime"

Precisamos de uma outra biblioteca para validação do df: junto ao pandas que foi denominado como pd, importe também "pandera" como "pa":

"import pandera as pa"

No meu caso, foi preciso utilizar o terminal do Jupyter para baixar (por qualquer motivo, o Jupyter não encontrava o módulo Pandera) e então chamar a função original para poder aciona-la, dessa forma:

"

! pip install --user pandera

import pandera as pa

"

Crie a condição de análise:

"

Schema = pa.DataFrameSchema(
    columns = {

​		[...]

​        "codigo_ocorrencia":pa.Column(pa.Int),

​		[...]

​    }
)

"

Para validar de fato, chame a função criada através do código:

"Schema.validate(df)"

### tipos de erro no Pandera

- Coluna com nome errado, erro: "column 'codigo_ocorrenci' not in dataframe";
- Tipo definido incorreto, erro: "SchemaError: expected series 'codigo_ocorrencia' to have type 'datetime64[ns]', got 'int64' "

## Validação parte IV: verificando erros pelo Pandera

Coluna com nome errado dentro do Schema, erro: "column 'codigo_ocorrenci' not in dataframe";

Tipo definido incorreto dentro do Schema, erro: "SchemaError: expected series 'codigo_ocorrencia' to have type 'datetime64[ns]', got 'int64' "

Em alguns casos, um valor dentro da tabela ser NULL não é o fim do mundo! Se assim for, basta dizer que a condição nullable é aceitável. A seguir está a mensagem que aparece caso um valor NULL surgir e, em seguida, o código a ser colocado:

```
non-nullable series 'ocorrencia_hora' contains null values: {4100: nan}
```

"

Schema = pa.DataFrameSchema(
    columns = {

​		[...]

​        "codigo_ocorrencia":pa.Column(pa.Int, nullable = True),

​		[...]

​    }
)

"

Caso haja um erro na hora dentro do banco de dados, que não condiza com a "expressão regular", significa que existe um dado com um horário estranho (12:60:00; 1200; -12:00:00) e o próprio Pandera informará o index e qual horário está causando o erro

## Validação parte V: Hora

É preciso garantir que a hora esteja na forma que ela seja reconhecida, como o formato 24 horas em HH:MM:SS entre o intervalo 00:00:00 - 23:59:59

Um método útil é usar o código:

"

Schema = pa.DataFrameSchema(
    columns = {

​		[...]

​        "ocorrencia_hora":pa.Column(pa.String,  pa.Check.str_matches(r'^([0-1]?[0-9]|[2][0-3]):([0-5][0-9]):([0-5][0-9])?$')),

​		[...]

​    }
)

"

## Validação Final: tamanho e condição false

No caso do destrito federal, ou UF (Unidade Federativa), temos certeza que nos 27 ocorrências existentes, existiram no máximo dois caracteres: AA. Assim, podemos restringir o número de caracteres que essa coluna deve conter com o código:

"

Schema = pa.DataFrameSchema(

​	columns = {

​		[...]

​		"ocorrencia_uf":pa.Column(pa.String, pa.Check.str_length(2,2))

​		[...]

​	}

)

"

No caso do estado, caso a ocorrência seja fora dos estados (em águas internacionais, por exemplo), há uma chance do dado ter um formato de célula NULL como "***", "0", " ", "null", ente outros casos. O limitador de tamanho não verifica se são dois números, dois asteriscos, ou duas letras aleatórias: apenas verifica se está igual a 2

**Sobre verificar vários df's com um mesmo DataFrameSchema:**

algumas tabelas podem ter colunas semelhantes, mas também podem ter colunas específicas. No caso das específicas, o Schema precisa saber que caso aquela coluna não seja encontrada para a df que está a ser validada, tudo bem. O código:

"

Schema = pa.DataFrameSchema(

​	columns = {

​		[...]

​		"codigo":pa.Column(pa.String, required = False),

​		[...]

​	}

)

"

