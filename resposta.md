# 📝 Resposta do Laboratório: A Wiki Perdida dos Arquivos Corporativos

> Preencha este arquivo com a sua proposta de solução.
>
> Sua resposta deve explicar como transformar os documentos brutos da pasta `raw/` em uma Wiki Corporativa Inteligente, pesquisável e segura usando apenas serviços da AWS.

---

## 👤 Identificação

**Nome:**  
Eduarda Gutterres 

**Data:**  
15.09.2026

**Link do repositório:**  
https://github.com/eduardagutterres/laboratorio-wiki-aws
---

# ✅ Quest 1: O Mapa dos Arquivos Perdidos

## 1.1 Formatos encontrados na pasta `raw/`

**Sua resposta:**

```md
- *PDF (ata_reuniao_vendas_sa.pdf)*: documento digital com texto que pode ser extraído diretamente, sem necessidade de OCR. Contém informações como participantes, indicadores, decisões, responsáveis, prazos, riscos e próximas ações.

- *PNG (ata_resultados_vendas_novos_dados.png)*: documento em formato de imagem, sem texto nativo. Precisa de OCR para reconhecer o conteúdo. Possui texto, tabelas, listas e anotações manuscritas, o que exige mais cuidado na extração.

- *CSV (vendas_sa_dados_ficticios_laboratorio.csv)*: arquivo estruturado de CRM, organizado em linhas e colunas. Não precisa de OCR. Contém 240 registros e 19 campos com informações de oportunidades comerciais.
```

---

## 1.2 Principais desafios encontrados

**Sua resposta:**

```md
- *PDF:* o principal desafio é manter o contexto das informações, já que o documento possui seções, listas e tabelas com indicadores, decisões, responsáveis, prazos, ações e riscos.

- *PNG:* como é uma imagem, precisa de OCR. As tabelas e as anotações manuscritas podem dificultar o reconhecimento do texto e exigir conferência do conteúdo extraído.

- *CSV:* o desafio é organizar e padronizar os campos sem perder a relação entre as informações de cada oportunidade comercial.

- *Desafio geral:* como os três arquivos têm formatos diferentes, será necessário organizar e padronizar as informações para que possam ser pesquisadas juntas na Wiki.
```

---

## 1.3 Informações importantes a serem extraídas

**Sua resposta:**

```md
- *PDF:* datas, participantes, indicadores, decisões, responsáveis, prazos, ações, riscos e informações sobre a próxima reunião.

- *PNG:* datas, resultados, indicadores, observações, decisões, responsáveis, prazos e também as anotações manuscritas presentes no documento.

- *CSV:* identificação da oportunidade, cliente, segmento, região, vendedor, origem do lead, produto, campanha, status, probabilidade, valores, desconto, ciclo de vendas, motivo de perda, próxima atividade e observações.

Essas informações devem ser mantidas junto com a identificação do arquivo de origem, para facilitar a pesquisa e permitir saber de onde cada informação foi retirada.
```

---

## 1.4 Estratégia de classificação inicial

**Sua resposta:**

```md
Os arquivos podem ser classificados por metadados, sem precisar criar subpastas. Para cada arquivo seriam registrados dados como nome do arquivo, tipo de documento, formato, data, assunto e origem.

Uma classificação inicial poderia ser:

- `tipo_documento`: ata de reunião, resultado comercial ou dados de CRM;
- `formato`: PDF, PNG ou CSV;
- `origem`: pasta raw;
- `area`: comercial/vendas;
- `data_documento`: data relacionada ao conteúdo, quando disponível.

Assim, os documentos continuam armazenados no mesmo local, mas podem ser filtrados e pesquisados de acordo com seus metadados.
```

---

# ✅ Quest 2: O Portal de Entrada na AWS

## 2.1 Armazenamento dos arquivos brutos

**Sua resposta:**

```md
Os arquivos da pasta raw/ podem ser enviados para um bucket do Amazon S3, que seria usado para armazenar os documentos originais.

O acesso a esses arquivos pode ser controlado pelo AWS IAM, permitindo que apenas usuários ou serviços autorizados consigam acessar ou enviar documentos para o bucket.

Dessa forma, o Amazon S3 seria responsável pelo armazenamento dos arquivos, enquanto o IAM ajudaria no controle de acesso e segurança.
```

