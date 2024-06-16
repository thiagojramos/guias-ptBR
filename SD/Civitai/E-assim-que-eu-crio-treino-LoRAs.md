# [Como eu crio e treino LoRAs](https://civitai.com/articles/3921/this-is-how-i-create-and-train-loras)

![This is how I create and train LoRAs](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/4328e77a-17a3-4a49-a644-fc53a8b3ff13/width=400/4328e77a-17a3-4a49-a644-fc53a8b3ff13.jpeg)

-Tags: `#training guide`, `#lora guide`

-Autor: [**Skullkid**](https://civitai.com/user/Skullkid)

<p></p>

## Introdução

---

### **Modelos** usados para o treinamento

Para SD 1.5 **fotos realistas**:

-   **Treinamento**: [Realistic Vision 5.1](https://civitai.com/models/4201?modelVersionId=130090)  
    _Este modelo foi adicionado em muitas fusões e também faz parte de uma fusão. Funciona bem com quase todos os outros modelos realistas. A v6, por algum motivo, não funciona tão bem da mesma maneira._

-   **Geração**: _Realistic Vision 5.1 e um img2img com um denoise de 0.1 usando_ [_PicX Real 1.0_](https://civitai.com/models/241415?modelVersionId=272376)_. RV sozinho faz o trabalho, mas gostei dessa combinação._


Para SD 1.5 **Anime**:

-   **Treinamento**: [AnyLora](https://civitai.com/models/23900?modelVersionId=272376)  
    _Este é um clássico para treinamentos._

-   **Geração**: [Azure Anime v5](https://civitai.com/models/130701?modelVersionId=168036)


Para SDXL **geral**:

-   **Treinamento**: [SDXL Base model](https://civitai.com/models/101055/sd-xl)

-   **Geração**: [Dreamshaper XL Turbo](https://civitai.com/models/112902/dreamshaper-xl). Eu uso 7 passos, e então faço um img2img com o mesmo prompt, mas uma nova seed, e o resultado fica bom!


---

### **Ferramentas** que uso

-   **Treinamento**: [Kohya_ss](https://github.com/bmaltais/kohya_ss)  
    _Mas às vezes eu só rodo um script que fiz para acelerar o processo, ele chama os scripts Kohya com todos os parâmetros. Falarei sobre isso no final._  
    **Instalar Kohya está além do escopo deste guia.**

-   **Geração**: [InvokeAI](https://github.com/invoke-ai/InvokeAI)  
    _Desculpa galera, eu tenho Automatic1111 e ComfyUI aqui, mas eu amo o InvokeAI._


---

## Preparação do Dataset

Esta é a parte mais importante. Você tem que coletar imagens da pessoa, objeto ou o que quer que você queira treinar.

### Há algumas coisas a considerar

-   Evite resoluções baixas ou imagens pixeladas. Não recomendo upscaling.

-   Evite imagens muito diferentes umas das outras. Eu geralmente me atenho a retratos.

-   Evite imagens com muita maquiagem, muitos brincos e colares, poses estranhas.

-   MÃOS. Evite imagens onde as mãos aparecem demais em posições estranhas.

-   Eu corto uma por uma para garantir que incluam apenas o sujeito a ser treinado, removendo logos, outras pessoas, espaços desperdiçados, etc.


Vou mostrar o dataset que consegui para o [LoRa Sílvio Santos](https://civitai.com/models/257578/silvio-santos-brazilian):

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/0e62c57b-2e16-43af-9a59-55e06a14e69c/width=525/0e62c57b-2e16-43af-9a59-55e06a14e69c.jpeg)

Você pode ver que são todos retratos. Ter fundos variados e cores de roupas diferentes é FUNDAMENTAL. Rostos olhando para diferentes lados também.

### Observação sobre a simetria

Verifique se os lados direito e esquerdo da pessoa são consistentes em todas as imagens. Isso pode não acontecer com selfies que geralmente são invertidas. 

O **rosto humano não é simétrico**, então se você tiver uma orientação mista dos lados durante o treinamento, o resultado pode ser este:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/56b7b20f-7962-403b-96ed-ce9b12464a5e/width=525/56b7b20f-7962-403b-96ed-ce9b12464a5e.jpeg)

Por favor, revise a **orientação** de todo o seu dataset!!

Se as imagens de pré-visualização começarem a ser todas iguais, essa pode ser a razão, pois o SD tentará aprender ambos os lados e a diferença na simetria pode causar um aumento da perda em algumas imagens invertidas, então o aprendizado ficará preso em algumas poucas imagens.

### Quantidade de imagens

-   `5 a 10`: Seu LoRa não terá muitas variações, mas pode funcionar.

-   `11 a 20`: Boa quantidade. Pode gerar um bom LoRA.

-   `21 a 50`: Essa é a faixa que queremos!

-   `50 a 100`: É muito, mas funciona se você quiser misturar retratos e corpo inteiro.

-   `>= 100`: Perda de tempo.


### Resolução

A resolução pode variar de 256px a 2048px. Evite imagens abaixo ou acima desses valores. Você não precisa redimensionar se estiverem dentro desses valores, pois o treinamento fará isso automaticamente em buckets:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c9446291-436e-4471-b317-c3374356021d/width=525/c9446291-436e-4471-b317-c3374356021d.jpeg)

### Estrutura da pasta:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/3fbab237-56ab-4251-9793-94ec6fa9a643/width=525/3fbab237-56ab-4251-9793-94ec6fa9a643.jpeg)

Criei uma pasta com o nome da string LoRA. Nomeei a string substituindo algumas letras por números (para garantir que será um token único em muitos modelos).

O `result_current` será o local onde Kohya salvará os resultados

As imagens de treinamento estarão no diretório contendo as imagens de treinamento. A convenção de nomes é: 400 / número de imagens, um sublinhado e a string lora. Esse será o número de repetições que o Kohya fará. Acho esse número bom para ter um intervalo adequado entre as épocas.

_`txt2img-images` é onde guardo imagens geradas usando o LoRA - Opcional._

---

## Legendagem

Este é o processo onde você descreverá o que cada imagem é, então o SD saberá como usar o modelo existente para construir as imagens de treinamento a partir do ruído.

No Kohya, na aba "Utilities", temos o "Blip Captioning". Eu uso isso com essas configurações:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/556a936d-0589-4e37-b6e3-ab831ea79615/width=525/556a936d-0589-4e37-b6e3-ab831ea79615.jpeg)

Eu altero só o seguinte:

-   Image folder to caption: É o caminho da pasta onde estão as imagens.

-   Prefix to add to BLIP caption: O prefixo é o `N0m3D0L0R4` seguido por uma vírgula.

-   Number of beams (Número de feixes): 9

-   Min length (Comprimento mínimo): 15

-   Batch size (Tamanho do lote): 4

-   Todos os outros valores eu mantenho o padrão.


Clique em "Caption images" e, depois de um tempo, as legendas serão geradas:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f6189457-457f-4bb1-854b-746e4c46924f/width=525/f6189457-457f-4bb1-854b-746e4c46924f.jpeg)

Se você tiver poucas imagens, pode corrigir as legendas, pois o Blip ADORA adicionar frases como "holding a remote control" ou "With a microphone in the hand" quando isso não é verdade. Eu apenas ignoro e tem funcionado dessa forma, pois a legenda completa ainda é boa.

---

## Pasta de Regularização

No passado, para os primeiros LoRas que publiquei, usei uma pasta de regularização com mais de 4K imagens de mulheres. **Parei de usar** pois só é necessário quando seu dataset não é legendado e é mais variado.

**Usar isso dobrará o tempo e os passos necessários.**

Então, não use!

---

## Executando o treinamento

Aqui vamos executar o treinamento. Para SD1.5 essas configurações requerem **8GB+** de Vram, SDXL requer **10GB+** de Vram.

**A instalação do Kohya está além do objetivo deste guia.**

### Configurações Kohya

Configurações SD1.5: [https://jsonformatter.org/a3213d](https://jsonformatter.org/a3213d)

Configurações SDXL: [https://jsonformatter.org/66e5c8](https://jsonformatter.org/66e5c8)

_\*Me digam caso os links expirem_

_\* Mudei o otimizador SDXL de `Adafactor` para `AdamW8bit` nos treinamentos mais recentes._

Basta carregar esses arquivos na aba **LORA** do Kohya -- _NÃO NA ABA DREAMBOOTH_ -- clicar em "configuration file" e carregá-lo.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/bc0ca2a0-d124-49a2-9666-3cebc44c6598/width=525/bc0ca2a0-d124-49a2-9666-3cebc44c6598.jpeg)

Configurações que VOCÊ DEVE alterar:

#### Aba Model

-   **Image folder (containing training images subfolders)**: Caminho da pasta `training_images` que criamos antes. NÃO é a pasta com o número, a pasta pai.

-   **Trained Model output name**: String do nome do LoRa.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/7d2cf44d-6911-4d29-a1e8-bba6d6ccf463/width=525/7d2cf44d-6911-4d29-a1e8-bba6d6ccf463.jpeg)

#### Aba Folders

-   **Output directory for trained model** - Caminho da pasta `result_current` criada, também anteriormente.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/173f0165-1e63-415a-9f95-9620ca186865/width=525/173f0165-1e63-415a-9f95-9620ca186865.jpeg)

#### Aba Parameters > Samples

-   Em `Sample prompts` coloque um prompt simples. Ele será usado para gerar as imagens de amostra.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/52f6c812-9dfe-499b-8a8a-551fe82e0356/width=525/52f6c812-9dfe-499b-8a8a-551fe82e0356.jpeg)

#### Aba Parameters > Basic

-   Você pode mudar o número de épocas, mas eu manteria 6 para começar.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/ed2383de-9951-4e1f-bbce-8a0f924f5bb4/width=400/ed2383de-9951-4e1f-bbce-8a0f924f5bb4.jpeg)

#### Aba Parameters > Advanced
Pesquise na internet sobre o que cada campo significa, está fora do escopo explicar todos agora. Um dia farei isso. A maioria das configurações altera os requisitos de hardware. Estas são as que funcionaram para mim, usando uma RTX2060 super com 12GB de Vram.

Por exemplo, minha RTX2060 não suporta bf16 como as 3060s, então eu uso fp16. Isso economizaria memória, mas é o que é e com 12G tem funcionado.

### Começando o treinamento

Depois de mudar tudo isso, clique em "**Start training**". Você verá isso no console:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/02a764f3-7018-4d36-b6f3-ae1fc3034d26/width=525/02a764f3-7018-4d36-b6f3-ae1fc3034d26.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/98c6bcae-c87b-4a9d-8cd6-5562f84a871f/width=525/98c6bcae-c87b-4a9d-8cd6-5562f84a871f.jpeg)

Com uma loooonga barra de progresso.

### Utilização de hardware

**SD1.5**: Eu não armazeno em cache os latentes no disco. É mais rápido, mas usa quase a mesma Vram que o SDXL.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/08ffed7f-c7d9-433a-96ff-845956482648/width=525/08ffed7f-c7d9-433a-96ff-845956482648.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/dea5891d-ac2d-46c3-93b6-4d9700de6fb5/width=525/dea5891d-ac2d-46c3-93b6-4d9700de6fb5.jpeg)

