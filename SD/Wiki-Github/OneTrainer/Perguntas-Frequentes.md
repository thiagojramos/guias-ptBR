* Onde posso colocar o conjunto de fotos para treinamento?
> Na aba de conceitos. Primeiro, adicione uma configuração e depois adicione um conceito a essa configuração. Quando você clica no conceito, abre uma janela onde você especifica o caminho para o seu conjunto de dados. Você pode definir vários conceitos para uma configuração.

* Quais tipos de imagem são suportados?
> A lista completa de extensões suportadas: 
['.bmp', '.jpg', '.jpeg', '.png', '.tif', '.tiff', '.webp']

* Onde posso colocar a palavra-chave de ativação?
> A palavra-chave usada nos prompts para embeddings é simplesmente o nome do arquivo, não existe para LoRA.
> Para embeddings e embeddings adicionais, as legendas devem conter o placeholder ou as imagens não serão treinadas.

* [Embedding] Como devo usar o placeholder `<embedding>` ao treinar embeddings e embeddings adicionais?
> É apenas um placeholder, você pode usar qualquer palavra. Antes da opção de embeddings adicionais, tinha que ser estritamente `<embedding>`, mas agora você pode usar qualquer palavra, só precisa corresponder à palavra-chave definida nas suas legendas.

* [Embedding] Como definir o texto inicial do embedding?
> Em resumo, o texto inicial do embedding deve descrever seu sujeito, mas não deve exceder em tokens a contagem de tokens do embedding.
>
> Aqui está uma explicação. Vamos pegar seu exemplo de prompt "photograph of `<embedding>` with", e (para simplicidade) assumir que cada palavra é codificada em um único token. Isso poderia resultar nos seguintes IDs de tokens: [1, 2, ..., 3], onde "..." é o lugar onde seu embedding é inserido. Se você tiver um embedding de 3 tokens, seria por exemplo [1, 2, 100, 101, 102, 3].
>
> Agora digamos que você defina um texto inicial como "blond woman wearing a pink hat". São 6 tokens. Mas o embedding suporta apenas 3 tokens, então apenas os primeiros 3 (dos 6) tokens são realmente usados. O resto é truncado.
>
> Isso também funciona ao contrário. Se você fornecer um texto mais curto (como "blond woman"), ele não sabe o que usar como o terceiro token. No OneTrainer, os tokens são preenchidos com o token " * ", então "blond woman" se torna "blond woman *".

* Os embeddings são comparáveis aos LoRAs em termos de qualidade/flexibilidade?
> O Embedding (Textual Inversion) trabalha ao contrário para encontrar os tokens que correspondem às suas imagens, então só pode aprender coisas que o modelo base já conhece. Por essa razão, eles podem ser muito flexíveis. São bons para pessoas porque o modelo já viu muitas pessoas diferentes. Mas não pode aprender algo completamente novo.
Eles não requerem muitas imagens, 15-30 imagens são suficientes, até mesmo um conjunto de dados minimalista (~ 6 imagens) pode funcionar.
Também note que eles podem ser usados com LoRAs.
>
> LoRA adiciona um conjunto de pesos ao modelo. Eles são geralmente usados para pessoas, estilo, poses e composição de imagens. Dependendo do que você está treinando, você pode torná-los mais flexíveis adicionando imagens com poses/composições diferentes... Eles podem ser treinados com um pequeno conjunto de dados, como 20 imagens, mas para mais flexibilidade, você precisará de mais.

* Quanto Vram eu preciso para treinamento?
> Depende do que você está treinando e dos parâmetros. SD1.5 LoRA ou embedding com resolução 512 podem ser treinados com apenas 6Gb. Mas para a maioria das coisas, você precisará de 10-12 Gb. Para ajuste fino do SDXL, 24Gb não são realmente suficientes.

* Eu realmente gosto de usar imagens de classe/imagens de regularização para meus LoRAs treinados, como no Kohya.
> Imagens de classe são um conceito meio inventado. Elas são apenas imagens regulares que são treinadas sem legendas complicadas e geralmente com um peso menor. Você pode facilmente fazer isso no OneTrainer adicionando-as como um novo conceito, definindo as repetições mais baixas e talvez reduzindo um pouco o peso da perda. Para as legendas, selecione "de arquivo único" e coloque seu prompt de classe nesse arquivo.