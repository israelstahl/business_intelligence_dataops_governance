# Inteligência Estratégica & Governança DataOps (CIEGES)

> Arquitetura analítica corporativa, governança do ciclo de vida da informação (DLM) e validação de qualidade de dados *Shift-Left* aplicadas ao ecossistema do Sistema Único de Saúde (SUS).

[![Governança DAMA-DMBOK](https://img.shields.io/badge/Governan%C3%A7a-DAMA--DMBOK-0A66C2.svg)](#)
[![Data Quality](https://img.shields.io/badge/Data_Quality-Shift--Left-2ea44f.svg)](#)
[![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub_Actions-2088FF.svg)](#)
[![Modelagem Dimensional](https://img.shields.io/badge/Modelagem-Fact_Constellation-orange.svg)](#)
[![Compliance](https://img.shields.io/badge/Conformidade-LGPD_Sa%C3%BAde-blueviolet.svg)](#)

---

## 1. Visão Executiva & Contexto de Negócio

A fragmentação dos sistemas de informação em saúde pública (e-SUS APS, SISREG, SIVEP-Gripe, SINAN, SIM, CNES) historicamente impõe a criação de silos de informação e relatórios manuais redundantes. No setor público e corporativo de saúde, a maioria das iniciativas de Business Intelligence sucumbe ao **"Cemitério de Dashboards"** — telas visuais isoladas que entram em desuso operacional por falta de governança estruturada.

Conforme demonstrado por Fardous (2023), **falhas de qualidade de dados ($\beta = -0,31$) e deficiências em metadados/documentação ($\beta = -0,27$) respondem por 52,7% da variância no fracasso de plataformas analíticas**.

Este repositório documenta a estrutura técnica, os contratos de governança e a modelagem analítica do **Centro de Inteligência Estratégica para Gestão Estadual do SUS (CIEGES-MS)**. A arquitetura foi desenvolvida para transicionar a gestão de saúde de um padrão reativo/descritivo (Nível 1 de Maturidade Gartner) para um modelo preditivo e prescritivo orientado a evidências (Níveis 3 e 4).

---

## 2. Portfólio Modular de Painéis Analíticos

O ecossistema analítico adota o conceito de governança modular. Cada produto de dados possui regras clínicas, dicionários e especificações técnicas documentadas em seções dedicadas:

| Painel / Produto de Dados | Escopo de Inteligência Sanitária | Status do Pipeline | Especificação Técnica |
| :--- | :--- | :--- | :--- |
| **01. Telessaúde & Telediagnóstico** | Monitoramento de produção de laudos (ECG, Espirometria, Oftalmo, Dermato) e sessões de teleatendimento com cálculo de absenteísmo. | Em Produção (Gold)[cite: 4] | [`docs/projects/01_telessaude.md`](docs/projects/01_telessaude.md) |
| **02. Segurança do Paciente (NEGESP)** | Gestão e estratificação de severidade de incidentes/eventos adversos em serviços hospitalares e ambulatoriais. | Fase de Contratos (Silver) |
| **03. Vigilância em Saúde & Endemias** | Detecção precoce de anomalias epidemiológicas, séries temporais e monitoramento de quebra de limites endêmicos. | Fase de Modelagem |

---

## 3. Macroprocesso de Engenharia & Atendimento (BPMN)

A operacionalização das demandas analíticas segue o **Macroprocesso de Solicitação de Dados**, desenhado sob o padrão BPMN para garantir segregação estrita de funções, rastreabilidade e governança de ponta a ponta[cite: 2, 3]:

+------------------------------------------------------------------------------------------------------------------+
| Áreas de Negócio (Alta Gestão)   [Problema de Negócio]                                                           |
|                                       │                                                                          |
| Coordenação                           ▼                                                                          |
|                         [Alinhamento Estratégico & Priorização] ──────────────────────────┐                      |
|                                       │                                                   │                      |
| Especialista de Dados                 ▼                                                   ▼                      |
|                         [Avaliação de Risco LGPD] ──> [Regras de Negócio] ──> [Data Contracts em YAML]           |
|                                                                                           │                      |
| Engenharia / DataOps                                                                      ▼                      |
|                         [Ingestão Raw/Staging] ──> [Pipeline Bronze -> Silver] ──> [Validação Shift-Left]        |
|                                                                                           │                      |
| Analytics Engineering                                                                     ▼                      |
|                         [Publicação em Produção / BI] <── [Homologação] <── [Modelagem Dimensional Gold]         |
+------------------------------------------------------------------------------------------------------------------+

### Raias de Responsabilidade (RACI Integrada)
* **Demandante / Área de Negócio:** Formaliza a necessidade sanitária e valida os critérios de aceite.
* **Coordenação CIEGES:** Estabelece prioridades do SUS, pactua patrocínio institucional e soluciona impeditivos institucionais.
* **Especialista em Saúde Digital:** Mapeia os requisitos técnicos, elabora os Contratos de Dados (YAML), conduz a modelagem dimensional e assegura a conformidade com a LGPD e regras de negócio.
* **Engenharia de Dados (DataOps):** Automatiza as rotinas de ingestão, sanitização, orquestração e testes unitários de schema (Raw $\rightarrow$ Silver), configura API's.
* **Parceiros Estratégicos (TI):** Gestão de acessos, segurança perimetral e disponibilidade de cluster.

---

## 4. Arquitetura Analítica: Fact Constellation Schema

Para integrar fontes heterogêneas sem duplicidade estrutural e sem a ocorrência de chaves sintéticas (*synthetic keys*), a camada Gold utiliza a modelagem dimensional **Fact Constellation (Esquema Galáxia)**:

## 5. Conformidade LGPD & Salvaguardas de Privacidade

Em conformidade com a Lei Geral de Proteção de Dados (Lei nº 13.709/2018, Arts. 7º e 11):

Pseudonimização e Anonimização: Identificadores pessoais (CPF, CNS do paciente).

## 6. 

├── .github/
├── contracts/
│   ├── templates/
│   │   └── data_contract_base.yaml    # Modelo de contrato corporativo
│   └── telessaude/
│       ├── contract_telediagnostico.yaml
│       └── contract_teleoftalmologia.yaml
├── docs/
│   ├── macroprocesso_dados_bpmn.png  # BPMN do Macroprocesso
│   └── projects/
│       ├── 01_telessaude.md              # Documentação detalhada da Telessaúde
        │   ├── constellation_data_model.png  # Diagrama estrutural das Fatos e Dimensões
        │   ├── qlik scripts
│       ├── 02_seguranca_paciente_negesp.md # Documentação de Segurança do Paciente
│       └── 03_vigilancia_saude.md        # Documentação do Pipeline de Vigilância
├── .gitignore
├── LICENSE
└── README.md                             # Documentação central do ecossistema