**SDXL**:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/49712702-10f0-421f-a9e1-f23a3fbda5be/width=525/49712702-10f0-421f-a9e1-f23a3fbda5be.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/09a5a288-b027-4db0-a25d-9d7d8acc9dfc/width=525/09a5a288-b027-4db0-a25d-9d7d8acc9dfc.jpeg)

### Erro de falta de memória

Se você receber erros de falta de memória CUDA, então você está no limite. Ative o cache latentes para o disco, mude de fp16 para bf16 se seu hardware suportar, reduza o tamanho do lote de 2 para 1.

Se você não conseguir resolver, pesquise na internet. Se ainda estiver recebendo erros, então desista e treine no CivitAI.

### Pré-visualização do treinamento

A cada 100 passos (você pode mudar isso) o treinamento criará uma imagem de amostra na pasta results_current/sample, então você pode ter uma ideia se está funcionando ou não.

Os resultados melhorarão com o tempo conforme ele aprende.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/41dc37a4-bbd4-4239-ad33-fc2622c012e8/width=525/41dc37a4-bbd4-4239-ad33-fc2622c012e8.jpeg)

---

## Resultados do treinamento

Quando terminar, o diretório ficará assim:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/606ecf53-b1a2-49de-9452-9f9638941b6a/width=525/606ecf53-b1a2-49de-9452-9f9638941b6a.jpeg)

