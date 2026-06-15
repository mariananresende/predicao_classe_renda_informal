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
 

## Variáveis utilizadas em modelos de Machine Learning de predição de renda 
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

## Variáveis do Cadastro Único utilizadas para o desenvolvimento do ML
De modo a utilizar no desenvolvimento do Modelo de Machine Learning as variáveis levantadas na revisão bibliográfica, foram utilizadas as seguintes variáveis do Cadastro Único, como características explicativas do modelo, com exceção da renda domiciliar per capita, que é utilizada apenas para a construção do alvo durante o treinamento. Nenhuma informação diretamente relacionada à elegibilidade a benefícios sociais é utilizada como entrada do modelo, de modo a evitar vazamento de informação:

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

Os dados foram acessados via DBeaver com conexão ao banco Teradata, exclusivamente por VPN institucional, sem extração local de dados identificados. Um script SQL gerou a base já anonimizada, aplicando as limpezas e a condensação ao nível familiar. O acesso seguiu o protocolo recomendado (Portal gov.br) e o respectivo Termo de Compromisso de Manutenção de Sigilo, em conformidade com a LGPD. Cabe destacar que, embora o povoamento do CNIS atualize também rendas de aposentadoria e BPC, apenas a origem vinculada à renda do trabalho foi utilizada como critério de seleção: a inclusão de aposentadorias elevaria a proporção de famílias com idosos, criando o risco de o modelo aprender padrões associados à presença de idosos, e não às demais características socioeconômicas de interesse.

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

Para o treinamento foi utilizada uma amostra de aproximadamente 1 milhão de famílias, construída de forma proporcional à distribuição real das classes de renda (≈41,5% até ½ SM e 58,5% acima). Amostras artificialmente balanceadas foram testadas, mas apresentaram desempenho inferior, com menor estabilidade e calibração. Avaliou-se ainda uma estratificação pela composição familiar — motivada pela diferença entre o Cadastro Único (25,1% de casais; 40,9% de responsáveis do sexo feminino sem cônjuge) e os Censos do IBGE —, mas a imposição de proporções fixas degradou o desempenho e foi descartada, ficando o tema indicado para investigações futuras.

**Variável alvo**: variável binária construída a partir da renda domiciliar per capita formal observada no CNIS, sendo definida como 1 quando o valor é superior a R$ 759 (½ salário mínimo) e 0 caso contrário.

Durante o treinamento, essa variável representa a classe real de renda formalmente observada. Na aplicação do modelo a famílias sem renda formal registrada, o valor previsto de y_bin deve ser interpretado como a probabilidade de a renda efetivamente auferida pela família ser superior à renda declarada no Cadastro Único, e não como uma medida direta de renda.

**Variáveis explicativas**: 'IN_TRABALHO_INFANTIL_FAM', 'CO_MUNIC_IBGE_2_FAM', 'CO_MUNIC_IBGE_5_FAM', 'IN_FORMULARIO_SUP2_FAM', 'QT_PESSOAS_DOMIC_FAM', 'QT_FAMILIAS_DOMIC_FAM', 'CO_ESPECIE_DOMIC_FAM', 'CO_LOCAL_DOMIC_FAM', 'QT_COMODOS_DOMIC_FAM', 'QT_COMODOS_DORMITORIO_FAM', 'CO_MATERIAL_DOMIC_FAM', 'CO_MATERIAL_PISO_FAM', 'CO_AGUA_CANALIZADA_FAM', 'CO_ABASTE_AGUA_DOMIC_FAM', 'CO_BANHEIRO_DOMIC_FAM', 'CO_ESCOA_SANITARIO_DOMIC_FAM', 'CO_ILUMINACAO_DOMIC_FAM', 'IN_FAMILIA_INDIGENA_FAM', 'IN_FAMILIA_QUILOMBOLA_FAM', 'IN_PARC_MDS_FAM', 'CO_EST_CADASTRAL_MEMB', 'CO_SEXO_PESSOA', 'IDADE_REFERENCIA', 'CO_RACA_COR_PESSOA', 'CO_DEFICIENCIA_MEMB', 'CO_SABE_LER_ESCREVER_MEMB', 'IN_FREQUENTA_ESCOLA_MEMB', 'CO_CURSO_FREQUENTA_MEMB', 'CO_CURSO_FREQ_PESSOA_MEMB', 'CO_TRABALHOU_SEMANA_MEMB', 'CO_AFASTADO_TRAB_MEMB', 'CO_AGRICULTURA_TRAB_MEMB', 'CO_PRINCIPAL_TRAB_MEMB', 'CO_TRABALHO_12_MESES_MEMB', 'QTD_PESSOAS', 'PCT_1_INFANCIA', 'PCT_CRIANCAS_7A11', 'PCT_ADOLESCENTES_12A18', 'PCT_JOVENS_19A29', 'PCT_ADULTOS_30A59', 'PCT_IDOSOS_60A64', 'PCT_IDOSOS_BPC', 'PCT_PES_DEFICIENCIA', 'TEM_CRIANCA_SEM_ESCOLA', 'TEM_ADOLESCENTE_SEM_ESCOLA', 'PCT_PES_ANALFABETA','PCT_ADULTO_NUNCA_FREQ_ESCOLA', 'PCT_7A18_ESCOLA_PUBLICA', 'PCT_MENOR6_FORA_CRECHE_PRE'

### Análise de correlação das variáveis

#### Variáveis quantitativas contínuas e discretas

A correlação de Pearson foi utilizada para avaliar a associação linear entre variáveis quantitativas contínuas e discretas derivadas do Cadastro Único, permitindo identificar relações de dependência linear elevada entre atributos que expressam dimensões semelhantes da composição familiar, escolaridade e densidade habitacional. 

