# [Treinamento de LoRa Pony Realism & Predefinição](https://civitai.com/articles/5545/pony-realism-lora-training-and-preset)


![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/84922823-a27a-41c9-b5c6-1e22070ad9b5/width=225/title.jpeg)


criado: 18-06-2024
tags: `training guide` `pony diffusion xl` `pony realism` `lora training`
autor: [ZyloO](https://civitai.com/user/ZyloO)

---
## 👋 <u>Introdução</u>

Este guia fornece uma visão geral do meu processo de treinamento para LoRas usando meu modelo, [Pony Realism](https://civitai.com/models/372465/pony-realism) para [Kohya_SS](https://github.com/bmaltais/kohya_ss).

Treinar um modelo LoRa envolve várias etapas, desde a preparação do seu conjunto de dados até o ajuste fino do modelo para um desempenho ideal. Embora este guia ofereça uma visão geral do meu processo, não entrará em detalhes sobre todas as configurações, pois estou compartilhando meu preset. Alguns valores precisarão ser ajustados com base no seu hardware e dados, como o número de **_epochs_** e **_batch size_**.

## 📦 Preparativos

Primeiro, você precisará de alguns itens indispensáveis:

1.  **Uma coleção de imagens para seu personagem/estilo**: A qualidade é mais importante que a quantidade. Eu geralmente seleciono de 14 a 50 imagens de alta qualidade.
    
2.  **Legendas**: Você pode gerar estas usando Kohya_SS _<u>Utilities -> Captioning</u>_. Eu uso principalmente **_WD14 Captioning_**, mas em algumas situações, **_BLIP_** pode ser melhor. Eu adiciono um prefixo único e executo as configurações padrão.
    

## 📝 Cálculo dos Passos

O cálculo que faço para os epochs/passo é:

```
número de imagens * passos = ~900
```

Então, por exemplo, no seguinte conjunto de dados:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c5097e5b-2fe5-455b-932e-e9fb2fa1d222/width=525/c5097e5b-2fe5-455b-932e-e9fb2fa1d222.jpeg)
23 imagens * 40 = 920.  
Esse valor **_40_** vai para a pasta como **_numbersubject_**:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/e0a37a47-8792-474f-b080-81f04fdfb3a8/width=525/e0a37a47-8792-474f-b080-81f04fdfb3a8.jpeg)
Por fim, para calcular os epochs eu faço:  

```
passos * epoch / batch size = ~2500
```

Isso me dá um epochs de 6:

```
920 * 6 / 2 = 2760
```

## 📜 Predefinições de Treinamento

As predefinições enviadas aqui é o que uso para tudo, personagem/estilo/conceito, devem estarem prontas para uso, você pode precisar mudar o batch size e os epochs com base no seu hardware, para referência, o meu é o seguinte:  

-   RTX 4090
    
-   i7 13700k
    
-   64 Gb Ram 6400 MHz
    

## 🖼 Resultados![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/5b7ab1e9-984c-47f7-ab12-2783115d976f/width=525/5b7ab1e9-984c-47f7-ab12-2783115d976f.jpeg)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/9715a730-5309-4463-96e7-b8f1fa91a8ae/width=525/9715a730-5309-4463-96e7-b8f1fa91a8ae.jpeg)

## Anexos

