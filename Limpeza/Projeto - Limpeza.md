# Projeto - Limpeza

## Como será feito a limpeza

no aerodromo, temos casos em que ele não foi informado no formato "***", assim como casos inválidos do ponto de vista lógico em UF com 2 asteriscos. uma forma rápida de verificar os tipos de valores na sua tabela csv, é ir no Excel, ir em "inserir", depois pedir por "Tabela" na sessão "Tabelas" e demarcar a sua planilha inteira. desta forma, você conseguirá acessar todos os tipos de valores em cada um das colunas.

## Limpeza parte I: Excel e localização

analise os dados para checar se há alguns dados que iremos trocar por outros valores que nós classificaremos como "não informados"

Lembrando que, no caso do Jupyter Lab, se localiza um valor específico do data frame ao fazer:

"df.loc[1,"ocorrencia_cidade"]"

Note que não estamos mexendo com um índices no sentido de Lista ou Array, mas com um índice no sentido de Lable (por isso não há correspondência quando se pede "df.loc[-1]"). Para se buscar linhas específicas em um data frame, faça-se tal qual:

"df.loc[(5, 10, 3333),]"

No caso de mostrar todos os valores de apenas uma coluna, é um pouco diferente:

"df.loc[:,"codigo_ocorrencia"]"

## Limpeza parte II: valores únicos e definindo índice

Para se verificar se uma coluna é composta inteiramente de valores diferentes, podemos verificar em:

"df.codigo_ocorrencia.is_unique"

Ademais, classificar uma coluna de valores como sendo o índice da tabela é possível através de:

"df.set_index('codigo_ocorrencia')"

Entretanto, isto aconteceu só para o código chamado. Para inserir no data frame de forma legítima, faça:

"df.set_index('codigo_ocorrencia', inplace = True)"

Se tudo ocorreu como planejado, coisas como:

"df.loc[79802]"

São possíveis agora :smile: 

Se houver a necessidade de se utilizar o index que o próprio Jupyter fornecia, é possível resetar o índice com:

"df.reset_index(drop = True, inplace = True)"

## Limpeza parte III: Alterando os dados

Sabendo do problema de limitar os caracteres disponíveis em uma coluna e das possibilidades de valores nulos, comecemos - 

Uma maneira de manualmente redefinir o valor em uma linha-coluna específica se dá por:

"df.loc[0, "ocorrencia_latitude"] = ''"

Tenha **CUIDADO,** pois definir apenas a linha ou a coluna corre o risco de atualizar todos os valores, Object ou Int, naquele valor ditado

Uma **Boa Prática** quando se trata de códigos que podem comprometer todo o banco de dados, é aconselhável criar uma coluna com final '_bkp' a fim de testar o código, com índices diferentes:

"df['codigo_ocorrencia1_bkp'] = df.codigo_ocorrencia1"

Temos como mudar todos os valores de forma filtrada dentro do Python:

"df.loc[df.ocorrencia_uf == "SP", ['ocorrencia_classificacao']] = "GRAVE""

**SEMPRE IMPORTANTE FRISAR QUE TODAS AS MUDANÇAS FEITAS NO DATA FRAME SÃO FEITAS NO PRÓPRIO AMBIENTE DE PROGRAMAÇÃO, E NÃO NO ARQUIVO ORIGINAL**

## Limpeza parte IV: normalizando os dados 

Voltando ao Excel e verificando os dados que deveremos mexer. Nesse caso, definir no Jupyter em textos quais são os parâmetros de nulidade dentro da tabela baixada ajuda a identificação para o Python

Assim que definido quais são as variáveis a serem alterradas para NA, podemos utilizar:

"df.loc[df.ocorrencia_aerodromo == "###!", ["ocorrencia_aerodromo"]] = pd.NA" 

Mas também temos a função 

"df.replace(["***", "NULL"], pd.NA, inplace = True)"

E como descobrir quantos dados não informados eu estou transformando?

"df.isna().sum()" 

OR 

"df.isnull().sum()"

Após encontrar e definir todos os dados nulos da tabela, é possível dar um valor 0 para todos os NAs ou NULLs com

"df.fillna(0)"

É importante pensar em como a tabela está estruturada para poder avaliar com exatidão qual valor melhor representaria o nulo (em alguns casos, o próprio 0 tem um valor relevante, e ao voltar atrás com o NA, pode se levar junto outros valores consequentes não-nulos inicialmente). Caso esteja claro que uma tabela, antes livre de NAs, agora possui alguns por conta deste tipo de erro citado, é possível corrigir com um dicionário:

"df.fillna(value = {'total_recomendacoes': 10}, inplace = True)"

## Limpeza Final: eliminação de aspectos do data frame 

Para se criar uma nova coluna, utilizamos o já familiar:

"df['total_recomendacoes_bkp'] = df.total_recomendacoes"

Entretanto, para se deletar o mesmo, se faz:

"df.drop(['total_recomendacoes_bkp'], axis = 1, inplace = True)"

Importante notar que foi necessário colocar o parâmetro 'axis = 1', já que o eixo padrão está padronizado em 0, que se traduz para 'linha'

Uma função perigosa quando se trata de exclusões, é:

"df.dropna()"

Já que ele retirará também as linhas que as contém

Uma prática para se eliminar duplicatas exatas, é:

"df.drop_duplicate()"

Em alguns casos, até dados Outliers são considerados para drop, depende da decisão da equipe 