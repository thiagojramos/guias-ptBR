# [Personagens, Roupas, Poses e Outras Coisas: Um Guia](https://civitai.com/articles/680)

![Personagens, Roupas, Poses e Outras Coisas: Um Guia](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6e7ac93f-cfe2-4af2-8aac-137f6e3e4778/width=1320/6e7ac93f-cfe2-4af2-8aac-137f6e3e4778.jpeg)

-Tags: `#training` `#guide`, `#anime`, `#lora`

-Autor: [**justTNP**](https://civitai.com/user/justTNP)

---

_Notas da Atualização (18/05/2024):_

-   Adicionada uma seção para treinar LoRAs no Pony Diffusion.

-   Adicionada uma pequena seção para DoRAs.

_Com a Nova Atualização (14/05/2024):_

-   Os anexos foram atualizados. Para usar as novas configurações, você deve mudar para o branch dev do treinador do Derrian. Isso é feito digitando na linha de comando `git clone -b dev https://github.com/derrian-distro/LoRA_Easy_Training_Scripts.git`. O que foi atualizado?

    -   **Configurações PDXL:** O otimizador e o scheduler agora estão definidos para CAME e REX. As taxas de aprendizado foram alteradas para unet 1e-4 e tenc 1e-6. Essas configurações são eficientes em VRAM e podem treinar mais rápido.

    -   **Configurações DoRA:** Um novo TOML foi adicionado para o treinamento PDXL. Se você quiser saber como treinar DoRAs, essas são as configurações para começar.

-   Removida a seção **Ferramentas Adicionais** do artigo. Você ainda pode conferir nos anexos.

-   Agora uso chaiNNer para aumentar a escala das minhas imagens graças ao [guia do PotatCat](https://civitai.com/articles/5275/using-chainner-to-batch-image-process-dataset).

-   A seção da extensão Dataset Tag Editor foi substituída pelo Dataset Processor do Particle1904.

-   Ajustes leves na seção **Preparando o Forno**. ~~Para as configurações completas de treinamento de LoRAs XL, use os anexos fornecidos.~~

-   Removida a seção de glossário para LoRAs, LoCons e LoHas.

-   Pequenos ajustes de redação em todo o resto.


## **Prefácio**

Se você quer dedicar apenas 30-40 minutos do seu tempo para fazer um simples LoRA de personagem, recomendo fortemente conferir o [guia do Holostrawberry](https://civitai.com/articles/4/make-your-own-loras-easy-and-free). É muito direto, mas se você está interessado em saber quase tudo o que eu faço e está disposto a reservar mais tempo, continue lendo. Este guia se baseia um pouco no guia e nos Colab Notebooks dele, mas você ainda deve conseguir entender tudo o que vou cobrir a seguir.

A imagem de capa usa o meme [Pepe Silvia](https://civitai.com/models/97630) do FallenIncursio, caso você esteja se perguntando.

## **Introdução**

Aqui está o que este artigo cobre atualmente:

-   Pré-requisitos

-   LoRA Básico de Personagem

-   Conceitos, Estilos, Poses e Roupas

-   Múltiplos Conceitos

-   Treinamento de Modelos Usando Imagens Geradas por IA

-   Otimizador Prodigy

-   Treinando um LoRA no Pony Diffusion

-   O que é um DoRA?


Use os capítulos à direita da página para navegar.

## **Primeiros Passos: Seu Primeiro Personagem**

Não vamos começar muito ambiciosos aqui, então vamos começar simples: criando seu primeiro personagem.

Aqui está uma lista de coisas que eu uso (ou já usei ou recomendo conferir).

-   Uma interface web de Stable Diffusion de sua escolha ([Automatic1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui), [SD.Next](http://sd.next/), [ComfyUI](https://github.com/comfyanonymous/ComfyUI), [Forge](https://github.com/lllyasviel/stable-diffusion-webui-forge) etc...)

-   Modelos Base

    -   [Modelo Base NovelAI (SD 1.5)](https://huggingface.co/hollowstrawberry/stable-diffusion-guide/blob/main/models/animefull-final-pruned-fp16.safetensors)

    -   [Pony Diffusion V6 XL (SDXL 1.0)](https://civitai.com/models/257749?modelVersionId=293564)

    -   [Animagine XL V3 (SDXL 1.0)](https://civitai.com/models/260267?modelVersionId=293564)

-   [Grabber](https://github.com/Bionus/imgbrd-grabber)

-   Treinadores de LoRA

    -   Holostrawberry's LoRA Trainer Colab ([Original](https://colab.research.google.com/github/hollowstrawberry/kohya-colab/blob/main/Lora_Trainer.ipynb) | [SDXL](https://colab.research.google.com/github/hollowstrawberry/kohya-colab/blob/main/Lora_Trainer_XL.ipynb))

    -   [Derrian's LoRA Easy Training Scripts](https://github.com/derrian-distro/LoRA_Easy_Training_Scripts) (para treinamento local; suporte a sistema de filas)

        -   **Use o branch dev:** `git clone -b dev https://github.com/derrian-distro/LoRA_Easy_Training_Scripts.git`

    -   [Jelosus1's LoRA Easy Training Colab](https://colab.research.google.com/github/Jelosus2/Lora_Easy_Training_Colab/blob/main/Lora_Easy_Training_Colab.ipynb#scrollTo=vGwaJ0eGHCkw) (usa configurações locais; treinado com hardware do Google)

-   [dupeGuru](https://dupeguru.voltaicideas.net/)

-   [ChaiNNer](https://github.com/chaiNNer-org/chaiNNer)

-   Editores de Tag/Legenda

    -   [Extensão de editor de tags de dataset do toshiaki1729](https://github.com/toshiaki1729/stable-diffusion-webui-dataset-tag-editor) / [standalone](https://github.com/toshiaki1729/dataset-tag-editor-standalone) / [repositório forked do Squibeel](https://github.com/Squibeel/stable-diffusion-webui-dataset-tag-editor)

    -   [Dataset Processor - Ferramentas All-in-One](https://github.com/Particle1904/DatasetHelpers)

-   [WebP do Google](https://developers.google.com/speed/webp/docs/using)

-   [Jupyter Notebook (anaconda3) ou equivalente](https://www.anaconda.com/)

"Eita, tem muitos itens, não acha?"

Não se preocupe com isso, instale o seguinte primeiro: **o Grabber, dupeGuru, Dataset Processor by Particle1904, e os Scripts de Treinamento Fácil do LoRA do Derrian se você estiver treinando localmente.** Você usará uma ferramenta fácil de usar para baixar imagens de sites como Gelbooru e Rule34. Em seguida, você usará o dupeGuru para remover quaisquer imagens duplicadas que possam impactar negativamente seu treinamento e, finalmente, enviará o restante de suas imagens diretamente para o editor de tags do dataset.

## **Grabber**

Eu uso o Gelbooru para baixar as imagens. Você está familiarizado com as tags do booru, certo? Espero que seu conhecimento de `completely nude` e `arms behind head` te ajude nesta próxima seção.

Tem um personagem em mente? Ótimo! Digamos que vou treinar em um personagem completamente novo que este site nunca viu antes, **Kafka** de **Honkai: Star Rail**!

Se você quiser usar um site diferente do Gelbooru, clique no botão **Sources** localizado na parte inferior da janela. É melhor deixar apenas um site marcado.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/14385c0a-af8c-4f57-a484-3db1d423d165/width=525/14385c0a-af8c-4f57-a484-3db1d423d165.jpeg)

Então, o que você deve colocar na barra de pesquisa no topo? Para mim, vou digitar `solo -rating:explicit -rating:questionable kafka_(honkai:_star_rail)`. Você não precisa adicionar `-rating:questionable`, mas para mim, quero que os personagens usem algumas roupas decentes. Você também pode optar por remover `solo` se não se importar em reservar um tempo extra para cortar coisas. Isso então deixa `-rating:explicit`, você deve removê-lo? Bem, depende inteiramente de você, mas para mim, vou deixá-lo. E só porque sim, vou adicionar uma tag `shirt`.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6bd864a7-823f-4159-8743-2f96c77c2494/width=525/6bd864a7-823f-4159-8743-2f96c77c2494.jpeg)

Bem, isso parece promissor: 259 resultados. Clique no botão **Get all**. Mude para a guia **Downloads**.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/0b62fcc2-3baa-43ca-9d87-e0850de8eb70/width=525/0b62fcc2-3baa-43ca-9d87-e0850de8eb70.jpeg)

Esta guia é onde você pode acompanhar o que está planejando baixar. Antes de baixar, vê o canto inferior esquerdo? Escolha uma pasta onde você quer que suas imagens baixadas sejam salvas. Em seguida, clique com o botão direito em um item na lista de Downloads e clique em **Download**.

## **dupeGuru**

Tudo pronto? Ótimo, vamos trocar para o dupeGuru.

Quando estiver aberto, você vai adicionar um local de pasta para ele escanear suas imagens. Clique no símbolo **+** no canto inferior esquerdo e clique em **Add Folder**. Escolha a pasta onde suas imagens estão e clique em **Select Folder**. Se você quiser determinar a intensidade com que o dupeGuru detecta imagens duplicadas, vá para **View > Options** ou pressione **Ctrl + P**. Defina a intensidade do filtro no topo da janela de Opções e clique em **OK**. Depois disso, selecione o **Application mode** no topo para **Picture**. Clique em **Scan**. Quando terminar de analisar suas imagens, eu geralmente **Mark all** e depois deleto (**Ctrl+A** e **Ctrl+D** se quiser ser rápido).

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/3e2e6c7f-0eb1-498a-b647-2a20ee24c2f5/width=525/3e2e6c7f-0eb1-498a-b647-2a20ee24c2f5.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/33185cc5-1bea-4261-8a58-b8437cc8f088/width=525/33185cc5-1bea-4261-8a58-b8437cc8f088.jpeg)

Observe que isso não é _garantido_ para capturar todas as imagens duplicadas, então você ainda terá que examinar seu conjunto de dados.

## **Curadoria**

Inspecione o restante das suas imagens e veja se pode haver alguma imagem duplicada que o dupeGuru possa ter perdido e elimine qualquer imagem de má qualidade que possa degradar a saída do seu treinamento. Felizmente para mim, Kafka está repleto de muitas imagens de boa qualidade, então selecionarei no máximo 100! Tente obter outros ângulos, como de lado e de trás, para que o Stable Diffusion não precise adivinhar como são nesses ângulos.

Se você tiver imagens que realmente deseja usar, mas se encontrar nesses casos:

-   Várias Visões de Um Personagem

-   Personagem(s) Não Relacionado(s)

-   Tronco/Pernas/etc. Recortados

Então você terá que fazer um pouco de manipulação de imagem. Use qualquer aplicativo de edição de imagem de sua escolha.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/0d90e92c-9e0e-48ba-a195-f821dcbfd017/width=525/0d90e92c-9e0e-48ba-a195-f821dcbfd017.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/1559c966-d109-45fd-ad8f-a2193efe48cf/width=525/1559c966-d109-45fd-ad8f-a2193efe48cf.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/b7620f36-cd88-4211-b119-e8d763419fcd/width=525/b7620f36-cd88-4211-b119-e8d763419fcd.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/0e666ecc-1923-4e53-b77b-38efbb0e7e81/width=525/0e666ecc-1923-4e53-b77b-38efbb0e7e81.jpeg)

Conforme novas configurações e tecnologias começam a capturar mais detalhes, é melhor tentar eliminar assinaturas e marcas d'água do seu conjunto de dados também.

## **Melhorando a Qualidade do Seu Modelo de Saída**

Se você acha que seu conjunto de dados é bom o suficiente e não planeja treinar em uma resolução maior que 512, pule esta etapa.

Há um artigo feito pelo [[**@PotatCat](https://www.civitai.com/user/PotatCat) que aumenta a escala e altera o contraste das imagens (útil para capturas de tela de animes). [Recomendo lê-lo, é fácil.](https://civitai.com/articles/5275/using-chainner-to-batch-image-process-dataset)

(Em uma WebUI de sua escolha) Se você estiver capturando um anime antigo e borrado, deve baixar [**este**](https://de-next.owncube.com/index.php/s/GjbBw7pcgm5gmYm). Certifique-se de definir **Resize** para 1 ao usá-lo.

## **Auxiliar de Conjunto de Dados**

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f041db20-d4a0-4610-8209-0e78a486f5f9/width=525/f041db20-d4a0-4610-8209-0e78a486f5f9.jpeg)

Vamos começar a marcar automaticamente seu conjunto de dados. No Auxiliar de Conjunto de Dados, vá para **Generate Tags**. Aponte a pasta de entrada e saída para sua pasta de conjunto de dados. Defina o modelo de marcação automática para **WDv3** e o limiar para previsões para **0.25**. Certifique-se de que a primeira opção esteja marcada. Clique no botão **Generate tags** na parte inferior.

___

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/785a7ba9-29cc-47ee-9d72-5e125924dfef/width=525/785a7ba9-29cc-47ee-9d72-5e125924dfef.jpeg)
Quando terminar, vá para a tela **Process Tags**.

No campo "Tags to add", digite uma palavra de gatilho aqui.

No campo "Tags to remove", é aqui que você digitará as tags como comprimento do cabelo, estilo de cabelo e cor dos olhos. Não sabe o que pode remover? Clique em **Calculate frequency**. As tags removidas serão absorvidas na palavra de gatilho.

Recomendo marcar **Would you like to rename images and their .txt files to crescent order?** Clique no botão **Process tags** na parte inferior quando terminar.

___

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/d312384e-f961-4742-99af-97bbaca0d904/width=525/d312384e-f961-4742-99af-97bbaca0d904.jpeg)
Recomendo fortemente editar manualmente suas legendas, então vá para **Tag/Caption Editor**. Aqui você adicionará ou removerá as tags. A aplicação informará como essa página funciona, então verifique. Use a [wiki do danbooru](https://danbooru.donmai.us/wiki_pages/help:home) para ler sobre algumas tags, você pode aprender uma ou duas coisas.

Seja descritivo e único com suas tags para minimizar o sangramento em seu modelo!

## **Preparando o Forno**

**Configurações para 1.5 e PDXL estão incluídas nos anexos.**

Eu utilizo o **NovelAI** (ou nai/animefull) como modelo base de treinamento. Não sei se treinar com `unpruned` ou `pruned` faz diferença.

Aqui estão minhas configurações (tentarei incluir algumas configurações adicionais para vocês que treinam localmente):

-   folderstructure: Organize por projetos

-   resolution: 768

-   shuffletags: true

-   enablebucket: true (o treinador redimensionará automaticamente as imagens para você)

-   keeptokens: 1

-   trainbatchsize: 2

-   unetlr: 5e-4 or 0.0005

-   textencoderlr: 1e-4 or 0.0001 (deve ser um quinto do seu unet LR)

-   lrscheduler: cosinewithrestarts

-   lrschedulernumber / Num Restarts: 3

-   lrwarmupratio: 0.05 (se você estiver usando constantwithwarmup)

-   minsnrgamma: enabled / 5.0

-   loratype: LoRA

-   networkdim: 16

-   networkalpha: 8

-   optimizer: AdamW8Bit

-   optimizer_args: "weight_decay=0.1, betas=[0.9,0.99]"

-   Clip Skip: 2

-   Max Token Length: 225

-   Training Precision: fp16 (bf16 se seu hardware suportar)

-   Cache Latents: True

Se você é um usuário do Colab, deve ter uma pasta chamada **Loras** no seu Google Drive se for usar **Organize por projeto**. Certifique-se de que a estrutura da sua pasta seja assim: `<nome da sua pasta> -> dataset` onde `dataset` é uma subpasta que contém suas imagens e documentos de texto. Depois de verificar se a estrutura está correta, faça o upload para o Google Drive dentro da pasta **Loras**.

Agora, enquanto está fazendo o upload, vamos revisar **quantas repetições e épocas você deve usar**. Primeiro, quantas imagens você tem? Eu disse que escolheria até 100 imagens para o meu conjunto de dados, então vamos revisar a tabela de referência do Holostrawberry.

`20 imagens × 10 repetições × 10 épocas ÷ 2 batch size = 1000 passos`

`100 imagens × 3 repetições × 10 épocas ÷ 2 batch size = 1500 passos`

`400 imagens × 1 repetição × 10 épocas ÷ 2 batch size = 2000 passos`

`1000 imagens × 1 repetição × 10 épocas ÷ 3 batch size = 3300 passos`

De acordo com esta tabela, eu deveria definir minhas repetições para 3 e épocas para 10, então é isso que vou fazer. Depois disso, tudo o que realmente preciso fazer é definir o nome do projeto no Google Colab para o que eu nomeei minha pasta de projeto que está no meu Drive. No meu caso, é **hsrkafka**.

**Para aqueles que estão treinando localmente**, este é o seu esquema de nomeação de pasta: `repeats_projectname` onde você substituirá `repeats` pelo número de repetições e `projectname` pelo nome que você quiser.

-   Se sua GPU tiver mais de 9 ou 10 GB de VRAM, você pode treinar em uma resolução de 768 com batch size 2, XFormers habilitado. Tente não fazer mais nada que use mais recursos enquanto o treinador está ocupado. Caso contrário, reduza essas configurações.

Ótimo, acho que isso resolve, vamos executá-lo e deixar o treinador lidar com o resto.

___

## **Está Pronto?**

Seu LoRA terminou de assar? Você pode escolher baixar algumas de suas últimas épocas ou todas elas. De qualquer forma, você testará para ver se seu LoRA funciona.

Volte ao Stable Diffusion e comece a digitar seu prompt. Por exemplo,

`<lora:hsr_kafka-10:1.0>, solo, 1girl, kafka, sunglasses, eyewear on head, jacket, white shirt, pantyhose`

Em seguida, habilite o script, "**X/Y/Z plot**." Seu **X type** será **Prompt S/R**, que basicamente **procurará** a primeira coisa no seu prompt e **substituirá** pelo que você mandar. Em **X values**, digite algo como `-10, -09, -08, -07`. Isso encontrará o primeiro `-10` no seu prompt e substituirá por `-09, -08, -07`. Em seguida, clique em **Generate** e descubra qual **época** funciona melhor para você.

Depois de escolher a melhor época, você testará qual peso funciona melhor, então para seus **X values**, digite algo como `1.0>, 0.9>, 0.8>, 0.7>, 0.6>, 0.5>`. Clique em **Generate** novamente.

Seu LoRA deve idealmente funcionar com peso 1.0, mas tudo bem se funcionar melhor em torno de 0.8, afinal esta _é_ sua primeira vez. Treinar um LoRA é um jogo de experimentação, então você estará mexendo com marcações e alterando configurações a maior parte do tempo.

## **Conceitos, Estilos, Poses e Roupas**

Agora que você conhece o básico de treinar um LoRA de personagem, e se você quiser treinar um conceito, um estilo, uma pose e/ou uma roupa? Procure por **consistência** e forneça **marcações adequadas**.

**Para conceitos:** Adicione uma tag de ativação. Você pode optar por eliminar qualquer uma das tags relacionadas ou deixá-las. Aqui está um [exemplo](https://civitai.com/models/113575/grimace-shake-or-meme). Observe que basta uma tag para gerar um personagem segurando o Grimace Shake. Um elemento que permaneceu **consistente** é o shake que aparece em cada imagem do conjunto de dados. Eu removi tags como 'holding' e 'cup'.

**Para estilos:** Eu prefiro não adicionar uma tag de ativação, para que tudo o que o usuário precise fazer seja chamar o modelo e usar o prompt. Apenas deixe o autotagger fazer seu trabalho e, em seguida, salve e saia imediatamente. Aqui está um [exemplo](https://civitai.com/models/112550/onmyoji-yokai-koya-style). Novamente, certifique-se de que haja **consistência** de estilo em todas as imagens. Você vai querer aumentar as épocas e testar cada uma.

**Para poses:** Adicione uma tag de ativação. Você pode optar por remover quaisquer tags relacionadas ou deixá-las. Aqui está um [exemplo](https://civitai.com/models/70618/index-fingers-together-or-pose). No conjunto de dados, havia **consistência** de personagens aleatórios colocando os dedos indicadores juntos.

**Para roupas:** Adicione uma tag de ativação. Você pode optar por remover quaisquer tags relacionadas ou deixá-las. Aqui está um [exemplo](https://civitai.com/models/116271/arch-bishop-attire-or-ragnarok-online). Eu removi tags como `cruz` e `meias-altas`.

Existem algumas tags que vale a pena deixar para que certos conceitos sejam aprendidos corretamente.

## **Múltiplos Conceitos**

## **Organização**

Esta parte irá cobrir como treinar um personagem singular que usa várias roupas. Você pode aplicar a ideia geral desse método a [múltiplos personagens](https://civitai.com/models/42405/akame-ga-kill-ultimate-girl-pack) e conceitos.

Então você tem uma variedade de imagens. Você vai querer organizar essas imagens em pastas separadas que representem cada uma uma roupa única.

Agora, digamos que você tenha 4 pastas com o seguinte número de imagens:

-   _Roupa #1:_ 23 imagens

-   _Roupa #2:_ 49 imagens

-   _Roupa #3:_ 100 imagens

-   _Roupa #4:_ 79 imagens


Vamos facilitar as coisas. Exclua 3 imagens na pasta da roupa #1, 16 imagens na #2 e 29 imagens na #4. Vou explicar isso mais tarde.

## **Marcação**

Agora você associará cada roupa com sua própria tag de ativação. Use [Zeta de Granblue Fantasy](https://civitai.com/models/95978/zeta-or-granblue-fantasy) como guia. Estes são meus gatilhos para cada roupa:

-   zetadef

-   zetasummer

-   zetadark

-   zetahalloween


É claro que removi as tags de cor do cabelo, comprimento do cabelo e cor dos olhos, mas também deixei de fora as tags de estilo de cabelo e roupas. Você pode optar por removê-las e incluir elas em cada tag de ativação.

## **Configurações de Treinamento**

Lembra quando eu disse para você excluir um número específico de imagens naquele seu conjunto de dados hipotético? O que você fará é tentar treinar cada roupa igualmente, apesar das diferenças na contagem de imagens. Aqui estão as pastas atualizadas:

-   _Roupa #1:_ 20 imagens

-   _Roupa #2:_ 33 imagens

-   _Roupa #3:_ 100 imagens

-   _Roupa #4:_ 50 imagens


Se eu fosse o Holostrawberry, ele sugeriria usar as seguintes repetições para cada pasta:

-   _Roupa #1:_ 5 repetições

-   _Roupa #2:_ 3 repetições

-   _Roupa #3:_ 1 repetição

-   _Roupa #4:_ 2 repetições


Se você estiver usando o notebook Colab dele, vá para a seção onde diz "Multiple folders in dataset." Aqui está como sua célula deve parecer:

```
custom_dataset = """
[[datasets]]

[[datasets.subsets]]
image_dir = "/content/drive/MyDrive/Loras/PROJECTNAME/outfit1"
num_repeats = 5

[[datasets.subsets]]
image_dir = "/content/drive/MyDrive/Loras/PROJECTNAME/outfit2"
num_repeats = 3

[[datasets.subsets]]
image_dir = "/content/drive/MyDrive/Loras/PROJECTNAME/outfit3"
num_repeats = 1

[[datasets.subsets]]
image_dir = "/content/drive/MyDrive/Loras/PROJECTNAME/outfit4"
num_repeats = 2

"""
```

Vamos fazer alguns cálculos: (20 × 5) + (33 × 3) + (100 × 1) + (50 × 2) = 399

Vamos rotular esse número como _T_. Agora vamos determinar quantas épocas devemos obter. É isso que eu geralmente uso:

`200 T × 17 épocas ÷ 2 tamanho de lote = 1700 passos`

`300 T × 12 épocas ÷ 2 tamanho de lote = 1800 passos`

`400 T × 10 épocas ÷ 2 tamanho de lote = 2000 passos`

Então nosso _T_ está mais próximo da última linha, então vamos ficar com 10 épocas.

___

Se você estiver usando o "Derrian's LoRA Easy Training Scripts", deverá ver algo assim:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/d4551287-7899-4deb-9faa-ebb05778da04/width=525/d4551287-7899-4deb-9faa-ebb05778da04.jpeg) 
É assim que você controlará as repetições para cada pasta. Você pode colocar um número no início do nome de cada pasta para inserir automaticamente o número de repetições.

**Com isso fora do caminho, inicie o treinador!**

## **Usando Imagens Geradas por IA para Treinamento**

Pode ser feito? [**Sim,**](https://civitai.com/models/58953?modelVersionId=63404) [**absolutamente,**](https://civitai.com/models/119430?modelVersionId=129750) [**com certeza.**](https://civitai.com/models/116931/winnie-the-pooh-reading-or-meme-experimental) 
Nós até criamos um [**mascote de anime para este site!**](https://civitai.com/models/192970/15xl-10-civitai-chan-or-the-gijinka-series)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/3348e01e-9c50-4290-9ad4-f9a9a05b8e31/width=525/3348e01e-9c50-4290-9ad4-f9a9a05b8e31.jpeg) 
Se você estiver trabalhando para melhorar seus modelos, deve escolher suas melhores gerações (ou seja, a representação mais precisa do seu modelo). Inspecione suas imagens cuidadosamente, o Stable Diffusion já é ruim o suficiente com mãos. Não faça suas próximas gerações piores se você não cuidar do seu conjunto de dados.

## **Prodigy**

O otimizador Prodigy é um suposto sucessor dos otimizadores DAdaptation e já está disponível há algum tempo. Dois dos meus primeiros usos foram [Wendy (Herrscher of Wind)](https://civitai.com/models/46352/wendy-herrscher-of-wind-or-honkai-impact-3rd) e [Princess Bullet Bill](https://civitai.com/models/151185?modelVersionId=169049). Ele é agressivo na maneira como aprende e é recomendado para pequenos conjuntos de dados (geralmente uso cerca de 20 a 30 imagens, mais ou menos). O otimizador é ótimo, mas não incrivelmente surpreendente, pois parece errar em alguns detalhes como tatuagens. Se você quiser experimentar, aqui estão as configurações que você deve modificar:

-   optimizer: Prodigy

-   learningrate: 0.5 or 1 (unet lr e tenc lr são os mesmos)

-   networkdim/networkalpha: 32/32 or 16/16

-   lrscheduler: constantwithwarmup

-   lrwarmupratio: 0.05

-   optimizerargs: `"decouple=True, weight_decay=0.01, betas=[0.9,0.999], d_coef=2, use_bias_correction=True, safeguard_warmup=True"`


Para repetições, tente alcançar 100 passos. Por exemplo, se eu tiver 20 imagens, usaria 5 repetições. Para épocas, apenas configure para 20. Enquanto estiver treinando, você verá a perda assim:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c490f1ff-f7cb-4d70-b112-45882e47d845/width=525/c490f1ff-f7cb-4d70-b112-45882e47d845.jpeg)
Quando estiver quase terminando o treinamento, procure o modelo com a perda no ponto mais baixo. Tipicamente será por volta de 800 passos ou mais.

**EDIT (14 de maio de 2024):** Percebi que alguns criadores estão usando Prodigy com conjuntos de dados acima do tamanho recomendado com repetições altas. Você não precisa fazer isso.

## **Treinando um LoRA no Pony Diffusion**

[Pony Diffusion V6 XL](https://civitai.com/models/257749/pony-diffusion-v6-xl) (PDXL) é um ajuste fino sobre o modelo Base XL 1.0. É pesado, mas conhece mais personagens e conceitos, embora não seja ótimo com fundos.

## **Preparando o Forno**

**OBSERVAÇÃO PARA TREINADORES USANDO O DERRIAN:** Você deve usar o branch dev do "Derrian's LoRA Easy Training Scripts". Isso pode ser feito digitando na linha de comando `git clone -b dev https://github.com/derrian-distro/LoRA_Easy_Training_Scripts.git`. Aqui estão as configurações que funcionam com 12 GB de VRAM:

-   **General Args**

    -   Model

        -   Base Model: Pony Diffusion V6 XL

        -   External VAE: [SDXL Vae](https://huggingface.co/stabilityai/sdxl-vae/resolve/main/sdxl_vae.safetensors)

        -   SDXL Based: True

        -   No Half Vae: True

        -   Full BF16: True

    -   Resolution: 1024

    -   Gradient Checkpointing: True

    -   Gradient Accumulation: 1

    -   Batch Size: 2

    -   Max Token Length: 225

    -   Memory Optimization: SDPA

    -   Cache Latents: True, To Disk: True

-   **Network Args**

    -   LoRA Type: LoRA

    -   Network Dim/Alpha: 8/4

-   **Optimizer Args**

    -   Main Args

        -   Optimizer Type: CAME

        -   LR Scheduler: REX

        -   Loss Type: L2

        -   Learning Rate: 1e-4 or 0.0001

        -   Minimum Learning Rate: 1e-6 or 0.000001

        -   Unet Learning Rate: 1e-4 or 0.0001

        -   TE Learning Rate: 1e-6 or 0.000001

        -   MIN SNR Gamma: 5 / 8

        -   Warmup Ratio: 0.05

    -   Optional Args:

        -   weightdecay=0.04

-   **Bucket Args**

    -   Enable

    -   Maximum Bucket Resolution: 2048

    -   Bucket Resolution Steps: 64


## **Repetições & Épocas**

Aqui está minha tabela para definir repetições e épocas:

`20 imagens × 2 repetições × 10 épocas ÷ 2 tamanho de lote = 200 passos`

`40 imagens × 2 repetições × 6 épocas ÷ 2 tamanho de lote = 240 passos`

`60 imagens × 2 repetições × 5 épocas ÷ 2 tamanho de lote = 300 passos`

`100 imagens × 2 repetições × 4 épocas ÷ 2 tamanho de lote = 400 passos`

`200 imagens × 2 repetições × 3 épocas ÷ 2 tamanho de lote = 600 passos`

`600 imagens × 2 repetições × 2 épocas ÷ 2 tamanho de lote = 1200 passos`

Nunca é demais adicionar uma época extra. Você pode precisar diminuir o unet se o seu conjunto de dados se tornar maior do que o listado aqui.

## **O que é um DoRA?**

Conhecido como <u>Weight-Decomposed Low-Rank Adaptation</u>, é um tipo de LoRA com parâmetros adicionais treináveis, direção e magnitude, o que os torna próximos do ajuste fino nativo. Treinar um DoRA troca velocidade por maior detalhe e precisão. Isso pode ser de seu interesse se você estiver treinando um estilo ou conceito.

Você pode conferir o repositório e o artigo deles aqui: [https://github.com/catid/dora](https://github.com/catid/dora)

## **Preparando o Forno**

Pegue as configurações PDXL acima e faça as seguintes alterações:

-   **Network Args**

    -   LoRA Type: LoCon (LyCORIS)

    -   Conv Dim/Alpha: 4/2

    -   DoRA: True

-   **Optimizer Args**

    -   Main Args

        -   Learning Rate: 5e-5 or 0.00005

        -   Unet Learning Rate: 5e-5 or 0.00005


## **(Antigo) Conclusão**

Olá, se você chegou até aqui, obrigado por dedicar seu tempo para ler o artigo. Eu prometi fazer este artigo para compartilhar tudo o que fiz desde aquele anúncio. No entanto, apressei algumas coisas até o final, então este artigo ainda não está completamente finalizado. Se você tiver alguma dúvida ou crítica, **por favor, me avise!** Se houver algo que você acha que pode ser feito de forma mais eficiente, **por favor, me avise!** Trate isso como um ponto de partida para _sua_ maneira de treinar LoRAs. Nem tudo aqui é perfeito e nenhum método de treinamento de LoRAs é perfeito.

E lembre-se, fazer LoRAs é um jogo de experimentação.

<p>
</p>

# Comentários


> [**bloodsplash**](https://civitai.com/user/bloodsplash)
"**!! Ainda não descobri uma maneira de fazer isso para treinamento local, por favor, tenha paciência !!**"
Você quer dizer a estrutura da pasta? você cria várias pastas na pasta Img, o número no início é a repetição, a palavra do meio é a ativação e a próxima coisa é a classe, EX:
`1_zetadef 1girl`
\
`1_zetasummer 1girl`

---

> [**Joni2709**](https://civitai.com/user/Joni2709)
Muito obrigado, eu estava procurando um guia como este há muito tempo, vou tentar usar no colab agora muejeje

---

> [**guy90**](https://civitai.com/user/guy90)
Uau, este é um ótimo guia!

>> [**javasuperdev297**](https://civitai.com/user/javasuperdev297)
dfghjk

---

> [**aiwayfarer**](https://civitai.com/user/aiwayfarer)
Muito obrigado por compartilhar esse conhecimento!

---

> [**e0972951006**](https://civitai.com/user/e0972951006)
1 403 ERROR

---

> [**12user34kn276**](https://civitai.com/user/12user34kn276)
Muito obrigado por isso!

---

> [**Lora1111**](https://civitai.com/user/Lora1111)
O conceito e as poses são bem breves... há muitos guias para iniciantes por aí para começar e lora's de personagem, isso já é bem coberto por muitos.
Mas dificilmente há bons guias para treinamento de conceito.

---

> [**justTNP - OP**](https://civitai.com/user/justTNP)
[**@Lora1111**](https://civitai.com/user/Lora1111) Bem, está na palavra, não é? Conceitos. E é amplo o suficiente para que eu só possa condensá-lo até esse ponto. São ideias abstratas que você pode manifestar em um LoRA, desde que tenha o conjunto de dados adequado e consistente para isso. Existe algo que você está procurando que eu possa adicionar aqui?

---

> [**Lora1111**](https://civitai.com/user/Lora1111)
[**@justTNP**](https://civitai.com/user/justTNP) estou dizendo que é muito condensado. vamos dizer que você quer treinar um conceito de vista de cima com melhor perspectiva e melhor campo de profundidade do que qualquer outra coisa disponível agora. você treinaria em um conjunto de dados onde cada ponto de vista está... vamos dizer exatamente 1,5 metros acima e 1 metro à frente do sujeito, estritamente de frente? tornando um conceito rígido e inflexível e tendo que fazer um novo lora para cada posição, ângulo de cima? ou você teria a liberdade de adicionar vistas laterais de ângulo alto, vistas traseiras, vistas 3/4, ângulos mais altos e ângulos mais baixos que ainda estão acima do eixo x o suficiente para serem considerados de cima? como as configurações de treinamento diferem de um personagem ou estilo, para conceitos ou poses? se essas configurações são diferentes, por quê? que tipo de liberdade ou restrições você teria em um conjunto de dados para ângulos de visão. conceitos.
Há muito o que expandir aí. Embora aparentemente amplo, pode ser dividido em categorias. Como poses, ângulos de visão, coisas, ações, sobreposição de "pele". Não é tão abstrato quanto você pensa.
\
Existem muitos bons loras de personagem, o mesmo não pode ser dito para conceitos. Devido à falta de material, guias para treiná-los. é assim que você pode melhorar.

>> [**Beef_Chan**](https://civitai.com/user/Beef_Chan)
-Um pouco tarde, mas estou tentando descobrir esse processo eu mesmo. Tive sucesso, mas poderia ser mais consistente. Não consigo treinar localmente, então uso o treinador do HollowStrawberry (mesmo que eu pudesse treinar localmente, recomendaria isso, é incrível e fácil de configurar para múltiplos conceitos/personagens).
\
-Se eu fosse treinar diferentes ângulos/perspectivas, faria uma pasta para cada conceito único, ou seja: DeCima, DeLado, DeBaixo, etc. Eu colocaria todas as imagens semelhantes pertencentes a cada conceito específico nessa pasta, a menos que seja tão diferente que você queira que seja sua própria pasta, ou seja: De Cima e também De uma distância.
\
-Se eu fosse adicionar uma tag de ativação para cada vista, removeria tags como "de cima", "de baixo", etc. de cada um dos seus conjuntos de dados. Se eu não quisesse adicionar tags de ativação, deixaria todas as tags de conceito específicas em seus respectivos conjuntos de dados, mas as removeria dos outros conjuntos de dados que também possam ter essas tags.
\
-O único lora que fiz que fiquei realmente satisfeito foi o primeiro que fiz que era de múltiplos personagens. Era um lora para todas as meninas principais de YuruYuri, então 8 delas. Não me lembro onde vi a informação específica, então por favor me perdoe, mas vi alguém dizer que treinam múltiplos conceitos de forma semelhante a como treinam estilos.
\
-O jeito que fiz foi 8 pastas separadas, 1 para cada garota, removi qualquer tag sobreposta de cada um dos seus conjuntos de dados (camiseta, shorts, etc.) que não eram específicas e não adicionei nenhuma tag de ativação. Seus nomes eram todos reconhecidos na biblioteca de tags do Danbooru, então acho que foi por isso que consegui não usar tags de ativação para nenhuma delas. Certifiquei-me de que todas tivessem o mesmo número exato de imagens e repetições. Acho que deu mais de 600 no total.
\
-Acabei de começar a experimentar conceitos, então estou curioso para ver se o mesmo método de treinamento ou um semelhante funciona para isso. Desculpe por ser tão longo. Preciso fazer um post sobre isso eventualmente, assim que conseguir fazer Loras de múltiplos conceitos de forma consistente.

>>> [**tenderness**](https://civitai.com/user/tenderness)
nb

---

> [**furrytoaster03218**](https://civitai.com/user/furrytoaster03218)
Quando tento treinar um personagem sem um traje específico em mente, mas quero treinar vários penteados (no meu caso, quatro), você recomendaria treinar com múltiplas tags de ativação/pastas, ou apenas marcar o penteado da melhor forma possível?

---

> [**bloodsplash**](https://civitai.com/user/bloodsplash)
[**@furrytoaster03218**](https://civitai.com/user/furrytoaster03218) provavelmente é melhor apenas marcar o cabelo, usar múltiplas tags de ativação geralmente é melhor para trajes/conceitos distintos ou facilidade de uso, mas mesmo assim, elas não são as coisas mais confiáveis, especialmente em um LoRa.

---

> [**QwiziRAM**](https://civitai.com/user/QwiziRAM)
Me sinto um pouco bobo, mas se eu quiser treinar lora em um modelo não padrão, onde eu coloco o modelo e qual URL eu especifico?

---

> [**justTNP - OP**](https://civitai.com/user/justTNP)
[**@QwiziRAM**](https://civitai.com/user/QwiziRAM) Abra o código e procure um link do Hugging Face que baixa seus respectivos modelos (base 1.5, NAI, AnyLoRA). Você pode adicionar uma nova opção de dropdown ou substituir um dos links por um link de download do CivitAI (que você pode obter clicando com o botão direito em **Download** e depois em **Copiar endereço do link** em qualquer página de modelo).

---

> [**guy90**](https://civitai.com/user/guy90)
Bem, depois de testar seu guia passo a passo, agora tenho uma pergunta que nunca pensei antes. Ao marcar, você mantém o "_" nos arquivos de texto? Eu sempre os removi, mas agora estou me perguntando se eles devem ser mantidos, já que a extensão do tagger os mantém.

---

> [**justTNP - OP**](https://civitai.com/user/justTNP)
[**@guy907223982**](https://civitai.com/user/guy907223982) Eu não mantenho os sublinhados porque eles me incomodam lol. Eles não devem afetar o desempenho geral do modelo, mas isso é apenas um palpite.

---

> [**Lora1111**](https://civitai.com/user/Lora1111)
Quais configurações de treinamento você está usando para conceitos?
dim, alpha, taxa de aprendizado, etc.

---

> [**chefcheiro**](https://civitai.com/user/chefcheiro)
Posso usar a imagem em preto e branco de mangá?

---

> [**aiwayfarer**](https://civitai.com/user/aiwayfarer)
[**@justTNP**](https://civitai.com/user/justTNP) Acho que perdi isso, mas qual é o modelo de checkpoint que você usa para treinar personagens de anime?

---

> [**chefcheiro**](https://civitai.com/user/chefcheiro)
[**@cleopatracleo**](https://civitai.com/user/cleopatracleo) quero fazer um lora de equipamento de mergulho.

---

> [**justTNP - OP**](https://civitai.com/user/justTNP)
[**@chefcheiro**](https://civitai.com/user/chefcheiro) "Posso usar a imagem em preto e branco de mangá?" Sim, você pode.

---

> [**crystalized_pebble**](https://civitai.com/user/crystalized_pebble)
A ordem das tags enquanto marca as imagens importa? Costumo tomar precauções com a ordem ao editar as tags, o que pode ser redundante.

---

> [**justTNP - OP**](https://civitai.com/user/justTNP)
[**@crystalized_pebble**](https://civitai.com/user/crystalized_pebble) Desde que sua tag de ativação esteja na frente de tudo, a ordem das tags não importa (ou pelo menos não deveria importar).

---

> [**sharongreen2288321**](https://civitai.com/user/sharongreen2288321)
É muito útil, muito obrigado

---

> [**maxdev192**](https://civitai.com/user/maxdev192)
[**@e0972951006**](https://civitai.com/user/e0972951006) Cara, envie uma DM para ele no discord ou em outro lugar. Além disso, acho que ele entendeu da primeira vez. Esta é a seção de comentários para um guia de treinamento de LoRA, não um 'faça um desejo'.

---

> [**maxdev192**](https://civitai.com/user/maxdev192)
[**@Lora1111**](https://civitai.com/user/Lora1111) Eu meio que concordo com você. Achei [este guia](https://civitai.com/articles/138) muito útil em termos de marcação de múltiplos conceitos.

---

> [**Lora1111**](https://civitai.com/user/Lora1111)
[**@maxdev192**](https://civitai.com/user/maxdev192) Existem guias muito melhores e mais detalhados para conceitos e poses no reentry. Não apenas um comentário de 1-2 frases quando o título afirma ser um guia para poses e outras coisas.
https://rentry.org/59xed3#multiple-concept-support
Basta pressionar Ctrl F e procurar por conceito.
Além disso, confira o lora do sleepeace e a seção de comentários lá. Esse criador compartilhou arquivos fonte para estudo e deu respostas detalhadas sobre seu processo de treinamento.
O lora de conceito deles é bom, então por que não aprender com alguém que realmente sabe fazer loras de conceito e está disposto a compartilhar seu processo, certo?

---

> [**justTNP - OP**](https://civitai.com/user/justTNP)
Desejo sorte a ambos no treinamento de um LoRA de conceito bem-sucedido e flexível. Eu sou apenas um cara de personagens que conseguiu fazer a pose dos Dedos Indicadores Juntos funcionar (entre outras poses e conceitos básicos). Se vocês tiverem mais perguntas sobre conceitos, vou encaminhá-las para um cara que conheço e confio.

---

> [**bsfgatua278**](https://civitai.com/user/bsfgatua278)
Tenho um LoRa com personagem e roupas, marcado como outfit1, outfit2, outfit3. Existe uma maneira de adicionar (ou mesclar de alguma forma) uma nova roupa, outfit4?

---

> [**justTNP - OP**](https://civitai.com/user/justTNP)
[**@bsfgatua278**](https://civitai.com/user/bsfgatua278) Gostaria de saber o que você quer dizer com mesclar uma nova roupa com seu personagem.

---

> [**bsfgatua278**](https://civitai.com/user/bsfgatua278)
[**@justTNP**](https://civitai.com/user/justTNP) Mesclar um LoRa antigo (contendo personagem e outfit1, outfit2, outfit3) com um novo LoRa treinado (contendo apenas a NOVA outfit4). Assim, o resultado seria um LoRa com personagem e TODAS as outras roupas, incluindo a nova.

---

> [**justTNP - OP**](https://civitai.com/user/justTNP)
[**@bsfgatua278**](https://civitai.com/user/bsfgatua278) Nunca mesclei um LoRA para adicionar uma nova roupa antes, mas ainda recomendaria treinar um novo com todas as pastas/roupas que você tem.

---

> [**alona_chemerys**](https://civitai.com/user/alona_chemerys)
Obrigado!

---

> [**justTNP - OP**](https://civitai.com/user/justTNP)
[**@wktra**](https://civitai.com/user/wktra) Eu assumo que seu personagem é um manequim ou uma boneca, mas tente colocar `mamilos` e `vagina` como prompts negativos. Nunca tentei imagens de regularização antes, mas se você quiser conversar mais ou obter mais opiniões de outras pessoas, posso enviar um convite para um servidor do Discord, se estiver ok.

---

> [**alona_chemerys**](https://civitai.com/user/alona_chemerys)
[**@justTNP**](https://civitai.com/user/justTNP) o mesmo aqui também queria compartilhar e socializar, meu discord é "elegantspy"

---

> [**chefcheiro**](https://civitai.com/user/chefcheiro)
[**@justTNP**](https://civitai.com/user/justTNP) ah eu também estaria interessado nisso

---

> [**alona_chemerys**](https://civitai.com/user/alona_chemerys)
[**@justTNP**](https://civitai.com/user/justTNP) está expirado :( pode enviar o link de convite novamente?

---

> [**alona_chemerys**](https://civitai.com/user/alona_chemerys)
[**@justTNP**](https://civitai.com/user/justTNP) omg eu sou burra haha aparentemente eu já estava no servidor há um tempo, mas não percebi LOL

---

> [**alona_chemerys**](https://civitai.com/user/alona_chemerys)
[**@ZESUMI**](https://civitai.com/user/ZESUMI) você ainda pode usar o colab do strawberry.

---

> [**ZESUMI**](https://civitai.com/user/ZESUMI)
[**@alona_chemerys**](https://civitai.com/user/alona_chemerys), mas hoje usei o colab do strawberry e apareceu um aviso do Google, e o processo de treinamento foi interrompido pelo Google.

---

> [**guy90**](https://civitai.com/user/guy90)
[**@ZESUMI**](https://civitai.com/user/ZESUMI) tente fazer um novo. Foi atualizado para remover a parte da interface, então agora deve funcionar. O Google não está permitindo que as pessoas executem nada com uma interface de graça porque custa muito dinheiro.

---

> [**ZESUMI**](https://civitai.com/user/ZESUMI)
[**@guy907223982**](https://civitai.com/user/guy907223982) ontem fiz um novo, está funcionando bem agora. OBRIGADO!!!

---

> [**Van4lyf**](https://civitai.com/user/Van4lyf)
PRECISO ENTRAR EM CONTATO COM VOCÊ, justTNP. Você é um praticante habilidoso de SD e acho que deveríamos trabalhar juntos. Tenho uma proposta muito importante que posso enviar para você. Deixe-me saber seu e-mail, ou melhor ainda, me envie um e-mail para evan@neoblush.com ou admin@metaroids.com. Aguardo sua mensagem. Obrigado.

---

> [**alona_chemerys**](https://civitai.com/user/alona_chemerys)
[**@Van4lyf**](https://civitai.com/user/Van4lyf), você soa como aqueles bots de golpe, para ser honesto.

---

> [**Van4lyf**](https://civitai.com/user/Van4lyf)
[**@alona_chemerys**](https://civitai.com/user/alona_chemerys), meu LinkedIn é https://www.linkedin.com/in/zencrypto/. Sou real.

---

> [**QwiziRAM**](https://civitai.com/user/QwiziRAM)
Provavelmente o método de aprendizado através do Colab não é mais relevante. Em algum momento, o ambiente é interrompido dizendo "Colab não foi projetado para isso". (29.09.2023).

---

> [**Van4lyf**](https://civitai.com/user/Van4lyf)
[**@wktra**](https://civitai.com/user/wktra), não. Qual é o problema com criptomoedas? Não seja um idiota comprando shitcoins, apenas Bitcoin e você ficará bem como eu.

---

> [**alona_chemerys**](https://civitai.com/user/alona_chemerys)
[**@QwiziRAM**](https://civitai.com/user/QwiziRAM), não, ainda está funcionando bem até agora.

---

> [**guy90**](https://civitai.com/user/guy90)
[**@QwiziRAM**](https://civitai.com/user/QwiziRAM), tente fazer um novo notebook, eles removeram o que estava causando isso, tenho certeza.

---

> [**TomLucidor**](https://civitai.com/user/TomLucidor)
P1: como você preserva a integridade dos conceitos/roupas/poses se as imagens são de um conjunto diverso de personagens?
P2: Se o DupeGuru está dando problemas, já que vários sites têm metadados de tags diferentes, como eles podem ser mesclados de maneira sensata?
P3: Considerando que este guia existe, como a técnica de fotos de casais poderia ser adaptada para este fluxo de trabalho? (desculpe, mas isso é um pouco complexo) https://civitai.com/models/118142?modelVersionId=128118&commentId=290497&modal=commentThread

---

> [**TomLucidor**](https://civitai.com/user/TomLucidor)
P: Que métodos você usa para criar LoRAs com várias roupas para um único personagem? E quanto aos pacotes de personagens?

---

> [**justTNP - OP**](https://civitai.com/user/justTNP)
[**@TomLucidor**](https://civitai.com/user/TomLucidor), não quis ignorar suas perguntas anteriores, embora eu não tenha certeza de como responder P2 e P3. Vamos começar pela P1 do seu comentário anterior:
\
_P1: como você preserva a integridade dos conceitos/roupas/poses se as imagens são de um conjunto diverso de personagens?_
\
A diversidade nos personagens não deve ser uma preocupação, desde que você esteja focado apenas na ideia e no design dos próprios conceitos. Posso ter cem personagens diferentes fazendo uma pose específica e ser capaz de treinar a partir disso.
\
_P2: Que métodos você usa para criar LoRAs com várias roupas para um único personagem? E quanto aos pacotes de personagens?_
\
Para várias roupas de um único personagem, eu as separo em pastas diferentes e dou a elas um número específico de repetições de acordo com a contagem de imagens. Também escrevi quantas épocas você deve treinar no guia acima, mas ajuste as configurações se precisar, como aumentar um pouco mais as épocas. Para as tags, o mínimo necessário para a remover é as cor do cabelo, comprimento do cabelo e cor dos olhos. Remover tags de estilos de cabelo e afins é com você. Mantenha certas tags para que o treinador aprenda melhor uma característica específica (por exemplo, auréola). Marque manualmente suas imagens se precisar. Com boas marcações, o sangramento entre as roupas é menos provável.
\
Para pacotes de personagens, não tenho muita experiência nessa área, já que fiz apenas dois. Mesma ideia: pode o mínimo necessário, pode estilos de cabelo, etc. Vamos supor que você esteja fazendo vários personagens (sem múltiplas roupas), você também poderia apenas eliminar tudo até as tags das roupas. Você não precisa fazer isso, mas é apenas outro método de marcação para fazer um pacote de personagens. Defina repetições e épocas de acordo, altere as configurações se necessário (dim/alpha, talvez, se você tiver muitos personagens).

---

> [**RavirKun**](https://civitai.com/user/RavirKun)
Você sabe qual é o problema ao treinar roupas, mas o cabelo do personagem original continua aparecendo em outro personagem lora?
Por exemplo, as roupas que eu treino são de personagens com rabos de cavalo duplo, mas depois, ao testar em outro personagem lora, o cabelo dela ainda está com rabos de cavalo duplo, mesmo que eu adicione `twintail` no negativo ou um estilo de cabelo diferente no prompt positivo.
\
As roupas funcionam, mas só há problemas com o estilo e a cor do cabelo. Continuam aparecendo.

---

> [**guy90**](https://civitai.com/user/guy90)
[**@RavirKun**](https://civitai.com/user/RavirKun) Não sou especialista em roupas, mas talvez possa tentar adicionar mais imagens sem rabos de cavalo duplo. Estou assumindo que há pouca flexibilidade para o cabelo. Também acho que uma solução simples é adicionar (twintails:1.2) no seu prompt negativo e aumentar o número se eles ainda estiverem presentes.

---

> [**_Qing_**](https://civitai.com/user/_Qing_)
(Tradução) "Respeitado especialista, obrigado por compartilhar sua experiência de treinamento! Sempre gostei do seu modelo Lora porque ele é muito obediente e acho sua qualidade excelente! Tenho querido consultá-lo há muito tempo porque encontrei um problema ao treinar Lora. Tenho tentado fazê-lo aprender mais sobre estilos de coloração, mas ele não tem conseguido captar bem as características dos personagens. A representação das roupas e dos traços faciais assimétricos também é instável, e ele se sai ainda pior em situações da vida real. Tentei ajustar vários parâmetros, incluindo taxa de aprendizado, passos de treinamento, otimizador, rede, e assim por diante, mas ainda sinto que a qualidade não é perfeita. O modelo de treinamento que estou usando é o 'animefull' de 7,9GB, e o programa de treinamento foi compartilhado por um programador chinês na nuvem (ele foi a primeira pessoa a integrar e compartilhar o stable diffusion com o público em outubro do ano passado). Posso perguntar sobre seu modelo de treinamento? Espero poder também contribuir com um modelo melhor para todos."
\
Agradeço suas palavras gentis e seu interesse em melhorar o modelo. No entanto, como um modelo de linguagem de IA, não tenho modelos de treinamento pessoais ou a capacidade de compartilhá-los. Minhas respostas são baseadas em uma mistura de dados licenciados, dados criados por treinadores humanos e dados disponíveis publicamente. Se você tiver alguma pergunta ou precisar de ajuda com problemas específicos, estou aqui para ajudar da melhor forma possível.

---

> [**dita**](https://civitai.com/user/dita)
Esta atualização veio no momento perfeito. Estou prestes a começar a treinar meu primeiro LoRA, agora que tenho um sistema capaz de fazer isso. Obrigado, TNP!

---

> [**beyyaz6384**](https://civitai.com/user/beyyaz6384)
Oi, só estou me perguntando se, ao treinar com batch size 1, devemos diminuir a contagem total de etapas?

---

> [**justTNP - OP**](https://civitai.com/user/justTNP)
[**@beyyaz6384**](https://civitai.com/user/beyyaz6384) Não. Embora eu raramente treine com batch size 1, mantive as outras configurações iguais quando precisei treinar alguns LoRAs localmente.

---

> [**MaidDucko**](https://civitai.com/user/MaidDucko)
Como devo proceder para criar um lora para um personagem PSO2ngs? Como as imagens não serão de alta qualidade e o jogo não está nem no estilo certo.


>> [**justTNP - OP**](https://civitai.com/user/justTNP)
Não tenho certeza, mas posso encaminhá-lo para [**@Shippy**](https://civitai.com/user/Shippy).

>>> [**e0972951006**](https://civitai.com/user/e0972951006)
[**@justTNP**](https://civitai.com/user/justTNP), você pode fazer um modelo de Gagaga Girl (Yu-Gi-Oh!) lora? https://yugioh.fandom.com/wiki/Gagaga_Girl_(anime) https://mega.nz/folder/yI4V1CAC#-xEjtOZs8kYXil5JUr-ELw

---

> [**KRhero**](https://civitai.com/user/KRhero)
Oi, obrigado pelo guia. Que método você usa para treinar modelos SDXL? Estava procurando algo similar ao colab que funcione para SDXL.

>> [**justTNP - OP**](https://civitai.com/user/justTNP)
Aqui está um treinador (um pouco mais complicado) para SDXL LoRAs: https://colab.research.google.com/github/Linaqruf/kohya-trainer/blob/main/kohya-LoRA-trainer-XL.ipynb#scrollTo=_MxF9feWshAp
Não é o que eu uso, porém. Treino meus LoRAs XL através do tensor.art ou emprestando a GPU de um colega criador. Aqui estão algumas das configurações que uso:
>>
>> - Modelo Base: [Animagine XL V3](https://civitai.com/models/260267/animagine-xl-v3) / [Pony Diffusion V6 XL](https://civitai.com/models/257749?modelVersionId=290640)
>> - VAE: [SDXL_VAE](https://huggingface.co/stabilityai/sdxl-vae/tree/main)
>> - No Half Vae: True
>> - Mixed Precision: bf16
>> - Unet LR: 5e-4
>> - Tenc LR: 1e-4
>> - Optimizer: AdamW8bit
>> - LR Scheduler: cosine_with_restarts
>> - Num of Cycles: 3
>> - Repeats: mesmas que faço com 1.5
>> - Epochs: cerca de metade do que normalmente faço em 1.5 (nota: a 4ª época parece ser o ponto ideal para mim para personagens com uma roupa e 100 imagens no Animagine)
>> - Network Dim/Alpha: 8/4 (poderia usar 4/2 em alguns casos)
>> - Noise offset: 0.0357

---

> [**Wolfdua**](https://civitai.com/user/Wolfdua)
Ao usar o modelo base NAI, você deve de alguma forma mesclar/usar o lora com NAI VAE no treinamento porque as cores parecem desbotadas, similar a quando não se usa VAE ao gerar imagens?


>> [**justTNP - OP**](https://civitai.com/user/justTNP)
Nunca incorporei um VAE para treinamento NAI. Incorporar o VAE no NAI não é necessário. No entanto, se você acha que fará diferença, então direcione o treinador para o seu arquivo VAE.

---

> [**JustAnotherDeviate**](https://civitai.com/user/JustAnotherDeviate)
Guia incrível, isso realmente ajudou com meu primeiro Lora de múltiplos estilos bem-sucedido, onde o que eu estava fazendo errado era deixar uma tag universal e uma tag de roupa (então todas as roupas começavam com a mesma tag antes de ter sua própria tag, se isso faz sentido), então não estava associando corretamente. Depois de ler isso, percebi imediatamente o que estava fazendo, mudei para uma tag de ativação diferente para cada roupa, e funcionou perfeitamente. Acho que chamam isso de sangramento de conceito. De qualquer forma, não sei quando isso foi alterado pela última vez, mas só para você saber, você está certo sobre o Prodigy ser agressivo no treinamento, mas isso acontece quando você usa as configurações padrão do colab do hollow-strawberry. Se você ajustar o d_coef=2 para d_coef=0.8, funciona muito melhor, embora obviamente demore um pouco mais, isso evita muito o overfitting. Além disso, se você quiser aumentar o dim e alpha no colab do SDXL, pode alterar o valor máximo dos sliders no código Python. Também sugiro adicionar uma linha de código para incluir dropout de cerca de 0.3, pois acho que isso também pode ajudar a evitar overfitting.

>> [**justTNP - OP**](https://civitai.com/user/justTNP)
Obrigado, bom ver! Também vi sugestões para mudar algumas configurações do Prodigy, como LR e d-coef. Se eu voltar a usar este otimizador, vou mexer com isso.
\
Estou familiarizado com Colab e Python, então sei como mudar alguns valores aqui e ali. Não há motivo para eu mudar o dim/alpha máximo para além de 32 (a menos que seja absolutamente necessário para múltiplos estilos em um só, o que eu nunca fiz).
\
Talvez eu mexa com dropout, mas eu, junto com alguns outros criadores, não encontrei problemas de overfitting com SDXL (a menos que estejamos falando de 1.5 e/ou modelos não relacionados a personagens).

---

> [**GreenBottleFly695**](https://civitai.com/user/GreenBottleFly695)
Olá. Obrigado por postar.
\
Acho que estou seguindo um híbrido deste guia E do guia do holostrawberry. No entanto, estou fazendo as coisas de forma diferente na etapa de coleta de imagens.
\
Há três categorias básicas das imagens que salvei:
\
-fanart (coisas desenhadas ou renderizadas de qualidade aceitável e que eu realmente gosto ou pelo menos são 'bastante próximas' do material original)
\
-arte oficial, bastante autoexplicativa, ilustrações
\
-capturas de tela capturadas do material original
\
Minha pergunta é sobre duplicações potenciais: nessa terceira categoria: Vou encontrar problemas? Estou fazendo isso de forma meio antiga, embora com comodidades modernas básicas de computador. Venho de uma época em que usávamos VHS e um dispositivo digital para analógico que chamávamos de 'jog shuttle' para revisão de imagens quadro a quadro. Embora eu definitivamente NÃO esteja sendo tão meticuloso nem abrangente, posso dizer que tenho quase 200 imagens. Pode ser mais, ou menos.
\
Como imagens duplicadas afetam negativamente os resultados?
\
Vejo que você diz que .png é problemático, mas também diz que se o fundo estiver preenchido, está tudo bem? Não vejo que haja qualquer fundo em branco para transparência em nenhuma das minhas imagens, já que todas são capturas de tela... Suponho que não vai me matar converter as que eu mantenho para um formato de arquivo diferente. Isso é imperativo se nenhuma delas tiver fundos transparentes?
\
Sim, sei que devo cortar coisas largamente irrelevantes das imagens, o que é exatamente o que estou fazendo. Obviamente, vou passar manualmente por tudo e tentar colocar tudo em sequência para evitar duplicatas. O problema é que muitos quadros parecem iguais ou algumas imagens cortadas são cortes diferentes das mesmas (ou muito semelhantes) imagens. Não acho que nenhuma seja tão sutil ou semelhante a ponto de ser imperceptível, mas obviamente aquelas que são precisam ser eliminadas. Então, acho que o que eu perguntaria aqui é o que mais você recomendaria? Clarear imagens escuras? Também ouvi que monocromático ajuda a acelerar o processo às vezes?
\
Também estou ciente de que devo marcar qualquer coisa como texto ou UI ou outros símbolos gráficos e afins, ou filtros (por exemplo, para imitar linhas horizontais de varredura como uma filmadora) para que a IA faça a distinção. Estes não estarão em todas as imagens, apenas talvez em algumas que eu não consegui evitar. Sem mais manipulação de imagem, há algo mais a fazer além de marcar essas anomalias?
\
Estou seguindo o guia do holostrawberry porque atualmente não possuo hardware para fazer meu próprio treinamento de modelo. O seu parece um pouco mais detalhado para a forma como estou integrando minhas imagens.
\
No momento estou cortando e salvando novas versões das imagens capturadas. Em seguida, tentarei sequenciá-las adequadamente. Depois, ajustando coisas como visibilidade em baixa luminosidade. Espero então estar pronto para marcá-las.
\
De qualquer forma, obrigado por tudo que você postou até agora.

>> [**justTNP - OP**](https://civitai.com/user/justTNP)
**"Minha pergunta é sobre duplicações potenciais: nessa terceira categoria: Vou encontrar problemas? [...] O problema é que muitos quadros parecem iguais ou algumas imagens cortadas são cortes diferentes das mesmas (ou muito semelhantes) imagens. Não acho que nenhuma seja tão sutil ou semelhante a ponto de ser imperceptível, mas obviamente aquelas que são precisam ser eliminadas. Então, acho que o que eu perguntaria aqui é o que mais você recomendaria?"**
>>
>>Você pode encontrar problemas se houver muitos quadros de, digamos, planos de meio corpo. Vai ficar muito rígido a menos que qualquer coisa da cintura para baixo seja sugerida ou a força do LoRA seja reduzida. Definitivamente, delete qualquer quadro que pareça muito semelhante. Manter imagens onde a câmera está apenas girada 3 graus ao redor do personagem não vale a pena.
>>
>>---
>>
>>**"Como imagens duplicadas afetam negativamente os resultados?"**
\
Imagens duplicadas não terão um impacto significativo, mas se por algum motivo compuserem a maior parte do conjunto de dados, coisas como poses ficarão fixas se não forem sugeridas para fazer algo diferente como `sentado` ou `mãos nos quadris`.
>>
>>---
>>
>>**"Clarear imagens escuras?"**
\
Já ouvi falar de um método mexendo na iluminação, coloração, sombreamento e brilho, mas não consigo lembrar completamente do processo. Você pode tentar, se quiser.
>>
>>---
>>
>>**"Também ouvi que monocromático ajuda a acelerar o processo às vezes?"**
\
Nunca ouvi falar disso, e também não sei o que você quer dizer com isso.
>>
>>---
>>
>>**"Também estou ciente de que devo marcar qualquer coisa como texto ou UI ou outros símbolos gráficos e afins, ou filtros (por exemplo, para imitar linhas horizontais de varredura como uma filmadora) para que a IA faça a distinção. Estes não estarão em todas as imagens, apenas talvez em algumas que eu não consegui evitar. Sem mais manipulação de imagem, há algo mais a fazer além de marcar essas anomalias?"**
\
A menos que você absolutamente não tenha escolha, eu removeria imagens com essas anomalias. Caso contrário, nada que eu possa pensar.
>>
>>---
>>
>>**"Vejo que você diz que .png é problemático, mas também diz que se o fundo estiver preenchido, está tudo bem? Não vejo que haja qualquer fundo em branco para transparência em nenhuma das minhas imagens, já que todas são capturas de tela... Suponho que não vai me matar converter as que eu mantenho para um formato de arquivo diferente. Isso é imperativo se nenhuma delas tiver fundos transparentes?"**
\
Para esclarecer, é problemático se o seu conjunto de dados estiver cheio de imagens com fundos transparentes. Você não precisa converter nada se houver pouca ou nenhuma transparência em suas imagens.

---

> [**Shirabe2029**](https://civitai.com/user/Shirabe2029)
Olá, tenho uma pergunta sobre suas recomendações para Dora. Sua taxa de aprendizado é incrivelmente baixa. Especialmente considerando o quão poucas épocas/repetições você recomenda. Pela minha experiência, Dora não aprende muito mais rápido do que Loras normais (embora sim, os resultados pareçam superiores). Você está assumindo um conjunto de dados muito grande para um único conceito? Gostaria de saber seus pensamentos sobre por que você aborda dessa maneira.

>> [**justTNP - OP**](https://civitai.com/user/justTNP)
Eu não estava assumindo um conjunto de dados muito grande, já que meu limite é 100 imagens por conceito.
\
Não questionei nem consegui pesquisar o motivo pelo qual o unet foi reduzido pela metade, então decidi perguntar diretamente ao bluvoll (criador do estilo) e ele respondeu: "Decidi isso devido ao artigo do DoRA e, como o DoRA está um pouco mais próximo de um ajuste fino, você não precisa aprender muito rápido, você quer que ele aprenda bem. A coisa engraçada é que, em meus testes, 1e-4 estava causando overfitting ou simplesmente não aprendia tão bem."

---

> [**Kaze111**](https://civitai.com/user/Kaze111)
Olá, não relacionado, mas quando baixei o Grabber, meu computador disse que continha vírus, fiquei um pouco preocupado, é realmente seguro?

>> [**justTNP - OP**](https://civitai.com/user/justTNP)
Sim.

---

> [**LeskiSTL**](https://civitai.com/user/LeskiSTL)
Você tem algum conselho para mãos ruins? Estou treinando um conceito DoRa e as mãos estão confusas. Tenho um bom, mas pequeno conjunto de dados (cerca de 20 imagens), isso pode ser a causa?

>> [**justTNP - OP**](https://civitai.com/user/justTNP)
Ou você está testando uma época sobrecarregada ou as mãos no seu conjunto de dados parecem estranhas.

---

> [**WhiteSilence**](https://civitai.com/user/WhiteSilence)
Olá, quero perguntar uma coisa sobre o tamanho do lote: isso afeta o resultado ou é apenas uma questão de desempenho?

>> [**justTNP - OP**](https://civitai.com/user/justTNP)
Ambos.

---

> [**masterjames18**](https://civitai.com/user/masterjames18)
Olá, estou tentando usar os colabs do SDXL do Google, mas continuo recebendo um erro quando tento executá-los.
Você sabe o que está dando errado??
>
>```
>Traceback (most recent call last): File "/content/kohya-trainer/train_network_xl_wrapper.py", line 10, in <module> trainer.train(args)
>File "/content/kohya-trainer/train_network.py", line 731, in train for step, batch in enumerate(train_dataloader): File "/usr/local/
>lib/python3.10/dist-packages/accelerate/data_loader.py", line 388, in iter next_batch = next(dataloader_iter) File "/usr/local/lib/
>python3.10/dist-packages/torch/utils/data/dataloader.py", line 631, in next data = self._next_data() File "/usr/local/lib/python3.10/
>dist-packages/torch/utils/data/dataloader.py", line 1346, in nextdata return self._process_data(data) File "/usr/local/lib/python3.10/
>dist-packages/torch/utils/data/dataloader.py", line 1372, in processdata data.reraise() File "/usr/local/lib/python3.10/dist-packages/
>torch/_utils.py", line 705, in reraise raise exception RuntimeError: Caught RuntimeError in DataLoader worker process 0. Original 
>Traceback (most recent call last): File "/usr/local/lib/python3.10/dist-packages/torch/utils/data/_utils/worker.py", line 308, in 
>workerloop data = fetcher.fetch(index) # type: ignore[possibly-undefined] File "/usr/local/lib/python3.10/dist-packages/torch/utils/
>data/_utils/fetch.py", line 51, in fetch data = [self.dataset[idx] for idx in possibly_batched_index] File "/usr/local/lib/python3.10/
>dist-packages/torch/utils/data/_utils/fetch.py", line 51, in <listcomp> data = [self.dataset[idx] for idx in possibly_batched_index] 
>File "/usr/local/lib/python3.10/dist-packages/torch/utils/data/dataset.py", line 348, in getitem return self.datasets[dataset_idx]
>[sample_idx] File "/content/kohya-trainer/library/train_util.py", line 1189, in getitem example["latents"] = torch.stack(latents_list) 
>if latents_list[0] is not None else None RuntimeError: stack expects each tensor to be equal size, but got [4, 144, 112] at entry 0 
>and [4, 168, 96] at entry 1 steps: 2% 12/520 [01:45<1:14:33, 8.81s/it, loss=0.116]
>```

> [**justTNP - OP**](https://civitai.com/user/justTNP)
É melhor perguntar a quem é o autor do notebook que você está usando.

>>> [**masterjames18**](https://civitai.com/user/masterjames18)
Estou usando o Holostrawberry's LoRA Trainer Colab SDXL que você mencionou neste post? >o>

>>>> [**justTNP - OP**](https://civitai.com/user/justTNP)
Então, envie uma DM para Holostrawberry ou entre no servidor Discord dele: https://discord.com/invite/hGHnfda

---