Você pode verificar pelas imagens de amostra se está supertreinado ou subtreinado. Você verá isso ao rodar o LoRA também.

### Supertreinado

Se o modelo estiver supertreinado, as imagens ficarão "pixeladas como se fossem feitas de argila"... Não sei como descrever. As imagens de pré-visualização começarão a distorcer.

Veja com seus olhos, uma imagem gerada:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/01fffbc9-435a-4ed9-bd08-65198f4d64ce/width=525/01fffbc9-435a-4ed9-bd08-65198f4d64ce.jpeg)

Às vezes, não fica tão ruim assim, mas o rosto PARA de parecer com a pessoa treinada até que se deforme em épocas posteriores.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/9e9cd0ab-ed9e-4c81-8017-647b52474159/width=525/9e9cd0ab-ed9e-4c81-8017-647b52474159.jpeg)

**A solução é simples:** Basta testar épocas anteriores e ver a mais recente que funciona bem. Com base na imagem de amostra, você pode facilmente encontrar a boa e encontrar o arquivo LoRA gerado por volta do mesmo horário.

É DIFÍCIL ESCOLHER UMA!!! Mas deve ser feito. Teste o máximo que puder.

### Subtreinado

Se estiver subtreinado (O rosto do retrato não parece com a pessoa e parece uma mistura de uma pessoa genérica do modelo SD, ou o objeto não tem os detalhes desejados ainda) você pode **retomar o treinamento**.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/e6112e8b-735f-4a07-9f4d-81a078b7c7a2/width=525/e6112e8b-735f-4a07-9f4d-81a078b7c7a2.jpeg)

