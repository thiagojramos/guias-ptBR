## Ferramentas de Conjunto de Dados

Uma ferramenta que ajuda a gerar legendas com BLIP, BLIP2 ou WD14 e máscaras para treinamento mascarado usando ClipSeg ou Rembg. Para a geração de legendas, se você definir uma legenda inicial, ele começa a gerar a partir desse texto em vez de uma string vazia (apenas BLIP e BLIP2).

### Geração de Legendas
Se você definir uma legenda inicial, ele começa a gerar a partir desse texto em vez de uma string vazia (apenas BLIP e BLIP2). Você também pode usar um prefixo e sufixo de legenda, útil com WD14, lembrando que ele não adiciona espaços, então você precisa adicionar ou "," ou ".".

### Geração de Máscaras
Existem algumas ferramentas diferentes para gerar automaticamente uma máscara, dependendo do tipo de imagem, incluídas no OneTrainer sob o botão de ferramentas de conjunto de dados.
Com a ferramenta de geração de máscaras em lote, você pode usar ClipSeg, Rembg, Rembg-human e Hex Color.

Com ClipSeg, você pode usar prompts como "a woman" ou "face of a woman" ou "face and hair of a woman" para que o modelo crie uma máscara fora das áreas que você especificar.

Com os recursos de pintura manual, você pode mascarar uma área e depois usar a opção de preenchimento para preencher o restante, em vez de tentar usar um pincel.

_**Não se esqueça de pressionar enter após terminar de editar manualmente sua máscara! As alterações não serão salvas a menos que você pressione enter**_

### Ferramentas de Conversão de Modelos

Converta entre diferentes formatos de modelos.