A identificação de pares de variáveis com correlação absoluta elevada (|r| ≥ 0,85) foi empregada como diagnóstico de redundância informacional, com o objetivo de reduzir o tamanho do conjunto de atributos, melhorar a estabilidade do modelo e aumentar sua interpretabilidade, especialmente considerando o uso operacional do modelo para apoio à qualificação cadastral. 

A remoção ou consolidação de variáveis altamente correlacionadas contribui para mitigar efeitos de multicolinearidade, reduzir ruído estatístico e fortalecer a explicabilidade dos resultados. 

Abaixo, segue um mapa de calor da matriz de correlação de Pearson entre as variáveis quantitativas utilizadas no modelo, permitindo identificar relações lineares fortes e potenciais redundâncias informacionais entre os atributos analisados. Destaca-se que como nenhuma variável apresentou limiar de correlação maior que 0.80, nenhuma variável quantitativa foi retirada para o desenvolvimento do modelo.

<img width="983" height="614" alt="image" src="https://github.com/user-attachments/assets/2e5b491b-a68a-47c7-adef-34b18cc14790" />


#### Variáveis categóricas 

Para avaliar a associação e possível redundância informacional entre variáveis categóricas utilizadas no modelo, foi adotada a associação de Cramér (Cramér’s V). Diferentemente do coeficiente de correlação de Pearson, apropriado para variáveis quantitativas contínuas, o Cramér’s V é indicado para mensurar a intensidade da associação entre variáveis nominais ou categóricas, independentemente do número de categorias envolvidas.

A estatística de Cramér é derivada do teste qui-quadrado de independência e assume valores no intervalo [0,1], em que valores próximos de 0 indicam ausência de associação e valores próximos de 1 indicam associação forte entre as variáveis. Essa característica permite identificar pares de variáveis categóricas altamente associadas, que podem representar informação redundante no modelo.

A identificação de colinearidade entre variáveis categóricas é particularmente relevante em modelos de aprendizado de máquina aplicados a dados administrativos, como o Cadastro Único, que apresentam grande número de atributos discretos relacionados a condições do domicílio, escolaridade, ocupação e características individuais. Variáveis fortemente associadas podem aumentar a complexidade do modelo sem ganho informacional, dificultar a interpretação dos resultados e, em alguns algoritmos, contribuir para instabilidade na estimação.

Dessa forma, a utilização do Cramér’s V permitiu orientar decisões de seleção e agrupamento de variáveis categóricas, contribuindo para a construção de um conjunto de atributos mais parcimonioso, interpretável e alinhado aos objetivos do modelo de triagem de inconsistências de renda.


<img width="714" height="714" alt="image" src="https://github.com/user-attachments/assets/26e968b9-b4af-4c38-95ea-512bfbb3fe27" />



Considerando que as variáveis abaixo apresentaram limiar de associação acima de 0.80, as seguintes variáveis foram excluídas: 'CO_TRABALHOU_SEMANA_MEMB', 'CO_AGRICULTURA_TRAB_MEMB', 'IN_FAMILIA_INDIGENA_FAM'.


<img width="525" height="104" alt="image" src="https://github.com/user-attachments/assets/f6d67785-72c7-45fe-84c5-306c92fe4f2c" />



## Pré-processamento das variáveis

A base analítica é carregada já pronta a partir do script SQL, que realiza toda a engenharia de variáveis — limpeza dos valores ausentes conforme as regras de preenchimento do Cadastro Único (preenchimento lógico com o código **-1** para "não se aplica" e exclusão das linhas com ausentes remanescentes) e condensação das informações do nível de pessoa para o nível de família. **Não é aplicada imputação de valores ausentes em nenhuma etapa**: a base chega ao ambiente de modelagem completa e sem valores nulos.

No ambiente de modelagem, aplica-se apenas um pré-processamento estatístico leve, necessário para adequar as escalas das variáveis aos algoritmos. As variáveis quantitativas contínuas são normalizadas por escalonamento Min–Max, assegurando comparabilidade entre atributos com diferentes ordens de grandeza, e as categóricas multicategóricas são codificadas via one-hot encoding, preservando a informação sem impor ordens artificiais entre categorias. As variáveis percentuais e booleanas, por já se encontrarem em escalas compatíveis, as binárias e as geográficas — estas últimas para evitar a criação excessiva de colunas — são mantidas em sua forma original (passthrough).

Esse pré-processamento é encapsulado em um ColumnTransformer, assegurando reprodutibilidade e coerência entre as fases de treinamento e aplicação do modelo.

## Treinamento e comparação de modelos de classificação

Para o desenvolvimento do modelo de classificação, foram treinados e comparados diferentes algoritmos de aprendizado de máquina amplamente utilizados em problemas de classificação supervisionada, contemplando abordagens lineares, baseadas em árvores e métodos de ensemble. 

A Regressão Logística foi incluída como modelo de referência por sua interpretabilidade e robustez em cenários lineares. 

Modelos baseados em árvores, como Random Forest, Gradient Boosting e HistGradientBoosting, foram avaliados por sua capacidade de capturar relações não lineares e interações complexas entre variáveis socioeconômicas e domiciliares. 

Adicionalmente, foram testados modelos de gradient boosting de alto desempenho, XGBoost e CatBoost, reconhecidos na literatura por sua eficiência em dados tabulares, tolerância a diferentes tipos de variáveis e bom desempenho em cenários com desbalanceamento de classes. 

