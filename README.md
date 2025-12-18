# Uso de Modelo de Machine Learning para apoiar a qualificação da renda informal no Cadastro Único

Este repositório reúne o desenvolvimento de um modelo de aprendizado de máquina aplicado a dados do Cadastro Único para Programas Sociais do Governo Federal, utilizando famílias com renda domiciliar per capita formalmente observada em registros administrativos do Cadastro Nacional de Informações Sociais (CNIS).

O objetivo do projeto é estimar, para famílias sem renda formal captada pelo CNIS, a probabilidade de existência de inconsistências entre a renda declarada no cadastro e a renda provável inferida a partir de informações socioeconômicas, da composição familiar e das características do domicílio. O modelo busca apoiar a priorização de ações de qualificação cadastral, oferecendo um critério objetivo e transparente para identificação de casos com maior risco de divergência de renda e potencial repercussão em políticas sociais com critério de renda.

---

## Introdução

O Cadastro Único constitui o principal instrumento do Estado brasileiro para a caracterização socioeconômica das famílias de baixa renda residentes no país. A partir de suas informações, é possível selecionar beneficiários de diversos programas sociais, com o objetivo de reduzir a vulnerabilidade socioeconômica e promover a inclusão social. Para que esses objetivos sejam alcançados de forma eficaz, é fundamental que os dados cadastrais sejam qualificados, garantindo melhor focalização das políticas públicas e maior aderência entre o perfil das famílias e os critérios de cada programa.

A gestão do Cadastro Único é descentralizada, cabendo aos municípios a identificação das famílias e o registro das informações no sistema. Ao governo federal compete, dentre outras, definir os formulários de coleta, promover a capacitação de operadores e entrevistadores, estabelecer diretrizes e boas práticas para o correto preenchimento das informações e adotar estratégias voltadas à qualificação contínua da base de dados.

A baixa qualidade das informações cadastrais pode resultar em distorções no acesso às políticas sociais, permitindo que famílias que não atendem plenamente aos critérios de elegibilidade acessem determinados benefícios, ao mesmo tempo em que dificulta a identificação e o atendimento de famílias em situação de maior vulnerabilidade, especialmente diante de limitações operacionais e orçamentárias.

A renda domiciliar per capita é uma das principais variáveis utilizadas na seleção de famílias para políticas públicas. Atualmente, informações sobre rendimentos formais são obtidas por meio da interoperabilidade com o Cadastro Nacional de Informações Sociais (CNIS). No entanto, as rendas de natureza informal — predominantes entre parte significativa do público do Cadastro Único — são captadas exclusivamente por meio da autodeclaração do Responsável Familiar, o que impõe desafios adicionais ao processo de qualificação cadastral.

Nesse contexto, o presente projeto, desenvolvido no âmbito do MBA em Ciência de Dados e Inteligência Artificial Aplicadas, propõe o uso de técnicas de aprendizado de máquina para apoiar a qualificação da informação de renda no Cadastro Único. A partir de padrões aprendidos em famílias com renda formal observada no CNIS, o modelo estima a probabilidade de inconsistência entre a renda declarada e a renda provável de famílias sem registro formal, utilizando exclusivamente características socioeconômicas declaradas, informações sobre a composição familiar e condições do domicílio.

O modelo desenvolvido consiste em um classificador binário treinado com dados de famílias com renda formal identificada no CNIS, capaz de aprender padrões associados a situações em que a renda domiciliar per capita observada supera o limite de ½ salário mínimo. Ressalta-se que o modelo não tem por finalidade automatizar decisões de elegibilidade ou exclusão de famílias de políticas públicas, mas sim oferecer um instrumento de apoio à gestão, orientando a priorização de ações de qualificação cadastral e contribuindo para uma maior focalização das políticas públicas sociais com critérios de renda e que utilizam o Cadastro Único para seleção de beneficiários.
 

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

Assim, de modo a utilizar no desenvolvimento do Modelo de Machine Learning as variáveis levantadas na revisão bibliográfica, foram utilizadas as seguintes variáveis do Cadastro Único, como características explicativas do modelo, com exceção da renda domiciliar per capita, que é utilizada apenas para a construção do alvo durante o treinamento. Nenhuma informação diretamente relacionada à elegibilidade a benefícios sociais é utilizada como entrada do modelo, de modo a evitar vazamento de informação:

