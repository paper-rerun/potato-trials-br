# Ensaios de manejo da batata no Brasil

English version: [README.md](README.md)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23670283.svg)](https://doi.org/10.5281/zenodo.23670283)

1.874 resultados de 45 ensaios com batata (*Solanum tuberosum*) no Brasil, tirados das tabelas de 20 publicações de acesso aberto da Embrapa: doenças e pragas (requeima, pinta-preta, murcha-bacteriana, canela-preta e podridão-mole, PVY e PLRV, nematoide-das-galhas, *Diabrotica speciosa* e outras vaquinhas, pulgões), fungicidas e produtos alternativos, pulverização orientada por sistema de previsão, inseticidas e nematicidas, ensaios de cultivares e clones, degenerescência da batata-semente, fósforo e potássio, adubação orgânica, densidade de plantio para semente pré-básica, dessecação da rama e descompactação do solo. Cada linha é um número exatamente como foi impresso, com a página e a tabela de onde saiu, e aponta para o ensaio a que pertence.

Boa parte dos dados de ensaios sobre doenças, pragas e manejo da batata no Brasil está em boletins da Embrapa e artigos de periódicos. O mais antigo foi datilografado em 1974, e dois estão escaneados. Este dataset torna 20 deles pesquisáveis e comparáveis, do Rio Grande do Sul e de Santa Catarina a Minas Gerais, São Paulo e o Distrito Federal.

Faz parte do [Paper Rerun](https://paper-rerun.github.io), na linha Open Tables. O trabalho foi feito por agentes de IA num computador doméstico e revisado por uma pessoa antes da publicação.

## O que tem

| | |
|---|---|
| Publicações | 20 (Alice e Infoteca-e, os repositórios abertos da Embrapa), 1974 a 2025 |
| Ensaios | 45 (41 experimentos, 3 levantamentos e 1 análise conjunta; 38 de campo, 4 em laboratório, 3 em vasos em casa de vegetação), no Rio Grande do Sul, Santa Catarina, Minas Gerais, São Paulo e Distrito Federal |
| Registros | 1.874 (número de tubérculos, produtividade total e comercial, produtividade e participação por classe de tamanho de tubérculo, severidade de doenças, AACPD e classe de reação, detecção de vírus, fitotoxicidade e dano vascular após a dessecação, contagens de nematoides, dano e densidade de pragas, características da planta, nutrientes nas folhas, infecção da batata-semente, dados econômicos e outros) |

Arquivos:
- `data/potato_trials.csv`: o dataset, um registro por valor impresso, com `doc_id` e `trial_id`. As colunas estão descritas em `method/schema.json`. Quando um registro herda um valor da sua coluna na tabela ou da tabela inteira (unidade, estatística, alvo, parte da planta, classe de tamanho de tubérculo, nível do fator, momento da avaliação), o valor vem preenchido e `inherited` diz de onde ele veio.
- `data/potato_trials_context.csv`: uma linha por ensaio (local, safra, datas de plantio e de colheita, batata-semente, regime hídrico, sistema de preparo do solo, cultura anterior, cultivar, pressão de doenças ou pragas, método de aplicação, volume de calda e momento das aplicações, pulverizações feitas em todas as parcelas, espaçamento ou densidade de plantio, adubação de base, delineamento). Junte os dois arquivos por `doc_id` + `trial_id`; `application_stages` ou `n_applications` vazios num registro significam que vale o valor do ensaio.
- `data/sources.csv`: as 20 publicações, com autores, link do registro e aviso de licença.
- `qa/decisions.csv`: cada divergência entre as duas transcrições independentes e como ela foi resolvida conferindo o PDF (2.026 decisões, com a página).
- `method/`: o schema, o script de comparação e o script de junção.

### Principais colunas

O arquivo está em UTF-8, com vírgula separando as colunas. Local, safra, batata-semente, cultivar, pulverizações de rotina e delineamento de cada ensaio estão em `data/potato_trials_context.csv`.

- `doc_id`: identificador da publicação de origem. Liga o registro a `data/sources.csv` e é também o nome do PDF, sem a extensão.
- `trial_id`: o ensaio a que o valor pertence, dentro da publicação. Com `doc_id`, liga o registro à sua linha no arquivo de contexto.
- `page`: página do PDF contada a partir de 1 (a primeira página do arquivo é 1), e não o número impresso na página. O rótulo da tabela está em `table_or_figure`, como impresso ("Tabela 3", "Quadro 1", "Figura 2"), ou `text` quando o número está no corpo do texto.
- `treatment`: rótulo da linha exatamente como impresso (cultivar, código de clone, produto, dose, época de plantio, tamanho de batata-semente, "Testemunha", "Média"), com as marcas de rodapé. Numa célula de fatorial, ou numa coluna que é um nível de um fator de tratamento, os níveis vêm unidos por " x ", na ordem da tabela ("Agata x 150 kg/ha de N").
- `biotic_target`: a doença, praga ou distúrbio a que a variável se refere, como `late_blight` (requeima), `early_blight` (pinta-preta), `bacterial_wilt` (murcha-bacteriana), `blackleg_soft_rot` (canela-preta e podridão-mole), `pvy`, `plrv`, `nematodes` (nematoides), `diabrotica`, `aphids` (pulgões) ou `physiological_disorder` (distúrbio fisiológico). É `multiple` quando um valor cobre vários.
- `variable`: variável medida, como impressa no cabeçalho da coluna ou na legenda. Quando o cabeçalho tem mais de um nível, os níveis vêm separados por " / " ("Amostras / Infectadas (%) / PVY").
- `variable_standardized`: a mesma variável numa lista fechada. Alguns exemplos: `tuber_yield_total_area` (produtividade total de tubérculos por área ou parcela, na unidade impressa), `tuber_yield_marketable_area` (produtividade de tubérculos comerciais), `tuber_yield_size_class` (produtividade de uma classe de tamanho, com a classe em `size_class`), `tuber_number` (número de tubérculos), `disease_severity_pct` (severidade em % da área), `disease_audpc` (AACPD), `virus_detection` (% de plantas ou tubérculos positivos em ELISA ou PCR), `phytotoxicity` (fitotoxicidade) e `nematode_measure` (galhas, ovos, juvenis, fator de reprodução). A lista completa está em `method/schema.json`.
- `value_as_printed`: a célula exatamente como impressa, sem letras, asteriscos, marcas de rodapé ou termo de dispersão ("2.920", "45,3", "Tr", "MS"). Quando `value_kind` é `class` (classes de reação, traços, datas) ou `illegible`, `value` fica vazio.
- `value`: o mesmo número pronto para cálculo, com ponto decimal e sem separador de milhar ("2.920" vira 2920, "45,3" vira 45.3). Nada foi calculado, convertido ou arredondado.
- `unit`: unidade como impressa ou informada na legenda ou no cabeçalho ("t/ha", "kg/parcela", "%"). Vazia quando não há unidade impressa.
- `statistic`: o que o número é: `mean` (média), `relative_to_check` (calculado pelos autores em relação à testemunha), `total`, `median`, `single_value` (uma parcela ou uma observação), `percentage_of_total` ou `other`.
- `significance_letter`: letra(s) ou asterisco(s) impressos com o valor, exatamente como no documento ("a", "AB", "a A", "*", "**"). Vazia quando não há. O teste e o significado das letras estão em `significance_test` e `letters_meaning`.

## Como foi feito

1. **Coleta.** Um agente buscou nos repositórios abertos da Embrapa publicações com tabelas de ensaios quantitativos sobre o tema e baixou 20, registrando cada candidata que não entrou, com o motivo.
2. **Duas transcrições independentes.** Dois agentes por publicação transcreveram o contexto dos ensaios, os rótulos que valem para uma tabela inteira ou para uma coluna (qual estatística, qual teste, o que as letras significam, quando a cultura foi avaliada, qual parte da planta, qual classe de tamanho de tubérculo) e cada célula de tabela sobre o tema num schema fixo, sem ver o trabalho um do outro, copiando os números exatamente como impressos e prestando contas de cada célula de dados.
3. **Comparação e arbitragem.** Um script pareou as duas transcrições, ensaio por ensaio, tabela por tabela e registro por registro, e listou cada divergência e cada valor presente em só uma delas. Cada caso foi resolvido conferindo o PDF, com a página como evidência.

## Qual a precisão

- **Números:** nenhum número ou letra de significância foi lido errado em nenhuma das duas transcrições. Dos 1.810 registros pareados, 1.806 bateram com o impresso. Os outros 4 vêm do texto de silveira2007, que escreve o número por extenso, em inglês ("three plots", "four observation dates"): uma transcrição pôs o algarismo no campo do valor impresso, a outra copiou a palavra. O valor é o mesmo nas duas, e o dataset mantém a palavra como impressa. Mais uma célula, em medeiros2002, traz "7,49*"; uma transcrição passou o asterisco para as notas em vez do campo de significância.
- **Contexto e rótulos:** sozinha, uma transcrição estava totalmente correta em 86,4% dos registros, e a outra em 83,8%, contando os registros que cada uma deixou de fora (29 e 27). A maioria dos erros veio de uma regra perdida num documento inteiro: o momento de avaliação dos testes com folíolos destacados, informado só na metodologia e não levado às tabelas (wolter2025, 230 campos), um adubo aplicado uma vez no plantio e não contado como uma aplicação (perez2001, 81), linhas de levantamento marcadas como efeitos principais (daniels2002, 46), a dose deixada de fora do nome do tratamento (salles1999, 42), uma tabela de resultados (o número de pulverizações que cada sistema indicou) lida como diagramação e pulada (gomes2002, 26 registros) e uma coluna de tamanho de amostra pulada (daniels2002, 24 registros). Uma transcrição também lançou 20 agrupamentos de genótipos do texto de daniels2004 que não batem com a tabela; o dataset fica com a tabela e sinaliza o conflito. As 2.026 decisões foram resolvidas conferindo o PDF, então o dataset não depende de nenhuma das duas transcrições sozinha.
- **Limites conhecidos:** valores mostrados só em gráficos, sem número impresso, não foram lidos; 4 registros vêm de uma figura (nava2017, Figura 2, que traz os números no gráfico) e 63 do texto. Sarna-comum, traça-da-batata, mosca-branca e canela-preta no campo não estão cobertas: os dois repositórios não têm tabela de ensaio sobre elas que servisse, só folhetos, notas em revista ou resumos de uma página (canela-preta e podridão-mole aparecem só num teste de laboratório com tubérculos, lucon1999). Nenhum documento é do Paraná; o único candidato de lá ficou de fora pelo limite de 4 documentos por autor. menschoy1974 é um documento escaneado com camada de texto ruidosa, e souza1991 é escaneado sem nenhuma; os dois foram lidos a partir da imagem da página. Um documento, souza1991 (doses e épocas de cinco dessecantes de rama para batata-semente em São Joaquim, SC), tem 323 registros, 17% do dataset; sem ele, uma transcrição sozinha está totalmente correta em 84,1% dos registros, e a outra em 82,3%. De charchar2007 só são usadas as páginas 4 e 5, já que a Tabela 2, na página 6, é sobre cenoura. Em 11 ensaios, todas as parcelas recebem as mesmas pulverizações de rotina; elas estão registradas em `uniform_sprays` do ensaio, e não no tratamento.

## Licença e citação

- O dataset (os arquivos CSV, o schema e os scripts) é publicado sob a licença [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Por favor, cite as publicações originais listadas em `data/sources.csv` e também este dataset.
- As publicações não são redistribuídas. Os repositórios da Embrapa exibem um aviso CC BY-NC-ND 4.0; os 11 artigos de periódico (5 na Horticultura Brasileira, os outros na Ciência Rural, Fitopatologia Brasileira, Nematologia Brasileira, Revista de Ciências Agroveterinárias, Pesquisa Agropecuária Brasileira e Summa Phytopathologica) e o trabalho de congresso medeiros2008 mantêm os próprios termos, e três boletins da Embrapa (wolter2025, lourenco2020 e lopes2021) trazem "Todos os direitos reservados". souza1991 não tem camada de texto, e não foi possível procurar nele um aviso. Aqui são usados só os números, que são fatos, e citações curtas.

Erros ou ensaios que faltam: por favor, [abra uma issue](https://github.com/paper-rerun/potato-trials-br/issues).

## Como este dataset foi feito

Este dataset foi montado por agentes de IA (Claude, da Anthropic) num computador doméstico e revisado por uma pessoa antes da publicação. Nenhum valor foi estimado ou digitado à mão: cada um foi transcrito do PDF original duas vezes, de forma independente, e cada divergência foi conferida na página. O método e os scripts estão em `method/`. Como funciona o projeto como um todo (em inglês): https://github.com/paper-rerun/paper-rerun/blob/main/METHOD.md

Contato: Filipe Tessaro, pelas issues deste repositório.