A comparação entre esses modelos permitiu avaliar o compromisso entre desempenho preditivo, estabilidade das probabilidades estimadas e adequação ao uso final do modelo como instrumento de triagem operacional, com foco na identificação de famílias com maior risco de inconsistência de renda.

## Comparação dos modelos

Os resultados da comparação entre os modelos indicam desempenho superior dos métodos de gradient boosting, com destaque para o XGBoost, que apresentou os maiores valores de PR-AUC e ROC-AUC, evidenciando melhor capacidade de discriminação entre famílias com e sem risco de inconsistência de renda. 

No cenário de classificação padrão (threshold = 0,50), o XGBoost obteve o maior equilíbrio entre precisão e revocação da classe positiva, refletido no maior F1-score, enquanto CatBoost e HistGradientBoosting apresentaram desempenhos muito próximos. 

Ao aplicar um threshold mais restritivo (0,80), voltado ao uso operacional do modelo como ferramenta de triagem, observou-se aumento substancial da precisão da classe positiva em todos os modelos, com redução esperada da revocação e da taxa de convocação. 

Nesse contexto, o XGBoost manteve o melhor compromisso entre precisão elevada, volume de casos sinalizados e desempenho global, mostrando-se mais adequado ao objetivo do projeto de apoiar ações focalizadas de qualificação cadastral sem sobrecarregar a gestão municipal.

## Análise aprofundada dos modelos com melhor desempenho

Após a avaliação inicial dos modelos, foi realizada uma análise mais aprofundada concentrada nos algoritmos com melhor desempenho global — XGBoost, CatBoost e HistGradientBoosting. 

Essa etapa teve como objetivo examinar com maior detalhe o comportamento dos modelos sob diferentes limiares de decisão, comparando o desempenho no threshold padrão (0,50) e em um threshold mais restritivo (0,80), compatível com o uso operacional do modelo como instrumento de triagem. 

A análise considerou métricas por classe, matriz de confusão e a taxa de convocação gerada, permitindo avaliar explicitamente o trade-off entre precisão, revocação e volume de casos sinalizados. 

Os resultados indicam que, embora os três modelos apresentem desempenho semelhante, o XGBoost mantém o melhor equilíbrio entre precisão elevada e maior capacidade de identificar famílias com potencial inconsistência de renda, enquanto o CatBoost apresenta ligeira vantagem em termos de precisão absoluta, com menor taxa de convocação. 

Essa análise subsidiou a escolha do modelo mais adequado ao objetivo do projeto, priorizando estabilidade, transparência e viabilidade operacional das ações de qualificação cadastral.

## Modelo selecionado - XGBoost

A escolha do XGBoost como modelo final se justifica pelo seu desempenho superior e mais estável no seu cenário de triagem operacional: entre os modelos avaliados, ele apresentou os melhores valores de PR-AUC e ROC-AUC (melhor capacidade de separação global) e manteve o melhor compromisso entre precisão alta e revocação suficiente quando aplicado um limiar mais restritivo (threshold = 0,80), que é exatamente o tipo de decisão necessária para priorizar convocações sem sobrecarregar as gestões municipais. Na prática, isso significa que o XGBoost oferece probabilidades mais úteis para ordenar famílias por risco e, ao mesmo tempo, permite controlar o volume de casos sinalizados via threshold, alinhando o modelo ao objetivo do projeto de identificar potenciais inconsistências entre renda declarada e renda provável inferida.

Os parâmetros adotados foram escolhidos para favorecer generalização e calibragem do score em dados tabulares heterogêneos. O par n_estimators=1200 e learning_rate=0.03 segue uma estratégia comum de boosting: usar muitas árvores com taxa de aprendizado menor, o que tende a produzir fronteiras mais suaves e reduzir instabilidades, especialmente quando há variáveis categóricas codificadas e múltiplos sinais fracos combinados. O max_depth=6 controla a complexidade das árvores, permitindo capturar interações relevantes (por exemplo, composição familiar × escolaridade × condições do domicílio) sem criar árvores excessivamente específicas. Os parâmetros subsample=0.8 e colsample_bytree=0.8 introduzem aleatoriedade no treinamento (linhas e colunas), reduzindo overfitting e tornando o modelo mais robusto a variações entre “famílias com CNIS” (treino) e “famílias sem CNIS” (aplicação). O objective="binary:logistic" é adequado ao problema binário (≤ ½ SM vs > ½ SM), enquanto eval_metric="logloss" favorece probabilidades bem comportadas (úteis para triagem por score). Por fim, tree_method="hist" melhora o desempenho computacional em bases grandes, e random_state/n_jobs garantem reprodutibilidade e eficiência.

A lógica do threshold_pos=0.80 foi incorporada explicitamente na classe XGBoostComThreshold porque o uso final é decisão de triagem, não apenas acurácia. Ao elevar o limiar de decisão, o modelo passa a sinalizar como “classe positiva” apenas os casos com maior probabilidade estimada, o que aumenta a precisão e reduz falsos positivos — crucial quando cada falso positivo implica uma convocação desnecessária e custo operacional no município. Assim, o threshold transforma o classificador em um mecanismo de priorização transparente e ajustável, permitindo calibrar o volume de convocações de acordo com a capacidade de execução e a política de risco adotada.

## Desempenho do modelo selecionado

Com o limiar de decisão ajustado para 0,80, o modelo foi configurado para atuar como instrumento de triagem, priorizando alta precisão na identificação de famílias com maior probabilidade de inconsistência entre a renda declarada e a renda provável. Nesse regime, o modelo apresenta desempenho consistente, com baixo risco de falsos positivos e volume controlado de convocações, alinhado à capacidade operacional das gestões municipais.

