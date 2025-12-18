# Sistema Integrado de Alocação de Vagas Docentes - UFRRJ

**Versão:** 3.1.0 (Edital CEPE 2025)

**Responsável:** Pró-Reitoria de Planejamento, Avaliação e Desenvolvimento Institucional (PROPLADI)

**Instituição:** Universidade Federal Rural do Rio de Janeiro (UFRRJ)

## 1. Apresentação

Este sistema é uma ferramenta de apoio à decisão desenvolvida para auxiliar o **Conselho de Ensino, Pesquisa e Extensão (CEPE)** no processo de distribuição e alocação de vagas docentes. A aplicação implementa estritamente os critérios matemáticos definidos nos **Anexos I e III** do Edital de Chamada Pública, garantindo isonomia, transparência e auditabilidade no processamento das solicitações das subunidades acadêmicas.

A ferramenta opera como uma *Single Page Application* (SPA), executando todo o processamento de dados localmente no navegador do usuário (Client-Side), assegurando que dados sensíveis ou estratégicos não sejam enviados a servidores externos.

## 2. Metodologia Matemática

O algoritmo de classificação baseia-se em um modelo de **Otimização Multicritério Ponderada**, combinando variáveis quantitativas de carga horária e avaliações qualitativas de mérito acadêmico.

### 2.1. Dimensão Quantitativa (Índice $W_i$)

Para cada solicitação $i$, calcula-se o esforço docente ponderado ($W_i$) através da combinação linear das cargas horárias médias, conforme **Anexo I**:

$$W_i = 0,25 \cdot \text{CHDEG}_i + 0,15 \cdot \text{CHDPG}_i + 0,10 \cdot \text{CHDAA}_i + 0,25 \cdot \text{CHMD}_i + 0,25 \cdot \text{CHDAE}_i$$

Onde:

- **CHDEG:** Carga Horária em Ensino de Graduação.
- **CHDPG:** Carga Horária em Pós-Graduação.
- **CHDAA:** Carga Horária em Atividades Acadêmicas.
- **CHMD:** Carga Horária Média Didática.
- **CHDAE:** Carga Horária em Extensão.

A normalização deste índice para o intervalo $[0,1]$ é dada por:

$$FI_i = \frac{W_i}{\max(W)}$$

Sendo $\max(W)$ o maior valor de $W$ encontrado no conjunto de solicitações.

### 2.2. Dimensão Qualitativa (Índice $Q_i$)

Baseada na avaliação de mérito descrita no **Anexo III**, onde cada departamento é avaliado em $K=5$ critérios com notas $s_{ik} \in [1, 5]$. A nota média bruta ($\bar{S}_i$) é normalizada para gerar o índice qualitativo:

$$QI_i = \frac{\bar{S}_i}{5} = \frac{\frac{1}{K}\sum_{k=1}^{K} s_{ik}}{5}$$

### 2.3. Fração Ideal Total (Classificação Final)

A pontuação final ($FI^{\text{total}}_i$) é a média ponderada das dimensões normalizadas, parametrizada por $\alpha$ e $\beta$ (atualmente $\alpha=0,5$ e $\beta=0,5$):

$$FI_i^{\text{total}} = 100 \cdot [\alpha \cdot FI_i + \beta \cdot QI_i]$$

## 3. Estrutura de Dados e Processamento

O sistema adota uma abordagem injetora para o tratamento de dados:

1. **Entrada Quantitativa:** Tratada como lista sequencial ($S_1, S_2, ..., S_n$), preservando múltiplas solicitações do mesmo departamento (ex: vagas distintas).
2. **Entrada Qualitativa:** Mapeada como filas de avaliação por departamento. O algoritmo realiza o cruzamento dos dados utilizando uma estratégia **FIFO (First-In, First-Out)** para associar as notas qualitativas às solicitações quantitativas correspondentes.

## 4. Tecnologias e Conformidade

- **Linguagens:** HTML5, JavaScript (ES6+), CSS3.
- **Bibliotecas:**
  - *PapaParse:* Processamento robusto de arquivos CSV.
  - *MathJax:* Renderização de equações matemáticas em LaTeX.
  - *Chart.js:* Visualização de dados estatísticos.
  - *Tailwind CSS:* Estilização responsiva.
- **Identidade Visual:** Conformidade com o Manual de Identidade Visual da UFRRJ (v1.2), utilizando a paleta oficial:
  - Azul Institucional: `#004388`
  - Amarelo Institucional: `#FFCC00`
  - Verde Institucional: `#008237`

## 5. Instruções de Uso

1. Acesse a página da ferramenta em https://marluciobarbosa.github.io/cepe.
2. **Etapa 1:** Realize o upload dos arquivos `.csv` referentes aos dados Quantitativos e Qualitativos (veja os arquivos .CSV de exemplo para replicar a estrutura) ou do arquivo `.xlsx`(veja o arquivo .XLSX de exemplo para sobrescrever os valores).
3. **Configuração:** Defina o número de vagas a classificar (Corte) no campo indicado.
4. Clique em **"Calcular Classificação Final"**.
5. Analise os gráficos de diagnóstico e a tabela analítica.
6. Utilize o botão **"Imprimir Relatório"** para gerar o PDF oficial. O layout de impressão é otimizado para ocultar elementos interativos e gráficos, focando na tabela de dados para formalização em ata.

*Desenvolvido sob diretrizes da Pró-Reitoria de Planejamento, Avaliação e Desenvolvimento Institucional da UFRRJ.*