Esta imagem de exemplo mostra que detalhes estão faltando, como o microfone se fundindo com a gravata.

A diferença do supertreinamento é a falta de detalhes.

Se você fechou o Kohya, sem problemas, basta carregar o json que ele cria no diretório de resultados e todas as configurações usadas serão carregadas.

Em Parameters > Basic, você tem o campo **LoRA network weights** onde pode adicionar qualquer LoRa que deseja continuar treinando.

Renomeie o último que você obteve para qualquer outro nome, copie sua localização para este campo, mude as épocas para 2 ou 3 (depende de quanto você precisará continuar treinando) e clique em **start training** novamente. Ele retomará o treinamento.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/871bc53e-b0af-4b36-a2bc-2a6e900a42e2/width=525/871bc53e-b0af-4b36-a2bc-2a6e900a42e2.jpeg)

Você pode fazer isso até ficar bom!

### Finalizado!

Renomeie a época final do LoRa que você deseja (ou mantenha se o resultado final for bom) e use-o.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/a8158f9a-0cc7-4b8f-851f-88c8d641f3f9/width=525/a8158f9a-0cc7-4b8f-851f-88c8d641f3f9.jpeg)

---

## Treinamento no CivitAI

Eu não tentei SD.15 no CivitAI, apenas SDXL.

Muitos dos loras SDXL que tenho foram treinados no CivitAI. O treinamento do Sd1.5 deve ser semelhante. Basta manter as configurações alinhadas com as offline.

**Etapas:**