| Métrica                     | Valor      |
| --------------------------- | ---------- |
| Acurácia                    | **0.6204** |
| Precisão (classe positiva)  | **0.8925** |
| Revocação (classe positiva) | **0.3615** |
| F1-score (classe positiva)  | **0.5146** |
| Taxa de convocação          | **22,56%** |

A elevada precisão indica que a maior parte das famílias sinalizadas pelo modelo apresenta, de fato, maior risco de inconsistência de renda, enquanto a taxa de convocação moderada assegura que o modelo possa ser utilizado como apoio à priorização de ações de qualificação cadastral, sem gerar sobrecarga excessiva aos municípios.

Ressalta-se, contudo, que o limiar de decisão para convocação é um parâmetro ajustável, podendo ser definido conforme a estratégia e a capacidade operacional da gestão. A adoção de limiares mais baixos tende a ampliar a captação de famílias potencialmente acima do limite de renda, aumentando a sensibilidade do modelo, ainda que ao custo de maior volume de convocações e, consequentemente, maior carga de trabalho para os entes municipais.

### Matriz de confusão

<img width="549" height="412" alt="image" src="https://github.com/user-attachments/assets/635042ca-4688-44ca-9b36-c29f34351a2f" />

A matriz de confusão evidencia o comportamento do modelo no regime de triagem com threshold = 0,80. 

Observa-se que a maior parte das famílias classificadas como "> ½ salário mínimo" corresponde de fato a casos positivos (40.277 verdadeiros positivos), enquanto o número de falsos positivos é relativamente reduzido (4.851), refletindo a alta precisão do modelo. 

Por outro lado, há um volume expressivo de falsos negativos (71.140), ou seja, famílias acima de ½ salário mínimo que não foram sinalizadas, o que é esperado dado o limiar conservador adotado.

Esse padrão confirma que o modelo foi intencionalmente calibrado para priorizar a confiabilidade dos casos convocados, reduzindo o risco de sobrecarga operacional nos municípios, ainda que isso implique menor sensibilidade na detecção de todos os casos potenciais.

### Curva ROC-AUC

<img width="535" height="445" alt="image" src="https://github.com/user-attachments/assets/0e6f084a-c956-476f-ac2c-dbcb323a0fcf" />

A curva ROC evidencia boa capacidade discriminatória do modelo XGBoost, com ROC-AUC de 0,826, indicando que o modelo consegue distinguir de forma consistente famílias com maior e menor risco de inconsistência de renda ao longo de diferentes limiares de decisão. 

A curva posiciona-se bem acima da linha aleatória, o que demonstra que, para uma ampla faixa de taxas de falsos positivos, o modelo mantém taxas relativamente elevadas de verdadeiros positivos, reforçando sua adequação como instrumento de triagem e priorização no processo de qualificação cadastral.

### Trade-off entre precisão, revocação e F1-score por threshold

<img width="648" height="438" alt="image" src="https://github.com/user-attachments/assets/8c049008-c8a1-4c56-b106-087709e935b1" />

O gráfico evidencia o comportamento clássico de trade-off na classificação binária: à medida que o threshold de decisão aumenta, a precisão cresce de forma consistente, indicando que as famílias convocadas apresentam maior probabilidade real de estarem acima de ½ salário mínimo per capita. 

Em contrapartida, a revocação diminui progressivamente, refletindo a perda de capacidade do modelo em capturar todos os casos positivos. 

O F1-score atinge seus melhores valores em thresholds intermediários, sintetizando esse equilíbrio entre precisão e revocação. 

Esse resultado reforça que a escolha do limiar não é puramente técnica, mas depende do objetivo operacional da política pública.

### Taxa de convocação por threshold

<img width="631" height="351" alt="image" src="https://github.com/user-attachments/assets/66e2cc93-71c0-490d-af7f-a14944ee5d1f" />

O segundo gráfico mostra que o aumento do threshold reduz significativamente a proporção de famílias convocadas para ações de qualificação cadastral. 

Em thresholds mais baixos, o modelo sinaliza um volume elevado de famílias, o que pode gerar sobrecarga operacional para os municípios. 

Já em thresholds mais altos, a taxa de convocação torna-se mais restrita, concentrando esforços em casos com maior probabilidade de inconsistência de renda. 

Assim, o gráfico ilustra de forma clara como o threshold funciona como um parâmetro de governança, permitindo ajustar o equilíbrio entre capacidade operacional e alcance da identificação de famílias potencialmente acima do limite de renda.

### Curva de calibração

<img width="625" height="435" alt="image" src="https://github.com/user-attachments/assets/cee30c27-0bed-4b25-8267-be5d516fa33b" />

O gráfico de curva de calibração indica que o modelo apresenta boa calibração das probabilidades previstas, uma vez que a curva observada permanece muito próxima da linha ideal (y = x) ao longo de todo o intervalo. 

As diferenças entre a probabilidade média prevista e a frequência observada são pequenas e próximas de zero na maioria dos bins. 

Por exemplo, no bin com probabilidade média prevista de 0,46, a frequência observada foi 0,47, com diferença praticamente nula (+0,007), enquanto no bin em torno de 0,65, a diferença foi de apenas -0,0011. 

Mesmo nos extremos, os desvios permanecem baixos, como no primeiro bin (0,0617 previsto vs. 0,0480 observado; −0,0137) e no último (0,943 previsto vs. 0,947 observado; +0,004). 

Esses resultados indicam que as probabilidades geradas pelo modelo são consistentes e confiáveis para apoiar decisões baseadas em limiares, reforçando a adequação do uso de thresholds operacionais para priorização de ações de qualificação cadastral.

