# Especificação Enxuta: Carrinho e Checkout (Revalidação)

**Contexto:** Marketplace e Checkout (`src/marketplace/`)

## 1. Critérios EARS
1. **Ubíquo:** O sistema de checkout deve sempre validar o preço e a disponibilidade dos Itens na API antes de confirmar o pedido (Origem: RESEARCH.md - A Dor do Colecionador / README §3 - Pagamento e confirmação).
2. **Baseado em Evento:** Sempre que o evento de atualização `nft.updated` for recebido via Socket.IO, a interface do carrinho deve atualizar imediatamente o resumo de valores (Origem: RESEARCH.md - O Que Deve Acontecer / README §7 - Tempo real).
3. **Estado Indesejado:** Se a cotação do Item for alterada enquanto o pagamento está a ser revisto, o sistema não deve permitir a submissão do pedido sem o aceite do novo preço (Origem: RESEARCH.md - A Dor do Colecionador / README §3 - Pagamento e confirmação).
4. **Opcional:** Onde um cupom de desconto estiver aplicado ao carrinho, o sistema de checkout deve revalidar o cupom na API, junto com a cotação, antes de confirmar o pedido (Origem: README §3 - Carrinho, "Aplicar e remover cupom" / README §3 - Pagamento e confirmação, "Revalidar preço, disponibilidade, cupom e taxas").
5. **Complexo:** Enquanto a criação do pedido estiver pendente, quando o colecionador clicar de novo em enviar, o sistema deve reaproveitar o mesmo pedido (chave de idempotência) em vez de criar outro (Origem: README §3 - Pagamento e confirmação).

## 2. Cenários BDD (Gherkin)

```gherkin
# language: pt
Funcionalidade: Checkout e Revalidação de Cotação

  Cenário: Bloqueio do checkout por desatualização de cotação (Cenário de Erro)
    Dado que tenho o Item "NFT #1" no carrinho com o preço de "1 ETH"
    Quando tento confirmar o pedido e a API retorna que o preço atualizou para "1.5 ETH"
    Então o sistema deve bloquear o envio e exibir um alerta para aceitar o novo valor

  Cenário: Aceite do novo preço após bloqueio
    Dado que o envio do pedido foi bloqueado porque o preço do Item "NFT #1" mudou de "1 ETH" para "1.5 ETH"
    Quando aceito o novo valor
    Então o sistema gera o pedido com o preço de "1.5 ETH"

  Cenário: Recusa do novo preço após bloqueio
    Dado que o envio do pedido foi bloqueado porque o preço do Item "NFT #1" mudou de "1 ETH" para "1.5 ETH"
    Quando recuso o novo valor
    Então nenhum pedido é gerado e o Item permanece no carrinho com o preço de "1.5 ETH"

  Cenário: Item esgotado por evento durante o checkout (Cenário Limite)
    Dado que o Item "NFT #2" está no meu carrinho aguardando pagamento
    Quando recebo um evento informando que a disponibilidade mudou para "0"
    Então o sistema deve remover o Item do carrinho, apresentar um aviso visual e exigir a confirmação do novo valor total antes de gerar o pedido

  Cenário: Item esgotado detetado apenas na revalidação (Cenário Limite)
    Dado que o Item "NFT #2" está no meu carrinho e nenhum evento de esgotamento foi recebido
    Quando tento confirmar o pedido e a API retorna que a disponibilidade do Item é "0"
    Então o sistema deve bloquear o envio, remover o Item do carrinho, apresentar um aviso visual e exigir a confirmação do novo valor total

  Cenário: Sucesso no pagamento com cotação válida (Caminho Feliz)
    Dado que os preços do meu carrinho estão sincronizados com a API
    Quando submeto o pedido de pagamento
    Então o sistema gera a confirmação e remove do carrinho apenas os Itens e quantidades comprados