Selecione o personagem e adicione o nome do LoRa.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/4a69fe18-c00f-4cd6-91d2-80417c343d4c/width=525/4a69fe18-c00f-4cd6-91d2-80417c343d4c.jpeg)

Faça o upload de um arquivo zip contendo as imagens e as legendas. Você pode legendar manualmente no CivitAI, mas ter isso legendado previamente é melhor:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/7b123e4e-ab23-41c9-bc7e-7a6492440afb/width=525/7b123e4e-ab23-41c9-bc7e-7a6492440afb.jpeg)

Então mude as configurações. Elas são um pouco diferentes das offline, mas não muito.

Os Repeats eu uso a mesma fórmula: 400/número de imagens. ISSO É PARA SDXL:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/dfa2063d-03d1-4c54-b876-50cb638c552c/width=525/dfa2063d-03d1-4c54-b876-50cb638c552c.jpeg)

```
{ "unetLR": 0.0004,
 "clipSkip": 1,
 "loraType": "lora",
 "keepTokens": 0,
 "networkDim": 32,
 "numRepeats": 12,
 "resolution": 1024,
 "lrScheduler": "cosine",
 "minSnrGamma": 5,
 "targetSteps": 3072, --> This changes dynamically depending on other values
 "enableBucket": true,
 "networkAlpha": 16,
 "optimizerArgs": "weight_decay=0.1",
 "optimizerType": "AdamW8Bit",
 "textEncoderLR": 0.00004,
 "maxTrainEpochs": 16,
 "shuffleCaption": false,
 "trainBatchSize": 2,
 "flipAugmentation": false,
 "lrSchedulerNumCycles": 3 }
```

Você pode ter configurações melhores, mas essas funcionaram bem para mim.

**O treinamento custa 500 Buzz**

Você pode ver o status na página Model > training:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/247044f4-7432-4b6d-baa6-07bc475efcab/width=525/247044f4-7432-4b6d-baa6-07bc475efcab.jpeg)

Você recebe um e-mail quando termina.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/1ae91f53-91c0-406b-9aca-795115ba4f01/width=525/1ae91f53-91c0-406b-9aca-795115ba4f01.jpeg)

As imagens de amostra não são tão boas quanto offline, mas ajudam a ter uma ideia.

Se o modelo estiver **supertreinado**, a solução é simples: Basta testar épocas anteriores uma por uma até encontrar uma boa.

A parte ruim é que se o modelo estiver **subtreinado**, você não pode retomar no CivitAI. Eu geralmente baixo e continuo o treinamento fazendo mais 2 ou 3 épocas. Alguns requerem até mais. Depende da pessoa, do dataset, etc, etc, etc. Mas em algum ponto, conseguimos o resultado.

Você pode **publicar** diretamente da página de resultados do treinamento e até compartilhar o dataset. Eu não faço isso. Eu baixo, testo e depois publico após a validação.

---

## Script e automação

O Kohya tem uma opção para apenas imprimir o comando de treinamento. Eu usei isso para obter o comando, então posso carregar o venv do Kohya manualmente e executar o comando diretamente.

Então eu criei este script de shell Linux que faz tudo para mim:

-   Uma vez que eu tenha todas as imagens, ele renomeia, converte para png e cria a estrutura de pastas

-   Legenda todas

-   Executa o treinamento


![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/2501121e-1c6f-40c7-bdb2-f090f957edd9/width=525/2501121e-1c6f-40c7-bdb2-f090f957edd9.jpeg)

Como é muito específico para minha configuração, sistemas de arquivos e ainda requer algum trabalho, não compartilharei por enquanto. Mas um dia compartilharei.

# Comentários