## Importância das variáveis

A explicabilidade é um elemento central para o uso responsável de modelos de aprendizado de máquina em políticas públicas, especialmente quando os resultados subsidiam decisões administrativas que podem afetar o acesso das famílias a programas sociais. A análise das variáveis que mais contribuem para a classificação permite compreender quais características socioeconômicas e do domicílio estão associadas às previsões do modelo, aumentando a transparência, a confiança institucional e a possibilidade de validação técnica e substantiva dos resultados. Além disso, a explicabilidade é fundamental para identificar eventuais vieses, apoiar ajustes metodológicos e facilitar o diálogo com gestores e equipes responsáveis pela qualificação cadastral.

<img width="714" height="410" alt="image" src="https://github.com/user-attachments/assets/c93e9df0-63c9-4fe7-bb7e-a5b0a631a6a5" />

A análise de importância das variáveis do modelo XGBoost evidencia que a classificação se apoia principalmente em três dimensões: características do responsável familiar, composição da família e condições do domicílio. As variáveis que não possuem o prefixo PCT e estão relacionadas à escolaridade, inserção no trabalho e idade — como CO_AFASTADO_TRAB_MEMB, CO_CURSO_FREQ_PESSOA_MEMB, CO_PRINCIPAL_TRAB_MEMB e CO_TRABALHO_12_MESES_MEMB — refletem atributos do responsável familiar, indicando que o modelo captura sinais de estabilidade ocupacional, nível educacional e ciclo de vida associados à renda provável do domicílio.

As variáveis iniciadas por PCT representam a composição da família, sintetizando a distribuição etária e educacional dos membros, com destaque para o percentual de adolescentes em escola pública (PCT_7A18_ESCOLA_PUBLICA), de adultos em idade ativa (PCT_ADULTOS_30A59) e a presença de crianças e adolescentes. Complementarmente, variáveis relacionadas às condições do domicílio, como material do piso e quantidade de cômodos, também contribuem para a classificação, funcionando como proxies de bem-estar material. Em conjunto, esses resultados indicam que o modelo aprende padrões socioeconômicos coerentes e interpretáveis, alinhados ao objetivo de apoiar a qualificação cadastral.

### Detalhamento - PCT_7A18_ESCOLA_PUBLICA

<img width="912" height="355" alt="image" src="https://github.com/user-attachments/assets/66cd88c3-2d74-4719-8e7a-450316f0a674" />

O gráfico mostra uma relação inversa clara entre o percentual de adolescentes de 7 a 18 anos matriculados em escola pública (PCT_7A18_ESCOLA_PUBLICA) e a probabilidade de a família pertencer à classe com renda per capita acima de ½ salário mínimo. À medida que o percentual de adolescentes em escola pública aumenta, cresce a proporção de famílias classificadas como ≤ ½ SM, enquanto diminui a proporção da classe > ½ SM. Esse padrão é consistente com a literatura e com o contexto do Cadastro Único, indicando que a maior dependência da rede pública de ensino funciona como um proxy de menor renda e maior vulnerabilidade socioeconômica. 

### Detalhamento - PCT_ADULTOS_30A59

<img width="906" height="346" alt="image" src="https://github.com/user-attachments/assets/b9671f4d-5082-4870-af0e-244194379ad1" />

O gráfico mostra uma relação clara entre a composição etária da família e a renda per capita. À medida que aumenta o percentual de adultos entre 30 e 59 anos no domicílio, cresce de forma consistente a proporção de famílias classificadas com renda acima de ½ salário mínimo, enquanto diminui a participação das famílias com renda ≤ ½ salário mínimo. Esse padrão é coerente do ponto de vista socioeconômico, pois domicílios com maior concentração de adultos em idade potencialmente ativa tendem a apresentar maior capacidade de inserção no mercado de trabalho e geração de renda, o que explica a forte contribuição dessa variável para a classificação do modelo.

### Detalhamento - CO_AFASTADO_TRAB_MEMB

<img width="742" height="361" alt="image" src="https://github.com/user-attachments/assets/e8a85215-81a5-46d1-9b7e-eae8248af0a5" />

O gráfico indica que a variável CO_AFASTADO_TRAB_MEMB, associada ao responsável familiar, apresenta diferença pouco acentuada na distribuição das classes de renda. Tanto entre responsáveis afastados do trabalho (1 – SIM) quanto entre aqueles não afastados (2 – NÃO), as proporções de famílias com renda ≤ ½ salário mínimo e > ½ salário mínimo permanecem muito próximas, em torno de 50%. Isso sugere que, isoladamente, o afastamento do trabalho do responsável familiar não é um fator fortemente discriminante da renda per capita no modelo, mas atua de forma complementar quando combinado com outras características de trabalho, escolaridade, composição familiar e condições do domicílio.

### Detalhamento - CO_CURSO_FREQ_PESSOA_MEMB

<img width="994" height="360" alt="image" src="https://github.com/user-attachments/assets/76b6e260-02ca-4a48-a13d-638b83e89ff6" />

O gráfico mostra uma clara relação entre o nível de escolaridade do responsável familiar e a renda domiciliar per capita. Observa-se que categorias associadas a menor escolarização — como alfabetização para adultos, ensino fundamental e EJA — concentram maior proporção de famílias com renda ≤ ½ salário mínimo. Em contraste, à medida que aumenta o nível educacional do responsável, especialmente nos grupos de ensino  pré-vestibular e ensino superior/pós-graduação, cresce de forma consistente a participação das famílias com renda acima de ½ salário mínimo. Esse padrão reforça o papel central da escolaridade do responsável familiar como proxy de inserção produtiva e capacidade de geração de renda, explicando sua elevada contribuição para a classificação realizada pelo modelo.