- [PonyRealismTrainingPreset.json](https://civitai.com/api/download/attachments/59878)
- [AnaArmas.zip](https://civitai.com/api/download/attachments/65149)
- [linda.zip](https://civitai.com/api/download/attachments/65150)

# Comentários
<p>
</p>


> [**fragility_abreast959691**](https://civitai.com/user/fragility_abreast959691)
Alguma sugestão de resolução ou proporção das imagens de treinamento?

>> [**ZyloO - OP**](https://civitai.com/user/ZyloO)
Não realmente, apenas verifique se é uma imagem de alta qualidade e pronto, há o bucketing habilitado em 1024x1024.

---

> [**jeremygin**](https://civitai.com/user/jeremygin)
Oi ZyloO, obrigado por compartilhar sua predefinição. Quando você legenda LoRAs para este modelo, você usa "female" ou "male" em vez de 1boy/1girl ou man/woman como na base Pony (baseado na sua recomendação nas informações do modelo)?

>>  [**ZyloO - OP**](https://civitai.com/user/ZyloO)
Na maioria das vezes, usando WD14 Captioning, eu uso 1girl/1boy. Quando uso BLIP, mudo para man/woman.

---

> [**ammor**](https://civitai.com/user/ammor)
> Ótimo artigo, pois você fornece números!
> 
> Diga-me, usando seu exemplo acima, se eu diminuir as repetições e aumentar as épocas, na mesma proporção, tipo 10 reps & 24 epochs, que tipo de influência eu teria no resultado final? O resultado seria muito diferente?
> 
> Obrigado!

>>  [**ZyloO - OP**](https://civitai.com/user/ZyloO)
Não acho, no final o que importa são os passos que você treina, você pode fazer mais épocas e menos repetições ou vice-versa. Uma coisa boa de fazer mais épocas do que repetições é que você pode salvar mais versões do lora de cada época.

---

> [**ic4rustheb1rd302**](https://civitai.com/user/ic4rustheb1rd302)
Quando você extrai lora/lycoris, qual é o rank/alpha configurado?

>>  [**ZyloO - OP**](https://civitai.com/user/ZyloO)
Se você quer dizer "Network Rank (Dimension)", então 32, alpha 0.

>>> [**ic4rustheb1rd302**](https://civitai.com/user/ic4rustheb1rd302)
Obrigado!

---

> [**countlippe**](https://civitai.com/user/countlippe)
Obrigado por compartilhar a predefinição.

---

> [**[deleted]**](https://civitai.com/user/[deleted])
Qual é a probabilidade de conseguir fazer um pequeno SDXL LoRa com uma GTX 1060 6GB antiga, mesmo que demore um dia inteiro?

>>  [**ZyloO - OP**](https://civitai.com/user/ZyloO)
As diretrizes do Kohya_ss dizem para usar uma GPU que tenha pelo menos 12GB de memória. Você pode tentar, mas não sei quanto tempo poderia levar. É melhor usar Runpod ou Collab.

---

> [**SadGolem**](https://civitai.com/user/SadGolem)
Obrigado pelo tutorial! Você precisa adicionar tags específicas como "score_9" nas imagens para o treinamento do pony lora?

>>  [**ZyloO - OP**](https://civitai.com/user/ZyloO)
Não, você só usa essas ao gerar imagens.

---

> [**hablaba**](https://civitai.com/user/hablaba)
Você usa um nome ou token único para seu treinamento de personagem Lora, tipo “ohwx” ou apenas aplica a 1girl/woman? É isso que você quer dizer com “prefixo único”?

>>  [**ZyloO - OP**](https://civitai.com/user/ZyloO)
Sim, eu adiciono um token único.

---

> [**otherworlderotic315**](https://civitai.com/user/otherworlderotic315)
Você já lidou com imagens de baixa resolução / upscaling? Estou portando 1.5 LORAs e só tenho imagens de baixa resolução. Planejei aumentar a resolução delas, mas se eu pudesse marcá-las de alguma forma para indicar que são de baixa qualidade e ainda assim ensinar a semelhança, isso pareceria funcionar intuitivamente.

>>  [**ZyloO - OP**](https://civitai.com/user/ZyloO)
Você pode tentar usar um upscaler, pessoalmente não tive muita sorte em obter bons resultados dessa forma, pois as imagens tendem a perder muitos detalhes.

---

> [**chrisss1**](https://civitai.com/user/chrisss1)
Oi, isso parece muito útil, obrigado. Existe alguma maneira de tornar o personagem menos uncanny valley? Não tentei o Pony Realism, mas usei o EverClear v2+VAE e os Loras de personagens reais que fiz tiveram resultados muito ruins. Criei um com 200 imagens de alta qualidade usando o treinamento lora do civitai, mas como disse, os resultados não se parecem com a pessoa. Sempre miro em mais de 1k passos. Estou fazendo algo errado?

>>  [**ZyloO - OP**](https://civitai.com/user/ZyloO)
Não tenho certeza, mas você pode precisar testar as saídas neste modelo. Tentei usar o site LoRa trainer duas vezes e ambas as vezes obtive resultados ruins. Portanto, prefiro fazer o treinamento local usando kohya_ss. Nos passos, miro em 2.500.

>>> [**chrisss1**](https://civitai.com/user/chrisss1)
Obrigado, vou tentar localmente e também o Pony Realism.

---

> [**karlobeat**](https://civitai.com/user/karlobeat)
Oi, primeiro de tudo, obrigado por todo esse trabalho!
Estou tentando treinar kohya ss com pony realism, usei seu arquivo json e 41 fotos originais do iPhone. Infelizmente, ao transferir o checkpoint lora para o arquivo lora dentro do stable diffusion, meus resultados foram bem ruins. Coloquei pony realism na seção do modelo do sd, VAE automático. Então todos os dpm++, euler a e DPM2a, mas nada parece funcionar. Os rostos do meu sujeito original estão errados. Tentei CFG 2, 5, 8 e ainda nada. O que você acha que estou fazendo de errado? (minhas fotos são da mesma pessoa, diferentes expressões, às vezes cortadas no rosto, às vezes no corpo inteiro em pé para que se concentre na pessoa e não nas pessoas ao redor). Obrigado se puder me ajudar!

>>  [**ZyloO - OP**](https://civitai.com/user/ZyloO)
Você legendou as imagens?
O único problema que consigo pensar é a qualidade das imagens. Notei que no treinamento para SDXL e Pony (PonyRealism) com conjuntos de dados de pessoas reais, a qualidade das imagens faz uma diferença significativa. Tente fazer suas imagens o mais alta qualidade possível, mesmo que isso signifique reduzir seu conjunto de dados pela metade. Além disso, em vez de reduzir o CFG, brinque com a força do lora.

>>> [**cosmoslayer26**](https://civitai.com/user/cosmoslayer26)
Não sou o OP, mas também tenho os mesmos problemas. Legendei as imagens e defini a quantidade apropriada de epochs/reps para obter 2.6k passos e a saída não se parece em nada com o sujeito no conjunto de dados.

>>>> [**arslan2012**](https://civitai.com/user/arslan2012)
Também tive os mesmos problemas, estava fazendo 150 imagens com 7 repetições, e acabei aumentando a taxa de aprendizado para 0.001 por 5 épocas antes que a imagem começasse a se parecer com o sujeito, e depois voltei a taxa de aprendizado para o padrão.

---

> [**RedDeltas**](https://civitai.com/user/RedDeltas)
Obrigado pelo artigo, é ótimo quando as pessoas compartilham seu processo. Você já tentou usar isso para treinamento Dreambooth?

>> [**ZyloO - OP**](https://civitai.com/user/ZyloO)
Não, ainda não tentei Dreambooth.

>>> [**RedDeltas**](https://civitai.com/user/RedDeltas)
Legal, eu tenho um conjunto de dados que já estou usando para treinamento LoRA, então posso tentar isso em ambos e ver como fica.

---

> [**RedDeltas**](https://civitai.com/user/RedDeltas)
Tentei treinar no Dreambooth em vez disso e os resultados foram incríveis, definitivamente vale a pena você tentar.

>> [**fr0g**](https://civitai.com/user/fr0g)
Você se importaria de compartilhar quais parâmetros você usou para dreambooth?

>>> [**RedDeltas**](https://civitai.com/user/RedDeltas)
Usei os parâmetros padrão do SECourses, que usam AdaFactor e suas imagens de reg. Vale a pena se inscrever no Patreon dele para obter todas as suas coisas - ele faz muitos testes que lhe custam dinheiro, então é bom retribuir - custa apenas $5.

---

> [**ainow**](https://civitai.com/user/ainow)
Para pony realism você sugere clip skip 2... mas a predefini

ção de treinamento LoRa usa clip skip 1... o clip skip não deveria estar definido para 2?

>> [**Clocksmith**](https://civitai.com/user/Clocksmith)
Eu verifiquei com uma configuração Everclear que me deu os melhores resultados e de fato tinha clip skip definido para 2, ao contrário do arquivo nesta página. Estou testando um lora com dados de entrada idênticos ao bom resultado do Everclear e mudando clip skip no arquivo desta página para 2. Vou relatar os resultados.

>> [**Clocksmith**](https://civitai.com/user/Clocksmith)
Sim. Confirmado. Treinei as mesmas imagens, legendas, passos e épocas com o arquivo como está e com clip skip 2. Literalmente, a única coisa que mudei. A semelhança com o arquivo atual não se parece nada com as imagens de treinamento e a semelhança com clip skip 2 está perfeita. Você precisa mudar seu arquivo para ter clip skip 2, ZyloO.

>>> [**ZyloO - OP**](https://civitai.com/user/ZyloO)
Até agora tive bons resultados usando clip skip 1, mas vou tentar usar 2, parece fazer mais sentido, obrigado.

---

> [**Clocksmith**](https://civitai.com/user/Clocksmith)
Você pode postar o exemplo de lora que tem as pastas para que possamos ter uma ideia dos resultados esperados para um lora baseado em foto?

>> [**ZyloO - OP**](https://civitai.com/user/ZyloO)
Claro, enviei os dois conjuntos de dados.

>>> [**Clocksmith**](https://civitai.com/user/Clocksmith)
Ótimo! Obrigado!

>>> [**Clocksmith**](https://civitai.com/user/Clocksmith)
Você também pode compartilhar o prompt que usou para criar sua imagem de amostra, especificamente com o lora `xanrms`, incluindo o peso do lora? Estou tentando testar o efeito do clip skip, mas não tenho certeza se estou fazendo o prompt da mesma forma que você.

>>>> [**ZyloO - OP**](https://civitai.com/user/ZyloO)
Positivo:
score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up, BREAK, RAW, xanrms, mid-shot, portrait, cozy bedroom, wearing dress, photo, 8k, very sharp, very detailed, realistic, real, hyper realistic, detailed eyes, natural lighting, god rays, cinematic view, <lora:ana_armas_pony_realism_v1:.85>
>>>>
>>>> Prompt negativo:
>>>>
>>>> score_1, score_2, score_3, text, 3d, deformed, deformed face, low quality, bad quality, worst quality, (drawn, furry, illustration, cartoon, anime, comic:1.5), 3d, cgi, extra fingers, (source_anime, source_cartoon, source_furyy, source_western, source_comic, source_pony), deformed teeth, plain background
>>>> 
>>>> Passos: 30, Amostrador: DPM++ 2S a Karras, Escala CFG: 6, Seed: 3281991511, Tamanho: 768x1280
>>>> 
>>>> Clip skip: 2, Modelo ADetailer: face_yolov8s.pt

---

> [**embis0126878**](https://civitai.com/user/embis0126878)
Sim, segui todos os passos exatamente como você disse e meus resultados foram terríveis. A semelhança que você mostra nos seus resultados é incrível, mas eu não consigo replicar.

>> [**mogmog**](https://civitai.com/user/mogmog)
Mesmo aqui. E eu tenho imagens de fonte incrivelmente detalhadas e de alta resolução, então sei que não é isso. Me pergunto se é porque minha fonte não é conhecida pelo modelo base, é uma pessoa personalizada.

---
