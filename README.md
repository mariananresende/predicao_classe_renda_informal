# Uso de Modelo de Machine Learning para apoiar a identificação da renda informal das famílias do Cadastro Único 
Repositório para desenvolvimento de modelo de Machine Learning para predição de classe de renda das famílias do cadastro único considerando a renda formal para apioar o processo de qualificação da renda informal.

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
| DT_NASC_PESSOA	| usada para calcular a idade da pessoa | avaliar se a idade dos membros da família interfere na predição de renda, tanto do responsável familiar, quanto das pessoas. Para agrupar as pessoas em um linha por família, será calculado o percentual de pessoas que compõem a família por faixa etária |
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

## Limpeza dos valores Nan
Tanto na **Base famílias**, quanto na **Base pessoas** existe um número considerável de valores vazios considerando as regras de preenchimento do formulário do Cadastro Único. Assim, foi necessário olhar para as regras de preenchimento do formulário do Cadastro Único, com base no Manual do Entrevistador do Cadastro Único, de modo a identificar os valores vazios pelo fato de não serem de preenchimento obrigatório.

Desta forma, os campos NaN foram povoados com os seguintes valores, de acordo com cada regra especificada abaixo:

### Base famílias
#### Campos relacionados às características do domicílio:
Os campos 'qtd_comodos_domic_fam', 'qtd_comodos_dormitorio_fam', 'cod_material_piso_fam', 'cod_material_domic_fam', 'cod_agua_canalizada_fam', 'cod_abaste_agua_domic_fam', 'cod_banheiro_domic_fam', 'cod_escoa_sanitario_domic_fam', só são de preenchimento obrigatório no caso da resposta ter sido "1- Particular permamente" no campo 'cod_especie_domic_fam'. Sendo assim, quando o campo 'cod_especie_domic_fam' é diferente de 1, os valores dos campos relacionados às características do domicílio foi preenchido com **-1**.
#### Campo relacionado ao escoamento sanitário:
O campo 'cod_escoa_sanitario_domic_fam' só é de preenchimento obrigatório quando o campo 'cod_banheiro_domic_fam' é preenchido com "1 - Sim". Desta forma, quando o campo 'cod_banheiro_domic_fam' é igual a 2 (ou seja, não possui banheiro), o valor do campo 'cod_escoa_sanitario_domic_fam' foi preenchido com **-1**.
#### Campo relacionado à família quilombola:
O campo 'ind_familia_quilombola_fam' só deve ser preenchido caso o campo 'cod_familia_indigena_fam' for "2 - Não". Assim, quando o campo 'cod_familia_indigena_fam' é igual a 1 (ou seja, a família for indígena), o valor do campo 'ind_familia_quilombola_fam' foi marcado como **2**, ou seja, a família não é quilombola.
#### Campos relacionados ao local do domicílio e à espécie do domicílio:
Foi identificado que os valores vazios dos campos 'cod_especie_domic_fam' e 'cod_local_domic_fam' coincidem, ou seja, quando um valor é vazio na campo 'cod_especie_domic_fam' também é vazio no campo 'cod_local_domic_fam'. Na base de dados amostral de 2018 disponível no portal de dados abertos, não tem a marcação se a família vive em situação de rua. No Manual do entrevistador há a orientação de que, caso a família esteja em situação de rua, o bloco 2 - características do domicílio não deve ser preenchido, pois existe um cadastramento diferenciado. Desta forma, este caso de valores ausentes pode estar relacionado à situação de rua. De modo a tentar captar se essa condição interfere no modelo, os valores dos dois campos foram preenchidos com **9**, representando nenhuma das outras repostas.
#### Campos relacionadas à Grupos tradicionais e específicos
O campo 'ind_parc_mds_fam' está relacionado à marcação de grupos tradicionais e específicos para além de indígena, quilombola, situação de rua ou resgatados do trabalho análogo ao de escravo. Foi avaliado se os valores ausentes estavam relacionados à marcação de indígena ou quilombola, ou ao valor 9 para 'cod_especie_domic_fam' e 'cod_local_domic_fam' criado na tentativa de captar as famílias em situação de rua. Não foi identificado o motivo dos valores vazios. Assim, de modo a tentar captar se a ausência de marcação deste campo pode representar alguma situação que impacte no modelo, os valores vazios foram preenchidos com **9**, representando nenhuma das outras repostas.
#### Valores vazios remanescentes
Após todas as limpezas descritas acima, foram excluídos os valores remanescentes Nan.