**Variáveis de famílias**
| Variável                     | Significado                                                    | Papel no modelo                |
| ---------------------------- | -------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| CO_FAMILIAR_FAM              | Código identificador da família                                | Chave técnica utilizada exclusivamente para agregação das informações das pessoas ao nível familiar. Não é utilizada como variável explicativa no modelo |
| CO_EST_CADASTRAL_FAM         | Estado cadastral da família                                    | Utilizada como critério de filtragem para inclusão apenas de famílias ativas ou em processo de cadastramento (exclui registros encerrados ou excluídos)  |
| VL_RENDA_MEDIA_FAM           | Renda domiciliar per capita da família (sem separador decimal) | Utilizada **exclusivamente** para a construção da variável-alvo durante o treinamento do modelo. Não integra o conjunto de variáveis explicativas        |
| IN_TRABALHO_INFANTIL_FAM     | Indica presença de trabalho infantil na família                | Proxy de vulnerabilidade socioeconômica, associada a contextos de inserção precoce no mercado de trabalho     |
| CO_MUNIC_IBGE_2_FAM          | Código da UF (IBGE – 2 dígitos)                                | Captura heterogeneidades territoriais amplas associadas a mercados de trabalho, custo de vida e oportunidades   |
| CO_MUNIC_IBGE_5_FAM          | Código do município (IBGE – 5 dígitos)                         | Permite capturar efeitos locais e contextuais relacionados à estrutura econômica e aos serviços disponíveis     |
| IN_FORMULARIO_SUP2_FAM       | Regitro no Formulário Suplementar 2                            | As famílias em situação de rua devem ser marcadas com "1 - sim" para esse campo e o formulário suplementar 2 deve ser preenchido com as características das pessoasque compõem a familia, relacionadas à situação de rua  |
| CO_LOCAL_DOMIC_FAM           | Tipo de localização do domicílio                               | Indicador das condições territoriais e de acesso a infraestrutura        |
| CO_ESPECIE_DOMIC_FAM         | Espécie do domicílio                                           | Característica estrutural do domicílio associada a padrões socioeconômicos     |
| QT_COMODOS_DOMIC_FAM         | Quantidade total de cômodos                                    | Proxy de qualidade e dimensão do domicílio    |
| QT_COMODOS_DORMITORIO_FAM    | Número de cômodos utilizados como dormitório                   | Indicador de adensamento domiciliar     |
| CO_MATERIAL_PISO_FAM         | Material predominante do piso                                  | Indicador indireto de condições habitacionais         |
| CO_MATERIAL_DOMIC_FAM        | Material predominante das paredes externas                     | Indicador indireto de qualidade da moradia     |
| CO_AGUA_CANALIZADA_FAM       | Existência de água canalizada                                  | Indicador de acesso a infraestrutura básica     |
| CO_ABASTE_AGUA_DOMIC_FAM     | Forma de abastecimento de água                                 | Indicador de condições sanitárias e infraestrutura local    |
| CO_BANHEIRO_DOMIC_FAM        | Existência de banheiro no domicílio                            | Indicador de saneamento básico   |
| CO_ESCOA_SANITARIO_DOMIC_FAM | Forma de escoamento sanitário                                  | Indicador combinado de saneamento e infraestrutura urbana/rural     |
| CO_ILUMINACAO_DOMIC_FAM      | Tipo de iluminação do domicílio                                | Proxy de acesso a serviços básicos    |
| IN_FAMILIA_INDIGENA_FAM      | Indica se a família é indígena                                 | Identifica pertencimento a Grupos Populacionais Tradicionais e Específicos (GPTEs), associado a contextos |
| IN_FAMILIA_QUILOMBOLA_FAM    | Indica se a família é quilombola                               | Identifica pertencimento a GPTEs, associado a contextos territoriais e socioeconômicos específicos    |
| QT_PESSOAS_DOMIC_FAM         | Número de pessoas no domicílio                                 | Proxy de tamanho familiar e potencial pressão sobre a renda       |
| QT_FAMILIAS_DOMIC_FAM        | Número de famílias no domicílio                                | Indicador de compartilhamento de moradia e densidade       |
| IN_PARC_MDS_FAM              | Identificação de GPTEs                                         | Utilizada como marcador de pertencimento a grupos com dinâmicas socioeconômicas específicas     |


As variáveis individuais são agregadas ao nível familiar por meio de estatísticas descritivas (percentuais, indicadores de presença e distribuição etária), de forma a preservar a unidade de análise do modelo no nível da família. As variáveis do Cadastro Único relacionadas a pessoas são destacadas na tabela abaixo:

**Variáveis de pessoas**
| Variável                  | Significado                                     | Papel no modelo                                                                             |
| ------------------------- | ----------------------------------------------- | ------------------------------------------------------------------------------------------- |
| CO_FAMILIAR_FAM           | Código identificador da família                 | Chave técnica para agregação das informações individuais ao nível familiar                  |
| CO_CHV_NATURAL_PESSOA     | Identificador da pessoa no núcleo familiar      | Utilizado apenas para organização e agregação dos dados                                     |
| CO_EST_CADASTRAL_MEMB     | Estado cadastral da pessoa                      | Utilizado para excluir pessoas com cadastro encerrado ou excluído                           |
| CO_SEXO_PESSOA            | Sexo da pessoa                                  | Avalia a composição de gênero da família, com foco no responsável familiar                  |
| DT_NASC_PESSOA            | Data de nascimento                              | Utilizada para cálculo da idade e posterior agrupamento em faixas etárias                   |
| CO_PARENTESCO_RF_PESSOA   | Parentesco com o Responsável Familiar           | Permite identificar características específicas do responsável familiar                     |
| CO_RACA_COR_PESSOA        | Raça/cor                                        | Utilizada para avaliar a composição racial da família, com foco no responsável familiar |
| CO_DEFICIENCIA_MEMB       | Indica se a pessoa possui deficiência           | Proxy de necessidades de cuidado e de possíveis restrições à inserção laboral               |
| CO_SABE_LER_ESCREVER_MEMB | Indica se a pessoa sabe ler e escrever          | Indicador básico de escolaridade                                                            |
| IN_FREQUENTA_ESCOLA_MEMB  | Indica se a pessoa frequenta escola             | Indicador de escolarização em curso                                                         |
| CO_CURSO_FREQUENTA_MEMB   | Curso que a pessoa frequenta                    | Indicador do nível educacional atual                                                        |
| CO_CURSO_FREQ_PESSOA_MEMB | Curso mais elevado já frequentado               | Indicador de capital educacional acumulado                                                  |
| CO_TRABALHOU_SEMANA_MEMB  | Indica se trabalhou na semana de referência     | Proxy de inserção no mercado de trabalho                                                    |
| CO_AFASTADO_TRAB_MEMB     | Pessoa afastada na semana passada por motivo de doença, falta voluntária, licença, férias ou outro motivo  | Indicador para captar afastamento do trabalho      |
| CO_AGRICULTURA_TRAB_MEMB  | Indica se o trabalho principal foi na agricultura, criação de animais, pesca ou coleta (extração vegetal) | Indicador para captar se o trabalho está relacionado a atividades do meio rural |
| CO_PRINCIPAL_TRAB_MEMB | Indica a função principal do trabalho do membro da família | Indicador que marca a função do trabalho principal, que pode ser: 1 - Trabalhador por conta própria (bico, autônomo); 2 - Trabalhador temporário em área rural; 3 - Empregado sem carteira de trabalho assinada; 4 - Empregado com carteira de trabalho assinada; 5 - Trabalhador doméstico sem carteira de trabalho assinada; 6 - Trabalhador doméstico com carteira de trabalho assinada; 7 - Trabalhador não-remunerado; 8 - Militar ou servidor público; 9 - Empregador; 10 - Estagiário e 11 - Aprendiz |
| CO_TRABALHO_12_MESES_MEMB | Indica trabalho remunerado nos últimos 12 meses | Proxy de vínculo laboral e estabilidade econômica          |


## Construção da base de dados e critérios de seleção

A base de dados utilizada neste projeto foi construída a partir de registros do Cadastro Único, da referência de 11/2025, que já se encontravam previamente povoados com informações de renda formal provenientes do Cadastro Nacional de Informações Sociais (CNIS), conforme os procedimentos estabelecidos na Instrução Normativa nº 1/SAGICAD/MDS, de 02 de junho de 2023. Não foi realizada, no âmbito deste trabalho, integração direta entre bases administrativas externas.

Foram selecionadas exclusivamente famílias cujos registros indicavam que a renda havia sido obtida a partir do CNIS, conforme identificado nos campos de origem da renda do Cadastro Único (CO_ORGM_VLR_RNDMO_MES_PASSADO = 2 e CO_ORGM_VLR_RNDMO_BRUTO_PRDO = 2). Esse critério garante que a renda utilizada como referência no treinamento do modelo corresponda a informações administrativas formalmente observadas, independentemente do valor da renda declarada no cadastro.

