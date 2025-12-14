# Uso de Modelo de Machine Learning para apoiar a identificação da renda informal das famílias do Cadastro Único 
Repositório para desenvolvimento de modelo de Machine Learning para predição de classe de renda das famílias do cadastro único considerando a renda informal.

## Introdução
O Cadastro Único é a principal ferramenta para caracterizar socioeconomicamente as famílias de baixa renda residentes no Brasil. A partir do Cadastro Único é possível selecionar beneficiários de programa sociais com o objetivo de diminuir a vulnerabilidade socioeconômica dessas famílias. Para tanto é preciso que os dados sejam qualificados, de maneira a melhorar a focalização dos programas e permitir que as famílias com perfil para cada programa sejam alcançadas.

A gestão do Cadastro Único é descentralizada, com os municípios sendo responsáveis pela identificação das famílias e inserção das informações no sistema. Ao governo federal compete fornecer os formulários utilizados na entrevista das famílias, capacitar os operadores e entrevistadores dos estados, para que estes, por sua vez, capacitem os operadores e entrevistados municipais, definir as diretrizes e boas práticas para o preenchimento correto das informações e lançar mão de diferentes estratégias para qualificar os dados. 

A falta de qualificação dos dados pode fazer com que famílias acessem programas sociais sem preencher os requisitos estabelecidos, dificultando a identificação e o acesso das famílias que são socioeconomicamente mais vulneráveis, tendo em vista limitações operacionais e orçamentárias. 

A renda é uma das principais informações utilizadas para a seleção das famílias. Atualmente os dados de renda formais são captados por meio da interoperabilidade com o Cadastro Nacional de Informações Sociais (CNIS). Entretanto, as rendas informais são captadas apenas por meio da autodeclaração do Responsável Familiar. 

De modo a apoiar a qualificação da informação relacionada à renda, o objetivo do projeto a ser desenvolvido no âmbito do MBA de ciência de dados e inteligência artificial aplicadas é apoiar o processo de qualificação da renda das famílias, por meio de um modelo de Machine Learning, para prever a classificação da faixa de renda das famílias com renda informal. 

A classificação da faixa de renda das famílias será feita a partir do desenvolvimento de um modelo treinado com os dados da renda formal, e depois utilizado para classificar as famílias com renda informal, de modo a ter um parâmetro para guiar uma maior qualificação dos dados de renda. Com isso, espera-se aumentar a focalização dos programas usuários, e reduzir a vulnerabilidade socioeconomica das famílias que mais precisam. 

## Variáveis do Cadastro Único utilizadas no modelo de Machine Learning
Para a seleção das variáveis do Cadastro Único a serem utilizadas no desenvolvimento do Modelo de Machine Learning para a predição da classe de renda das famílias com renda informal, foi feito um levantamento bibliográfico para identificar, na literatura especializada, as variáveis que mais contribuíram para o desenvolvimento de modelos de predição de renda, conforme lista abaixo:

**1. Variáveis sociodemográficas**
| Tipo | 	Exemplos | Evidência de importância |
| ------| ---------| -------------------------------|
| Composição familiar	| número de membros, razão de dependência, número de crianças e idosos, presença de cônjuge	| Fortemente preditiva em Britto et al. (2024) e McBride & Nichols (2018) — famílias menores e com maior razão de adultos ocupados → maior renda; maior número de dependentes → pobreza. |
| Idade e sexo do chefe| 	idade média dos adultos, sexo do chefe do domicílio |	Idade tem relação não linear com renda (parabólica) em Linhares et al. (2025); domicílios chefiados por mulheres apresentam menor renda média (Mostafa, 2016; Silva & França, 2021). |
| Raça/cor e etnia	| classificação autodeclarada	| Quando disponível, é relevante: Britto et al. (2024) mostra penalidade persistente para pretos e pardos na renda estimada.|

