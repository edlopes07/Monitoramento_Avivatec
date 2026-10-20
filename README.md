# Monitoramento Avivatec

Sistema de monitoramento e análise estratégica de vagas de emprego da **Avivatec**, desenvolvido em Python como projeto acadêmico do **3º termo do curso de Ciência da Computação** em parceria com a empresa.

A aplicação coleta informações das vagas publicadas no portal de recrutamento da empresa, armazena os dados em um banco SQLite e compara diferentes registros históricos para identificar alterações. As mudanças detectadas são encaminhadas à API do Google Gemini, responsável por gerar uma análise sobre possíveis tendências de contratação e direcionamentos estratégicos da organização.

## Objetivo do projeto

O objetivo é utilizar dados públicos de recrutamento para investigar possíveis mudanças no perfil profissional e tecnológico buscado pela Avivatec ao longo do tempo.

Por meio do acompanhamento das vagas, o sistema procura identificar alterações nas tecnologias exigidas, nos requisitos profissionais e nas informações divulgadas, relacionando essas mudanças a possíveis decisões estratégicas da empresa.

## Funcionalidades

- **Web scraping:** coleta informações das vagas disponíveis no portal da Avivatec utilizando Playwright.
- **Persistência de dados:** armazena vagas, registros históricos e informações coletadas em um banco de dados SQLite.
- **Monitoramento de alterações:** compara os dados atuais com o último registro armazenado de cada vaga.
- **Identificação de novas vagas:** registra oportunidades que ainda não constam no banco de dados.
- **Detecção de vagas removidas:** identifica vagas anteriormente registradas que não aparecem mais no portal durante a coleta.
- **Análise com inteligência artificial:** utiliza a API do Google Gemini para interpretar as alterações identificadas e produzir um diagnóstico estratégico.
- **Relatório textual:** apresenta os resultados da análise diretamente no terminal.

## Tecnologias utilizadas

| Tecnologia | Aplicação no projeto |
|---|---|
| Python | Linguagem principal da aplicação |
| Playwright | Automação do navegador e coleta de informações das páginas |
| SQLite | Armazenamento persistente das vagas e de seus históricos |
| Google Gemini API | Interpretação das alterações e geração de análises estratégicas |

## Como funciona

O sistema segue um fluxo de coleta, comparação e interpretação dos dados.

1. **Coleta:** acessa o portal de vagas da Avivatec e identifica as oportunidades publicadas.
2. **Extração:** acessa cada vaga e coleta as informações textuais relevantes disponíveis na página.
3. **Armazenamento:** registra as vagas e salva snapshots das informações coletadas no banco SQLite.
4. **Comparação:** confronta as informações atuais com o snapshot anterior da mesma vaga, identificando itens adicionados e removidos.
5. **Detecção de mudanças no portal:** identifica novas vagas e vagas anteriormente registradas que não foram encontradas na coleta atual.
6. **Análise com IA:** reúne as alterações detectadas em uma estrutura JSON e as envia ao Google Gemini.
7. **Diagnóstico:** exibe a interpretação gerada pela IA, organizada em resumo das alterações e análise da estratégia corporativa.

## Estrutura do banco de dados

O sistema utiliza três tabelas principais no arquivo `vagas.db`.

### `vagas`

Armazena os dados de identificação das oportunidades.

- `id`: identificador único da vaga.
- `titulo`: título da oportunidade.
- `link`: endereço da vaga, com restrição de unicidade.
- `data`: data de registro da vaga.

### `snapshots`

Armazena os registros históricos das coletas realizadas para cada vaga.

- `id`: identificador do snapshot.
- `vaga_id`: referência à vaga correspondente.
- `data_coleta`: data e horário da coleta.

### `infos`

Armazena os itens textuais extraídos de cada snapshot.

- `id`: identificador do registro.
- `snapshot_id`: referência ao snapshot correspondente.
- `texto`: informação textual coletada da página.

Essa estrutura permite relacionar cada vaga aos seus registros históricos e comparar as informações de diferentes coletas.

## Análise com inteligência artificial

Após identificar as alterações, o sistema organiza os resultados em JSON e os envia ao modelo `gemini-2.5-flash`, por meio da biblioteca oficial `google-genai`.

A instrução enviada ao modelo estabelece que a análise deve:

- Priorizar mudanças relevantes, ignorando alterações superficiais ou meramente estéticas.
- Resumir as informações adicionadas e removidas em cada vaga.
- Identificar possíveis mudanças no perfil tecnológico ou profissional buscado.
- Apontar possíveis transições de paradigma tecnológico quando sustentadas pelos dados.
- Elaborar uma interpretação geral sobre o possível direcionamento estratégico da empresa.
- Evitar conclusões sem fundamento nas informações fornecidas.

A resposta é organizada em dois blocos:

**Resumo das Alterações:** descreve as mudanças identificadas em cada vaga.

**Estratégia Corporativa:** apresenta uma interpretação geral das possíveis tendências observadas no conjunto de alterações.

Os resultados representam hipóteses analíticas baseadas nos dados coletados. Não constituem confirmação das decisões internas, da situação financeira ou da estratégia oficial da Avivatec.

## Observações e limitações

- A comparação histórica depende da existência de registros anteriores no banco de dados.
- A coleta depende da disponibilidade do portal, da estrutura das páginas e dos seletores utilizados no código.
- O scraping utiliza um tempo de espera fixo para carregar as páginas, o que pode afetar a coleta em conexões lentas.
- As informações são extraídas de elementos textuais das páginas; portanto, alterações na estrutura do portal podem exigir ajustes no código.
- A ausência de uma vaga em uma coleta pode indicar sua remoção do portal, mas também pode decorrer de falhas temporárias de acesso ou coleta.
- O sistema utiliza o Gemini para interpretar as alterações, de modo que a qualidade do diagnóstico depende tanto dos dados fornecidos quanto da resposta do modelo.
- As conclusões geradas não substituem uma análise empresarial baseada em fontes complementares.

## Contexto acadêmico

Projeto desenvolvido no **3º termo do curso de Ciência da Computação**, explorando conceitos de automação web, persistência de dados, comparação histórica, integração com APIs e aplicação de inteligência artificial à análise de informações empresariais.

O projeto demonstra como informações públicas de recrutamento podem ser estruturadas e utilizadas para investigar possíveis tendências organizacionais ao longo do tempo.

---

**Aviso:** este é um projeto acadêmico. As análises produzidas não representam necessariamente a opinião, as decisões ou a estratégia real da empresa.