### Detalhamento - PCT_1_INFANCIA

<img width="920" height="347" alt="image" src="https://github.com/user-attachments/assets/42aaef3b-1d48-4f34-825a-ffc0321df9d2" />

O gráfico evidencia uma associação inversa entre a presença de crianças na primeira infância e a renda per capita familiar. À medida que aumenta o percentual de crianças pequenas no domicílio, cresce a proporção de famílias classificadas com renda ≤ ½ salário mínimo, enquanto diminui de forma acentuada a participação das famílias com renda acima de ½ salário mínimo. Esse comportamento é consistente com a literatura socioeconômica, pois domicílios com maior presença de crianças muito pequenas tendem a enfrentar maiores restrições à inserção produtiva dos adultos e maior pressão sobre a renda, o que explica a relevância dessa variável na capacidade do modelo em sinalizar potenciais inconsistências de renda.

### Detalhamento - CO_PRINCIPAL_TRAB_MEMB

<img width="736" height="352" alt="image" src="https://github.com/user-attachments/assets/1413170a-aa16-41b5-9239-e9152163eea8" />

O gráfico evidencia que a condição de trabalho do responsável familiar está fortemente associada ao perfil de renda das famílias. Situações mais precárias ou instáveis de inserção no mercado de trabalho, como conta própria/bico, trabalho temporário em área rural, trabalho doméstico sem carteira e condição de não remunerado, concentram maior proporção de famílias com renda ≤ ½ salário mínimo. Em contraste, vínculos mais formais e estáveis, como empregado e doméstico com carteira, militar/servidor público e, sobretudo, empregador, apresentam predominância de famílias com renda acima de ½ salário mínimo. Esse padrão reforça a relevância das informações de ocupação do responsável familiar como um dos principais sinais socioeconômicos utilizados pelo modelo para identificar potenciais inconsistências entre renda declarada e renda provável.

## Análises adicionais considerando as classes de renda pobreza, baixa-renda e acima de 1/2 SM

As famílias do Cadastro Único foram inicialmente classificadas em três classes de renda domiciliar per capita, com base nos valores utilizados pelo MDS:
(i) classe 0 – pobreza (≤ R$ 218),
(ii) classe 1 – baixa renda (> R$ 218 e ≤ R$ 759) e
(iii) classe 2 – acima de ½ salário mínimo (> R$ 759).

Embora o modelo final tenha sido desenvolvido como um classificador binário, distinguindo famílias com renda provável acima de ½ salário mínimo daquelas abaixo desse limiar, a variável classe_renda foi preservada ao longo das análises. Essa decisão permitiu uma avaliação mais aprofundada do comportamento do modelo em relação às classes originais, possibilitando verificar se a sinalização de risco está coerente com os diferentes perfis de renda e com o objetivo de apoiar ações de qualificação cadastral.

Assim, a análise cruzada entre classe_renda, probabilidades previstas e decisões de convocação evidencia que o modelo apresenta um gradiente claro de risco, coerente com a hierarquia das classes de renda. As famílias da classe 2 concentram as maiores probabilidades previstas e a maior proporção de convocações, enquanto as classes 0 e 1 permanecem majoritariamente não convocadas, mesmo quando apresentam probabilidades intermediárias.

Em particular, observa-se que:

* 89,2% das famílias convocadas pertencem à classe 2, indicando forte aderência do modelo ao objetivo de identificar potenciais casos acima do limiar de ½ salário mínimo;
* As classes 0 e 1 apresentam probabilidades médias significativamente menores, com raríssimos casos classificados como alto risco;
* O threshold elevado (0,80) atua como um filtro conservador, reduzindo o risco de convocar famílias efetivamente pobres ou de baixa renda.

Esse comportamento confirma que o modelo opera como um instrumento de priorização, e não de exclusão automática, concentrando a ação administrativa nos casos com maior evidência estatística de inconsistência de renda. Abaixo segue o detalhamento dos resultados da análise:

**Distribuição das convocações por classe de renda**

| Classe de renda | Não convocado (pred=0) | Convocado (pred=1) |
| --------------- | ---------------------- | ------------------ |
| 0 – Pobreza     | 	25.396                 | 761              |
| 1 – Baixa renda | 58.336                | 4.090              |
| 2 – > ½ SM      | 71.140                 | 40.277            |

**Probabilidade prevista por classe de renda**

| Classe de renda | Média | P25   | Mediana | P75   | P90   | N       |
| --------------- | ----- | ----- | ------- | ----- | ----- | ------- |
| 0 – Pobreza     | 0.283 | 0.067 | 0.199   | 0.472 | 0.680 | 26.157  |
| 1 – Baixa renda | 0.420 | 0.196 | 0.407   | 0.635 | 0.762 | 62.426  |
| 2 – > ½ SM      | 0.698 | 0.586 | 0.738   | 0.849 | 0.933 | 111.417 |

**Proporção de classes entre convocados e não convocados**

| Situação      | Classe 0 | Classe 1 | Classe 2  |
| ------------- | -------- | -------- | --------- |
| Não convocado | 16,4%    | 37,6%    | 45,9%     |
| Convocado     | 1,7%     | 9,0%     | **89,2%** |