**2. Condições de moradia e infraestrutura**
| Tipo	| Exemplos	| Evidência de importância |
| --- | ----- | ------- |
| Tipo e material da moradia	| tipo de construção, material das paredes, piso, teto |	Entre as variáveis com maior ganho GINI em Silva & França (2021) e Wobcke et al. (2023) — refletem riqueza acumulada. |
| Acesso a saneamento e água |	água canalizada, banheiro, rede de esgoto, coleta de lixo	| Mostafa (2016): indicadores habitacionais estão entre as variáveis mais explicativas da renda no CadÚnico. |
| Densidade domiciliar |	pessoas por cômodo/dormitório	| Preditor consistente de pobreza em Britto et al. (2024) e Ohlenburg (2022). |
| Iluminação/energia elétrica |	acesso regular à energia, tipo de combustível usado	| Zheng et al. (2025) mostra relação direta com renda e pobreza energética.|

**3. Educação**
| Tipo |	Exemplos	| Evidência de importância |
| ---- | ---- | ----- |
| Escolaridade do chefe e dos adultos |	anos de estudo, ensino fundamental/médio/superior |	É o preditor mais estável e poderoso em quase todos os estudos (Britto et al. 2024; Dang et al. 2024; Silva & França 2021). |
| Taxa de analfabetismo	| indicador binário de saber ler/escrever	| Alta importância em modelos do CadÚnico (Mostafa 2016) e Indonesia DTKS (Wobcke 2023). |

**4. Trabalho e ocupação**
| Tipo |	Exemplos |	Evidência de importância |
|--- | ----- | ------ |
| Situação ocupacional |	empregado formal, informal, autônomo, desempregado |	Britto et al. (2024) mostra que a formalização é o preditor mais relevante de renda informal (SHAP > 0.20). |
| Setor de atividade / posição no trabalho |	agricultura, serviços, indústria	| Diferenciação crucial em Linhares et al. (2025) e McBride & Nichols (2018) — trabalho agrícola e autônomo fortemente associados à pobreza.|
| Número de ocupados no domicílio	 | total de pessoas com renda	| Variável-chave em Mostafa (2016) e Ohlenburg (2022) — sinaliza capacidade produtiva. |

**5. Benefícios sociais e transferências**
| Tipo	| Exemplos |	Evidência de importância |
| ----- | --------- | ----------- |
| Recebimento de programas sociais |	Bolsa Família, BPC, outros |	Em Britto et al. (2024) e Silva & França (2021), o recebimento de benefícios é altamente correlacionado com renda baixa, servindo também como label noise detector. |
| Valor dos benefícios |	total mensal recebido |	Moderadamente explicativo; melhora a classificação quando combinado com variáveis de ocupação (Britto 2024). |

**6. Localização e território**
| Tipo |	Exemplos	| Evidência de importância |
| ------ | -------- | --------- |
| Região/município/zona urbana-rural	| dummies regionais, IBGE codes |	Alta importância em Britto et al. (2024) e World Bank DTKS (2023) — diferencia fortemente padrões regionais de pobreza. |
| Infraestrutura territorial |	distância de serviços, densidade urbana |	Usada em Dang et al. (2024) e Zheng et al. (2025); substitui variáveis de geolocalização detalhada, com bom desempenho sem recorrer a satélite. |

**7. Variáveis derivadas e sintéticas**
| Tipo	| Exemplos |	Evidência de importância |
|----- | ------ | ------- |
| Razões e índices compostos	| razão de dependentes, índice de ativos domésticos, escolaridade média ponderada |	Em Ohlenburg (2022) e Dang et al. (2024), variáveis agregadas mostraram melhor estabilidade temporal e reduziram overfitting.|
| Interações automáticas (ML) |	combinações de educação × ocupação, sexo × estado civil	| Detectadas automaticamente por XGBoost/LASSO em Silva & França (2021) e Linhares et al. (2025), indicando sinergia entre dimensões sociais. |

Assim, de modo a utilizar no desenvolvimento do Modelo de Machine Learning as variáveis levantadas na revisão bibliográfica, foram utilizadas as seguintes variáveis do Cadastro Único:

**Variáveis de famílias**
| Variável | significado | Motivo para a seleção |
| ---- | ----- | --------- |
| CO_FAMILIAR_FAM	| Código Familiar | chave - identificador da família para associar às características das pessoas que compõem cada família  |
| DT_CADASTRO_FAM	| Data do cadastramento da Família, formato YYYY-MM-DD | avaliar se o tempo de cadastramento da família interfere na capacidade de predição da renda.|	
| CO_EST_CADASTRAL_FAM | Estado cadastral da família |	utilizada para selecionar apenas as famílias com estado cadastral 1, 2 ou 3, pois 1 - Em cadastramento, 2 - Sem Registro Civil, 3 - Cadastrado e 4 - Excluído |	
| VL_RENDA_MEDIA_FAM	| Valor da renda média (per capita) da família, formato NNNNNNNNN (não tem a vírgula) | O valor da renda da família será utilizado para treinar o modelo para predição da classe de renda - variável alvo |
| IN_TRABALHO_INFANTIL_PESSOA	| Trabalho infantil na família | Para o desenvolvimento do Modelo de Machine Learning será preciso avaliar se possuir membro da família em situação de trabalho infantil interfere na predição de renda |
| CO_MUNIC_IBGE_2_FAM |	Primeira parte do código IBGE: numérico de dois algarismos descrevendo sua UF | avaliar se a variável de localidade interfere de capacidade de predição da renda.	|
| CO_MUNIC_IBGE_5_FAM |	Segunda parte do código IBGE | avaliar se a variável de localidade interfere de capacidade de predição da renda.	|
|CO_LOCAL_DOMIC_FAM	| Características do local onde está situado o domicílio | avaliar se as caracerísticas dos domicílios das famílias interferem na capacidade de predição da renda.	|
| CO_ESPECIE_DOMIC_FAM	| Espécie do domicílio| avaliar se as caracerísticas dos domicílios das famílias interferem na capacidade de predição da renda.	|
| QT_COMODOS_DOMIC_FAM |	Qtd de comodos do domicilioa | avaliar se as caracerísticas dos domicílios das famílias interferem na capacidade de predição da renda.	|
| QT_COMODOS_DORMITORIO_FAM |Qtd de comodo servindo como dormitório do domicilio |	avaliar se as características dos domicílios das famílias interferem na capacidade de predição da renda.	|
|CO_MATERIAL_PISO_FAM	| Material predominante no piso do domicílio | avaliar se as caracerísticas dos domicílios das famílias interferem na capacidade de predição da renda.	|
| CO_MATERIAL_DOMIC_FAM	| Material predominante nas paredes externas do domicílio | avaliar se as caracerísticas dos domicílios das famílias interferem na capacidade de predição da renda.	|
| CO_AGUA_CANALIZADA_FAM |	Se o domicílio tem água encanada | avaliar se as caracerísticas dos domicílios das famílias interferem na capacidade de predição da renda.|
| CO_ABASTE_AGUA_DOMIC_FAM |	Forma de abastecimento de água | avaliar se as caracerísticas dos domicílios das famílias interferem na capacidade de predição da renda.	|
| CO_BANHEIRO_DOMIC_FAM	|Existência de banheiro | avaliar se as caracerísticas dos domicílios das famílias interferem na capacidade de predição da renda.	|
|CO_ESCOA_SANITARIO_DOMIC_FAM | Forma de escoamento sanitário | avaliar se as caracerísticas dos domicílios e infraestutura de serviços da localidade das famílias interferem na capacidade de predição da renda. |
| CO_ILUMINACAO_DOMIC_FAM  | Tipo de iluminação | avaliar se as caracerísticas dos domicílios interferem na capacidade de predição da renda. |
| IN_FAMILIA_INDIGENA_FAM	| Família indígena | avaliar se a marcação de pertencimento a um Grupo Populacional, Tradicional e Específico (GPTEs) interfere na predição de renda	|
| IN_FAMILIA_QUILOMBOLA_FAM |	Família quilombola | avaliar se a marcação de pertencimento a um Grupo Populacional, Tradicional e Específico (GPTEs) interfere na predição de renda  |
| QT_PESSOAS_DOMIC_FAM	| Quantidade de pessoas no domicílio | avaliar se a densidade do domicílio interfere na predição de renda |
| QT_FAMILIAS_DOMIC_FAM	| Quantidade de famílias no domicílio | avaliar se a densidade do domicílio interfere na predição de renda |
| IN_PARC_MDS_FAM	| Grupos tradicionais e específicos | avaliar se a marcação de pertencimento a um Grupo Populacional, Tradicional e Específico (GPTEs) interfere na predição de renda	 |