> [**thiagojramos**](https://civitai.com/user/thiagojramos)
Muito obrigado! Espero que um dia eu consiga fazer alguns da minha 'lista':
https://i.imgur.com/1MlnwEK.png

---

> [**Malessar**](https://civitai.com/user/Malessar)
Como alguém que está realmente tentando e lutando para fazer uma primeira LoRa aceitável, eu realmente apreciaria alguma ajuda com as configurações reais, como otimizadores e o que tudo isso significa, para tentar conhecer os valores recomendados xD.

>> [**suvendroseal**](https://civitai.com/user/suvendroseal)
Mesmo aqui, cara. Mas para mim é ainda pior, esse mundo de geração de imagens por IA é tão novo para mim, estou confuso com todos os termos, opções, modelos, tags, TUDO. Eu gostaria que houvesse um vídeo no YouTube com horas de conteúdo sobre isso em um só lugar.

---

> [**luisa_pinguin**](https://civitai.com/user/luisa_pinguin)
maooooooooeeeeeeee, quem quer aviãozinho?

>> [**Skullkid - OP**](https://civitai.com/user/Skullkid)
mah vem pra cá, vem pra cá!!!

>>> [**luisa_pinguin**](https://civitai.com/user/luisa_pinguin)
sabia que eu treino minhas LoRa com apenas 3 imagens com 4dim 1 alpha? [ imagem do gato encarando ]

>>>> [**Skullkid - OP**](https://civitai.com/user/Skullkid)
Interessante, é possível, até porque não tem fórmula única... eu mesmo preciso explorar mais... mas como as pessoas são criativas e tentam o impossível com o LoRa, para disponibilizar ao público eu prefiro manter valores mais altos para o modelo ficar mais versátil.

>>>>> [**luisa_pinguin**](https://civitai.com/user/luisa_pinguin)
atualização, 1 imagem, 6 mb AHEUHAEUHAU, se quiser ver umas prints só colar no meu disc luisapinguin

>>>> [**ruka25**](https://civitai.com/user/ruka25)
Uso de hack é feio hein

>>>>> [**Skullkid - OP**](https://civitai.com/user/Skullkid)
É só o recomendado que li em vários artigos, usar network dim 128 para fotos realistas, porque abre mais espaço para separar os itens em tokens específicos (colares, brincos, olhos, óculos, etc), e 32 para SDXL.
Dá para reduzir para 64 no SD1.5 sem perder muito, mas menos que isso começa a ficar engessado.... é o que é, não é hack kkkkk

---

> [**Dlzeze**](https://civitai.com/user/Dlzeze)
Muito obrigado!

---

> [**thiagojramos**](https://civitai.com/user/thiagojramos)
Se puder, me ajude com uma dúvida... Não estou muito claro sobre o número de repetições. Para LoRas 1.5 (ainda não tentei xl), geralmente uso cerca de 30~40 imagens, e até recentemente, eu estava usando praticamente o mesmo número de imagens no número de repetições, por exemplo: "44_Lul4dr40". Mas eu estava lendo alguns posts no Reddit ontem, que sugeriam um máximo de cerca de 20 repetições e um número maior de epochs (na verdade, a maioria das minhas tentativas bem-sucedidas foram melhores com epochs acima de 10). Não sei se li errado, mas na imagem daquele artigo, você usou apenas 12 repetições? E sobre aquele "400 / número de imagens, um sublinhado e a string LoRa"?
Nota: Estou usando o mesmo .json 1.5 que você forneceu (só mudei os caminhos, clip skip, threads da CPU e outros pequenos ajustes).

>> [**Skullkid - OP**](https://civitai.com/user/Skullkid)
O que mais importa é o número total de etapas. Então o número de repetições vai impactar no número de epochs necessários.
>>
>> Para mim, esse cálculo funcionou porque quanto maior o dataset, mais etapas são necessárias. Com 15 imagens, o treinamento é concluído perto de 1200 etapas. Com 50 imagens, pode chegar até 4000 etapas.
>>
>> Então minha ideia com 400/número de imagens é ter o número de etapas por epoch próximo de 400. Então o número de epochs eu ajusto de acordo. Não há uma fórmula fixa, já que cada dataset pode exigir mais ou menos etapas no total.
>> 
>> Você pode reduzir as repetições e aumentar os epochs, ou o inverso. Você precisa encontrar o que funciona melhor para você. Eu sei que o número de repetições pode impactar os resultados se você usar cosseno com reinícios, por exemplo (eu não uso). Então, não há um único jeito certo, mas ter 400 etapas por epoch funciona para mim.

---

> [**ucas**](https://civitai.com/user/ucas)
Cara, pode treinar um LoRa para mim?

>> [**Skullkid - OP**](https://civitai.com/user/Skullkid)
Se for celebridade eu posso fazer e postar.

---

> [**Ulohen**](https://civitai.com/user/Ulohen)
Parabéns pela dedicação na criação deste tutorial irmão! Vindo de um usuário Brasileiro, isso me enche de orgulho! Muito completo!

>> [**Skullkid**](https://civitai.com/user/Skullkid)
Opa!! Valeu!!! Espero que tenha ajudado :)

---

> [**Aratea**](https://civitai.com/user/Aratea)
Quando eu abro a configuração do SDXL como faço para baixá-la e carregá-la na configuração? :c

>> [**Skullkid - OP**](https://civitai.com/user/Skullkid)
Basta copiar o conteúdo do lado direito, salvar como um arquivo de texto simples (notepad se estiver usando windows) e mudar a extensão de txt para .json. Depois, carregue o arquivo na aba Kohya LORA -- NÃO NA ABA DREAMBOOT -- clique em arquivo de configuração e carregue-o.

>>> [**Aratea**](https://civitai.com/user/Aratea)
Muito obrigada!!

---

> [**Crissaegrim**](https://civitai.com/user/Crissaegrim)
É possível treinar com imagens já feitas por IA? Ficam boas? Não tenho uma GPU boa... queria treinar um estilo de Anime, você pode treinar para mim ou cobra por algo?

>> [**Skullkid - OP**](https://civitai.com/user/Skullkid)
Opa, infelizmente não tenho experiência em treino de estilos, só personagens/pessoas. Mas se você não tem GPU, recomendo treinar aqui pelo CivitAI, não é caro.

>>> [**ucas**](https://civitai.com/user/ucas)
Alexis Roth andava

>>>> [**Skullkid - OP**](https://civitai.com/user/Skullkid)
@ucas posso tentar, não prometo prazo

>> [**ruka25**](https://civitai.com/user/ruka25)
É sim. Se as imagens não tiverem artefatos, deve ficar bom.

---

> [**yajukun**](https://civitai.com/user/yajukun)
Ótimo artigo/tutorial. Eu ajustei seu arquivo JSON do SDXL e consegui um LoRa SDXL decente pela primeira vez. Obrigado, irmão.

>> [**Skullkid - OP**](https://civitai.com/user/Skullkid)
Bom saber que foi útil!!!

---

> [**thrasherx**](https://civitai.com/user/thrasherx)
Obrigado pelo artigo muito detalhado, tem sido muito útil até agora. Tenho algumas perguntas que surgiram ontem à noite durante meu treinamento. Estou usando uma combinação de imagens de corpo inteiro e imagens de rosto. O corpo foi capturado bem, e depois eu executei uma pasta com apenas rosto para ajustar as imprecisões. Essa é a melhor metodologia a seguir?
>
> Em segundo lugar, estou tendo um problema com tudo o que estava no fundo das fotos de corpo inteiro aparecendo persistentemente nas minhas fotos criadas. Percebi que voltando alguns epochs isso diminuiu, então sei que devo ter super treinado o corpo. Qual é a melhor maneira de fazer isso para capturar apenas o corpo e evitar que o fundo apareça?

---

> [**kamentim**](https://civitai.com/user/kamentim)
Bom artigo, obrigado.

---

> [**thecrack101**](https://civitai.com/user/thecrack101)
Ótimo artigo. Obrigado!

---

> [**gabrielottj**](https://civitai.com/user/gabrielottj)
maaa oooooe!!! hahahaha muito obrigado por essas dicas! Eu tenho uma 4060 Ti de 8GB, você teria algumas orientações de configs pra funcionar bem o treinamento de LoRA SDXL só com 8GB? Obrigado!

---
