# Automação de Relatórios de Atendimento

Este projeto automatiza a geração e envio de relatórios de atendimento. Ele utiliza Google Sheets para armazenar dados, Make (antigo Integromat) para processar e consolidar informações, Google Docs para criar relatórios, e Outlook para enviar os relatórios como PDF anexado por e-mail. 

O objetivo principal é demonstrar o uso de tecnologias de automação em um fluxo simples, funcional, e replicável.

![PIPELINE DO MAKE](images/make-complete-pipeline)

---

## **Tecnologias Utilizadas**

- **Make (Integromat):** Plataforma de integração e automação para criar cenários conectando serviços como Google Sheets, Google Docs e Outlook.
- **Google Sheets:** Armazenamento dos dados de atendimento (base fictícia utilizada para o projeto).
- **Google Docs:** Geração do relatório com base nos dados consolidados.
- **Outlook:** Envio do relatório gerado como anexo de e-mail.

---

## **Fluxo do Projeto**

### **Passo a Passo do Fluxo**

1. **Dados no Google Sheets:**
   - Criamos uma planilha chamada `Métricas de Atendimento` com as seguintes colunas:
     - **Data:** Data do atendimento.
     - **Número de Consultas:** Total de consultas realizadas no dia.
     - **Cancelamentos:** Total de consultas canceladas (não utilizado na configuração final).
     - **Duração Média:** Duração média das consultas (não utilizado na configuração final).

2. **Conexão com o Google Sheets (Make):**
   - Módulo usado: **Google Sheets > Search Rows (Advanced)**.
   - Configuração:
     - **Spreadsheet ID:** Selecionada a planilha `Métricas de Atendimento`.
     - **Sheet ID:** Selecionada a aba `Página1`.
     - **Query:** `select * where A is not null` para capturar todas as linhas com a coluna `Data` preenchida.

3. **Consolidação das Métricas (Make):**
   - Módulo usado: **Tools > Numeric Aggregator**.
   - Configuração:
     - **Source Module:** Google Sheets - Search Rows.
     - **Aggregate Function:** `SUM`.
     - **Value:** Coluna **Número de Consultas**.
   - Resultado: Total de consultas consolidado.

4. **Geração do Relatório (Google Docs):**
   - Módulo usado: **Google Docs > Create a Document**.
   - Configuração:
     - **Name:** Nome do documento gerado dinamicamente: `Relatorio-Atendimento-{{DataAtual}}`.
     - **Content:** Modelo HTML simples:
       ```html
       <h1>Relatório de Atendimento</h1>
       <p><strong>Total de Consultas:</strong> {{TotalConsultas}}</p>
       <p><strong>Data de Geração:</strong> {{DataAtual}}</p>
       ```
     - **Document ID:** Não necessário, pois o documento é criado dinamicamente.

5. **Exportação para PDF (Make):**
   - Módulo usado: **Google Docs > Download a Document**.
   - Configuração:
     - **Document ID:** Mapeado dinamicamente do módulo **Google Docs > Create a Document**.
     - **Type:** `PDF Document (.pdf)`.

6. **Envio por E-mail (Make):**
   - Módulo usado: **Email > Send Email (Outlook)**.
   - Configuração:
     - **To:** Endereço de e-mail do destinatário.
     - **Subject:** `Relatório de Atendimento`.
     - **Content:** Corpo do e-mail em HTML:
       ```html
       <p>Prezados,</p>
       <p>Segue em anexo o Relatório de Atendimento gerado automaticamente.</p>
       <p>Atenciosamente,<br>Equipe</p>
       ```
     - **Attachment:** Mapeado do PDF gerado no módulo anterior.

---

## **Detalhes Importantes**

- Este projeto utiliza um **modelo simples** de relatório e configurações para demonstrar o funcionamento das tecnologias de automação. O foco está na integração entre os serviços e no fluxo funcional.
- **Dados Fictícios:** A base de dados utilizada no Google Sheets contém valores fictícios, criados exclusivamente para testar o fluxo.
- **Extensibilidade:** O fluxo pode ser expandido para incluir métricas adicionais ou relatórios mais detalhados conforme necessário.

---

## **Como Reproduzir o Projeto**

1. **Configurar o Make:**
   - Crie uma conta no [Make](https://www.make.com/).
   - Configure os módulos seguindo o fluxo descrito.

2. **Criar Planilha no Google Sheets:**
   - Estruture a planilha conforme descrito na seção de dados.

3. **Conectar Google Docs e Outlook:**
   - Conecte suas contas ao Make para permitir o acesso.
   - Configure as permissões necessárias.

4. **Testar o Fluxo:**
   - Execute o cenário no Make clicando em **Run Once** e verifique os resultados.

---

## **Arquivos Disponíveis**
- Exemplo do PDF gerado.
- Screenshots das configurações no Make.

---

Se houver dúvidas ou necessidade de suporte adicional, fico à disposição.