**Variáveis de pessoas**
| Variável | significado | Motivo para a seleção |
| ---- | ----- | --------- |
| CO_FAMILIAR_FAM	| Código Familiar | chave - identificador da família para associar às características das pessoas que compõem cada família. As características das pessoas que compõem a família serão agrupadas e associadas ao identificador da família, o qual será utilizado para identificar se o modelo foi efetivo na predição da renda da família analisada  |
| CO_CHV_NATURAL_PESSOA	| Número do membro da Família | identificador da pessoa. O modelo é treinado a partir das características de cada pessoa que compõe a família, para posteriormente ser agrupada e associar ao identificador da família, o qual será utilizado para identificar se o modelo foi efetivo na predição da renda da família analisada |
| CO_EST_CADASTRAL_MEMB	| Estado cadastral da pessoa | Para o desenvolvimento do Modelo de Machine Learning não serão usados os dados de pessoas excluídas |	
| CO_SEXO_PESSOA	| sexo da pessoa | avaliar se o sexo dos membros da família, especialmente do responsável familiar, interfere na predição de renda	|
| DT_NASC_PESSOA	| avaliar se a idade dos membros da família interfere na predição de renda, tanto do responsável familiar, quanto das pessoas. Para agrupar as pessoas em um linha por família, será calculado o percentual de pessoas que compõem a família por faixa etária |
| CO_PARENTESCO_RF_PESSOA | Relaçao de parentesco com o RF | avaliar se as características de raça/cor, gênero, idade, ter deficiência física, escolaridade, trabalho do responsável familiar interfere na predição de renda |
| CO_RACA_COR_PESSOA	| cor ou raça | avaliar se as características de raça/cor dos membros da família, especialmente do responsável familiar,  interferem na predição de renda.	|
| CO_DEFICIENCIA_MEMB	| Pessoa tem deficiência | avaliar se a presença de pessoas com deficiência na família interfere na predição de renda |
| CO_SABE_LER_ESCREVER_MEMB	| Pessoa sabe ler e escrever | avaliar se as informações relacionadas a escolaridade interferem na predição de renda	|
| IN_FREQUENTA_ESCOLA_MEMB	| Pessoa frequenta escola | avaliar se as informações relacionadas a escolaridade interferem na predição de renda	|
| CO_CURSO_FREQUENTA_MEMB | Curso que a pessoa frequenta |	avaliar se as informações relacionadas a escolaridade interferem na predição de renda	|
| CO_CURSO_FREQ_PESSOA_MEMB	| Curso mais elevado que a pessoa frequentou | avaliar se as informações relacionadas a escolaridade interferem na predição de renda	|
| CO_TRABALHOU_SEMANA_MEMB	| Pessoa trabalhou na semana passada | avaliar se as informações relacionadas a trabalho interferem na predição de renda |
| CO_AFASTADO_TRAB_MEMB	| Pessoa afastada na semana passada | avaliar se as informações relacionadas a trabalho interferem na predição de renda |
| CO_AGRICULTURA_TRAB_MEMB	| É atividade extrativista | avaliar se as informações relacionadas a trabalho interferem na predição de renda	|
| CO_PRINCIPAL_TRAB_MEMB |	Função principal | avaliar se as informações relacionadas a trabalho interferem na predição de renda |
| CO_TRABALHO_12_MESES_MEMB	| Pessoa com trabalho remunerado 12 meses | avaliar se as informações relacionadas a trabalho interferem na predição de renda	 |






### Variáveis relacionadas a famílias

