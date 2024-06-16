Esta aba aparece apenas para treinar Embeddings.

![Capture d'écran 2024-04-19 120113](https://github.com/Nerogar/OneTrainer/assets/129741936/a57bed6f-b73e-493c-9e23-a9c756c19a54)

* Base embedding (padrão - em branco): preencha somente se quiser treinar mais sobre um embedding existente, deixe em branco para um novo embedding.
* Contagem de Tokens (padrão - 1): O número de tokens a ser usado para o seu embedding. O tamanho de um token varia por modelo. 2 podem ser suficientes para embeddings simples como um rosto, mais tokens podem ajudar a aprender mais, como características do corpo, e serão mais fortes em prompts mais complexos (por exemplo, de 6 a 10).
* Texto inicial do embedding (padrão - * ): O texto inicial do embedding usado ao criar um novo embedding, ajuda o modelo a entender o que você está treinando. Você pode mantê-lo curto, como apenas '*', mas quanto mais preciso for, mais rápido será o treinamento, o que também pode aumentar a qualidade final do embedding. Lembre-se de que deve usar menos tokens do que a contagem de tokens, caso contrário, será truncado.
* Tipo de Dados do Peso do Embedding (padrão - float32): Em que o embedding será carregado na memória. Como os embeddings são pequenos, há pouca razão para mudar para bfloat16, mas é uma opção.
* Placeholder (padrão - `<embedding>`): deve corresponder ao placeholder que você está usando nas legendas das suas imagens. Se você mantiver o valor padrão, suas legendas devem usar o mesmo (photo of `<embedding>`), mas você pode usar qualquer placeholder que quiser, como "TomCruise", com legendas como "photo of TomCruise". Não precisa ser uma única palavra e pode incluir espaços.