No nível individual, foram consideradas apenas pessoas pertencentes a essas famílias e que apresentavam registros válidos no Cadastro Único e com CPF regular.
As características socioeconômicas individuais — relacionadas à escolaridade, idade, condição de trabalho e outras dimensões observáveis — foram posteriormente agregadas ao nível familiar, compondo o conjunto de variáveis explicativas utilizado pelo modelo.

O recorte adotado permite que o modelo aprenda padrões socioeconômicos associados à presença de renda formal observada no CNIS, sendo posteriormente aplicado para estimar a probabilidade de inconsistência entre a renda declarada e a renda provável de famílias sem renda formal captada, com o objetivo de apoiar a priorização de ações de qualificação cadastral.

## Preparação da base

Tanto na **Base famílias**, quanto na **Base pessoas** existe um número considerável de valores vazios considerando as regras de preenchimento do formulário do Cadastro Único. Assim, foi necessário olhar para as regras de preenchimento do formulário, com base no Manual do Entrevistador do Cadastro Único, de modo a identificar os valores vazios pelo fato de não serem de preenchimento obrigatório.

No processo de limpeza, os campos vazios foram povoados com os seguintes valores, de acordo com cada regra especificada abaixo:

### Base famílias
#### Campos de domicílio relacionados a famílias em situação de rua
O "Bloco 2 - Características do domicílio" não deve ser preenchido para famílias em situação de rua. Assim, os campos "CO_LOCAL_DOMIC_FAM", "CO_ESPECIE_DOMIC_FAM", "QT_COMODOS_DOMIC_FAM", "QT_COMODOS_DORMITORIO_FAM",  "CO_MATERIAL_PISO_FAM", "CO_MATERIAL_DOMIC_FAM", "CO_AGUA_CANALIZADA_FAM", "CO_ABASTE_AGUA_DOMIC_FAM", "CO_BANHEIRO_DOMIC_FAM", "CO_ESCOA_SANITARIO_DOMIC_FAM", "CO_ILUMINACAO_DOMIC_FAM", e os campos "QT_PESSOAS_DOMIC_FAM" e "QT_FAMILIAS_DOMIC_FAM", que também possuem a orientação de não serem preenchidos para famílias em situação de rua, tiveram os valores NaN preenchidos com **-1**, significando "Não se aplica", quando o campo "IN_FORMULARIO_SUP2_FAM" está marcado como "1 - Sim".
#### Campos relacionados às características do domicílio:
Os campos "QT_COMODOS_DOMIC_FAM", "QT_COMODOS_DORMITORIO_FAM",  "CO_MATERIAL_PISO_FAM", "CO_MATERIAL_DOMIC_FAM", "CO_AGUA_CANALIZADA_FAM", "CO_ABASTE_AGUA_DOMIC_FAM", "CO_BANHEIRO_DOMIC_FAM",  "CO_ESCOA_SANITARIO_DOMIC_FAM" e "CO_ILUMINACAO_DOMIC_FAM" só devem ser preenchidos no caso da resposta ter sido "1 - Particular permamente" no campo "CO_ESPECIE_DOMIC_FAM". Sendo assim, quando o campo "CO_ESPECIE_DOMIC_FAM" é diferente de 1, os valores dos campos relacionados às características do domicílio foi preenchido com **-1**.
#### Campo relacionado ao escoamento sanitário:
O campo  "CO_ESCOA_SANITARIO_DOMIC_FAM" só deve ser preenchido quando o campo "CO_BANHEIRO_DOMIC_FAM" é preenchido com "1 - Sim". Desta forma, quando o campo "CO_BANHEIRO_DOMIC_FAM" é igual a 2 (ou seja, não possui banheiro), o valor do campo "CO_ESCOA_SANITARIO_DOMIC_FAM" foi preenchido com **-1**.
#### Campo relacionado à família quilombola:
O campo "IN_FAMILIA_QUILOMBOLA_FAM" só deve ser preenchido caso o campo "IN_FAMILIA_INDIGENA_FAM" for "2 - Não". Assim, quando o campo "IN_FAMILIA_INDIGENA_FAM" é igual a 1 (ou seja, a família for indígena), o valor NaN do campo "IN_FAMILIA_QUILOMBOLA_FAM" foi marcado como **2**, ou seja, a família não é quilombola.
#### Valores vazios remanescentes
Após todas as limpezas descritas acima, foram excluídos os valores remanescentes Nan, passando a base de famílias de 9.847.475 para 9.776.551 linhas.