---

## 2.2 Preservação dos arquivos originais

**Sua resposta:**

```md
Os arquivos originais podem ser mantidos no Amazon S3 em uma área específica para documentos brutos, sem alteração do conteúdo.

O acesso a esses arquivos pode ser controlado pelo AWS IAM, permitindo apenas a leitura para usuários e serviços que não precisam modificar os documentos.

Os arquivos processados ou transformados podem ser armazenados separadamente, evitando alterações nos arquivos originais. Assim, é possível manter o documento bruto preservado e identificar de onde vieram as informações utilizadas pela Wiki.
```

---

## 2.3 Extração de texto dos documentos

**Sua resposta:**

```md
- *PDF digital:* como já possui texto nativo, o conteúdo pode ser extraído diretamente, sem necessidade de OCR.

- *PNG:* por ser uma imagem, pode ser processado pelo Amazon Textract para reconhecer o texto, as tabelas e as anotações presentes no documento.

- *CSV:* como já possui dados estruturados em linhas e colunas, não precisa de OCR. Os campos podem ser lidos diretamente e organizados para pesquisa.

O AWS Step Functions pode organizar esse fluxo e direcionar cada tipo de arquivo para o processamento adequado.

Os arquivos originais continuam armazenados no Amazon S3, enquanto o conteúdo extraído segue para as próximas etapas da solução.
```

---

## 2.4 Tratamento de falhas

**Sua resposta:**

```md
Se ocorrer algum erro durante o processamento, o AWS Step Functions pode identificar em qual etapa o fluxo falhou.

O erro pode ser registrado com informações como nome do arquivo, etapa do processamento e motivo da falha.

O arquivo com erro pode continuar armazenado no Amazon S3 para ser verificado e processado novamente depois, sem perder o documento original.
```

---

# ✅ Quest 3: A Relíquia dos Metadados

## 3.1 Padronização dos textos processados

**Sua resposta:**

```md
Depois da extração, os textos podem passar por uma etapa de limpeza para remover espaços extras, caracteres desnecessários e possíveis erros de leitura.

Também é importante padronizar informações como datas, nomes de campos e formatos de valores, para que os dados fiquem mais organizados.

No caso do CSV, os nomes das colunas podem ser padronizados de forma consistente. Já nos documentos em PDF e PNG, o conteúdo pode ser dividido em partes menores, preservando títulos, tabelas e seções importantes.

Assim, o conteúdo fica mais limpo e preparado para ser pesquisado posteriormente.
```

---

## 3.2 Metadados propostos

Defina quais metadados você extrairia de cada documento.

| Metadado | Por que ele é importante? |
|---|---|
| Nome do documento | Permite identificar o arquivo de origem. |
| Tipo do documento | Ajuda a diferenciar atas, imagens e dados de CRM. |
| Data identificada | Permite localizar informações de determinado período. |
| Tema principal | Facilita a busca pelo assunto tratado no documento. |
| Participantes | Ajuda a encontrar informações relacionadas às pessoas citadas. |
| Decisões tomadas | Permite localizar decisões registradas nos documentos. |
| Responsáveis | Identifica quem ficou responsável por uma ação ou atividade. |
| Próximos passos | Facilita o acompanhamento das ações definidas. |
| Nível de confidencialidade | Ajuda a controlar quem pode acessar determinado conteúdo. |
| Caminho do arquivo original | Permite localizar o documento original no Amazon S3. |

---

## 3.3 Uso de IA para enriquecimento dos documentos

**Sua resposta:**

```md
O Amazon Bedrock pode analisar o conteúdo dos documentos já processados e ajudar a identificar informações importantes.

Ele pode reconhecer temas principais, decisões tomadas, responsáveis, pendências e próximos passos, além de gerar resumos dos documentos.

Essas informações podem ser adicionadas aos metadados para facilitar a organização e a pesquisa na Wiki.
```

---

## 3.4 Armazenamento dos metadados

**Sua resposta:**

```md
Os metadados podem ser armazenados junto com os conteúdos processados no Amazon S3, mantendo sempre uma referência ao arquivo original.

Cada registro pode guardar informações como nome do documento, tipo, data, tema principal e caminho do arquivo no S3.

Depois, essas informações podem ser usadas pelo Amazon Bedrock Knowledge Bases para relacionar o conteúdo processado aos documentos originais e facilitar as buscas na Wiki.
```

