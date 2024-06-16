# Opções da aba LoRA
Esta aba só é visível se você estiver criando uma LoRA.

Há algumas opções nesta aba.

![imagem](https://github.com/Nerogar/OneTrainer/assets/132208482/b15aca6a-534c-411f-bbe5-fde3edf91581)

* Modelo base da LoRA (padrão: Vazio): Isso permite carregar uma LoRA para continuar trabalhando nela. Alternativamente, uma pasta de backup pode ser usada, mas atenção, ao carregar uma pasta de backup você terá mais limitações no que pode ser feito.
* Rank da LoRA (padrão: 16): Este é o valor que determina o número de camadas que sua LoRA terá. Quanto mais camadas, mais Vram você precisará, mas mais dados poderão ser armazenados.
* Alpha da LoRA (padrão: 1.0): Um hiperparâmetro ajustável. O valor de alpha dividido pelo rank será multiplicado pela sua taxa de aprendizado. Por exemplo, os valores padrão fornecerão um multiplicador de 0.0625 (1/16) para sua taxa de aprendizado. Otimizadores adaptativos (dAdapt, Prodigy) irão, como o nome sugere, se adaptar ao seu valor de alpha.
* Probabilidade de dropout (padrão: 0.0): Uma técnica para ajudar a evitar overfitting, ignorando aleatoriamente uma porcentagem dos nós de treinamento a cada etapa. Um guia (o link está abaixo) recomenda valores entre 0.1 e 0.5. Consulte esse guia se quiser aprender mais.
* Tipo de dados de peso da LoRA (padrão: float32): Qual a precisão usada ao carregar a LoRA na memória.

## Valores de Alpha da LoRA
Valores de Alpha da LoRA e Ranks têm gerado muitas conversas no Discord, e o Discord é um bom lugar para obter mais informações e também buscar informações que não estão no wiki!

## Mais informações sobre LoRAs
[Se você está interessado em ler mais sobre LoRA, aqui está o link de um guia do CivitAI com mais informações.](https://civitai.com/articles/3105/essential-to-advanced-guide-to-training-a-lora)