### Base pessoas

**Limpeza dos valores vazios**

Para alguns critérios de limpeza dos valores NaN da base de pessoas foi utilizada a informação da idade da pessoa. Assim, foi criada uma coluna "IDADE_REFERENCIA", calculada a partir da subtração da data 31/11/2025 (último dia da referência da base do Cadastro Único utilizada) pela coluna "DT_NASC_PESSOA", dividido por 365.25 (dias).

#### Campos relacionados ao trabalho
Os campos 'CO_TRABALHOU_SEMANA_MEMB', 'CO_AFASTADO_TRAB_MEMB', 'CO_AGRICULTURA_TRAB_MEMB', 'CO_PRINCIPAL_TRAB_MEMB', 'CO_TRABALHO_12_MESES_MEMB' só devem ser preenchidos para pessoas com 10 anos ou mais. Essa mudança aconteceu a partir de amrço/2022. Entretanto, observa-se uma quantidade muito alta de valores NaN desses campos para pessoas menores de 14 anos, conforme regra anterior.  Assim, para o caso de pessoas com idade menor que 14 anos, no campo "IDADE_REFERENCIA", os valores dos campos relacionados ao trabalho foram preenchidos com **-1**.
#### Campo relacionado ao afastamento do trabalho
O campo 'CO_AFASTADO_TRAB_MEMB' só deve ser respondido se o campo 'CO_TRABALHOU_SEMANA_MEMB' (Pessoa trabalhou na semana passada?) for respondido como "2 - Não". Assim, para os casos de resposta diferente de 2, os valores vazios do campo 'CO_AFASTADO_TRAB_MEMB' foram preenchidos com **-1**.
#### Campo relacionado à atividade extrativista e ao trabalho principal
O campo 'CO_AGRICULTURA_TRAB_MEMB' (É atividade extrativista? 1 - Sim e 2 - Não) e o campo 'CO_AGRICULTURA_TRAB_MEMB' só são obrigatórios se a pessoa respondeu 1 - Sim no campo 'CO_TRABALHOU_SEMANA_MEMB'(Pessoa trabalhou na semana passada?) ou no campo 'CO_AFASTADO_TRAB_MEMB' (Pessoa afastada na semana passada?). Assim, para o caso em que os 'CO_TRABALHOU_SEMANA_MEMB' e 'CO_AFASTADO_TRAB_MEMB' tiveram resposta "2 - Não" os valores vazios de 'CO_AGRICULTURA_TRAB_MEMB' e de 'CO_AGRICULTURA_TRAB_MEMB' foram substituídos por **-1**. 
#### Campo frequenta escola
Os campos  'CO_CURSO_FREQUENTA_MEMB' só deve ser preenchido se a pessoa respondeu "1 - Sim, rede pública", "2 - Sim, rede particular" no campo 'IN_FREQUENTA_ESCOLA_MEMB'. Desta forma, quando o campo 'IN_FREQUENTA_ESCOLA_MEMB' foi preenchido com valor diferente de 1 ou 2, os campos vazios de 'CO_CURSO_FREQUENTA_MEMB' foram preenchidos com **-1**.
#### Campo curso que a pessoa < 6 anos frequenta
Foi identificado que um percentual muito grande de pessoas menores de 6 anos estão com valores NaN para os campos 'CO_CURSO_FREQUENTA_MEMB' e 'CO_CURSO_FREQ_PESSOA_MEMB'. Assim, de modo a não excluir todas essas pessoas da base, quando a pessoa era menor de 6 anos de idade e respondeu "4 - Nunca frequentou" creche ou escola no campo 'IN_FREQUENTA_ESCOLA_MEMB', o campo 'CO_CURSO_FREQUENTA_MEMB' e 'CO_CURSO_FREQ_PESSOA_MEMB' foi marcado como **-1**, significando que não é nenhuma das respostas possíveis. 
#### Campo curso mais elevado que frequentou
O campo 'CO_CURSO_FREQ_PESSOA_MEMB' só deve ser respondido quando o campo 'IN_FREQUENTA_ESCOLA_MEMB' (Pessoa frequenta escola?) for respondido com "3 - Não, já frequentou". Assim, quando o campo 'IN_FREQUENTA_ESCOLA_MEMB' foi diferente de 3, os valores vazios do campo 'CO_CURSO_FREQ_PESSOA_MEMB' foram preenchidos com **-1**.
#### Valores vazios remanescentes
Foram feitas diversas análises adicionais para identificar possíveis ajustes nos valores vazios finais. Entretanto, não foi identificada outra situação de valor vazio que parece estar relacionado à regra de preenchimento do Cadastro Único. Desta forma, as linhas com valores vazios remanescentes foram excluídos.

