---
name: siagri-rtm-field-audit
description: Mapear campos de notas de entrada e saída do Siagri em relatórios RTM, conferir SQL, metadados e colunas visíveis, e preparar importações fiscais sem confundir nomes cadastrados com valores realmente exportados.
---

# Auditoria de campos Siagri em RTM

Use esta skill ao investigar quais campos do Siagri alimentam uma conferência de notas, ao corrigir um RTM ou ao montar uma matriz entre um importador, o relatório e um catálogo de tabelas. O [mapa de campos](references/field-map.md) reúne candidatos já identificados; confirme sua disponibilidade e significado na instalação em análise.

## Como estabelecer o mapeamento

1. Comece pelos campos que o sistema de destino efetivamente lê. Registre o nome esperado na importação, o nível (nota ou item), a unidade (total, unitário, por tonelada, percentual) e a finalidade.
2. Localize o candidato no catálogo de tabelas/colunas fornecido para aquela instalação. Guarde o nome físico da tabela e do campo e a descrição original. Não use uma semelhança de texto como prova de equivalência fiscal.
3. No RTM, examine separadamente:
   - `SQLText.Strings`: o campo físico realmente selecionado e seu alias de saída;
   - `TdaField`: metadado da consulta;
   - `TppField`: campo cadastrado no pipeline;
   - `TppDBText.DataField` e o `TppLabel.Caption`: valor e título efetivamente exibidos.
4. Classifique cada mapeamento como **exportado**, **somente SQL**, **somente cadastrado**, **candidato no catálogo** ou **não identificado**. Só considere exportado quando consulta e coluna de saída apontarem para o mesmo valor. Um `TppField` isolado não prova que o campo está no resultado.
5. Compare valores de notas reais, incluindo casos com valor zero, valor no cabeçalho e valor lançado diretamente no item. Registre a evidência antes de afirmar se um campo traz valor bruto, líquido, rateado ou unitário.

## Ao alterar um RTM

- Preserve a hierarquia dos objetos Delphi/ReportBuilder e os nomes de pipeline e data view existentes.
- Inclua o campo na consulta e, quando o RTM usar metadados explícitos, no `TdaField` e no `TppField`. Dê nome de coluna distinto ao valor total, unitário e diagnóstico.
- Ligue `TppDBText.DataField` ao nome retornado pela consulta e confira o título em `TppLabel.Caption`. Verifique posições, largura da página e colisões entre colunas.
- Faça uma checagem estática de todos os campos exibidos, mas trate a execução no Siagri como validação separada. Erros de leitura do RTM e diferenças nos valores exigem diagnóstico com a versão e a base em uso.
- Se um campo for apenas cadastrado no designer e não for necessário à importação, corrija um título enganoso sem acrescentá-lo automaticamente à consulta.

## Decisões fiscais que exigem teste de valores

Frete de item, desconto/acréscimo, valores líquidos e tributos retidos podem existir tanto no cabeçalho como no item e podem ter rateio interno. Não some ou substitua esses valores com base apenas na descrição do catálogo. Para rateios, conserve o total do cabeçalho até os centavos e documente o critério de arredondamento. Ao comparar origem do CST, aplique as regras do contexto da operação e da legislação vigente, sem deduzir a regra a partir do nome de uma coluna.

## Saída esperada

Entregue uma matriz com: necessidade do importador, direção, nível, campo físico, coluna/alias do RTM, situação estrutural, transformação proposta e resultado do teste em nota real. Separe fatos verificados, hipóteses e pendências.

