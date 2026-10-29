# ADR 0001: Revalidação Síncrona de Cotação no Checkout

## Status
Aceite

## Contexto
No marketplace de NFTs, a cotação e a disponibilidade de um ativo podem mudar rapidamente através de eventos em tempo real. Se um colecionador mantiver um Item no carrinho e o preço alterar em segundo plano, permitir que o checkout confirme a compra com o valor antigo gera um estado financeiro inconsistente e prejuízo (conforme mapeado na nossa pesquisa).

## Decisão
Adotamos uma abordagem de revalidação estrita: no exato momento em que o utilizador submete o pagamento, o sistema deve consultar a API para verificar a cotação atualizada do carrinho. Se houver divergência de preço ou Item esgotado, a transação é bloqueada, e o sistema exige uma nova confirmação do utilizador com o novo valor total antes de gerar o pedido. Um Item esgotado é sempre removido automaticamente do carrinho, com aviso visual, seja detetado por evento em tempo real ou por esta revalidação; o novo valor total a confirmar já exclui esse Item.

## Consequências
- **Positivas:** Garante a integridade financeira e evita compras de itens já vendidos a terceiros.
- **Negativas:** Adiciona latência ao botão de compra e exige uma interface clara para comunicar o erro de divergência ao utilizador sem o frustrar.