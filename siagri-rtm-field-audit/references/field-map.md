# Mapa inicial de campos Siagri

Este mapa foi extraído de um catálogo de campos fornecido por um usuário e de RTMs de exemplo. Ele **não é documentação oficial do Siagri** nem garante que todas as versões mantenham os mesmos nomes ou a mesma semântica. Consulte o catálogo da instalação e teste valores antes de automatizar uma conferência.

## Tabelas principais

| Direção | Cabeçalho | Itens | Ligação encontrada nos RTMs analisados |
|---|---|---|---|
| Entradas de terceiros | `NFENTRA` | `INFENTRA` | `CTRL_NFE`, `NUME_NFE`, `CODI_EMP`, `CODI_TRA` |
| Saídas | `NOTA` | `INOTA` | `NPRE_NOT` |

Os RTMs também consultavam `TRANSAC` (parceiro), `CADEMP` (empresa), `PRODSERV` e `PRODUTO`. Reavalie a cardinalidade dos joins para não multiplicar itens.

## Identificação e valores da nota

| Uso | Entradas | Saídas | Observação |
|---|---|---|---|
| Chave da NF-e | `NFENTRA.CHAV_NFE` | `NOTA.CHAV_NOT` | Use como chave de conciliação quando disponível. |
| Número e série | `NUME_NFE`, `SERI_NFE` | `NOTA_NOT`, `SERI_NOT` | Combine com empresa/parceiro conforme a operação. |
| Código do parceiro | `NFENTRA.CODI_TRA` | `NOTA.CODI_TRA` | Não confundir com código da empresa. |
| Total da nota | `NFENTRA.TOTA_NFE` | `NOTA.TOTA_NOT` | Valor do cabeçalho. |
| Total de produtos | `NFENTRA.TPRO_NFE` | `NOTA.TPRO_NOT` | Pode diferir do total da nota. |
| Frete da nota | `NFENTRA.FRET_NFE` | `NOTA.FRET_NOT` | Valor do cabeçalho; teste o rateio praticado pela instalação. |
| Seguro da nota | `NFENTRA.SEGU_NFE` | `NOTA.SEGU_NOT` | Valor do cabeçalho. |
| Desconto/acréscimo | `NFENTRA.DSAC_NFE` | `NOTA.DESC_NOT` e `NOTA.ACRE_NOT` | Em entradas, verifique sinal e significado em notas reais. |
| ISS do cabeçalho | `NFENTRA.VISS_NFE` | Sem equivalente confirmado neste mapa | Verifique tipo de documento. |
| SENAR do cabeçalho | `NFENTRA.VLSE_NFE` | Sem equivalente confirmado neste mapa | Compare com a soma dos itens. |
| Funrural/SENAR do cabeçalho | `NFENTRA.VLFR_NFE` | Sem equivalente confirmado neste mapa | Campo descrito como valor total da alíquota combinada. |
| IRRF total | `NFENTRA.VLIR_NFE` | `NOTA.VLIR_NOT` | Retenção do cabeçalho. |
| Contribuição social retida | `NFENTRA.VRCS_NFE` | `NOTA.VRCS_NOT` | Descrição do catálogo menciona serviços; confirme se corresponde a CSLL no caso concreto. |

## Itens e tributos

| Uso | Entradas | Saídas | Observação |
|---|---|---|---|
| Sequência do item | `INFENTRA.ITEM_INF` | `INOTA.ITEM_INO` | Identifica linhas da nota. |
| Produto e quantidade | `CODI_PSV`, `QUAN_INF` | `CODI_PSV`, `QTDE_INO` | Use a quantidade da unidade apropriada. |
| Valor unitário | `INFENTRA.VLOR_INF` | `INOTA.VLOR_INO` | Não equivale necessariamente ao valor líquido. |
| Desconto/acréscimo do item | `INFENTRA.DSAC_INF` | `INOTA.DSAC_INO` | Verifique sinal e parcelas rateadas. |
| Ajuste rateado do cabeçalho | `INFENTRA.DART_INF` | `INOTA.DART_INO` | Não some ao desconto do item sem verificar duplicidade. |
| Valor líquido | `INFENTRA.VLIQ_INF`, `VLI2_INF`, `VLI3_INF` | `INOTA.VLIQ_INO` | As variantes incluem componentes diferentes; não use como valor bruto sem conciliar. |
| Frete total do item | `INFENTRA.VFRE_INF` | `INOTA.TFRE_INO` | Candidato direto ao valor monetário por item. |
| Frete por unidade | `INFENTRA.VFUN_INF` | Não confirmado neste mapa | Multiplique pela quantidade apenas após confirmar a unidade. |
| Frete por tonelada | `INFENTRA.VFRT_INF` | Não confirmado neste mapa | Não equivale ao frete total do item. |
| ISS do item | `INFENTRA.VISS_INF` | `INOTA.VISS_INO` | A descrição de entrada menciona cupom; teste o tipo de nota. |
| SENAR do item | `INFENTRA.VLSE_INF` | `INOTA.VLSE_INO` | Valor por item. |
| Funrural/SENAR do item | `INFENTRA.VLFR_INF` | `INOTA.VLFR_INO` | Valor por item. |
| CST/CSOSN | `INFENTRA.TRIB_INF`, `CSOS_INF` | `INOTA.TRIB_INO`, `CSOS_INO` | A origem do CST pode exigir regra de conversão conforme a operação. |
| ICMS | `BICM_INF`, `AICM_INF`, `VICM_INF` | `BICM_INO`, `AICM_INO`, `VICM_INO` | Base, alíquota e valor. |
| ICMS ST | `INFENTRA.VICS_INF` | `INOTA.VICS_INO` | Valor do item. |
| PIS/COFINS | `BPIS_INF`, `PIS_INF`, `BCOF_INF`, `COFI_INF` | `BPIS_INO`, `PIS_INO`, `BCOF_INO`, `COFI_INO` | Bases e valores; consulte também CST próprios. |
| IPI | `INFENTRA.TIPI_INF` | `INOTA.VIPI_INO` | Valor do item. |

## Alerta de RTM

Um RTM analisado continha `VFRE_INF` apenas em `TppField`, com título de alíquota de IPI. A consulta selecionava `PIPI_INF` para a coluna visível. Esse caso demonstra por que o cadastro de campos do designer não basta: confirme `SQLText.Strings`, metadados e `TppDBText.DataField` separadamente.

