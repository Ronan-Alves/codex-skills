# Catálogo de tabelas e campos

`catalogo-campos-siagri.tsv` é uma conversão da planilha **Campos e Tabelas do Siagri.xlsx** fornecida pelo usuário. Contém 44.100 linhas de campos em 2.329 tabelas, com as sete colunas da planilha na mesma ordem:

1. Nome da tabela
2. Apelido da tabela
3. Nome do campo
4. Apelido do campo
5. Tipo do campo
6. Selecionado
7. Obrigatório

O formato é UTF-8 com colunas separadas por tabulação. Abra em editor de texto, planilha ou use busca exata por identificador (`NFENTRA`, `VFRE_INF` etc.). Campos de texto com tabulação ou quebra de linha foram colocados em uma só linha para facilitar busca e leitura por IA.

As descrições foram mantidas como estavam no arquivo de origem, incluindo caracteres `�` que já apareciam na leitura da planilha. O catálogo não é documentação oficial do fornecedor e pode variar entre versões. Ele lista metadados de campos, sem dados de notas ou clientes. Confirme a semântica e o valor efetivo no banco antes de usar um campo em cálculo ou importação.