---

# ✅ Quest 4: O Oráculo da Wiki Inteligente

## 4.1 Estratégia de indexação

**Sua resposta:**

```md
Os documentos podem ser divididos em trechos menores para facilitar a busca.

No PDF e no PNG, a divisão pode respeitar seções, títulos, tabelas e blocos de texto, evitando separar informações que precisam ficar juntas.

No CSV, cada oportunidade pode ser tratada como um registro individual, mantendo seus campos relacionados.

Cada trecho deve continuar ligado aos seus metadados e ao arquivo original, para que a Wiki consiga localizar a fonte da informação.
```

---

## 4.2 Busca semântica e base vetorial

**Sua resposta:**

```md
Os trechos dos documentos podem ser transformados em embeddings por um modelo de embeddings disponível no Amazon Bedrock.

Esses embeddings representam o significado do conteúdo em formato vetorial e permitem encontrar informações pelo significado, e não apenas por palavras iguais.

O Amazon Bedrock Knowledge Bases pode organizar esse processo e usar o Amazon OpenSearch Serverless para armazenar os vetores.

Assim, quando o usuário fizer uma pergunta, a solução poderá localizar os trechos mais relacionados ao assunto pesquisado.
```

---

## 4.3 Geração de respostas com IA

**Sua resposta:**

```md
Quando o usuário fizer uma pergunta, a Wiki poderá buscar na base os trechos mais relacionados ao assunto.

O Amazon Bedrock pode usar esses trechos como contexto para gerar uma resposta em linguagem natural, baseada nas informações encontradas nos documentos.

A resposta também pode apresentar a referência do arquivo de origem, para que o usuário consiga conferir de onde a informação foi retirada.

Dessa forma, a Wiki gera respostas com base no conteúdo dos documentos armazenados, usando a IA para organizar e apresentar essas informações.
```

---

## 4.4 Interface de consulta

**Sua resposta:**

```md
Os usuários poderiam acessar a Wiki por meio de uma interface web simples.

O AWS Amplify pode ser usado para disponibilizar essa interface, enquanto o Amazon Cognito pode controlar o login e o acesso dos usuários.

Depois de entrar no sistema, o usuário poderia digitar perguntas em linguagem natural e receber respostas baseadas nos documentos armazenados na solução.

A interface também pode mostrar a referência do arquivo utilizado na resposta, permitindo que o usuário consulte a fonte original.
```

---

## 4.5 Segurança, auditoria e monitoramento

**Sua resposta:**

```md
O acesso aos arquivos e recursos pode ser controlado pelo AWS IAM, definindo quais usuários e serviços têm permissão para acessar cada recurso.

O Amazon Cognito pode ser usado para controlar o login dos usuários da Wiki e garantir que apenas pessoas autorizadas consigam acessar a aplicação.

As consultas e possíveis erros de processamento também podem ser registrados para facilitar o acompanhamento do funcionamento da solução.

Assim, a solução mantém controle de acesso, proteção dos documentos e possibilidade de acompanhar falhas e uso da Wiki.
```

---

# 🧩 Arquitetura Final da Solução

Agora reúna tudo em uma visão única.

## 1. Visão geral

**Sua resposta:**

```md
A solução recebe os arquivos da pasta raw/, armazena os documentos originais no Amazon S3 e processa cada formato de acordo com sua necessidade.

As imagens podem ser tratadas com Amazon Textract, enquanto PDF digital e CSV seguem por extração direta. O fluxo pode ser organizado pelo AWS Step Functions.

Depois, os conteúdos são padronizados, enriquecidos com metadados e utilizados pelo Amazon Bedrock Knowledge Bases para busca semântica e geração de respostas em linguagem natural.

O acesso à Wiki pode ser feito por uma interface web, com controle de usuários e permissões por Amazon Cognito e AWS IAM.
```

---

## 2. Serviços AWS utilizados

 Serviço AWS | Papel na solução |