Em conjunto, os resultados indicam que o modelo é bem calibrado para diferenciar os perfis de renda, mantendo baixo risco de sobre-inclusão das classes mais vulneráveis e concentrando a sinalização de risco nas famílias com maior probabilidade de renda acima do limite de ½ salário mínimo. A manutenção da variável classe_renda mostrou-se fundamental para validar a coerência interna do modelo e reforçar sua utilidade como ferramenta técnica de apoio à gestão, permitindo transparência, rastreabilidade e interpretação substantiva dos resultados.

## Comparação com o modelo usando variáveis do IVCAD

Dado o extenso trabalho de engenharia das variáveis diretas do Cadastro Único, foi avaliado se o Índice de Vulnerabilidade do Cadastro Único (IVCAD) — índice multidimensional já disponível — poderia cumprir função semelhante de triagem a menor custo. Foram treinados dois modelos adicionais com o mesmo objetivo e metodologia: um apenas com as variáveis do IVCAD (IVCAD puro) e outro combinando o IVCAD às variáveis individuais do responsável familiar (IVCAD+RF). Em todos os casos foram mapeadas e removidas as variáveis do IVCAD com informação direta de renda, para evitar vazamento de informação.

O modelo com variáveis diretas do Cadastro Único superou ambas as alternativas em todas as métricas:

| Modelo | PR-AUC | ROC-AUC | Famílias sinalizadas (thr 0,80) | Taxa de convocação |
| ------ | ------ | ------- | -------------------------------- | ------------------ |
| CadÚnico (completo) | 0,846 | 0,826 | 40.277 | 22,6% |
| IVCAD + RF | 0,807 | 0,813 | 26.900 | 15,1% |
| IVCAD puro | 0,774 | 0,785 | 21.866 | 12,6% |

A precisão é equivalente nos três (~0,89), de modo que a diferença operacionalmente relevante está na **cobertura**, e não na taxa de falsos positivos: o IVCAD puro deixa de sinalizar cerca de 46% das famílias que o modelo completo capturaria. A inclusão do perfil individual do responsável familiar (IVCAD+RF) recupera parte expressiva dessa lacuna (+5.034 famílias frente ao IVCAD puro), confirmando que a ausência de variáveis individuais — escolaridade, ocupação e tipo de trabalho principal — é uma das causas centrais da diferença de desempenho. A lacuna residual decorre da ausência dos indicadores de composição etária da família e das condições do domicílio em sua forma original, que o IVCAD resume em marcadores binários de privação.

Esses resultados não invalidam o IVCAD, concebido como instrumento de diagnóstico multidimensional da vulnerabilidade, mas evidenciam os limites de seu uso para classificação de renda específica. Sugerem, ainda, três direções de aperfeiçoamento do índice para esse fim: indicadores mais granulares na dimensão Trabalho e Qualificação de Adultos (distinguindo o tipo de vínculo); inclusão do perfil individual do responsável familiar; e uma referência territorial intermediária (UF ou região).

## Organização do ML em pipeline e salvamento

A engenharia de variáveis é realizada integralmente no script SQL de geração da base: limpeza dos valores ausentes segundo as regras do Cadastro Único e condensação das informações do nível de pessoa para o nível de família. Assim, a base chega ao ambiente de modelagem pronta e sem valores ausentes, e o mesmo script é utilizado para gerar a base de aplicação às famílias com renda informal, garantindo que treinamento e aplicação compartilhem exatamente o mesmo processamento.

O pipeline de Machine Learning encapsula, portanto, apenas o pré-processamento estatístico (normalização Min–Max das variáveis quantitativas contínuas, codificação one-hot das categóricas multicategóricas e preservação das percentuais, booleanas, binárias e geográficas) e o modelo final de classificação binária, incluindo explicitamente a regra operacional de decisão baseada em threshold de probabilidade. Como a base já é entregue sem ausências, o pipeline não incorpora etapas de imputação.

Após o treinamento, o pipeline completo é salvo em disco como um único artefato, assegurando que futuras aplicações utilizem exatamente as mesmas transformações e parâmetros. Adicionalmente, são gerados metadados estruturados com data de treinamento, tamanho das bases, variáveis utilizadas e limiar de decisão adotado, garantindo transparência, rastreabilidade e reutilização do modelo.

## Uso ético de inteligência artificial

O modelo foi concebido como instrumento auxiliar de triagem, e não como mecanismo autônomo de decisão. Suas saídas não produzem efeitos administrativos diretos: sinalizam casos a serem encaminhados às gestões locais para verificação, cabendo aos servidores confirmar ou refutar as indicações antes de qualquer repercussão cadastral. Esse arranjo alinha-se ao conceito de inteligência assistida e às exigências de supervisão humana, explicabilidade e mitigação de riscos previstas para sistemas de alto risco no guia de IA Generativa no Serviço Público e no Plano Brasileiro de Inteligência Artificial.

A mitigação de vieses também orientou as escolhas metodológicas. Como os erros de classificação em modelos de focalização tendem a se concentrar nos grupos mais pobres — risco frequentemente subestimado pela acurácia global —, o limiar operacional foi definido para reduzir falsos positivos entre famílias vulneráveis: no threshold de 0,80, apenas 2,9% das famílias em situação de pobreza são sinalizadas, com precisão zero nesse estrato, indicando que o modelo não gera risco operacional para esse segmento. O uso da base completa do Cadastro Único como fonte de treinamento reforça a representatividade da amostra, e a responsabilidade pela decisão administrativa permanece integralmente com os gestores locais.

## Conclusão

O modelo desenvolvido apresentou desempenho consistente e aderente ao objetivo proposto, alcançando elevada precisão na identificação de famílias com maior probabilidade de inconsistência entre a renda declarada e a renda provável inferida a partir de características socioeconômicas, da composição familiar e das condições do domicílio. 

