# Retenção LGPD: Política de Ciclo de Vida e Retenção de Dados Corporativos (Data Retention & Purge)

## 1. Contexto e Problema de Negócio
No contexto de empresas com operações digitais contínuas, a ausência de diretrizes claras para o ciclo de vida da informação acarreta dois problemas críticos:
1. **Risco Regulatório e de Conformidade:** A manutenção indeterminada de registos com dados pessoais infringe diretamente princípios basilares da legislação brasileira de proteção de dados (LGPD - Lei 13.709/2018), nomeadamente a necessidade e a limitação da conservação.
2. **Ineficiência de Infraestrutura:** A acumulação indiscriminada de bases transacionais em camadas de armazenamento de alta performance (*Hot Storage*) eleva desnecessariamente os custos operacionais e degrada a performance de consultas analíticas.

## 2. Abordagem Metodológica
Este projeto transpõe os fundamentos da **Teoria das Três Idades** e da **Tabela de Temporalidade Documental (TTD)** — consolidados na Ciência da Informação e Arquivologia — para o ecossistema moderno de engenharia e governação de dados alinhado ao framework **DAMA-DMBOK2**.

O ciclo de vida dos dados foi estruturado em três fases técnicas:
* **Fase Corrente (*Hot Storage*):** Armazenamento em bases transacionais de alta disponibilidade enquanto persistir a finalidade operacional primária.
* **Fase Intermediária (*Cold Storage*):** Migração para repositórios de armazenamento de menor custo (com encriptação e acesso restrito) para resguardo legal ou fiscal pendente.
* **Destinação Final (*Purge* ou Anonimização):** Execução de expurgo irreversível para dados desprovidos de base de retenção legal, ou anonimização estatística (descaracterização conforme o Art. 12 da LGPD) para preservação de séries temporais de negócio.
# Matriz de Temporalidade e Ciclo de Vida de Dados

**Versão:** 1.0  
**Área Responsável:** Governança de Dados & DPO / Compliance  
**Periodicidade de Revisão:** Anual  

---

### Tabela Estruturada de Retenção

| ID | Categoria / Entidade de Dados | Tabelas / Campos Críticos | Finalidade do Tratamento | Base Legal / Fundamentação | Fase Corrente (*Hot Storage*) | Fase Intermediária (*Cold Storage*) | Destinação Final | Data Owner (Responsável) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **RET-01** | **Logs de Acesso a Aplicações** | `tb_access_logs` (IP, data/hora, porta lógica, ID usuário) | Auditoria de segurança e rastreamento de acessos a sistemas web. | **Marco Civil da Internet** (Lei 12.965/14, art. 15) | **6 meses** (base operacional) | Não aplicável | **Expurgo Permanente** (Eliminação física automatizada) | CISO / Segurança da Informação |
| **RET-02** | **Dados Fiscais e Transacionais** | `tb_pedidos`, `tb_notas_fiscais`, `tb_pagamentos` (Valor, itens, chave NFe) | Emissão fiscal, escrituração contábil e resposta a auditorias tributárias. | **Código Tributário Nacional** (CTN, art. 173 e 174); **Código Civil** (art. 206, §5º) | **1 ano** (após conclusão da compra) | **5 anos** (armazenamento frio criptografado) | **Anonimização** (Remove identificadores pessoais; retém métricas de receita para séries históricas) | Head Financeiro / Controladoria |
| **RET-03** | **Atendimento ao Cliente (SAC)** | `tb_chamados`, `tb_protocolos_sac` (Áudios gravados, transcrições, mensagens) | Registro de reclamações, pedidos de cancelamento e suporte pós-venda. | **Decreto do SAC** (Decreto 11.034/22, art. 11, §3º); **CDC** (Lei 8.078/90) | **90 dias** (chamado ativo/resolvido) | **2 anos** (a contar da finalização do atendimento) | **Expurgo Permanente** (Exclusão definitiva de gravações e logs de chat) | Gerente de Atendimento & CX |
| **RET-04** | **Cadastro de Clientes Ativos** | `tb_clientes` (Nome, CPF, e-mail, telefone, endereço) | Execução de contrato, faturamento e logística de entrega. | **LGPD** (Lei 13.709/18, art. 7º, V - Execução de Contrato) | **Vigência da relação contratual** | Não aplicável | Ver regra de *Inatividade* (RET-05) | Diretor Comercial / E-commerce |
| **RET-05** | **Cadastro de Clientes Inativos / Churn** | `tb_clientes`, `tb_enderecos` (Contas inativas ou solicitações de cancelamento) | Resguardo para defesa judicial em ações de consumo e responsabilidade civil. | **Código de Defesa do Consumidor** (art. 27); **Código Civil** (art. 206, §5º); **LGPD** (art. 16, I e II) | **1 ano** após inatividade declarada | **4 anos** (acesso restrito exclusivamente à equipe jurídica) | **Anonimização Irreversível** ou **Expurgo Permanente** | DPO / Jurídico |
| **RET-06** | **Banco de Talentos (Recrutamento)** | `tb_candidatos`, `tb_curriculos` (Formação, histórico, dados de contato) | Triagem de perfis para vagas abertas e processos seletivos futuros. | **LGPD** (art. 7º, I - Consentimento ou art. 7º, IX - Legítimo Interesse) | **Período da seleção ativa** | **6 meses** a contar do encerramento da vaga | **Expurgo Permanente** (Exclusão do banco relacional e anexos em storage) | Gerente de Recursos Humanos |
| **RET-07** | **Registros Trabalhistas de Ex-Colaboradores** | `tb_folha_pagamento`, `tb_ponto_eletronico`, `tb_rescisao` | Comprovação previdenciária e resguardo contra reclamatórias trabalhistas. | **CLT** (art. 11 - prazo prescricional bienal/quinquenal); **Legislação Previdenciária** | **2 anos** após rescisão | **3 anos** (totalizando 5 anos para reclamatórias); Histórico previdenciário retido em arquivo frio | **Expurgo Seletivo** após expiração do prazo prescricional decadencial | Head de Recursos Humanos / DP |