|---|---|
| Amazon S3 | Armazena os arquivos originais, conteúdos processados e referências aos arquivos. |
| Amazon Textract | Extrai texto, tabelas e informações de documentos em formato de imagem. |
| Amazon Bedrock | Analisa o conteúdo e ajuda a gerar respostas em linguagem natural. |
| Amazon Bedrock Knowledge Bases | Organiza o conhecimento, relaciona os documentos e permite busca semântica. |
| AWS Step Functions | Organiza o fluxo de processamento dos diferentes tipos de arquivos. |
| Amazon OpenSearch Serverless | Armazena os vetores usados na busca semântica. |
| AWS IAM | Controla permissões e acesso aos recursos da solução. |
| Amazon Cognito | Controla o login e o acesso dos usuários à Wiki. |
| AWS Amplify | Disponibiliza a interface web usada para acessar a Wiki. |

---

## 3. Fluxo de dados de ponta a ponta

**Sua resposta:**

```md
1. Os arquivos ficam inicialmente na pasta raw/.
2. Os arquivos são enviados para o Amazon S3, onde ficam armazenados os originais.
3. As imagens são processadas pelo Amazon Textract para extração do texto.
4. O PDF digital e o CSV têm seus conteúdos extraídos diretamente.
5. O AWS Step Functions organiza o fluxo de processamento de cada tipo de arquivo.
6. Os conteúdos extraídos são limpos, padronizados e recebem metadados.
7. O Amazon Bedrock Knowledge Bases organiza os conteúdos para busca semântica, utilizando o Amazon OpenSearch Serverless para armazenar os vetores.
8. O usuário acessa a Wiki por uma interface web disponibilizada pelo AWS Amplify, com login pelo Amazon Cognito.
9. O Amazon Bedrock usa os trechos encontrados nos documentos para gerar a resposta em linguagem natural e apresentar a referência do arquivo de origem.
```

---

## 4. Diagrama textual da arquitetura

**Sua resposta:**

```md
raw/ → Amazon S3 → AWS Step Functions → Amazon Textract / extração direta → conteúdos processados e metadados → Amazon Bedrock Knowledge Bases → Amazon OpenSearch Serverless → Amazon Bedrock → AWS Amplify → Amazon Cognito / AWS IAM → Usuário
```

---

## 5. Riscos e limitações

**Sua resposta:**

```md
- Imagens com baixa qualidade ou anotações manuscritas podem dificultar a extração correta pelo Amazon Textract.
- Informações extraídas automaticamente podem precisar de conferência antes de serem usadas na Wiki.
- A qualidade das respostas depende da qualidade dos documentos e dos metadados.
- O aumento da quantidade de arquivos e consultas pode aumentar os custos dos serviços utilizados.
- As respostas devem manter referência ao arquivo de origem para facilitar a conferência das informações.
```

---

## 6. Melhorias futuras

**Sua resposta:**

```md
- Melhorar a interface web para facilitar as consultas.
- Adicionar novos tipos de documentos à base de conhecimento.
- Criar alertas para erros de processamento.
- Melhorar a validação de textos extraídos de imagens e anotações manuscritas.
- Adicionar filtros de busca por data, tipo de documento, responsável ou assunto.
```

---

# 🧠 Checklist Final

Antes de entregar, confirme se sua solução responde:

- [x] Como transformar documentos escaneados em texto?
- [x] Como lidar com diferentes formatos dentro da mesma pasta `raw/`?
- [x] Como armazenar os documentos originais?
- [x] Como preservar a rastreabilidade entre resposta e documento fonte?
- [x] Como organizar metadados?
- [x] Como criar busca semântica?
- [x] Como usar Amazon Bedrock na solução?
- [x] Como proteger documentos sensíveis?
- [x] Como monitorar falhas?
- [x] Como a empresa usaria essa Wiki no dia a dia?

---

# 🏁 Conclusão

**Sua resposta:**

```md
A solução proposta permite transformar arquivos de formatos diferentes em uma base de conhecimento organizada e pesquisável.

Com os serviços da AWS, os documentos originais permanecem armazenados e protegidos, enquanto seus conteúdos são extraídos, organizados e preparados para busca.

A Wiki Inteligente facilita o acesso às informações da empresa, permitindo consultas em linguagem natural e mantendo a referência aos documentos de origem.

Além de facilitar a localização das informações, a solução pode evoluir conforme a quantidade de documentos e as necessidades da empresa aumentarem.
```
