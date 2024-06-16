## [Guia de Treinamento SDXL Lora com Opiniões](https://civitai.com/articles/1716/opinionated-guide-to-sdxl-lora-training-updated)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/96848841-fae5-4857-8465-f491bd5eb76c/width=150/58C081EDA1C4772C2AE1B6FB2E3EB5E212D8677A3CD82BFFD3CBA9B9CAC5A5CF.jpeg)

-Tags: `#TRAINING GUIDE` `#TRAINING METHOD` `#SDXL LORA` `#LORA TRAINING`
-Autor: [**duskfallcrew**](https://civitai.com/user/duskfallcrew)

---

## Prefácio

**_<u><span>Originalmente escrito em agosto de 2023</span></u>_**

Esta é uma edição EXTREMAMENTE OPINATIVA "CIÊNCIA NESSA BISCA" de treinamento SDXL Lora - NADA DISSO é perfeito, e NADA DISSO É "você deve fazer do meu jeito ou você é um trouxa!". Na verdade, isso é APENAS UM TRIBUTO - não é o maior treinamento do mundo - é, sabe -- um TRIBUTO! _toque Lucifer e Jack Black_

Faça do seu jeito - se você tiver uma maneira mais legal de fazer isso - INCRÍVEL!

Edit: ISTO AGORA ESTÁ ATUALIZADO PARA 2024. - Não vou discutir aqui sobre como faço as coisas porque, como dito, isso é opinativo, eu não sou um deus da IA - e cada um tem seu jeito de fazer as coisas. Houve algumas críticas sobre pessoas treinando com Pony, e este não é o artigo para discutir isso. Você escolhe seu modelo, faz o que precisa fazer e segue em frente - Cada um tem seu estilo!

## Requisitos de Software

Ano passado em agosto: Eu estava usando BMALTAIS para isso porque no momento atual, sim, Linaqruf tinha uma configuração 0.9 Lora e SIM, LastBen também tinha um notebook para SDXL 1 - Eu estava tentando aprender bmaltais para isso.

Este ano: O Colab deu problema para a maioria dos USUÁRIOS PAGOS, Bmaltais ainda me confunde até hoje, eeeee eu uso principalmente o Civitai Trainer.

Dito isso:  
Acho que o LastBen pode estar usando o mesmo código que o bmaltais, só que sem a interface gráfica, então a maioria dessas configurações deve funcionar de qualquer maneira.

### Links de Repositório (*Local & Servidor)

Eu NÃO usei todos esses e AVISO PRÉVIO: a AUP do Colab é duvidosa PRA CARAMBA e pode levar a lentidões. E também nem todos esses podem funcionar como esperado. Tenha cuidado!

Bmaltais repo: [https://github.com/bmaltais/kohya_ss](https://github.com/bmaltais/kohya_ss)

LastBen: [https://github.com/TheLastBen/fast-stable-diffusion](https://github.com/TheLastBen/fast-stable-diffusion)

Holo's SDXL for Colab: [https://github.com/hollowstrawberry/kohya-colab](https://github.com/hollowstrawberry/kohya-colab)

Derrian's forked for Colab: [https://github.com/Jelosus2/LoRA_Easy_Training_Colab_Frontend](https://github.com/Jelosus2/LoRA_Easy_Training_Colab_Frontend)

SD Web UI Extension: [https://github.com/hako-mikan/sd-webui-traintrain](https://github.com/hako-mikan/sd-webui-traintrain)

Image Captioning for ComfyUI: [https://github.com/LarryJane491/Image-Captioning-in-ComfyUI](https://github.com/LarryJane491/Image-Captioning-in-ComfyUI)

Derrian's back end for SErver: [https://github.com/derrian-distro/LoRA_Easy_Training_scripts_Backend](https://github.com/derrian-distro/LoRA_Easy_Training_scripts_Backend)

ComfyUI Lora Trainer: [https://github.com/LarryJane491/Lora-Training-in-Comfy](https://github.com/LarryJane491/Lora-Training-in-Comfy)

Train WebUI: [https://github.com/Akegarasu/lora-scripts](https://github.com/Akegarasu/lora-scripts)

OneTrainer: [https://github.com/Nerogar/OneTrainer](https://github.com/Nerogar/OneTrainer)

Lora Studio: [https://github.com/michaelringholm/lora-studio](https://github.com/michaelringholm/lora-studio)

Derrian Main scripts: [https://github.com/derrian-distro/LoRA_Easy_Training_Scripts](https://github.com/derrian-distro/LoRA_Easy_Training_Scripts)

**OS TEMPLATES DOCKER A PARTIR DE 2024 são configurados automaticamente, exceto no caso de algum conteúdo personalizado como o do Derrian que não está dockerizado:**

Runpod: [https://runpod.io/](https://runpod.io/)

VastAI: [https://cloud.vast.ai/](https://cloud.vast.ai/)

### AVISO LEGAL:

A maior parte da minha burrice é baseada no notebook extremamente útil do HoloStrawberry para SD 1.5 ao qual me apeguei para salvar minha vida + o notebook SD 1.5 do Linaqruf anterior a isso. Se você ainda é um camarada do SD 1.5, PARABÉNS PARA VOCÊ! Eu não estou deixando o 1.5, só estou brincando com os novos brinquedos enquanto ainda estão frescos e quentes!

Então basicamente: Não grite comigo se eu não souber o que estou fazendo - aprendi com os mais experientes e ainda não sei o que estou fazendo - eu odeio matemática, números me irritam e eu apenas sigo o quão BEM fica quando está pronto.

## Tempo de Treinamento

### Coleta de Dados e Preparação

Primeiro de tudo, você precisa ter DADOS - e neste caso realmente não importa mais quantas imagens você tem (nunca importou, só sou preguiçoso)

Eu recomendaria pelo menos 10 imagens. Você pode literalmente se virar repetindo e ajustando os epochs dessa maneira.

Lembrete: ESTE É UM NÍVEL DE TREINAMENTO "CIÊNCIA NESSA BISCA", ninguém é perfeito - na verdade, vou admitir isso: Aprendi meu SDXL com Envy e um pouco com GoofyAI - você sempre tem alguém para te ajudar!

A preparação do CONJUNTO DE DADOS em geral está em outro artigo meu, EU ACHO, mas há muitos artigos sobre isso por aí.

Então seus passos para isso devem ser:

1 - COLETE SEUS DADOS

2 - FAÇA UPSCALE SE NECESSÁRIO - Certifique-se de que pelo menos 75% dos seus dados estejam acima de 512, se não acima de 1024, pode ser importante, mas nem sempre.

3 - PREPARE SUAS PASTAS

### NOTA DE ATUALIZAÇÃO:

Você PODE FAZER GAMBIARRA com alguns tamanhos menores, pois Civitai e a maioria das coisas usarão bucketing. De alguma forma, você não era permitido usar bucketing ou qualquer coisa e era instruído a usar apenas 1-1 no começo - descobrimos em ALGUNS CONTEÚDOS - não todos, isso pode produzir MAIS problemas no treinamento de IA do que o contrário. ISSO é mais um "Vimos isso" do que "É a bíblia, siga isso".

### Estrutura BMaltais:

A ESTRUTURA DA SUA PASTA fica um pouco estranha se você estiver usando BMALTAIS/Kohya, isso não importará tanto no LastBen ou quando outros começarem a fazer notebooks Colab - mas se você estiver usando Bmaltais no Runpod ou local:

_NOME DA PASTA DE DADOS_

- IMG
    
- MODEL
    
- LOGS
    

Dentro de "IMG" você vai querer estruturá-la com um conceito básico - a documentação é um pouco confusa, mas aqui está como eu tenho feito:

\+ Número de Repetições_ NOME DO CONCEITO

por exemplo: 2_ilustração

Seu número de repetições vai depender totalmente de QUANTO TEMPO e quão forte você quer seu lora. Vamos chegar a isso na próxima seção, no entanto.

### ESTRUTURA DO CONJUNTO DE DADOS NO LOCAL:

Keka para Mac + PASTA GRANDE.  
É isso. Sem estrutura.

Se você não estiver em um Mac, apenas compacte.

Além disso, uma BOA ideia é possivelmente renomear os arquivos, havia algo nos dias de treinamento antigos onde o nome do arquivo talvez influenciasse o treinamento de palavras-chave.

Também, honestamente, se você estiver procurando reorganizar seus conjuntos de dados mais tarde, isso ajuda MUITO!

### MODELO BASE? (SE USANDO FORA DO LOCAL)

Envy recomenda a base SDXL. Eu tenho usado uma mistura do modelo do Linaqruf, Overdrive XL do Envy e base SDXL para treinar coisas. Como o SD 1.5, isso é totalmente preferencial. O modelo do Envy deu resultados fortes, mas VAI QUEBRAR o lora em outros modelos. Infelizmente, qualquer coisa treinada no Envy Overdrive não funciona no modelo OSEA SDXL.

### MODELO BASE (NO LOCAL)

Custa MAIS TREINAR em um modelo CUSTOMIZADO, e PODE falhar mais facilmente do que os modelos padrão. Eu não sou seu pai: você pode fazer o que quiser. Eu prefiro não treinar REALISMO com tanta frequência, na verdade, engraçado o suficiente, EU GOSTO DE TREINAR SEMI REALISMO EM CHECKPOINTS DE ANIME porque não estou realmente interessado em realismo completo o tempo todo.

Pessoal do Animagine: Não venham brigar comigo, não estou contra treinar no Animagine, só que requer cuidado e um pouco de magia para fazê-lo funcionar - e eu sou meio que -- Eu gosto da fluidez do SDXL, mas não da dureza do Animagine para o que _EU_ faço. é uma opção no local, mas você paga o risco.

SDXL: Eu prefiro pony, mas vou treinar tanto na base quanto no Pony.

SD 1.5: Eu quase desisto a essa altura, mas este é um ARTIGO SOBRE SDXL, NÃO É :3

### Repetições + Épocas (MAIORIA DOS LUGARES, MAS ISSO É ANTIGO)

Novamente, tudo isso é uma preferência.

Suas repetições são um jogo de matemática, suas épocas são um jogo de matemática. Eu não sou bom em matemática, sou o AUTISTA ARTÍSTICO - então aqui está como eu resolvo isso:

10-20 imagens precisam de PELO MENOS 10 repetições POR ÉPOCA. Com 10 imagens, na verdade fui para 40 repetições, porque as épocas nem sempre correspondem aos passos por repetição - então você infelizmente ainda tem um jogo de matemática - mas eu tenho feito 5 épocas e 1-2 tamanho de lote máximo.

Seu tamanho de lote ajudará a calcular os passos.

### Logs do Kohya SS/Bmaltais (DESATUALIZADO para 2023)

Atualmente, tenho um treino de mais ou menos uma hora em um conjunto de 2 repetições com 500 imagens - Ainda estou experimentando, e este pode DAR ERRADO, mas aqui está a configuração:

executando treinamento / 学習開始

num train images * repeats / 学習画像の数×繰り返し回数: 1006

num reg images / 正則化画像の数: 0

num batches per epoch / 1epochのバッチ数: 503

num epochs / epoch数: 5

batch size per device / バッチサイズ: 2

gradient accumulation steps / 勾配を合計するステップ数 = 1

total optimization steps / 学習ステップ数: 2515

Usando método DreamBooth.

ignore directory without repeats / 繰り返し回数のないディレクトリを無視します: .ipynb_checkpoints

prepare images.

found directory dataset/Yashahime_Style/img/2_anime contains 503 image files

1006 train images with repeating.

0 reg images.

no regularization images / 正則化画像が見つかりませんでした

[Dataset 0]

batch_size: 2

resolution: (1024, 1024)

enable_bucket: True

min_bucket_reso: 256

max_bucket_reso: 2048

bucket_reso_steps: 64

bucket_no_upscale: True

[Subset 0 of Dataset 0]

image_dir: "dataset/Yashahime_Style/img/2_anime"

image_count: 503

num_repeats: 2

shuffle_caption: True

keep_tokens: 0

caption_dropout_rate: 0.05

caption_dropout_every_n_epoches: 0

caption_tag_dropout_rate: 0.0

color_aug: False

flip_aug: False

face_crop_aug_range: None

random_crop: False

token_warmup_min: 1,

token_warmup_step: 0,

is_reg: False

class_tokens: anime

caption_extension: .txt

### Repetições + Épocas (NO LOCAL)

Os padrões me deixam louco, porque às vezes você obtém um treino MUITO FRACO. É POSSÍVEL, e pode funcionar dependendo do que você está fazendo.

No entanto, se você estiver fazendo personagens, a menos que seja um dos mestres OG _aponta para os colegas_ e saiba sua magia --- Eu recomendaria o seguinte:

-   Certifique-se de que seus passos não ultrapassem 3k tanto no SDXL quanto no Pony, não me pergunte sobre o Animagine, não faço isso há um tempo.
    
-   Absolutamente certifique-se de equilibrar suas repetições com o tamanho do lote.
    
-   Os níveis de treinamento, pelo que aprendi com o Holostrawberry, é que o TAMANHO DO LOTE não deve ser maior que 2-4. Se você fizer mais do que isso, há algo sobre repetições.
    
-   VOCÊ

 NÃO PRECISA de uma contagem de repetições de 10 no SDXL, mas se você tiver algo como 50 imagens, pode aumentar para dez.
    

### Taxa de Aprendizado + Outras Coisas _FORA DO LOCAL_

Ok, A TAXA DE APRENDIZADO VAI CAUSAR UMA DISCUSSÃO NÍVEL TERCEIRA GUERRA MUNDIAL aqui, mas deixe-me lembrar antes de você me criticar: SDXL funciona diferente do 1.5. Também é uma preferência em COMO e por que e outras coisas.

Taxa de Aprendizado que tenho usado com sucesso moderado a alto: 1e-7  
Taxa de aprendizado no SD 1.5 que PODE FUNCIONAR se você souber o que está fazendo, mas não funcionou para mim no SDXL: 5e4

Anexei outro JSON das configurações que correspondem ao ADAFACTOR, que funciona, mas não achei que funcionou para MIM, então voltei para as outras configurações - Isso é LITERALMENTE uma preferência, aliás.

Tenho usado DADAPTATION, com COSINE - e não tenho usado reinicializações a menos que fosse a versão Adafactor.

Exceção: Yashahime está quase terminando e anexarei o JSON para estudo também.

Então, com o Estilo Yashahime, voltei e adicionei COSINE COM REINICIALIZAÇÕES, como SD 1.5 e GoofyAI têm usado.

Também adicionei o 'MIN SNR GAMMA' sem saber se influencia o SDXL ou não, então configurei o MIN SNR GAMMA para 5, como os notebooks originais do 1.5 indicavam.

Até agora, suas configurações devem ser:

-   Lora padrão (não tenho ideia do que mais funciona no SD XL ainda)
    
-   Tamanho do lote de treino NÃO MAIS QUE 3 - Você pode aumentar para 3 no SDXL, mas isso enfraquece muito.
    
-   FP16
    
-   Épocas são uma preferência, mas 3-5 devem ser suficientes. (Isso também depende se você quer mais repetições ou mais tempo de cozimento, não queime seu lora ou eu vou te bater, rs)
    
-   LR SCHEDULER: Cosine ou Cosine com Reinicializações (Não sei se configurei o meu corretamente, estou cansado, confira minhas configurações para Yashahime)
    
-   LR CYCLES - Configurei para 3 porque acho que era isso que eu estava fazendo nas coisas do Holostrawberry.
    
-   Cache Latents & CACHE THEM TO DISK (mesmo no runpod, faça isso)
    
-   SEED: Eu não sei, apenas -- Eu configurei o meu da mesma forma que o Envy fez 12345 - Eu sei que normalmente o seed é como -1 no 1.5, mas estou esgotado, shht.
    
-   Otimizador: DaDaptation - Para mim isso funciona, OU use Adafactor (Verifique o arquivo JSON do Pinkspider para quaisquer argumentos extras para o otimizador)
    
-   L% Warmup: Esqueci, como um idiota, de realmente corrigir isso, não me lembro da configuração original.
    
-   NO HALF VAE: você não clica nisso, você obtém "NANS IN LATENT" - e você vai gritar.
    

### Taxa de Aprendizado + Outras Coisas

-   Codificador de Texto **DESLIGADO** para a maioria das coisas, na verdade, mesmo para personagens, achei que funciona MUITO MELHOR. (sou um chato, não tome isso como uma declaração bíblica XD)
    
-   Taxa de Aprendizado: DEIXE ISSO QUIETO. tipo sabe, 2005's "DEIXEM A BRITNEY EM PAZ" -- ou 2024's "DEIXEM O JOJO EM PAZ (ou não?)"
    
-   YOLO: não mexa com outras configurações se estiver fazendo PONYXL - não tente aumentar o ruído ou o MIN SNR GAMMA: MIN SNR GAMMA NÃO É APENAS MATEMÁTICA, é na verdade um conceito de fotografia chamado: SUA COISA VAI QUEIMAR. XD
    

### TAMANHO DIM? (DESATUALIZADO)

_DELETED OLD STATEMENT CAUSE OUT OF DATE INFO_ - Isso é preferência, só por favor esteja ciente de que se você lançar um lora de 3 gb em alguém, eles vão redimensioná-lo. 32/16 deve ser totalmente aceitável na maioria dos casos, mas isso é preferência.

### TAMANHO DIM? (NO LOCAL)

Basta deixar o dim/alpha para os padrões no local.

### Outras Configurações? (DESATUALIZADO)

*(FORA DO LOCAL, MAS NÃO PONY, CONFIGURAÇÕES PODEM SER, NÃO VOU MEXER COM ISSO)

Ah sim, vamos terminar isso, não?

-   Checkpoint de Gradiente
    
-   Embaralhar legenda (SEMPRE SEMPRE!)
    
-   Atenção eficiente de memória
    
-   Xformers (se você tiver, use)
    
-   MIN SNR GAMMA - falamos sobre isso anteriormente -5.
    
-   NÃO FAÇA UPSCALE NA RESOLUÇÃO DO BUCKET (Não pergunte, não conte, é auto-clicar para mim)
    
-   Passos de resolução do bucket 64.
    
-   (UM MONTE de configurações para passos de tempo, deixe essas sozinhas, são automáticas)
    
-   Desvio de ruído: OG e configurado para: 0.0357
    
-   Escala de ruído adaptativo: 0.00357
    
-   Algo sobre Taxa de dropout de legenda (não uso porque embaralho legenda, mas está configurado para: 0.05)
    
-   Se você tiver uma API do WANDB (Weights and Balances) configurada - vá em frente, às vezes gosto disso porque o tensorboard me confunde às vezes
    

## 2023 - COMPARAÇÕES DE OUTPUT DO SDXL

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/5bfe7107-1660-40a4-a318-9b9b092eae66/width=525/5bfe7107-1660-40a4-a318-9b9b092eae66.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/9861eda5-a2ca-47a4-9f99-b75d3b5e3c52/width=525/9861eda5-a2ca-47a4-9f99-b75d3b5e3c52.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f22ae137-a6c8-4ee4-9f48-10b7365238dc/width=525/f22ae137-a6c8-4ee4-9f48-10b7365238dc.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/731e7407-56a9-45da-bfc6-20336576a4c1/width=525/731e7407-56a9-45da-bfc6-20336576a4c1.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/382562a5-1284-46cc-a8f5-87d10e020c32/width=525/382562a5-1284-46cc-a8f5-87d10e020c32.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/5ada77f6-3027-4ac2-80c3-73dee58a0638/width=525/5ada77f6-3027-4ac2-80c3-73dee58a0638.jpeg)

### Comparações de outputs XL & Pony 2024

Alguns destes são loras, e alguns são loras aplicados em um modelo, mas foram deixados para comparação de como as coisas estão evoluindo.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/1eff82fe-e599-481d-b817-64b1dea7d79e/width=525/1eff82fe-e599-481d-b817-64b1dea7d79e.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/225961bd-c9e9-42fa-8445-f172d57ba469/width=525/225961bd-c9e9-42fa-8445-f172d57ba469.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/af6c3a37-af59-4879-a03d-58aa4af86c6e/width=525/af6c3a37-af59-4879-a03d-58aa4af86c6e.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/0e328f7f-8fc5-4cce-ba61-e4c24bb4edb8/width=525/0e328f7f-8fc5-4cce-ba61-e4c24bb4edb8.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c5181151-9cad-4d7d-b060-ce858329fc31/width=525/c5181151-9cad-4d7d-b060-ce858329fc31.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/74804b73-0d1f-4918-9168-ae92f3bf1407/width=525/74804b73-0d1f-4918-9168-ae92f3bf1407.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/972e04ec-7479-48aa-abd7-f8df1d531d0a/width=525/972e04ec-7479-48aa-abd7-f8df1d531d0a.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/932648e6-e348-453d-93dc-d24461e360d0/width=525/932648e6-e348-453d-93dc-d24461e360d0.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/febd6648-89a9-4b87-856a-0f5ebede9dca/width=525/febd6648-89a9-4b87-856a-0f5ebede9dca.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f3ae4348-004e-491a-a0fb-74f8c6ebfb87/width=525/f3ae4348-004e-491a-a0fb-74f8c6ebfb87.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/cb2f32e2-a714-44f7-aadc-4254a92236c0/width=525/cb2f32e2-a714-44f7-aadc-4254a92236c0.jpeg)

## Conclusões

SDXL supera o SD1.5 -- exceto no hardware, rs, aí você chora até o banco.

Sempre que você entra na discussão de treinamento de lora, é como política e religião - ou você vai concordar ou vai arruinar o jantar de Ação de Graças.

Lembre-se, pessoal: Esta é uma opinião, esta é a minha maneira de fazer - e eu não sou a autoridade máxima.

EDIT:

Os `.json` incluídos são de 2023, eles PODEM funcionar ou não. Em grande parte, a razão de não funcionarem PARA MIM é que meu estilo sempre foi DROP AND RUN em vez de meticulosamente criar um conjunto de dados.. (Exceto para personagens) - Além disso, esses eram de MUITO CEDO, em agosto de 2023....

Também não tenho mais presentes de embedding para adicionar ao monte -- desculpe XD