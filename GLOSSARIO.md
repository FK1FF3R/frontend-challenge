# Glossário do Domínio - Marketplace de NFTs

## Contexto 1: Wallet e Ativos (`src/wallet/`)
* **NFT (Non-Fungible Token):** Ativo digital colecionável e único que pertence ao inventário de um utilizador.
* **Carteira (Wallet):** O meio utilizado para armazenar fundos (ETH) e gerir os ativos digitais associados à conta.
* **Item:** Termo genérico usado na camada de armazenamento para representar qualquer objeto digital bruto antes de entrar na vitrine.

## Contexto 2: Marketplace e Checkout (`src/marketplace/`)
* **Lance (Bid):** Oferta de valor em criptomoeda feita por um colecionador durante um leilão ativo.
* **Leilão (Auction):** Evento temporizado de venda onde o maior lance arremata o direito sobre o NFT.
* **Item:** O mesmo ativo, mas agora encarado sob a perspetiva comercial — um lote ativo na vitrine, sujeito a cotações dinâmicas e regras de carrinho e checkout.
* **Edição:** Cada exemplar disponível de um mesmo Item na vitrine. A quantidade no carrinho conta edições (sempre um número inteiro) e está limitada ao stock de edições disponíveis.

## Palavra Polissêmica (Muda de sentido):
* **Item:** Na `Wallet`, é apenas um objeto estático guardado no perfil. No `Marketplace`, é o produto dinâmico sujeito a alterações de preço em tempo real (via Socket.IO) e validação de stock.