**Condensando os campos de pessoas em famílias**

De modo a garantir que cada família ocupe apenas uma linha do Dataframe final utilizado para o desenvolvimento do ML, as características das pessoas foram agrupadas, da forma especificada abaixo:
#### PCT_1_INFANCIA - Percentual de crianças na primeira infância
Foi calculado o percentual de pessoas em uma mesma família que estão na primeira infância, ou seja, que possuem 6 anos ou menos de idade, de modo a tentar identificar se a necessidade de cuidado está relacionada à renda.
#### PCT_CRIANCAS_7A11 - Percental de crianças
Foi calculado o percentual de pessoas em uma mesma família que são crianças, ou seja, estão entre 7 e 11 anos de idade.
#### PCT_ADOLESCENTES_12A18 - Percentual de adolescentes
Foi calculado o percentual de pessoas em uma mesma família que são adolescentes, ou seja, estão entre 12 e 18 anos de idade.
#### PCT_JOVENS_19A29 - Percentual de jovens
Foi calculado o percentual de pessoas em uma mesma família que são jovens, ou seja, estão entre 19 e 29 anos.
#### PCT_ADULTOS_30A59 - Percentual de adultos
Foi calculado o percentual de pessoas em uma mesma família que são adultos, ou seja, estão entre 30 e 59 anos.
#### PCT_IDOSOS_60A64 - Percentual de idosos
Foi calculado o percentual de pessoas em uma mesma família que são idosos e não estão na faixa etária de concessão do Benefício de Prestação Continuada (BPC).
#### PCT_IDOSOS_BPC - Percentual de idosos com 65 anos ou mais
Foi calculado o percentual de pessoas em uma mesma família que estão na faixa etária da concessão do BPC, para avalair se esse perfil contribui para uma alteração na renda.
#### TEM_CRIANCA_SEM_ESCOLA - Marcação tem criança fora da escola
Foi criado um marcador para identificar as famílias que possuem (=1) pelo menos uma criança entre 7 e 11 anos fora da escola, ou seja, 'IN_FREQUENTA_ESCOLA_MEMB' = 3.
#### TEM_ADOLESCENTE_SEM_ESCOLA - Marcação tem adolescente fora da escola
Foi criado um marcador para identificar as famílias que possuem (=1) pelo menos um adolescente entre 12 e 18 anos fora da escola, ou seja, 'IN_FREQUENTA_ESCOLA_MEMB' = 3.
#### PCT_PES_NAO_ANALFABETIZADA - Percentual de pessoas não alfabetizadas
Foi calculado o percentual de pessoas acima de 10 anos que marcaram "2 - não", no campo "CO_SABE_LER_ESCREVER_MEMB".
#### PCT_ADULTO_NUNCA_FREQ_ESCOLA - Percentual de adultos que nunca frequentaram escola
Foi calculado o percentual de pessoas acima de 18 anos que marcaram "4 - Nunca frequentou" no campo 'IN_FREQUENTA_ESCOLA_MEMB'.
#### PCT_7A18_ESCOLA_PUBLICA - Percentual de crianças e adolescentes em escola pública
Foi calculado o percentual de pessoas entre 7 e 18 anos que marcaram "1 - Sim, escola pública no campo 'IN_FREQUENTA_ESCOLA_MEMB'.
#### PCT_MENOR6_FORA_CRECHE_PRE - Percentual de crianãs da 1ª infância fora da creche ou pré-escola
Foi calculado o percentual de pessoas menores de 6 anos que marcaram "3 - Não, mas já frequentou" ou "4 - Nunca frequentou" no campo 'IN_FREQUENTA_ESCOLA_MEMB'.
#### PCT_PES_DEFICIENCIA - Percentual de pessoas com deficiência na família
Foi calculado o percentual de pessoas na família com marcação do campo 'CO_DEFICIENCIA_MEMB' igual a "1 - Sim".