A estratégia de classificação binária, aliada à definição de um limiar de decisão ajustável, permitiu equilibrar capacidade preditiva e viabilidade operacional, oferecendo um critério objetivo e transparente para apoiar ações de qualificação cadastral. 

Ressalta-se que o modelo não estima renda individual nem substitui procedimentos formais de verificação, tampouco implica julgamento automático sobre veracidade da informação declarada. Seu resultado deve ser interpretado exclusivamente como um score de priorização, que auxilia a gestão pública a direcionar esforços de qualificação cadastral de forma mais focalizada, transparente e proporcional à capacidade operacional.

Como próximo passo, o modelo poderá ser aplicado às famílias sem renda formal captada pelo CNIS, gerando uma lista priorizada de casos com maior risco de divergência de renda, a ser utilizada pelas gestões na condução de processos de verificação e atualização cadastral. 

Os resultados obtidos mostram-se coerentes com a literatura especializada sobre focalização de políticas sociais e uso de aprendizado de máquina para diagnóstico de pobreza, reforçando a adequação metodológica das escolhas realizadas e o potencial do modelo como instrumento de apoio à gestão pública.

## Referências bibliográficas 

O levantamento bibliográfico contou com apoio de um modelo de linguagem (Claude Sonnet 4.6, Anthropic) para ampliar o vocabulário de busca e localizar publicações relevantes; todas as referências foram posteriormente verificadas e selecionadas manualmente.

As referências completas, em formato ABNT, encontram-se no artigo do projeto. A seguir, as principais fontes que embasaram as escolhas metodológicas, organizadas por tema.

**Machine learning para predição de pobreza e focalização**
- Linhares et al. (2026) — *A new era in poverty diagnostics.* [DOI](https://doi.org/10.1016/j.ject.2025.05.002)
- Corral, Henderson & Segovia (2025) — *Poverty mapping in the age of machine learning.* [DOI](https://doi.org/10.1016/j.jdeveco.2024.103377)
- Verme (2025) — *Predicting poverty.* World Bank Economic Review. [DOI](https://doi.org/10.1093/wber/lhae044)
- Altindağ et al. (2021) — *Targeting humanitarian aid using administrative data.* [DOI](https://doi.org/10.1016/j.jdeveco.2020.102564)
- McBride & Nichols (2016) — *Retooling poverty targeting using machine learning.* World Bank. [DOI](https://doi.org/10.1596/33525)
- Wobcke & Mariyah (2023) — *ML and data augmentation in the proxy means test.* [DOI](https://doi.org/10.3233/SJI-230033)
- Herrera et al. (2023) — *ICTs and income distribution in Brazil: a SHAP explanation.* [DOI](https://doi.org/10.1016/j.telpol.2023.102598)

**Cadastro Único, renda informal e contexto brasileiro**
- Mostafa & Santos (2016) — *Limitações de um teste de meios via predição de renda.* Ipea. [link](http://repositorio.ipea.gov.br/handle/11058/7234)
- Costa, Santos & Ferreira (2025) — *O IVCAD no MDS.* Caderno de Estudos n. 39. [link](https://aplicacoes.mds.gov.br/sagicad/pesquisas/documentos/estudo_pesquisa/estudo_pesquisa_326.pdf)
- Engbom et al. (2022) — *Earnings inequality and dynamics in the presence of informality: Brazil.* [DOI](https://doi.org/10.3982/QE1855)
- Direito, Licio & Curralero (2024) — *Cadastro Único e a crise da maior idade.* Ipea TD n. 2989. [DOI](http://dx.doi.org/10.38116/td2989-port)
- Dietrich, Malerba & Gassmann (2024) — *Predicting social assistance beneficiaries: the welfare damage of data biases.* [DOI](https://doi.org/10.1017/dap.2023.38)

**Uso ético e governança de IA no setor público**
- Brasil/MCTI (2025) — *Plano Brasileiro de Inteligência Artificial (PBIA).* [link](https://www.gov.br/mcti/pt-br/centrais-de-conteudo/publicacoes-mcti/plano-brasileiro-de-inteligencia-artificial/pbia_mcti_2025.pdf)
- Brasil/MGI–SERPRO (2025) — *IA Generativa no Serviço Público: definições, usos e boas práticas.* [link](https://www.gov.br/governodigital/pt-br/infraestrutura-nacional-de-dados/inteligencia-artificial-1/ia-generativa-no-servico-publico.pdf)
- Denis et al. (2021) — *Uso responsável da IA para políticas públicas.* BID. [DOI](https://doi.org/10.18235/0003631)
- Transparência Brasil (2020) — *Recomendações de governança: uso de IA pelo poder público.* [link](https://transparencia.org.br)
- ONU (2019) — *Digital technology, social protection and human rights.* [link](https://www.ohchr.org/en/calls-for-input/digital-technology-social-protection-and-human-rights-report)
- Harari (2024) — *Nexus: uma breve história das redes de informação.* Companhia das Letras.

**Marco normativo do Cadastro Único**
- Brasil (2022) — Decreto nº 11.016/2022 (regulamenta o Cadastro Único). [link](https://www.planalto.gov.br/ccivil_03/_ato2019-2022/2022/decreto/d11016.htm)
- Brasil/MDS (2023) — Instrução Normativa nº 1/SAGICAD (integração Cadastro Único–CNIS). [link](https://www.gov.br/mds/pt-br/acesso-a-informacao/legislacao/instrucoes/instrucao-normativa-no-1-sagicad-mds-de-02-de-junho-de-2023)
- Brasil/MDS (2022) — *Manual do entrevistador do Cadastro Único*, 5. ed.
