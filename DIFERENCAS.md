# Análise de Diferenças na Spec (Leitura Estática)

Como não pude executar o validador via CLI por restrições de permissões no Windows e falta de Python, o agente Claude conferiu a spec apenas lendo os arquivos. Durante a geração inicial:
1. O agente sugeriu adicionar validações mais estritas de stock nas mensagens de erro do carrinho (Cenário 2).
2. Ele alterou o meu cenário inicial que usava "NFT", substituindo para "Item" após eu forçar o alinhamento com o Glossário.
3. Não fez perguntas para desambiguar a relação entre Edições e Itens, assumindo que eram sinónimos, o que me obrigou a adicionar o termo Edição ao Glossário manualmente.