**Dados do Responsável Familiar**

A coluna 'CO_PARENTESCO_RF_PESSOA' foi filtrada para apresentar apenas as linhas com resultado "1 - Pessoa responsável pela unidade familiar" de modo a permitir que as características das pessoas que não estavam condensadas remanescentes, fossem relacionadas ao responsável familiar.

## Justificativa metodológica: classificação binária 
A opção metodológica por um modelo de classificação binária, distinguindo famílias com renda domiciliar per capita até ½ salário mínimo daquelas acima desse limiar, está alinhada tanto ao desenho das políticas sociais brasileiras quanto às evidências da literatura internacional sobre testes de meios baseados em aprendizado de máquina. Estudos recentes demonstram que abordagens binárias tendem a ser mais estáveis e interpretáveis do que classificações multiclasses ou ordinais quando o objetivo é triagem administrativa e priorização de ações, especialmente em contextos de renda informal (ALTMAN et al., 2025; HARTWIG et al., 2025).

A literatura de focalização da pobreza destaca que métricas agregadas, como acurácia global, podem ser enganosas, uma vez que modelos com bom desempenho médio podem falhar justamente na identificação dos casos mais relevantes para a política pública (ELBERS et al., 2007; RAVALLION, 2016). Por esse motivo, o presente projeto prioriza o bom desempenho na identificação das famílias com maior probabilidade de renda acima de ½ salário mínimo, buscando alta precisão (precision) para esse grupo, de modo a reduzir o número de falsos positivos e evitar sobrecarga operacional das gestões municipais responsáveis pela qualificação cadastral.

Essa escolha é respaldada por trabalhos como A New Era in Poverty Diagnostics, que enfatizam o uso de thresholds mais conservadores para produzir listas menores e mais confiáveis quando o custo de verificação é elevado (ALTMAN et al., 2025), bem como por estudos da OCDE e do IPEA, que ressaltam a importância de alinhar modelos preditivos às capacidades institucionais de implementação, evitando estratégias que ampliem excessivamente o número de casos a serem verificados (OECD, 2018; IPEA, 2019).

Além disso, a exclusão de variáveis diretamente associadas à concessão de benefícios segue a recomendação recorrente da literatura sobre testes de meios e targeting, que alerta para o risco de vazamento de informação e de perda de utilidade prática do modelo quando se utilizam proxies muito próximos das regras institucionais vigentes (ALTMAN et al., 2025; IPEA, 2019). Dessa forma, o modelo é treinado exclusivamente com características socioeconômicas observáveis, reforçando seu papel como instrumento complementar de apoio à gestão, e não como mecanismo automático de decisão.

## Variáveis utilizadas no ML

**Variável alvo**: y_bin - nova coluna criada a partir do valor da renda média familiar (renda per capita) sendo 1 > 706 e 0 diferente desse valor.

**Variáveis explicativas**: 'IN_TRABALHO_INFANTIL_FAM', 'CO_MUNIC_IBGE_2_FAM', 'CO_MUNIC_IBGE_5_FAM', 'IN_FORMULARIO_SUP2_FAM', 'QT_PESSOAS_DOMIC_FAM', 'QT_FAMILIAS_DOMIC_FAM', 'CO_ESPECIE_DOMIC_FAM', 'CO_LOCAL_DOMIC_FAM', 'QT_COMODOS_DOMIC_FAM', 'QT_COMODOS_DORMITORIO_FAM', 'CO_MATERIAL_DOMIC_FAM', 'CO_MATERIAL_PISO_FAM', 'CO_AGUA_CANALIZADA_FAM', 'CO_ABASTE_AGUA_DOMIC_FAM', 'CO_BANHEIRO_DOMIC_FAM', 'CO_ESCOA_SANITARIO_DOMIC_FAM', 'CO_ILUMINACAO_DOMIC_FAM', 'IN_FAMILIA_INDIGENA_FAM', 'IN_FAMILIA_QUILOMBOLA_FAM', 'IN_PARC_MDS_FAM', 'CO_EST_CADASTRAL_MEMB', 'CO_SEXO_PESSOA', 'IDADE_REFERENCIA', 'CO_RACA_COR_PESSOA', 'CO_DEFICIENCIA_MEMB', 'CO_SABE_LER_ESCREVER_MEMB',        'IN_FREQUENTA_ESCOLA_MEMB', 'CO_CURSO_FREQUENTA_MEMB', 'CO_CURSO_FREQ_PESSOA_MEMB', 'CO_TRABALHOU_SEMANA_MEMB', 'CO_AFASTADO_TRAB_MEMB', 'CO_AGRICULTURA_TRAB_MEMB', 'CO_PRINCIPAL_TRAB_MEMB', 'CO_TRABALHO_12_MESES_MEMB', 'QTD_PESSOAS', 'PCT_1_INFANCIA', 'PCT_CRIANCAS_7A11', 'PCT_ADOLESCENTES_12A18', 'PCT_JOVENS_19A29', 'PCT_ADULTOS_30A59', 'PCT_IDOSOS_60A64', 'PCT_IDOSOS_BPC', 'PCT_PES_DEFICIENCIA', 'TEM_CRIANCA_SEM_ESCOLA', 'TEM_ADOLESCENTE_SEM_ESCOLA', 'PCT_PES_ANALFABETA','PCT_ADULTO_NUNCA_FREQ_ESCOLA', 'PCT_7A18_ESCOLA_PUBLICA', 'PCT_MENOR6_FORA_CRECHE_PRE'

