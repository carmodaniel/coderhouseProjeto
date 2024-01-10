# Pacotes necessários para o projeto
Até a fase em que o projeto se encontra hoje 10/01/2024 18:5 é necessário a utilização dos pacotes, conforme requirements.txt:<br>
`requests` necessário para ler as APIS. <br>
`pandas` para transformar os dados das APIS em dataframes e tratar esses dados.<br>
`plyer` realizar a notificação caso de algum erro para receber os dados das APIS.<br>
`sqlite3` salvar dataframe em um banco de dados.
`matplotlib` gerar gráfico<br><br>
versões: <br>
requests==2.31.0<br>
pandas==2.1.4<br>
DateTime==5.4<br>
plyer==2.1.0<br>
matplotlib==3.8.2
<strong>Observação:<i>O pacote sqlite3 não é neceessário instalação pois já e nativo do python</i></strong>

# projetoFinal
Ler Apis do Banco Central: (utilizando pacote requests) <br><br>
URL_dinheiro_em_circulacao<br>
URL_chancelas<br>
URL_moedas_comemorativas<br>

## No projeto final utilizado três APIS do Banco Central
[API - Dinheiro em Circulação](https://olinda.bcb.gov.br/olinda/servico/mecir_dinheiro_em_circulacao/versao/v1/odata/informacoes_diarias_com_categoria?$format=json) <br>
[Documentação - API - Dinheiro em Circulação](https://dadosabertos.bcb.gov.br/dataset/dinheiro-em-circulao)<br>
Informações diárias das quantidades de cédulas e moedas em circulação (não estão incluídas as moedas comemorativas). As informações estão separadas para cada espécie (cédula ou moeda) e denominação do Real (símbolos : R$, BRL) <br><br>

[API - Chancelas de cédulas nacionais produzidas](https://olinda.bcb.gov.br/olinda/servico/mecir_chancelas/versao/v1/odata/TodosDadosChancelas?$format=json)<br>
[Documentaçã - API - Chancelas de cédulas nacionais produzidas](https://dadosabertos.bcb.gov.br/dataset/chancelas-de-cedulas-nacionais-produzidas)<br>
Dados de chancelas de Ministro da Economia e Presidente do Banco Central do Brasil para os conjuntos de numerações de cédulas nacionais produzidas<br><br>

[API - Moedas Comemorativas](https://olinda.bcb.gov.br/olinda/servico/mecir_moedas_comemorativas/versao/v1/odata/informacoes_diarias?$format=json)<br>
[Documentação - API - Moedas Comemorativas](https://dadosabertos.bcb.gov.br/dataset/moedas-comemorativas)<br>
O arquivo disponibilizado apresenta informações diárias das quantidades de moedas comemorativas. As informações estão separadas para cada família (categoria) e denominação do Real (símbolos : R$, BRL).<br><br>

<strong>Objetivo do projeto é ler API, caso seja dado um erro par ler API apresentar um alerta com erro da leitura. Após a leitura da API salvar os dados em dataframe, fazer limpeza dos dados e salvar o daframe em um banco de dados para consulta. </strong><br><br>

### 1ª Fase.
Criado a função `ler_api_bc`, a função lê as APIS do banco central `url_dinheiro_em_circulacao,    url_chancelas , url_moedas_comemorativas`. Também foi criado a função ler_e_notificar que recebe os parâmetros url, df, api_bc essa função é responsável para notificar erros quando a função ler_api_bc lê as APIS.<br>

Caso a leitura seja processada sem erro os dados das apis são salvos nos dataframes <i>dados_dataframe_dinheiro_em_circulacao</i>, <i>dados_dataframe_chancelas</i> e <i>dados_moedas_comemorativas</i> <br><br>

### 2ª Fase.
Verificado se os dataframes possuíam dados ausentes, validado que não consta dados nulos ou em branco.<br>
Nos dataframes `dados_dataframe_dinheiro_em_circulacao`, `dados_moedas_comemorativa` o campo `Data` estava no formato <b>YYYY-MM-DD</b>, executado a formatação para <i>`DD-MM-YYYY`</i>. <br>
Criado uma coluna nos dataframes `Data_formatada` que recebe a Data formatada.<br>
Passado os dados dos `dados_dataframe_dinheiro_em_circulacao` para o dataframe `dinheiro_em_circulacao` os dados do `dados_moedas_comemorativa` para o dataframe `moedas_comemorativa`, também feito um filtro para que nesses dataframes somente seja exibido dados do ano de 2023.<br><br>

### 3ª Fase.
Criado função `dinheiro_em_circulacao_titulo` para criar a coluna Titulo com base na coluna `Categoria` do Dataframe `dinheiro_em_circulacao`.<br> 
Criado função `dados_dataframe_chancelas_titulo` para criar a coluna Titulo com base na coluna `descricaoDenominacao` do Dataframe dados_dataframe_chancelas_titulo<br>
Renomeada colunas com o nome igaul desses dataframes<br>.
Feito o a junção dos `dataframes dinheiro_em_circulacao` e `moedas_comemorati`vas utilizando a coluna `Data` como chave de relacionamento,  o merge desses dataframes gerou o dataframe `dincircu_moedCome`.<br>
Criado um dicionário de mapeamento com base na coluna 'Titulo' do dataframe dados_dataframe_chancelas, criado coluna 'Titulo_Chancela' no dataframe dincircu_moedCome usando map e inserindo os dados pegos no dataframe dados_dataframe_chancelas.<br>

Apartir  do dataframe `dincircu_moedCome` criado o banco de dados `projetoFinal.db`, feito leitura e exibição das tabelas do banco de dados.<br><br>

### 4ª Fase.

Para Facilitar o entendimento das informações foi criado o dicionário `nome_mes` para exibir e ordernar os meses utilizado  no dincircu_moedCome.<br>


Para exibir somente os valores em circulação cuja Espécie seja igual a Moeda criado o dataframe `moeda`, nesse dataframe foi executado o agrupamento para exibir o valor em circulação por Mês <br>
Também criado o dataframe `cedula` que somente exibi informação de Espécie igual Cédula, também agrupado a exibição por mês.<br>

O intuído da criação dos dataframes `moeda` e `cedula` é exibir a quantidade  de dinheiro que foi circulado por mês para os dois tipos de Espécie.<br>

Como a documentação da API não informa qual é a separação decimal dos valores optamos em não exibir em gráfico para não  ter distorção dos valores que existem.<br>
