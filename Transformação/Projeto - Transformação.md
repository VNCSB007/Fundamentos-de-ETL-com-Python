# Projeto - Transformação 

## Epílogo sobre Limpeza

Uma outra maneira de se poder normalizar os valores nulos é utilizando o script:

"

valores_nulos = ["$$$", "***", "OOO", "&%"]

df = pd.read_csv([...]dayfirst = True, na_values = valores _nulos)

"



df.loc e def.iloc tem funções iguais, mas funcionam diferente: o iloc funciona como índice (então um df.iloc(10:15) mostrará entre o 10 e o 14), já o df.loc mostrará todos os _lables_ daquele intervalo (10 até o 15)

Para localizar as ocorrencias em suas linhas, com valores nao disponiveis, pedir

"df.cocorrencia_uf.isnull()"

mas para localizar  A LINHA ESPECÍFICA, é preciso 

"

filtro = df.ocorrencia_uf.isnull()

df.loc[filtro]

"

Para contar as ocorrências totais (ou seja, as vezes em que não apareceu um valor nulo):

"df.count()"

## Parte I: localizando 



para filtrar ocorrências em específicas )por exemplo, uma pessoa ter mias de 10 recomendações), uma boa prática seria:

"

filtro = df.ocorrencia_recomendacoes > 10

df.loc[filtro]

"

lembrando que aonde este filtro está sendo usado no espaço das LINHAS, o que traz todas as colunas. Colocando uma vírgula e o nome das colunas, faz com que apenas colunas específicas apareçam

para qu uma coluna, com um valor espe´cifico dentor dela, apareça nos loc, use:

"

filtro = df.ocorrencia_classificacao == 'INCIDENTE GRAVE'

df.loc[filtro]

"

Caso exista outra condição que precise estar junta para explorar, adicione filtroN e coloque o operador & dentro do filtro

"

[...]

[...]

df.loc[filtro & filtro2]

"

caso possa ter outra ou não, vamos de:

"

[...]

[...]

df.loc[filtro | filtro2]

"

Como esses filtros se tratam de condições booleanas, caso exista mais de uma condição dentro de um filtro (como uma condição "|"), é OBRIGATÓRIO QUE ESTEJAM AS BOOLEANAS ENTRE PARÊNTESES PARA QUE NÃO HAJA CONFLITO COM O FILTRO 

"filtro = (pinhamunhagaba == 'ok') | (condicao == 'grave')"

Se dentro de um booleano é aceitavel respostas diferentes, podemos trabalhar com isso ! (seria uma outra forma de utilizar o "|", só que utilizando a função "isin()")

"filtro = df.ocorrencia_classificacao.isni('INCIDENTE GRAVE', 'INCIDENTE')"

MAs e se eu lembrar apenas de parte do valor possível dentro de uma coluna? No worries!

"

filtro = df.ocorrencia_cidade.str[0] == "C"

filtro2 = df.ocorrencia_cidade.str[-1] == "A"

[...]

"

MAs e se eu souber que tem um valor, mas não faço ideia de onde pode estar? Gotcha!

"

filtro = df.ocorrencia_cidade.str.contains("MA")

"

Ah, mas eu não lembro se era um valor perdido ou outro valor agora que eu to pensando melhor? Jura? mais avançado, mas sem problemas!

"filtro = df.ocorrencia_cidade.str.contains("AL|MA")"

Putz mas eu acho que seria melhor lozalizar pelo ano, lembro que era 2015? Diferencia, mas tem um jeito tambem!

"filtro = df.ocorrencia_dia.dt.year == 2015"

Caso queira fazer por mês, podemos utilizar outro filtro ou o mesmo:

"

filtro = df.ocorrencia_dia.dt.year == 2015

filtro2 = df.ocorrencia_dia.dt.month == 12

"

OOOUUU

"filtro = (df.ocorrencia_dia.dt.year == 2015) & (df.ocorrencia_dia.dt.month == 12)"

Mas eu gostaria que imprimissem todas as ocorrencias de data e hora pq sim? ta... ne! (importante colocar em str para não haver conflito na impressão!)

"df.ocorrencia_dia.astype(str) + ' ' + df.ocorrencia_hora"

alias, boa! faz uma coluna só disso? Claro!

"df['ocorrencia_data_e_hora'] = pd.to_datetime(df.ocorrencia_dia.astype(str) + ' ' + df.ocorrencia_hora)"

sabe o que seria bom? saber o que aconteceu das 11 do dia 3 até as 13 do dia 8, de Dezembro de 2015... bem especifico, mas com essa nova coluna eu consigo dar um jeito!

"

filtro = df.ocorrencia_data_e_hora >= '2015-12-03 11:00:00'

filtro2 = df.ocorrencia_data_e_hora == '2015-12-08 13:00:00'

"

# Agrupamento de dados

que tal criar um data frame com uma seção do df? agrupamento 1 saindo! (lembrando da possibilidade de não haverem todas as ocorrências possíveis devido à falta de dados 'NaN': não estaríamos mostrando a VERDADE  da situação)

"

filtro1 = df.ocorrencia_dia.dt.year == 2015

filtro2 = df.ocorrencia_dia.dt.month == 3

df2015_03 = df.loc[filtro1 & filtro2]

"

uma forma de verificar, incluindo os dados NaN, a quantidade de ocorrências é através do:

"df2015_03.groupby(['ocorrencia_classificacao']).size()"