### Análise de correlação das variáveis
A correlação de Pearson foi utilizada para avaliar a associação linear entre variáveis quantitativas contínuas e discretas derivadas do Cadastro Único, permitindo identificar relações de dependência linear elevada entre atributos que expressam dimensões semelhantes da composição familiar, escolaridade e densidade habitacional. 

A identificação de pares de variáveis com correlação absoluta elevada (|r| ≥ 0,85) foi empregada como diagnóstico de redundância informacional, com o objetivo de reduzir o tamanho do conjunto de atributos, melhorar a estabilidade do modelo e aumentar sua interpretabilidade, especialmente considerando o uso operacional do modelo para apoio à qualificação cadastral. 

A remoção ou consolidação de variáveis altamente correlacionadas contribui para mitigar efeitos de multicolinearidade, reduzir ruído estatístico e fortalecer a explicabilidade dos resultados. 

Abaixo, segue um mapa de calor da matriz de correlação de Pearson entre as variáveis quantitativas utilizadas no modelo, permitindo identificar relações lineares fortes e potenciais redundâncias informacionais entre os atributos analisados. Destaca-se que, como nenhuma variável 



## Resultados
Após treinamento
Entre os modelos avaliados, o XGBoost apresentou o melhor desempenho global para o objetivo de triagem proposto, alcançando o maior valor de PR-AUC (0,86) e mantendo elevada precisão sob limiar de decisão mais restritivo. Esse resultado indica maior capacidade de ordenar as famílias por risco de inconsistência cadastral, permitindo a priorização de ações de qualificação com controle da carga operacional. Embora CatBoost e HistGradientBoosting tenham apresentado desempenhos próximos, o XGBoost mostrou ligeira superioridade no equilíbrio entre sensibilidade e precisão, sendo, portanto, selecionado como modelo final do estudo.

## Referências bibliográficas 

ALTMAN, M.; ARDINGTON, C.; WEGNER, E.; ZANZIBAR, N. A new era in poverty diagnostics: experiential knowledge from machine learning to improve the efficiency of social programs in combating poverty. Washington, DC: World Bank, 2025.

ELBERS, C.; LANJOUW, J. O.; LANJOUW, P. Imputing poverty: a review of methods and applications. Journal of Economic Inequality, v. 1, n. 2, p. 161–189, 2003.

ELBERS, C.; FUJII, T.; Lanjouw, P.; ÖZLER, B.; YIN, W. Poverty alleviation through geographic targeting: how much does disaggregation help? Journal of Development Economics, v. 83, n. 1, p. 198–213, 2007.

HARTWIG, F.; SCHRÖDER, C.; SCHULZ, B. Machine learning with administrative data for energy poverty identification in the UK. Energy Policy, v. 185, 2025.

INSTITUTO DE PESQUISA ECONÔMICA APLICADA (IPEA). Limitações de um teste de meios via predição de renda no contexto brasileiro. Brasília: IPEA, 2019.

ORGANISATION FOR ECONOMIC CO-OPERATION AND DEVELOPMENT (OECD). Modernising access to social protection: strategies, technologies and data advances in OECD countries. Paris: OECD Publishing, 2018.

RAVALLION, M. Retooling poverty targeting using a capability approach. World Bank Research Observer, v. 31, n. 2, p. 263–292, 2016.