### Base pessoas
#### Campo frequenta escola
Os campos 'cod_escola_local_memb', 'cod_curso_frequenta_memb', 'cod_ano_serie_frequenta_memb' só são obrigatório se a pessoa respondeu "1 - Sim, rede pública", "2 - Sim, rede particular" no campo 'ind_frequenta_escola_memb'. Desta forma, quando o campo 'ind_frequenta_escola_memb' foi preenchido com valor diferente de 1 ou 2, os campos vazios relacionados foram preenchidos com **-1**.
#### Campo ano e série que a pessoa frequenta
O campo 'cod_ano_serie_frequenta_memb' só é obrigatório ser preenchido quando no campo 'cod_curso_frequenta_memb' (curso que a pessoa frequenta) a pessoa respondeu uma das opções: 4 - Ensino Fundamental regular (duração 8 anos), 5 - Ensino Fundamental regular (duração 9 anos), 6 - Ensino Fundamental especial, 7 - Ensino Médio regular ou 8 - Ensino Médio especial. Assim, quando o campo 'cod_curso_frequenta_memb' foi preenchido com valor difente de 4, 5, 6, 7 ou 8 os valores vazios de 'cod_ano_serie_frequenta_memb' foram preenchidos com **-1**.
#### Campo curso mais elevado que frequentou
O campo 'cod_curso_frequentou_pessoa_memb' só é obrigatório quando o campo 'ind_frequenta_escola_memb' (Pessoa frequenta escola?) for respondido com "3 - Não, já frequentou". Assim, quando o campo 'ind_frequenta_escola_memb' estava diferente de 3, os valores vazios do campo 'cod_curso_frequentou_pessoa_memb' foram preenchidos com **-1**.
#### Campos relacionados ao curso frequentado
Os campos 'cod_ano_serie_frequentou_memb' e 'cod_concluiu_frequentou_memb' só devem ser preenchidos caso a pessoa frequentou algum dos cursos 4 - Ensino Fundamental 1ª a 4ª séries, Elementar (Primário), Primeira fase do 1º grau, 5 - Ensino Fundamental 5ª a 8ª séries, Médio 1º ciclo (Ginasial), Segunda fase do 1º grau, 6 - Ensino Fundamental (duração 9 anos), 7 - Ensino Fundamental Especial, 8 - Ensino Médio, 2º grau, Médio 2º ciclo (Científico, Clássico, Técnico, Normal) ou 9 - Ensino Médio Especial no campo 'cod_curso_frequentou_pessoa_memb'. Desta forma, quando a resposta do campo 'cod_curso_frequentou_pessoa_memb' foi diferente de 4, 5, 6, 7, 8 e 9 os valores vazios dos campos 'cod_ano_serie_frequentou_memb' e 'cod_concluiu_frequentou_memb' foi preenchido com **-1**.
#### Campos relacionados ao trabalho
Os campos 'cod_trabalhou_memb', 'cod_afastado_trab_memb', 'cod_agricultura_trab_memb', 'cod_principal_trab_memb', 'cod_trabalho_12_meses_memb', 'qtd_meses_12_meses_memb' só devem ser preenchidos para pessoas com 14 anos ou mais. Assim, para o caso de pessoas com idade menor que 14 anos, no campo 'idade', os valores dos campos relacionados ao trabalho foram preenchidos com **-1**.
#### Campo relacionado ao afastamento do trabalho
O campo 'cod_afastado_trab_memb' só deve ser respondido se o campo 'cod_trabalhou_memb' (Pessoa trabalhou na semana passada?) for respondido como "2 - Não". Assim, para os casos de resposta diferente de 2, os valores vazios do campo 'cod_afastado_trab_memb' foram preenchidos com **-1**.
#### Campo relacionado à atividade extrativista e ao trabalho principal
O campo 'cod_agricultura_trab_memb' (É atividade extrativista? 1 - Sim e 2 - Não) e o campo 'cod_principal_trab_memb' só são obrigatórios se a pessoa respondeu 1 - Sim no campo 'cod_trabalhou_memb' (Pessoa trabalhou na semana passada?) ou no campo 'cod_afastado_trab_memb' (Pessoa afastada na semana passada?). Assim, para o caso em que os 'cod_trabalhou_memb' e 'cod_afastado_trab_memb' tiveram resposta diferente de 1, os valores vazios de 'cod_agricultura_trab_memb' e de 'cod_principal_trab_memb' foram substituídos por **-1**. 
Além disso, observou-se ainda alguns valores vazios remanescentes para o campo 'cod_agricultura_trab_memb'. Desta forma, foi avaliado o preenchimento do campo 'cod_principal_trab_memb'. Quando este estava marcado com "2 - Trabalhador temporário em área rural", o valor vazio do campo 'cod_agricultura_trab_memb' foi alterado para "**1** - Sim" e para os demais casos para **9** - Desconhecido.  
#### Campo relacionado à quantidade de meses trabalhados
O campo 'qtd_meses_12_meses_memb' (Quantidade de meses trabalhados nos últimos 12 meses) só é obrigatório se a pessoa tiver respondido "1 - Sim" no campo 'cod_trabalho_12_meses_memb' (Pessoa com trabalho remunerado em algum período nos último 12 meses). Desta forma, os valores vazios do campo 'qtd_meses_12_meses_memb' foi alterado para **-1** quando o campo 'cod_trabalho_12_meses_memb' for diferente de 1.
#### Valores vazios remanescentes
Foram feitas diversas análises adicionais para identificar possíveis ajustes nos valores vazios finais. Entretanto, não foi identificada outra situação de valor vazio que parece estar relacionado à regra de preenchimento do Cadastro Único. Desta forma, as linhas com valores vazios remanescentes foram excluídas,



## Resultados
Após treinamento
Entre os modelos avaliados, o XGBoost apresentou o melhor desempenho global para o objetivo de triagem proposto, alcançando o maior valor de PR-AUC (0,86) e mantendo elevada precisão sob limiar de decisão mais restritivo. Esse resultado indica maior capacidade de ordenar as famílias por risco de inconsistência cadastral, permitindo a priorização de ações de qualificação com controle da carga operacional. Embora CatBoost e HistGradientBoosting tenham apresentado desempenhos próximos, o XGBoost mostrou ligeira superioridade no equilíbrio entre sensibilidade e precisão, sendo, portanto, selecionado como modelo final do estudo.
