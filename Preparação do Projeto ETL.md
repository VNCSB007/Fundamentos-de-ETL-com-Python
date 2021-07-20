# Preparação do Projeto ETL

## Configuração do ambiente (Jupyter)

Vou experimentar fazer a codificação em Web

site do Jupyter: https://jupyter.org (não se esquecer que tem a possibilidade de baixa-lo na sua máquina para usa-lo de forma nativa)

vamos utilizar o JupyterLab e ao criar um laboratório, clicar para criar um Notebook em Python (pode demorar um pouco para se criar o laboratório; **DETALHE**, pode haver um número grande de pessoas utilizando os contêineres no dia: caso dê falha na criação por conta do número máximo de usuários excedido, tente novamente ou aguarde um pouco para realizar uma nova tentativa)

Dentro do notebook, cada linha de comando pode ser classificada com o botão na barra de ferramentas como Code, Markdown ou Raw; além disso, é possível arrastar as linhas criadas para organizar da forma que achar melhor puxando pela barra azul lateral

Dentro desse Lab, existe a função Save: **CUIDADO,** pois esta função serve para salvar o código dentro do LabBrowser e não na sua máquina. Para Salvar os arquivos criados no Laboratório do Jupyter, basta fazer o Download  

## Origem dos dados acessíveis para o nosso projeto

site: https://www2.fab.mil.br/cenipa/

Ir na sessão "Acesso à Informação" e clicar em "Dados abertos"

Lá, seremos direcionado aos arquivos da base de dados de ocorrências aeronáuticas, onde teremos acesso a informações disponíveis sobre as aeronaves, fatalidades, horário dos eventos e informações típicas das investigações de acidentes.

Todos os dados estarão em arquivo **.CSV**. Ao final da página, aparecem os arquivos mencionados, numa seção denominada "Dados e recursos". No arquivo "Modelo de dados", é possível verificar como as informações estão relacionadas. Depois de clicar no link, vá em "ir para recurso" e será fornecido a imagem de como estão relacionados os dados.

iremos extrair todos os dados presentes nos codigo_ocorrencia[numero] de 1 a 4 e o próprio codigo_ocorrencia

Obteremos em "Dados e recursos" a "Tabela de ocorrência" e, após verificar a última atualização da tabela, ir em "ir para recurso". Verifique se o arquivo em csv foi baixado em seus Downloads

## Visualização dos dados na planilha

Aberto no Microsoft Excel (verificar de quando a quando foram gravados os dados, no nome do arquivo .csv)

**O Objetivo do nosso projeto** é extrair os dados dessa planilha: valida-los (se as datas são constantes), limpa-los (no caso do aeródromo) e transforma-los (ocorrência por cidade, estado, por classificação, dentre outras, utilizando Pandas + Python, oferecendo novas informações importantes)