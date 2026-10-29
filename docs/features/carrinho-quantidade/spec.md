# language: pt
Funcionalidade: Ajuste de quantidade de Itens no carrinho
  Como um colecionador
  Quero alterar a quantidade de edições de um Item no carrinho
  Para garantir que o subtotal reflete exatamente o que desejo comprar antes de ir para o pagamento

  Cenário: Alterar quantidade com sucesso
    Dado que tenho "1" edição do Item "CryptoPunk #123" no carrinho
    Quando altero a quantidade para "2" edições
    Então o subtotal deve ser recalculado com base no novo valor unitário da cotação

  Cenário: Tentar ultrapassar o limite de stock disponível (Cenário de Erro)
    Dado que o Item "Art Block #789" tem apenas "1" edição restante na vitrine
    Quando tento alterar a quantidade no carrinho para "3" edições
    Então o sistema deve bloquear a ação, exibir uma mensagem de erro de limite de stock excedido e manter a quantidade anterior

  Cenário: Remoção automática por indisponibilidade em tempo real
    Dado que possuo o Item "Bored Ape #456" selecionado no carrinho
    Quando recebo um evento informando que o Item foi esgotado na vitrine
    Então o sistema deve remover o Item do carrinho e apresentar um aviso visual