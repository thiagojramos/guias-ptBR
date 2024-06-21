# [[Insights para Intermediários] - Como criar as imagens que você quer com A1111](https://civitai.com/articles/983/insights-for-intermediates-how-to-craft-the-images-you-want-with-a1111)


data: 16-06-2024
tags: `generation guide` `intermediate` `tips and tricks` `generation` `list`
autor: [RestlessDiffusion](https://civitai.com/user/RestlessDiffusion)

---
## Introdução

### Para quem é este guia?

Este guia é para o usuário do Stable Diffusion que já domina o básico, mas às vezes tem dificuldades em transformar as imagens que vê na cabeça em pixels na tela. Este guia é para o usuário que não entende por que seu prompt bem pensado e detalhado não produz os resultados desejados.

Mais uma vez, este guia não se concentrará em inpanting, img2img, controlnet ou qualquer outra coisa legal que você pode fazer com o Stable Diffusion. Se você tem interesse nesses tópicos ou está procurando um guia especificamente para eles, recomendo muito [o guia do A13JM - Fazendo Imagens Incríveis Novamente!](https://civitai.com/articles/4990/making-images-great-again)

Sinta-se à vontade para me enviar uma DM com qualquer pergunta ou comentário, e eu responderei com prazer. Eu gosto de responder perguntas!

  
Agora estou acompanhando as atualizações no final da página, se você estiver interessado em ver o que mudou.  

### Suposições

Este guia assumirá que você tem algum grau de familiaridade básica com o Automatic1111 e a geração de imagens do Stable Diffusion. Se você é um iniciante absoluto, este guia não é para você. Recomendo que leia meu [guia anterior](https://civitai.com/articles/904/settings-recommendations-for-novices-a-guide-for-understanding-the-settings-of-txt2img), ou pelo menos dê uma olhada para garantir que você saiba tudo nele, pois este guia se baseia nesses conceitos.

Este guia também assume que você entende o básico sobre pesos de prompt. Por exemplo,

```
blue eyes = (blue eyes:1.0) 

(blue eyes) = (blue eyes:1.1)

((blue eyes)) = (blue eyes:1.21)

(((blue eyes))) = (blue eyes:1.331)
```

Este guia considera o estilo de múltiplos parênteses desatualizado e desencorajado, pois oferece menos controle do que usar pesos numéricos, e portanto não será usado.

### Aviso de Conteúdo

Este guia usa algumas imagens sugestivas e provocantes para manter os leitores engajados, no entanto, não há nada sexualmente explícito neste guia. Se fosse um filme, seria classificado como PG-13. Dito isto, embora este guia não caia exatamente no território NSFW, ele flerta com essa linha, então talvez não leia isso durante o intervalo de almoço no escritório.

### Tema Automatic1111 Lobe

Se o meu Stable Diffusion parecer diferente do seu, é provável que eu esteja usando uma extensão de skin chamada [sd-webui-lobe-theme](https://github.com/canisminor1990/sd-webui-lobe-theme#-installation), que pode ser encontrada como qualquer outra extensão na aba de extensões. Altamente recomendado.

### É, eu não vou ler isso tudo. Apenas me diga o que eu preciso saber.

Cada seção tem um parágrafo final de resumo que começa com **<u>Insight</u>**. Basta ler esse parágrafo para cada seção que te interessa.

### Modelos Usados

Este guia usará alguns modelos populares, mas ligeiramente menos conhecidos, que eu gosto de usar. Aqui estão eles como referência, em ordem de aparição:

[DarkSushi2.5 V3](https://civitai.com/models/48671)

[Colorful V3.1](https://civitai.com/models/7279)

[StingerMix V3.2](https://civitai.com/models/78843)

[3DAnimationDiffusion V1](https://civitai.com/models/118086)

[epiC2.5D V1](https://civitai.com/models/46745?modelVersionId=51342)

[RestlessExistence V2](https://civitai.com/models/111379?modelVersionId=134718) - Uma propaganda descarada do meu próprio modelo. Confira [RestlessExistence V3](https://civitai.com/models/111379?modelVersionId=193447)!

## Visualização em tempo real da geração de imagem

Antes de entrarmos em qualquer outra coisa, quero que você mude uma configuração no seu Automatic1111 Stable Diffusion Webui.

Vá para a seção "Live Previews" e mude o "Live preview display period" para 1, ou 2.

Então mude o "Progressbar and preview update period" para 50 milissegundos.

Se você tiver uma **<u>boa GPU</u>** como a RTX 3070, pode definir isso para um valor ainda menor como 10ms.

Se você tiver uma **<u>GPU decente</u>** como a RTX 2060 TI, tente definir o atraso para 300-500 ms e mude a exibição de visualização ao vivo para algo como 5.

Se você tiver uma **<u>GPU de baixo desempenho</u>** como uma RTX 1060, eu recomendaria não seguir este passo.

Aplique suas configurações. Você não deve precisar reiniciar a UI.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/5be598f0-bc3d-448c-8dcb-0595fe0d33c5/width=100/5be598f0-bc3d-448c-8dcb-0595fe0d33c5.jpeg)

Vamos voltar para o txt2img. Se você o tiver baixado, selecione darkSushi25D25D_v30 e cole este texto na caixa de prompt positivo:

```
(medium shot:1.4), 1girl, sultry, seductive, risque, long hair, braids, braided, hair up in ponytail, blonde hair, (blue eyes:0.75), (tavern wench:1.3), (cleavage:1.2), (bustier:1.1), (full skirt:1.1), (medieval tavern:1.3), flirty, smirk, tease, playful expression, smug, smirk, (looking at viewer:1.4), standing, hands on hips, hands on waist, leaning forward, (masterpiece:1.5), (high quality:1.4), subsurface scattering, heavy shadow, (intricate, high detail:1.2)
Negative prompt: (worst quality:2), (low quality:2), (normal quality:2), (b&amp;w:1.2), (black and white:1.2), (blurry:1.2), (out of frame:1.2), (ugly:1.2), (cross-eyed:1.2), (disfigured:1.2),(deformed:1.2), (extra limbs:1.2), (b&amp;w:1.2), (blurry:1.2), (duplicate:1.2), (morbid:1.2), (mutilated:1.2), (poorly drawn hands:1.2), (poorly drawn face:1.2), (mutation:1.2), (deformed:1.2), (bad anatomy:1.2), (bad proportions:1.2), (watermark:1.5), (text:1.5), (logo:1.5)
Steps: 30, Sampler: DPM++ 2M Karras, CFG scale: 7, Seed: 1701800230, Size: 512x768, Model hash: 54d8a62d61, Model: darkSushi25D25D_v30, Denoising strength: 0.4, Clip skip: 2, Hires upscale: 2, Hires steps: 15, Hires upscaler: 4x-UltraSharp, Version: v1.5.1
```

Agora clique na seta para baixo e esquerda abaixo de Generate (ignore meu botão Enqueue, é uma extensão extra).

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/11a9a056-eaa3-4c05-aacf-a0ac13630761/width=100/11a9a056-eaa3-4c05-aacf-a0ac13630761.jpeg)  
Isso aplicará todas essas configurações ao seu A1111 de uma vez. Se você receber um erro dizendo que não consegue encontrar 4x-UltraSharp, por favor, leia essa seção do meu [primeiro guia](https://civitai.com/articles/904/settings-recommendations-for-novices-a-guide-for-understanding-the-settings-of-txt2img#heading-28061).

**Estas serão, na maioria, as configurações padrão para o resto do guia também, se você quiser tentar seguir. Eu não vou colar o PNG para cada imagem, mas darei a Seed quando for relevante.**

**Clique em Generate e você deve ver o Stable Diffusion desenhando a imagem em quase tempo real.  
(Não posso ter GIFs neste artigo, então clique no link abaixo. Links para o Civitai, e abre em uma nova aba).**

[\>>> Clique para GIF <<<](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/e5a929d7-9e89-46ca-8ee4-fa297559e63f/transcode=true,width=700/Screencast%20from%202023-08-04%2018-54-27.webm)

Que legal é isso!? Agora podemos assistir como o Stable Diffusion realmente gera nossas imagens!

Algumas coisas a notar sobre o GIF. Primeiro, a _composição e os principais atributos_ da imagem são gerados pelo Sampler. Então, há um ponto claro no meio do GIF onde a imagem de repente fica nítida. Este é o momento em que o Upscaler assume. Como você pode ver no GIF, a maior parte dos detalhes do Upscaler acontece durante os primeiros 5-7 passos, então passos muito altos no Upscaler geralmente não valem tanto a pena, a menos que sua imagem tenha _muitos_ detalhes.

Aqui está a imagem resultante para referência:

[Full Res](https://civitai.com/images/1877512?postId=463178)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6dd3db70-2720-4443-b50b-5496a2db56d7/width=100/6dd3db70-2720-4443-b50b-5496a2db56d7.jpeg)  

**<u>Insight</u>**: Assista como o Stable Diffusion gera sua imagem. Preste atenção aos primeiros quadros de geração. Eles revelam como o sampler está tentando desenhar seu prompt inicialmente. Isso é extremamente útil porque se o SD mostrar algo inesperado, você pode adicioná-lo ao prompt negativo para ajudar a guiar ainda mais a geração.

Um exemplo disso pode ser, você prompt que está chovendo, mas sempre aparece um guarda-chuva, então você adiciona guarda-chuva ao negativo. Então, de repente, seu sujeito está às vezes usando um chapéu, mesmo que você não tenha promptado isso. Bem, se você colocar "guarda-chuva" no prompt negativo, e os primeiros quadros apresentarem um guarda-chuva, então o sampler tem que mesclar o guarda-chuva para fora da existência, então ele transforma isso em um chapéu. Às vezes, o sampler é bem-sucedido e simplesmente remove o guarda-chuva completamente. Outras vezes, você acaba com o chapéu.

## Samplers Revisitados

Vamos revisitar os samplers do meu guia para iniciantes. Existem três tipos principais de samplers:

1.  Ancestral
    
2.  Avaliativo
    
3.  Estranhos
    

Se o funcionamento dos samplers realmente te interessa, recomendo [este guia](https://stable-diffusion-art.com/samplers/). No entanto, sinto que a melhor maneira de aprender sobre samplers é simplesmente escolher um e gerar uma imagem com ele. Assista como eles geram imagens. Por exemplo, samplers ancestrais como Euler a e DPM++ 2S a Karras são do mesmo tipo, mas geram imagens de maneira diferente.  

**<u>Insight</u>**: Samplers ancestrais tendem a ser mais "criativos" estritamente em relação à sua amostragem de peso nas primeiras imagens. Samplers avaliativos são mais "lineares", ou seja, parecem saber como querem gerar uma imagem desde o início. Então, há os samplers estranhos que fazem coisas esquisitas que não fazem sentido como o UniPC. Nenhum desses samplers é melhor que os outros, então encontre seus favoritos. Eu, pessoalmente, prefiro os Samplers DPM++ porque acho que as imagens são mais "completas". O melhor conselho é simplesmente assistir como cada um gera imagens.

## Insights Críticos sobre o Stable Diffusion

Assistir ao Stable Diffusion gerar dezenas de milhares de imagens me deu dois insights críticos sobre como o Stable Diffusion funciona.

### Um modelo não pode desenhar algo que nunca viu

Isso é, espero, autoexplicativo e mais óbvio, então não vou gastar muito tempo aqui. Isso é tão crítico, no entanto, que vale a pena mencionar.

Um modelo não pode gerar uma imagem de algo que nunca viu antes.

Por exemplo, se você digitar um prompt que diz "mulher, jaqueta, chapéu", todos os modelos podem gerar uma imagem com esse prompt. No entanto, digite seu nome em um prompt e, por mais que você tente descrever sua semelhança com o modelo stable diffusion, ele não gerará você, porque o modelo não tem pesos de conjunto de dados de treinamento que possa correlacionar com seu nome.

**<u>Insight</u>**: É melhor usar uma linguagem de prompt tão precisa quanto possível para garantir que seu prompt tenha a melhor chance de entender o que você quer na sua imagem. Não diga "aquário" quando quiser dizer "tanque de peixes", a menos que você queira uma imagem de um peixe com um canhão M1 Abrams. Hmmm, pensando bem... isso pode ser uma imagem bem legal.

### Stable Diffusion tentará mesclar coisas que estão no mesmo espaço lógico

Este é mais difícil de entender, mas é mais importante. A melhor maneira de ilustrar isso é com um exemplo.

Para esta seção, usaremos o maravilhoso modelo: [Colorful_v3.1](https://civitai.com/models/7279).

Digamos que você quer uma cena como esta:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/a5f9d6b6-310e-4210-af28-753c36310d96/width=100/a5f9d6b6-310e-4210-af28-753c36310d96.jpeg)

Esta é uma mulher, em um vestido transparente, sentada em uma cadeira, em uma sala luxuosa, em algum tipo de habitat subaquático, com uma janela que está vendo o oceano do lado de fora.

Ok, então vamos escrever o prompt básico:

```
Prompt: a woman, sheer dress, sitting on a chair, luxurious room, underwater habitat, ocean floor, windows, (masterpiece:1.4), (high quality:1.4)

Negative: (low quality:2)
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/da598eca-fbc4-4019-a381-f3ec0305078f/width=100/da598eca-fbc4-4019-a381-f3ec0305078f.jpeg)

Espere, por que não há sala? Por que é apenas uma cadeira subaquática? Temos a mulher, subaquática, a cadeira e o vestido transparente certos. Então, e quanto à sala, ao habitat e às janelas? Por que isso não funciona? Vamos ajustar o prompt e tentar dizer ao modelo que queremos uma sala com janelas.

```
Prompt: a woman, sheer dress, sitting on a chair, (luxurious room:1.2), (underwater habitat:1.2), ocean floor, (windows:1.2), (masterpiece:1.4), (high quality:1.4)

Negative: (low quality:2)
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/57ea0c9b-aa3d-4aaa-b037-953fc334287d/width=100/57ea0c9b-aa3d-4aaa-b037-953fc334287d.jpeg)

Isso ainda não está funcionando. Claramente temos uma sala com janelas agora, mas a água está dentro da sala. Queremos a água do lado de fora. Vamos dizer ao modelo que a cena é um _habitat subaquático_. Vamos também dizer ao modelo que não queremos que o sujeito esteja molhado.

```
Prompt: a dry woman, sheer dress, sitting on a chair, (luxurious room:1.2), (underwater habitat:1.4), ocean floor, (windows:1.3), (masterpiece:1.4), (high quality:1.4)

Negative: (wet:1.3), (low quality:2)
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/e66849c9-c53f-4813-82bb-8267810fcf63/width=100/e66849c9-c53f-4813-82bb-8267810fcf63.jpeg)

Não chegaremos a lugar nenhum por esse caminho. A razão é simples. O problema-chave é o termo "underwater", pois está no "espaço lógico de nível superior". Underwater afeta tudo no prompt. Está no mesmo espaço lógico que tudo o que você poderia colocar no prompt, então o modelo tenta mesclar underwater em tudo.

Nota: Quando digo "espaço lógico de nível superior", esses são termos que eu inventei. Não são termos do Stable Diffusion.

**<u>Insight</u>**:

 Certifique-se de pensar nos termos dos seus prompts no nível lógico em que estão, tomando cuidado para não desalinhá-los. Vamos ver como fazer isso com sucesso na próxima seção.

## Desenhe o que você vê

Há um ditado bem conhecido dado a estudantes que estão estudando arte que diz algo como:

```
Desenhe o que você vê, não o que você acha que vê.
```

O mesmo é verdadeiro para modelos do Stable Diffusion. No exemplo acima, nós _achamos_ que estamos subaquáticos, e logicamente, isso é verdade. Mas o modelo não sabe nada sobre nossa lógica. Ele só conhece imagens.

Vamos então pensar sobre o que realmente vemos. Bem, vemos uma sala, com uma janela de vidro com água e peixes do outro lado.

Que palavras podemos usar para descrever uma janela de vidro com água e peixes do outro lado? Mais importante, as palavras têm que estar no mesmo escopo que os atributos da sala e do mobiliário.

Que tal um aquário?

Vamos tentar isso, lembrando que o modelo quer mesclar coisas que estão no mesmo espaço lógico. Então, neste caso, o aquário e as janelas da sala são provavelmente semelhantes o suficiente para que o modelo possa mesclá-los sem muitos problemas.

```
Prompt: a woman, sheer dress, sitting on a chair, luxurious room, aquarium windows, (masterpiece:1.4), (high quality:1.4)

Negative: (low quality:2)
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/58dcb44b-82af-488f-9f21-2560bd4537a4/width=100/58dcb44b-82af-488f-9f21-2560bd4537a4.jpeg)

Encaixa perfeitamente. Instantaneamente, estamos quase lá. O modelo parece entender exatamente o que queremos agora. Vamos refinar um pouco mais. Vamos adicionar "ocean" para dar mais profundidade à janela (entendeu, entendeu? profundidade haha)

```
Prompt: a woman, sheer dress, sitting on a chair, luxurious room, aquarium windows, ocean, (masterpiece:1.4), (high quality:1.4)

Negative: city, (low quality:2)
```

E literalmente após algumas tentativas de geração...

[Imagem Final (Full Res)](https://civitai.com/images/1881184?postId=463178)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/b0f8944f-4c2d-45da-b3ec-25e2408a2c1b/width=100/b0f8944f-4c2d-45da-b3ec-25e2408a2c1b.jpeg)

Agora isso está mais parecido. Poderíamos continuar refinando esse prompt para extrair mais qualidade, mas isso está fora do escopo deste guia. Por enquanto, só queremos garantir que estamos escolhendo nossas palavras com o máximo de cuidado possível ao construir prompts de boa qualidade.

**<u>Insight</u>**: Pense em como o modelo quer gerar imagens. Pense no que o modelo está tentando mesclar. Configure o modelo para mesclar atributos que estão na mesma "camada lógica". Faça isso pensando claramente sobre o que a imagem na sua cabeça realmente é _visualmente_.

## Compreendendo a posição e os pesos dos termos

Para esta seção, vamos usar o maravilhoso [StingerMix](https://civitai.com/models/78843).

Digamos que temos este prompt:

```
Prompt: (extreme close up:1.1), (ecu:1.4), (detailed face:1.4), (bokeh:1.3), (depth of field:1.3), 1girl, (succubus:1.4), (horns:1.3), (medieval dungeon:1.3), (stone walls:1.2), (dank and damp:1.2), (torchlight:1.1), (rusty bars:1.2), (chains:1.1), (narrow passages:1.2), (shadowy corners:1.2), (octane render:1.3), (masterpiece:1.3), (hires, high resolution:1.3), (:1.3), subsurface scattering, realistic, heavy shadow, ultra realistic, high resolution

Negative: (text:2)(medium shot:1.2), (low quality:2), (normal quality:2), (lowres, low resolution:2), BadDream, (UnrealisticDream:1.25)
```

... e ele produz esta imagem (Seed é 2888135920).

  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/30444e93-9d9e-4bec-af15-57a9615bb4e9/width=100/30444e93-9d9e-4bec-af15-57a9615bb4e9.jpeg)

Esta é uma imagem boa, mas digamos que queremos que os olhos dela sejam amarelos? Ok, então vamos adicionar "yellow eyes".

```
yellow eyes, (extreme close up:1.1), (ecu:1.4), (detailed face:1.4), (bokeh:1.3), (depth of field:1.3), 1girl, (succubus:1.4), ...
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/8300d6e9-4277-4b09-9715-23c97402a827/width=100/8300d6e9-4277-4b09-9715-23c97402a827.jpeg)

Bem, os olhos dela estão certamente amarelos agora, mas também está sua roupa, o que não é desejado. Vamos ajustar o peso. Vamos adicionar (yellow eyes:0.8).

```
(yellow eyes:0.8), (extreme close up:1.1), (ecu:1.4), (detailed face:1.4), (bokeh:1.3), (depth of field:1.3), 1girl, (succubus:1.4), ...
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/7c7ac1cf-7682-4338-bf7f-f135a77ced38/width=100/7c7ac1cf-7682-4338-bf7f-f135a77ced38.jpeg)

A imagem é ligeiramente diferente, mas a roupa dela ainda está amarela. Vamos reduzir o peso para 0.1.

```
(yellow eyes:0.1), (extreme close up:1.1), (ecu:1.4), (detailed face:1.4), (bokeh:1.3), (depth of field:1.3), 1girl, (succubus:1.4), ...
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/2a4f94ed-5581-4c03-a59f-26bcea22629d/width=100/2a4f94ed-5581-4c03-a59f-26bcea22629d.jpeg)

Espere, por que a roupa dela ainda está amarela? Por que o peso aparentemente não está fazendo nada?

Isso ocorre porque os itens podem ser dependentes da posição. Quando o Stable Diffusion "digiere" seu prompt, ele o faz em pedaços. Às vezes, certas coisas são deslocadas para fora dos pedaços para dar lugar a outros termos. No entanto, a primeira coisa em cada pedaço sempre será processada. É por isso que você pode definir o peso para 0.1 e ainda assim ter um efeito na imagem.

O parágrafo acima é uma simplificação, mas passa a ideia.

  
Vamos tentar mover o (yellow eyes:0.1) para logo após 1girl.

```
(extreme close up:1.1), (ecu:1.4), (detailed face:1.4), (bokeh:1.3), (depth of field:1.3), 1girl, (yellow eyes:0.1), (succubus:1.4), ...
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c86e832b-d79a-46f6-81ca-dfbb42dc08ae/width=100/c86e832b-d79a-46f6-81ca-dfbb42dc08ae.jpeg)

Se você olhar bem de perto, pode ver que os olhos dela têm um leve toque de amarelo. Isso não é o que queríamos. Acabamos de mover yellow eyes, então voltamos a aumentar o peso.

```
(extreme close up:1.1), (ecu:1.4), (detailed face:1.4), (bokeh:1.3), (depth of field:1.3), 1girl, (yellow eyes:0.5), (succubus:1.4), ...
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/495e8ed9-2bac-47aa-92fc-6f622183b71b/width=100/495e8ed9-2bac-47aa-92fc-6f622183b71b.jpeg)

Bem, as coisas estão indo na direção certa, vamos tentar o peso padrão de nenhum.

```
(extreme close up:1.1), (ecu:1.4), (detailed face:1.4), (bokeh:1.3), (depth of field:1.3), 1girl, yellow eyes, (succubus:1.4), ...
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6929f5ef-2189-4c06-a7a6-4720a40fabb3/width=100/6929f5ef-2189-4c06-a7a6-4720a40fabb3.jpeg)

Excelente, isso é exatamente o que queríamos! Agora que temos os olhos dela amarelos, vamos mudar a roupa dela para vermelha.

```
(extreme close up:1.1), (ecu:1.4), (detailed face:1.4), (bokeh:1.3), (depth of field:1.3), 1girl, yellow eyes, red outfit, (succubus:1.4), ...
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c987423d-2ab6-49ee-b220-90351756ed80/width=100/c987423d-2ab6-49ee-b220-90351756ed80.jpeg)

...e estamos de volta ao ponto de partida.

É tentador pensar "bem, vou apenas aumentar o peso da roupa vermelha então!", mas isso seria um erro. Se você seguir esse caminho, descobrirá que está ajustando pesos infinitamente tentando refinar seu prompt, sem perceber que a posição também importa.

Em vez de ajustar o peso da "roupa vermelha", vamos movê-lo para logo antes de "1girl".

```
(extreme close up:1.1), (ecu:1.4), (detailed face:1.4), (bokeh:1.3), (depth of field:1.3), red outfit, 1girl, yellow eyes, (succubus:1.4), ...
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/15ba8ade-38ae-4ca8-82ae-9821b5e628a1/width=100/15ba8ade-38ae-4ca8-82ae-9821b5e628a1.jpeg)

Excelente! [Full Res](https://civitai.com/images/1910629?postId=463178)

**<u>Insight</u>**: A posição de um termo no seu prompt carrega um "peso" significativo por si só. Esse peso posicional às vezes é suficiente para sobrepujar outros pesos no seu prompt. Se você estiver tendo dificuldades em fazer o Stable Diffusion respeitar seus pesos, tente mudar os termos de posição. Portanto, você deve...

## Focar primeiro na Composição

Concentre-se na Composição primeiro, depois adicione os detalhes. Isso se baseia na seção anterior e não terá imagens para acompanhar.

Coloque as coisas mais importantes no seu prompt primeiro. Então, uma vez que você tenha encontrado tudo o que é crítico para sua imagem, comece a adicionar detalhes de refinamento. Se você criar suas imagens dessa forma incremental, não terá problemas como algum termo aleatório que você não esperava ancorando toda a imagem, como o termo yellow eyes na seção anterior. Na seção anterior, era óbvio qual era o termo ofensivo, mas nem sempre será tão óbvio.

Sugestão: Desligue o hi-res fix quando estiver tentando refinar sua composição. O hi-res fix não "corrigirá" sua imagem, a menos que você esteja usando resoluções base realmente altas. Mantenha sua resolução em 512x768 e simplesmente gere imagens procurando a composição perfeita.

**<u>Insight</u>**: Construa seus prompts com a composição em mente primeiro. Uma vez que o modelo comece a te dar os traços largos do que você quer, então adicione detalhes ao seu prompt. Caso contrário, você estará tentando resolver conflitos entre detalhes e composição ao mesmo tempo.

## Aprenda alguns termos básicos de fotografia

Muitos modelos do stable diffusion estão cheios de dados fotográficos. Muitos dessas fotografias são rotuladas com termos formais de fotografia que descrevem a foto. Portanto, aprender uma base de termos fotográficos é uma ótima ideia para ganhar instantaneamente controle sobre seus prompts e imagens.

Para esta seção, vamos usar o excelente modelo [3dAnimationDiffusion](https://civitai.com/models/118086) de Lykon.

O prompt para todas essas imagens será:

```
Prompt: a woman, redhead, hazel eyes, <photography term>

Negative: FastNegativeV2, BadDream, UnrealisticDream
```

NOTA IMPORTANTE: Se você colocar algo no prompt, o modelo tentará gerar isso. Por exemplo, se você disser "close up face shot", e depois colocar "shoes", o modelo terá dificuldade e não saberá como mesclar esses termos, e provavelmente ignorará seu termo de close up. Da mesma forma, se você detalhar a roupa do sujeito, não se surpreenda quando o modelo se recusar a obedecer seus termos de fotografia e apenas fazer uma imagem completa, porque tem todos esses dados de prompt dizendo ao modelo que precisa desenhar essa roupa específica.

Como o modelo está tentando gerar todas as coisas no seu prompt, se seu termo fotográfico for o mais fácil de ignorar, bem, adivinhe o que acontece... ele é ignorado.

**Extreme close up** - Captura o rosto, antes de tudo.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/00142f98-9f80-4fb1-a3e9-7d8ffce319b0/width=100/00142f98-9f80-4fb1-a3e9-7d8ffce319b0.jpeg)

**Close Up** - O rosto ainda é o foco principal, mas não é tão close up.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/499b64d4-bbf5-4f07-bb68-430a84f02669/width=100/499b64d4-bbf5-4f07-bb68-430a84f02669.jpeg)

**Medium shot** - Normalmente da cintura ou peito para cima. Sua foto de retrato padrão.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/dc578fae-e360-46cb-b060-a82a5a374808/width=100/dc578fae-e360-46cb-b060-a82a5a374808.jpeg)

**Cowboy shot** \- Usado para mostrar cowboys em filmes, onde a pistola estava no quadril, mas o medium shot era muito close up para capturar a pistola. Normalmente dos joelhos para cima. Tem que ter um pouco de cuidado com este, pois se você colocar (cowboy shot:1.7) no seu prompt, você vai conseguir um vaqueiro de 1880 em um cavalo, dirigindo gado. Melhor colocar "cowboy" no prompt negativo. Também conhecido como "American shot" ou às vezes "3/4 shot". Cowboy shot é mais encontrado em tags Booru (modelos de anime) e, portanto, pode ser muito dependente do modelo.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6cc64d88-54c4-4e72-92d6-60f85feeb848/width=100/6cc64d88-54c4-4e72-92d6-60f85feeb848.jpeg)

**Full shot / Full Frame / Full Body** - Todos significam a mesma coisa. Você quer o corpo inteiro do sujeito.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/1691daa5-c25b-43dd-9534-11cf98c47bc0/width=100/1691daa5-c25b-43dd-9534-11cf98c47bc0.jpeg)

**Wide shot** - Usado com dimensões de paisagem (a largura é maior que a altura). Basicamente é um cowboy shot, mas com mais arredores.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f642c4c8-5891-48b4-bce4-2c04ec3892e5/width=100/f642c4c8-5891-48b4-bce4-2c04ec3892e5.jpeg)

**Dutch Angle / Dutch Tilt** - Este é o efeito "câmera inclinada". Pode ser usado com retrato ou paisagem, mas é

 mais pronunciado em paisagem.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/1287706d-573d-4b69-9d97-ea4b6a4e5cc2/width=100/1287706d-573d-4b69-9d97-ea4b6a4e5cc2.jpeg)

**High Angle Shot** - Esta é a vista de cima para baixo.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c9397b3f-e905-45f5-aeef-8f219c7e654d/width=100/c9397b3f-e905-45f5-aeef-8f219c7e654d.jpeg)

**Low Angle Shot** - Vou te dar um palpite sobre o que é. É o ângulo do chão, olhando para cima. Acho que o stable diffusion geralmente tem problemas com este shot. Como tal, não tenho uma boa imagem para isso. Então vamos pular.

**Bokeh** - Este é o desfoque estético que você às vezes vê, especialmente com fotografia noturna. O objetivo é desfocar tudo tanto que apenas as luzes são visíveis como círculos à distância.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/4cf898f7-83ac-4a15-8015-d98bd9ce1d0f/width=100/4cf898f7-83ac-4a15-8015-d98bd9ce1d0f.jpeg)

**Depth of Field** - Similar ao Bokeh, mas não é tão pronunciado. O fundo não está em foco, mas não é completamente invisível. Isso foi gerado com "cowboy shot".

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/2fab75d2-e026-49af-afb6-5f44f7b0f8c5/width=100/2fab75d2-e026-49af-afb6-5f44f7b0f8c5.jpeg)

**<u>Insight</u>**: Ter um conhecimento básico dos termos de fotografia pode instantaneamente te dar mais controle sobre suas imagens. No entanto, em minha experiência pessoal, o Stable Diffusion às vezes precisa ser persuadido a respeitar seus termos de fotografia nos seus prompts, então o peso pode ter que ser um pouco mais alto do que você espera. Se eu tivesse que adivinhar, é porque o termo fotográfico é o mais fácil para o modelo ignorar se houver muitas coisas conflitantes no prompt, mas essa é a minha especulação. Os shots abordados acima são apenas a ponta do iceberg. Se você quiser saber mais, sugiro este [post no blog do StudioBinder](https://www.studiobinder.com/blog/ultimate-guide-to-camera-shots/).

## Use Textual Inversions (Embeddings)

Textual Inversions (TI) são basicamente "super termos" que podem ser colocados nos seus prompts. A grande vantagem dos TI's é que eles podem ser facilmente deslocados, e _você pode aplicar pesos a eles._ Essa última parte é enorme e não pode ser subestimada, então vou repetir.

**<u>Você pode aplicar valores de peso às suas textual inversions.</u>**

Por exemplo, um dos TI's mais populares é o bad-hands-5. Ele geralmente é muito bom, mas às vezes simplesmente não entrega o que promete. Bem, você pode aumentar o peso dele como (bad-hands-5:1.5) e, de repente, você notará que as mãos nas suas imagens estão melhores.

Agora eu sei o que você está pensando. "Ok, então vou simplesmente aumentar os pesos desses TI's o dia todo e gerar as melhores imagens da minha vida". Não tão rápido. Aumentar os pesos dos TI's vem com um custo. Pode limitar a criatividade da imagem. Se você aumentar o FastNegativeV2 para (FastNegativeV2:2), você sentirá a IA começar a lutar, pois você a colocou em uma caixa muito pequena onde tudo tem que ser exato, e não é assim que o Stable Diffusion gera suas melhores imagens. Mas e se você ainda quisesse o FastNegativeV2, mas não quisesse que ele limitasse tão fortemente? Bem, você pode reduzir o peso dele, como (FastNegativeV2:0.8).

Vamos relacionar os TI's a guard rails de estrada. Guard rails são ótimos, mas não se eles te obrigarem a dirigir em linha reta. Você quer guard rails suficientes para não sair da estrada, mas ainda poder ir para onde quiser.

Aqui está uma lista de alguns excelentes TI's que você deve considerar usar:

[FastNegativeV2](https://civitai.com/models/71961/fast-negative-embedding-fastnegativev2) - Negativo para todos os tipos de conteúdo. Mestre de nenhum. Bom o suficiente para quase tudo.

[BadDream / UnrealisticDream](https://civitai.com/models/72437/baddream-unrealisticdream-negative-embeddings) - Aumentos instantâneos de qualidade. BadDream é para qualidade geral. UnrealisticDream é mais para foto-realismo. O aumento de qualidade é mais sutil do que o FastNegativeV2. Os detalhes são aumentados.

[fcPortrait Suite](https://civitai.com/models/81575/amazing-embeddings-fcnegative-fcportrait-suite) - A maneira mais fácil de conseguir retratos, sem dúvida. Esses TI's são criminalmente subestimados. Também inclui o fcNegative, que é outro ótimo negativo com um toque mais leve do que o FastNegativeV2 (isso é subjetivo).

[CyberRealistic Negative](https://civitai.com/models/77976/cyberrealistic-negative) - Um ótimo negativo voltado para o foto-realismo.

[bad-hands-5](https://civitai.com/models/116230?modelVersionId=125849) - Bastante autoexplicativo. Foca em fazer as mãos parecerem normais.

**<u>Insight</u>**: Textual Inversions, às vezes chamadas de Embeddings, são uma maneira muito fácil de aumentar a qualidade da imagem. Eles oferecem um amplo grau de controle, especialmente considerando que seus pesos podem ser ajustados.

## Use LoRAs

Se uma imagem vale mil palavras, então um LoRA vale mil prompts. Os LoRA's são tão poderosos que existem inúmeros artigos escritos sobre eles. Não vou cobrir esses. Em vez disso, vou me concentrar em alguns LoRA's que acho mais úteis.

Vamos usar o modelo [epic2.5D](https://civitai.com/models/46745/epic25d) do epicRealism e vamos escrever um prompt para uma imagem que podemos testar com os LoRAs.

```
Prompt: (cowboy shot:1.4), (3/4 shot:1.4), 1girl, skimpy, revealing, racy, (ancient priestess:1.3), (egyptian:1.2), (aztec:1.2), (mayan:1.2), (ornate headdress:1.1), (jewelry:1.2), (ceremonial robes:1.1), (gold:1.1), tattoos, (looking at viewer:1.4), standing, hands behind back, tropical rainforest, lush foliage, exotic plants, jungle, (octane render:1.3), (masterpiece:1.3), (hires, high resolution:1.3), subsurface scattering, realistic, heavy shadow, ultra realistic, high resolution

Negative: (cowboy:1.2), (medium shot:1.2), (nude, naked, nsfw, nipples, vagina, pussy, topless, bare breasts:1.5), (modern:1.5), (casual:1.4), (minimalist:1.3), (jeans:1.3), (t-shirt:1.3), (low quality:2), (normal quality:2), (lowres, low resolution:2), BadDream, UnrealisticDream, (bad-hands-5:1.25), (FastNegativeV2:0.75)

Seed: 3304805436
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/37e877ea-9695-44ce-b3fe-d54f097e468a/width=100/37e877ea-9695-44ce-b3fe-d54f097e468a.jpeg)

Vamos usar esta imagem como base.

[Detail Tweaker](https://civitai.com/models/58390?modelVersionId=62833) \- Um LoRA absurdamente bom. Se você não está usando isso, está gerando imagens no modo difícil. Adiciona ou remove pequenos detalhes das suas imagens. Se eu pudesse escolher apenas um LoRA, seria este.

Aqui está a imagem base com o detail LoRA com peso de 0.6.

```
<lora:more_details:0.6>
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c507d2e8-f4ac-430a-9be7-ff8c294a792e/width=100/c507d2e8-f4ac-430a-9be7-ff8c294a792e.jpeg)

Como você pode ver, ele adiciona mais detalhes, mas também mudou a cor da roupa dela. No entanto, não especificamos a cor da roupa no nosso prompt original, então não é culpa do LoRA.

[LowRA](https://civitai.com/models/221845/lowra-offset-noise) - O Stable Diffusion realmente gosta de imagens brilhantes. Às vezes você não quer imagens brilhantes. Nesses casos, você usa o LowRA. Aqui está a imagem base com LowRA definido para 0.7.

```
<lora:LowRA:0.7>
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/9323b301-5abc-4cbf-8475-f0e25d6160af/width=100/9323b301-5abc-4cbf-8475-f0e25d6160af.jpeg)

Se compararmos isso com a nossa primeira imagem, está claramente mais escuro.

Agora, aqui estão ambos combinados.

```
<lora:LowRA:0.7> <lora:more_details:1>
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/26314bda-3aa0-45c3-8e31-4a480f2372e8/width=100/26314bda-3aa0-45c3-8e31-4a480f2372e8.jpeg)

Como podemos ver, simplesmente aplicar LoRAs pode trazer propriedades úteis para nossas imagens, sem qualquer esforço

tentando descrever para o modelo o que você está procurando. Mas considere o que você digitaria nos seus prompts se quisesse alcançar esses efeitos? Você pode usar "darker" e "ultradetailed", etc., mas esses termos se tornarão complicados rapidamente e podem não ter o efeito desejado.

**<u>Insight</u>**: Se você não está usando LoRAs, deveria. Eles são frequentemente a única maneira de alcançar certos efeitos. São extremamente poderosos quando usados corretamente e podem instantaneamente fornecer as ferramentas necessárias para criar sua imagem.

## Use ADetailer

Você pode ter visto [ADetailer](https://github.com/Bing-su/adetailer) sendo mencionado aqui e ali, e se perguntado o que é. ADetailer é o "Restore faces" em esteroides. É uma coleção de IAs menores que realizam efeitos de pós-processamento nas suas imagens, após o Hires Fix. Por que isso é útil? Ele corrige rostos. Corrige olhos. Corrige mãos. Em outras palavras, corrige tudo o que o Stable Diffusion normalmente tem dificuldade.

Ele usa inpainting simples para fazer isso. Você pode até especificar prompts diferentes para cada correção.

Como exemplo, você pode dizer ao ADetailer para escanear o rosto, colorir os olhos de uma certa cor, e então você nem precisa colocar a cor dos olhos no seu prompt principal, o que reduz instantaneamente o conflito de termos.

Essas duas últimas seções usarão meu próprio modelo, [RestlessExistence](https://civitai.com/models/111379?modelVersionId=134718). Sim, eu sei que é uma propaganda descarada, mas esperei até o final, então por favor não me odeie.

De qualquer forma, vamos gerar a imagem base, como antes, usando todas as técnicas que discutimos.

```
Prompt: (cowboy shot:1.4), (3/4 shot:1.4), 1girl, brunette hair, (shoulder length hair:1.2), (short black cocktail dress:1.3), simple pendant necklace, strapless, bare shoulders, makeup, mascara, rosy cheeks, smokey eyes, lipstick, (beautiful, gorgeous:1.1), content, relaxed, soft smile, (looking at viewer:1.4), standing, luxurious night club, dim mood lighting, accent lighting, high end, upscale, posh, fancy ornate bar, sultry, seductive, risque, leaning forward, colorful, (octane render:1.3), (masterpiece:1.3), (hires, high resolution:1.3), subsurface scattering, realistic, heavy shadow, ultra realistic, high resolution, (fcHeatPortrait:0.8), <lora:more_details:0.5>

Negative: (ugly, ugly face, average face, imperfect complexion:1.3), nude, naked, nsfw, nipples, vagina, pussy, topless, bare breasts, (low quality:2), (normal quality:2), (lowres, low resolution:2), (FastNegativeV2:0.75), BadDream, (UnrealisticDream:1.25), (bad-hands-5:1.25), (cowboy:1.2), (medium shot:1.2)

Seed: 612507832
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f1c8873f-cb21-4802-a6c0-0e313de35caa/width=100/f1c8873f-cb21-4802-a6c0-0e313de35caa.jpeg)

Eu realmente gosto do meu modelo, rs...

Depois de instalar o ADetailer, você verá isso como um painel extra nas suas configurações. A seta amarela aponta para o modelo ADetailer que queremos usar para escanear, neste caso, face_yolov8n.pt. Isso diz ao ADetailer para escanear rostos e aplicar o Prompt Positivo (seta verde) e o Prompt Negativo (seta vermelha), via inpainting.

Vamos mudar os olhos dela para azul usando o ADetailer.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/e3ba3b00-63d3-4487-aac7-406dbebe758e/width=100/e3ba3b00-63d3-4487-aac7-406dbebe758e.jpeg)

Aqui está o resultado, após gerar a imagem com o ADetailer, com estes prompts:

```
Prompt: feminine face, blue eyes

Negative: BadDream, UnrealisticDream
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/4ed010b4-f61a-41f1-b83c-304c2e8c8e59/width=100/4ed010b4-f61a-41f1-b83c-304c2e8c8e59.jpeg)

Lembre-se, não há menção de olhos azuis em nenhum lugar no nosso prompt original, e a imagem original não tinha olhos azuis. Esses olhos azuis vieram puramente do ADetailer. Mas também olhe para os dois rostos. O ADetailer também retocou o rosto na segunda imagem, pois agora parece um pouco mais suave.

Agora, na verdade, acho que os olhos estão um pouco AZUIS DEMAIS, então vamos tentar torná-los mais realistas. Vamos ajustar o peso para torná-los um pouco menos azuis.

```
Prompt: feminine face, (blue eyes:0.5)

Negative: BadDream, UnrealisticDream
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/5309bfc6-0fc8-4fb5-9fda-fd10e45857a9/width=100/5309bfc6-0fc8-4fb5-9fda-fd10e45857a9.jpeg)

Muito melhor, mas eu gostei mais do rosto original, para ser honesto. Vamos mudar o modelo de face_yolov8n para mediapipe_face_mesh_eyes_only. Este modelo ADetailer só vai escanear os olhos e retocá-los, deixando os detalhes do rosto intactos. Vamos tentar. Mas como este não é mais o modelo de rosto, precisamos editar nosso prompt.

```
Prompt: blue eyes

Negative: BadDream, UnrealisticDream
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f48fd173-4706-4db6-9ba6-dfb806758658/width=100/f48fd173-4706-4db6-9ba6-dfb806758658.jpeg)

Olhe isso, olhos azuis perfeitos, e foi tão fácil mudar porque só mexi nas configurações do ADetailer.

Se você olhar para a primeira imagem do ADetailer que postei, notará que há um "2nd", então podemos rodar outro modelo ADetailer.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f04c5660-6aa1-4397-8b64-5ae6af44c921/width=100/f04c5660-6aa1-4397-8b64-5ae6af44c921.jpeg)

Assim como com o rosto e os olhos, vamos especificar um prompt positivo e negativo, mas desta vez vamos selecionar o modelo hand_yolov8n.pt. Vamos também aumentar o limiar de confiança do modelo de detecção, porque este modelo avaliará muitas coisas como mãos. Além disso, observe o (bad-hands-5:1.5). Como estamos fazendo inpainting apenas nas mãos, podemos realmente aumentar a potência desse TI.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/2089971b-fe93-4613-9b5e-0e67ba3df109/width=100/2089971b-fe93-4613-9b5e-0e67ba3df109.jpeg)

Agora vamos gerar a imagem novamente.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/7aec3d81-0a79-4103-ba6f-e21e072eef17/width=100/7aec3d81-0a79-4103-ba6f-e21e072eef17.jpeg)

É difícil dizer com essas imagens comprimidas o quanto o ADetailer é melhor. Então aqui estão as duas versões finais, e suas resoluções completas. Sugiro abrir ambas em novas abas e alternar entre as duas imagens, observando os olhos e as mãos.

[Versão Base (Full Res)](https://civitai.com/images/1921521?postId=463178)

[ADetailer (Full Res)](https://civitai.com/images/1921533?postId=463178)

**<u>Insight</u>**: Se você alguma vez ver uma imagem com mãos e olhos perfeitos, provavelmente foi feita com ADetailer. Facilita muito a vida em relação ao refinamento das coisas que o stable diffusion tem dificuldade (rostos, olhos, mãos).

Agora, tenho uma confissão. Criei o RestlessExistence apenas por sua capacidade de gerar ótimos trajes de Coelhinha da Playboy (isso não é verdade, mas ele realmente gera uma ótima garota coelha). Por exemplo:

```
Prompt: (cowboy shot:1.4), (3/4 shot:1.4), 1girl, long hair, ponytail, blonde hair, (black one piece:1.2), (satin leotard costume:1.2), (traditional playboy bunny suit:1.3), (seamless full black pantyhose:1.6), (bunny ears:1.3), (detached collar:1.2), (bow tie:1.2), white cuffs, (french satin wrist cuffs:1.4), (bare shoulders:1.3), (strapless:1.3), (bunny tail:1.6), (high heels with ankle strap:1.1), makeup, mascara, rosy cheeks, smokey eyes, lipstick, (beautiful, gorgeous:1.1), flirty, smirk, tease, playful expression, (looking at viewer:1.4), sitting, leaning back, reclined, (legs crossed:1.3), hands behind head, detailed background, balcony, modern city skyline, skyscrapers, glass buildings, bustling city, (night, midnight, night sky:1.25), cinematic composition, colorful, (octane render:1.3), (masterpiece:1.3), (hires, high resolution:1.3), subsurface scattering, realistic, heavy shadow, ultra realistic, high resolution, (fcDetailPortrait:0.75), <lora:more_details:0.5>

Negative: (cowboy:1.2), (medium shot:1.2), (seams:1.5), (garters:1.4), (thigh high stockings:1.7), (thighhighs:1.4), (thighband:1.4), (stay-ups:1.4), (hold-ups:1.4), (stockings with thigh bands:1.4), (intricate leotard:1.1), (latex:1.3), (opera gloves:1.5), (detached sleeves:1.5), (bardot:1.4), (gloves:1.4), lingerie, zipper front, (ugly, ugly face, average face, imperfect complexion:1.3), (pussy:1.4), (simple background:1.4), (low quality:2), (normal quality:2), (lowres, low resolution:2), (FastNegativeV2:0.75), BadDream, (UnrealisticDream:1.25), (bad-hands-5:1.25)

Seed: 64572525
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/14488653-6674-45f8-8f1e-0fec94d08a81/width=100/14488653-6674-45f8-8f1e-0fec94d08a81.jpeg)

Está quase perfeita, com exceção de que os punhos e a gola são pretos, e não brancos. Então, vamos usar a função Extra Seed no A1111 e ver se conseguimos alguns punhos brancos e uma gola branca, mantendo a composição (e o prompt) do jeito que está.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/30d6bdf4-107f-4251-a12b-2e4932545fb8/width=100/30d6bdf4-107f-4251-a12b-2e4932545fb8.jpeg)

Agora, o que isso fará é gerar pequenas mudanças sobre a sua seed original. Basicamente, é um denoiser que fica sobre a imagem original, e você determina quão potente ele é, e controla qual seed o denoiser usa. O que estamos fazendo aqui é dizer à função extra seed para gerar uma mudança de 5% sobre a imagem original. Esperançosamente, isso será suficiente para obter alguns punhos brancos, se apenas deixarmos rodar.

**Sempre que faço isso, desligo o hires fix e o ADetailer, porque estou procurando que a seed me dê algo específico. Se eu conseguir os punhos brancos, posso reutilizar o extra seed, e então ligar o hires fix e o ADetailer.**

Mas primeiro, vamos deixar funcionar.

...e depois de 23 tentativas, consegui uma com a gola branca. Lembre-se de que esta imagem não tem hires fix ou ADetailer, então a qualidade é inferior, no entanto, você pode ver que ainda é muito similar à imagem base.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/d9b23e00-dd74-47d1-9a70-b557d8aa5260/width=100/d9b23e00-dd74-47d1-9a70-b557d8aa5260.jpeg)

Vamos reutilizar a seed de variação (1039736832) que me deu esta gola branca, e reduzir a força para algo como 0.02, para manter o máximo possível da imagem base. Também vou ligar o hires fix e o ADetailer.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/e97d36cc-4ad9-4e8f-8741-ab90ba951714/width=100/e97d36cc-4ad9-4e8f-8741-ab90ba951714.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/5ef1f68d-a4ec-4945-9bb8-c0f965d968ac/width=100/5ef1f68d-a4ec-4945-9bb8-c0f965d968ac.jpeg)

Está certamente muito boa, mas os olhos e as mãos não estão perfeitos. Vamos brincar um pouco com a força da variação e ver se conseguimos mãos e olhos melhores.

Vamos tentar uma força de variação de 0.04.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/1c4f109b-1ad8-4fcb-8159-395ab2694656/width=100/1c4f109b-1ad8-4fcb-8159-395ab2694656.jpeg)

[(Full Res)](https://civitai.com/images/1922518?postId=463178)

Vou ficar com esta imagem. Se eu quiser retocar ainda mais esta imagem, terei que recorrer a mais refinamento do prompt, rodar mais seeds, straight up inpainting, ou outros métodos mais avançados.

**<u>Insight</u>:** Usar Seed + Extra Seed pode às vezes ajudar a eliminar arestas ásperas nas imagens que você gera. Se você gerar uma imagem que está quase perfeita, tente a função extra seed e veja se consegue fazer o modelo gerar uma imagem que seja levemente diferente o suficiente, para que retenha seu brilho original, mas remova a aresta indesejada.

### Concluindo

Quero agradecer, se você leu tudo isso. Trabalhei duro neste guia. Isso é o que eu gostaria que existisse quando comecei minha jornada no stable diffusion. Essas são as coisas que tive que aprender do jeito difícil, através de amargas tentativas e erros, então espero que possam te ajudar. Estou aberto a qualquer crítica construtiva que você tenha, pois quero que este guia seja o melhor possível. Sério, se algo não fizer sentido, por favor, me avise, e farei o meu melhor para corrigir.

**Agora vá em frente e crie conteúdo adulto de qualidade como os Deuses do Stable Diffusion pretendiam.**

----

<u>Atualizações:</u>

2024-MAY-23: URL atualizado para Lowra.

2024-JUNE-16: Atualizado Making Images Great Again

# Comentários
<p>
</p>

> [**maxhitman**](https://civitai.com/user/maxhitman)
Ótimo tutorial para todos aprenderem novas dicas e truques.
Parabéns. Trabalho bem feito. Merece uma avaliação de estrela.
Eu também tenho feito muitos testes no SD-1.5 desde janeiro e descobri muitas coisas que as pessoas ainda não sabem. Especialmente para fotografia foto-realista, que é uma maneira totalmente diferente de escrever um prompt do que o seu estilo.
.
Obrigado e divirta-se com suas futuras obras de arte
Abraços
MAX

---

> [**gentlemanjackdaniels7**](https://civitai.com/user/gentlemanjackdaniels7)
Onde posso encontrar um tutorial tão detalhado para imagem para imagem? Tenho uma imagem e estou mascarando-as para gerar novas roupas ou adicionar ou remover algumas para deixá-las bonitas ou para mostrar como ficariam com essas roupas. Estou tendo dificuldades para obter resultados com tantos tipos diferentes de prompts que tentei e Loras que baixei. Sou muito novo no SD e qualquer ajuda seria apreciada.

---

> [**monkeyman007**](https://civitai.com/user/monkeyman007)
[**@maxhitman**](https://civitai.com/user/maxhitman) Por favor, compartilhe suas dicas :)

---

> [**maxhitman**](https://civitai.com/user/maxhitman)
Abraços
MAX

---

> [**arnorwing**](https://civitai.com/user/arnorwing)
Bom tutorial. Eu uso o Adetailer o tempo todo e nunca pensei em colocar badhands:1.5 lá. Funciona bem. Haha

---

> [**monkeyman007**](https://civitai.com/user/monkeyman007)
[**@maxhitman**](https://civitai.com/user/maxhitman) Muito obrigado pelo prompt incrível e pelas informações. Esse link com imagens de artistas criadas com SD é muito útil. Se você tiver mais links úteis como esse, compartilhe. Obrigado novamente :)

---

> [**monkeyman007**](https://civitai.com/user/monkeyman007)
[**@maxhitman**](https://civitai.com/user/maxhitman) A razão para colocar parênteses assim ser ineficaz é porque os parênteses só dizem à IA para focar mais nisso e menos nas outras palavras. Se você colocar a mesma quantidade de foco em tudo com a mesma quantidade de parênteses, isso se cancela e não faz muito.

---

> [**maxhitman**](https://civitai.com/user/maxhitman)
[**@monkeyman007**](https://civitai.com/user/monkeyman007) Sim, isso é correto. É isso que ele faz, eu sei.
...
Vou deletar as coisas que postei até meia-noite de hoje.
Este é um post do RestlessDiffusion e eu não quero "spammar" aqui.
...
Abraços
MAX

---

> [**monkeyman007**](https://civitai.com/user/monkeyman007)
[**@maxhitman**](https://civitai.com/user/maxhitman) Tudo bem. Eu já copiei. Obrigado

---

> [**ragegiraffe808**](https://civitai.com/user/ragegiraffe808)
Aprendi algo. 👍

---

> [**Twoone**](https://civitai.com/user/Twoone)
ÓTIMO！！！！！

---

> [**Beef_Chan**](https://civitai.com/user/Beef_Chan)
Incrivelmente perspicaz. Ótimo guia! Obrigado por isso :)

---

> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
[**@Beef_Chan**](https://civitai.com/user/Beef_Chan) Estou feliz que você e todos os outros gostaram!

---

> [**HDelarouxAI**](https://civitai.com/user/HDelarouxAI)
Este é um excelente guia, obrigado por escrevê-lo!

---

> [**ThaiSalon**](https://civitai.com/user/ThaiSalon)
Você é incrível! Isso é ironicamente no nível de um curso direto que alguém faria em uma semana

---

> [**MetaSamsara**](https://civitai.com/user/MetaSamsara)
Na minha GTX1060, o ADetailer transforma um render de 10 minutos em um render de 3 horas, então eu simplesmente não uso. Não vejo qual é a grande coisa com ele. Parece que é bom apenas para inpainting de detalhes faltantes e detalhamento que as loras já fazem.

---

> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
[**@MetaSamsara**](https://civitai.com/user/MetaSamsara) Uau, realmente demora tanto assim na 1060? Isso é brutal. Como você corretamente afirmou, o ADetailer usa inpainting, o que significa que tem todo o poder do inpainting. Isso significa que pode corrigir problemas (até certo ponto), como corrigir a forma da íris para torná-las mais nítidas e circulares. Também pode girar as mãos um pouco, o suficiente para corrigir problemas de dedos, embora isso seja mais difícil.
No entanto, eu não acho que você obteria nem perto de 2 horas de valor do ADetailer.
Para referência, se você assistir ao gif na seção de Geração de Imagens em Tempo Real, você verá que leva cerca de 20 segundos para fazer uma imagem. O ADetailer faz mais algumas passagens e essas levam talvez 7 segundos cada. A imagem inteira fica pronta em cerca de 40 segundos, então eu acho de grande valor.

---

> [**MetaSamsara**](https://civitai.com/user/MetaSamsara)
[**@RestlessDiffusion**](https://civitai.com/user/RestlessDiffusion) Sim, eu posso entender. Me pergunto qual é o gargalo de desempenho. Eu rodo meu SD1.5 no automatic1111 com controlnet1.1 e tenho 6Gb de VRAM. --medvram e --xformers nas opções de inicialização. Entendo que está longe de ser ideal, mas acho que muitos tutoriais são apenas "experimente e você descobrirá" quando para eles é um processo de 1 hora para descobrir tudo, mas para mim é um experimento científico de uma semana inteira que pode falhar antes de eu desistir e seguir em frente ^^ #truestory

---

> [**arnorwing**](https://civitai.com/user/arnorwing)
[**@MetaSamsara**](https://civitai.com/user/MetaSamsara) você está usando a extensão tiled VAE? É imprescindível.

---

> [**MetaSamsara**](https://civitai.com/user/MetaSamsara)
Vou tentar, obrigado

---

> [**razorstfx**](https://civitai.com/user/razorstfx)
Provavelmente a leitura mais esclarecedora que encontrei hoje. Muito obrigado.

---

> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
[**@razorstfx**](https://civitai.com/user/razorstfx) Estou feliz que você gostou!

---

> [**maxhitman**](https://civitai.com/user/maxhitman)
[**@MetaSamsara**](https://civitai.com/user/MetaSamsara) Eu costumava ter uma dessas há muito tempo. Recentemente instalei uma nova NVidia 3060 que vem com mais memória RAM e me custou 500 euros (o que também incluiu um novo e muito mais potente ventilador) - faz 6 imagens de IA em menos de um minuto e você pode aumentar a resolução de qualquer coisa em apenas 2 minutos. É um gadget muito poderoso. Atualize sua máquina.

---

> [**MetaSamsara**](https://civitai.com/user/MetaSamsara)
[**@maxhitman**](https://civitai.com/user/maxhitman) Não posso pagar por um upgrade, rsrs. Mesmo as GPUs da próxima geração da AMD custam 500 euros na base e isso sem um PC para hospedá-las, rsrs.

---

> [**ptk314**](https://civitai.com/user/ptk314)
[**@RestlessDiffusion**](https://civitai.com/user/RestlessDiffusion) Obrigado pelo ótimo tutorial! Foi realmente esclarecedor. E parcialmente respondeu às perguntas que fiz alguns dias atrás no seu tutorial da parte 1 sobre variações de seed. Notei algumas diferenças nas imagens geradas usando os prompts no texto e as geradas a partir do PNG Info nas imagens de exemplo. Você poderia dar uma olhada. Isso é mais notável na seção do aquário. No geral, foi excelente e ser forçado a tentar outras coisas para acompanhar foi um pouco lento, mas mesmo assim bastante formativo!
Sobre o desempenho, qual configuração você está usando para obter resultados tão rápidos?

---

> [**ptk314**](https://civitai.com/user/ptk314)
[**@maxhitman**](https://civitai.com/user/maxhitman) É uma pena que eu tenha descoberto este guia tarde demais para ver o link que você postou. Alguma chance de ter acesso às informações novamente?
Você parece ter experiência com melhorias de desempenho e me pergunto se poderia me dar alguns conselhos.
Pelo que li aqui em seus posts e nas respostas do [**@RestlessDiffusion**](https://civitai.com/user/RestlessDiffusion), percebi que o desempenho que obtenho do meu sistema é abaixo do esperado, para dizer o mínimo. Tenho o que eu pensava ser um sistema bastante decente: CPU i9, 32GB de RAM, RTX3070 (laptop), 8GB de VRAM. Inicio o A1111 com --xformers --medvram e para gerar as imagens na última seção do tutorial (o coelhinho) leva cerca de 62s para passar pelos 35 passos e depois 170s para os 15 passos do upscaler e mais 20s para cada passagem do ADetailer. Isso não parece corresponder aos tempos que vejo aqui. Definitivamente vou dar uma olhada nas configurações de desempenho no Windows que podem estar influenciando, mas acredito que há mais sobre as configurações do A1111 que estão erradas.

---

> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
[**@ptk314**](https://civitai.com/user/ptk314) Quais são os tamanhos da imagem base? 512x768? Percebi que a largura e a altura base são o maior aumento no tempo e na VRAM necessários para fazer uma imagem, perdendo até para os passos de amostragem. A próxima coisa mais lenta é o sampler. DPM++ SDE Karras é muito, muito lento. DPM++ 2M Karras é muito mais rápido e é meu sampler favorito atualmente. Depois, a próxima coisa mais lenta é o número de passos de alta resolução. Esta é a minha intuição, mas não é científica. Tenho exatamente as mesmas especificações que você, só que não em um laptop.

---

> [**girlspeevr**](https://civitai.com/user/girlspeevr)
Adorei este artigo e o seu outro também! Passei um fim de semana inteiro explorando cada passo peça por peça.

---

> [**ptk314**](https://civitai.com/user/ptk314)
[**@RestlessDiffusion**](https://civitai.com/user/RestlessDiffusion) Sim, 512x768. Os números que mencionei são os mesmos parâmetros exatos (incluindo o sampler) que você está usando em seus exemplos. De fato, tentar gerar imagens grandes com SD-XL é muito mais lento do que uma simples 512x512 ou até 512x768. Mas isso não é nada comparado ao upscaler, que às vezes pode ser um pesadelo.
Vi um vídeo no YouTube hoje mais cedo de um cara que conduziu alguns experimentos com uma placa gráfica de 8GB de VRAM e sua conclusão é que devemos deixar o parâmetro --medvram de fora, já que usar apenas --xformers é cerca de 15% mais rápido.

---

> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
[**@ptk314**](https://civitai.com/user/ptk314) Não tenho certeza do porquê demora tanto para você. Leva menos de 30 segundos para todo o processo, excluindo o ADetailer, e mesmo assim vai para cerca de 40 segundos no total.
Você está usando modelos SDXL ou SD1.5?

---

> [**ptk314**](https://civitai.com/user/ptk314)
[**@RestlessDiffusion**](https://civitai.com/user/RestlessDiffusion) Como eu disse, apenas reutilizei os prompts e parâmetros das imagens que você publicou em seus tutoriais: mesmos tamanhos, mesmos samplers, etc., usando o A1111. Então, basicamente, SD 1.5 em 512x768, upscale x2 usando qualquer upscaler que você selecionou. Devo tentar medir o desempenho usando o ComfyUI nas mesmas imagens, se eu conseguir reproduzir exatamente a mesma configuração. Estou com a impressão de que o Comfy é significativamente mais rápido que o A1111, mas preciso obter números objetivos. Vou te informar sobre minhas descobertas.

---

> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
[**@ptk314**](https://civitai.com/user/ptk314) Desculpe, não posso ajudar mais! Não tenho ideia do porquê eles demoram tanto para você gerar 😐😐😐

---

> [**Cuajoneta**](https://civitai.com/user/Cuajoneta)
Muito bom guia!

---

> [**ptk314**](https://civitai.com/user/ptk314)
[**@RestlessDiffusion**](https://civitai.com/user/RestlessDiffusion) Uma atualização rápida para você (se estiver interessado)
Depois de experimentar com os parâmetros de inicialização do A1111, descobri o principal culpado do meu desempenho lento: o parâmetro --medvram!!!
Se sua placa GPU tem 8GB de VRAM, usar esse parâmetro de inicialização definitivamente vai desacelerar suas gerações por um fator enorme.
Os tempos variam um pouco de uma execução para outra, mas eu passei de cerca de 58s para cerca de 9s quando iniciei o A1111 SEM o --medvram (mas com o --xformers)
Suponho que o parâmetro pode ser útil se você tiver problemas de VRAM insuficiente, mas 8GB parecem ser suficientes para rodar sem ele.
O upscaling ainda é significativamente mais lento do que no seu sistema, no entanto. Para 17 passos, eu preciso de 1:40s (Remacri) a 1:50s (Ultrasharp) a 2:02s (Siax)
Devem haver mais truques de otimização para descobrir para cortar o tempo desses 17 passos pela metade, mas já estou feliz com os tempos de geração de resolução base.
Posso também confirmar que o ComfyUI não tinha o problema e está rodando a 7s/geração no mesmo caso.

---

> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
[**@ptk314**](https://civitai.com/user/ptk314) Isso é muito estranho. Eu não teria adivinhado que era o caso. Acho que isso significa que não posso fazer nenhuma suposição sobre as GPUs das pessoas daqui para frente.

---

> [**stone2052**](https://civitai.com/user/stone2052)
Obrigado por compartilhar

---

> [**emilyhearts**](https://civitai.com/user/emilyhearts)
Nunca sinta que promover suas próprias coisas (especialmente um modelo) é vergonhoso. Você fez este guia inteiro para todos lerem e está incrivelmente bem montado. Na verdade, vou experimentar seu modelo porque realmente parece ótimo.

---

> [**AI_Fire_Rabbit**](https://civitai.com/user/AI_Fire_Rabbit)
非常好的教程，适合所有人学习新的提示和技巧。
祝贺。工作做得很好。值得星级评价。
自一月份以来，我也一直在对SD-1.5进行大量测试，并发现了许多东西
人们仍然不知道。特别适用于照片级逼真的摄影，这是完全不同的
编写提示的方式比你的风格。

---

> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
[**@nbsuda003**](https://civitai.com/user/nbsuda003) Obrigado! Eu não sei chinês, então espero que o GPT-4 não tenha mentido para mim quando traduziu suas palavras, rsrs

---

> [**ptk314**](https://civitai.com/user/ptk314)
[**@RestlessDiffusion**](https://civitai.com/user/RestlessDiffusion) Pelo que o Google Translate me diz, parece ser uma tradução chinesa da primeira mensagem do [**@maxhitman**](https://civitai.com/user/maxhitman)

---

> [**Wolfsjong**](https://civitai.com/user/Wolfsjong)
Muito bom artigo e muito útil. Obrigado!

---

> [**KotatsuAi**](https://civitai.com/user/KotatsuAi)
Obrigado por este (alta qualidade:1.3), (altamente detalhado:1.2) tutorial, guia, artigo. Me pergunto por que um usuário com seu nível de habilidade está preso no auto1111 em vez de usar o ComfyUI. Eu me mudei para o Comfy por sua excelente gestão de memória (tenho 6GB de VRAM) e porque a GUI do a1111 é desesperadora. Um breve tutorial para upscaling decente (com o Comfy) seria muito apreciado, todas as vezes que tento aumentar a resolução de uma imagem o resultado está longe do ideal.

---

> [**ptk314**](https://civitai.com/user/ptk314)
[**@Kotatsu_AI**](https://civitai.com/user/Kotatsu_AI)
Você já assistiu a algum dos tutoriais do Olivio Sarikas, Sebastian Kamph ou Scott Detweiler no YouTube? Scott, em particular, cobre quase exclusivamente o ComfyUI. Muito esclarecedor, embora alguns de seus vídeos sejam bastante avançados e não sejam para novatos.
Agora, cuidado com a diferença na interpretação de prompts entre A1 e Comfy. Se você copiar literalmente os prompts neste guia para o ComfyUI, pode se surpreender com os resultados, (mas há alguns nós para ajudar)
[**@RestlessDiffusion**](https://civitai.com/user/RestlessDiffusion) Eu apoio o pedido do Kotatsu_AI por um guia orientado ao ComfyUI.

---

> [**DonghyukKim**](https://civitai.com/user/DonghyukKim)
Dois polegares para cima, este é o melhor tutorial que já vi!!

---

> [**udiai**](https://civitai.com/user/udiai)
Obrigado!!

---

> [**Silencer**](https://civitai.com/user/Silencer)
Obrigado pelo seu guia. A parte sobre o ADetailer foi particularmente valiosa para mim.
Guias como o seu enriquecem imensamente esta comunidade.
Trabalho incrível!
Abraços

---

> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
[**@ptk314**](https://civitai.com/user/ptk314) [**@Kotatsu_AI**](https://civitai.com/user/Kotatsu_AI) Ah, cara, parece que tenho trabalho pela frente então. 😅

---

> [**ptk314**](https://civitai.com/user/ptk314)
[**@RestlessDiffusion**](https://civitai.com/user/RestlessDiffusion) Estou ganhando experiência lentamente com o ComfyUI. Se eu puder ajudar e retribuir o favor que você nos fez com seus guias, será uma grande honra e prazer. Você pode me encontrar no Discord com o mesmo ID.

---

> [**Ragamuffin20**](https://civitai.com/user/Ragamuffin20)
Este guia foi perfeito e desculpe por ter lido apenas as percepções, mas eu já sabia dessas coisas, então os detalhes não foram necessários, mas isso será extremamente útil para novos usuários, então obrigado por isso.

---

> [**dstsckr**](https://civitai.com/user/dstsckr)
Muito obrigado por guias tão bem escritos. Um focado em inpainting seria incrível.



---

> [**unknowncity**](https://civitai.com/user/unknowncity)
Há algumas coisas muito úteis aqui!

---

> [**MrDrizzyJunior**](https://civitai.com/user/MrDrizzyJunior)
Novo no Stable Diffusion, mas aprendendo rápido, obrigado pelo seu trabalho, Deus abençoe!

---

> [**Rolick**](https://civitai.com/user/Rolick)
[**@RestlessDiffusion**](https://civitai.com/user/RestlessDiffusion) Tenho uma RTX3070 e para gerar a imagem na seção de visualização ao vivo levou 8 minutos T_T

---

> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
[**@Rolick**](https://civitai.com/user/Rolick) Certifique-se de estar usando a configuração NN para a qualidade da imagem na visualização ao vivo, é a primeira coisa que vem à mente. Além de ver suas configurações, não saberia dizer. 😐

---

> [**BradOh**](https://civitai.com/user/BradOh)
[**@maxhitman**](https://civitai.com/user/maxhitman) Ótimos tutoriais, você realmente me ajudou a começar - os renders iniciais eram horríveis! Rsrs. Tanto a aprender e tanto evoluindo a cada poucas semanas :) Obrigado

---

> [**Geniuss**](https://civitai.com/user/Geniuss)
[**@MetaSamsara**](https://civitai.com/user/MetaSamsara) 👍

---

> [**alby128**](https://civitai.com/user/alby128)
[**@RestlessDiffusion**](https://civitai.com/user/RestlessDiffusion) Obrigado por este tutorial incrível! Aprendi muito, especialmente sobre a posição dos termos no prompt e os embeddings.
Tenho algo que me intriga: apesar de ter baixado todos os modelos corretos (com hashes correspondentes), não consigo reproduzir exatamente seus resultados. As imagens resultantes são semelhantes (usei o PNG info para obter a configuração exata), mas não são tão boas quanto as suas. Alguma sugestão?

---

> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
[**@alby128**](https://civitai.com/user/alby128) Estou satisfeito em saber que você achou útil!
Quanto ao motivo pelo qual suas imagens podem não ser tão boas ou diferentes, sem ver as imagens para comparação, não posso dizer o que pode estar errado. Se você quiser fazer o upload dessas imagens em um post e responder a isso com o link, ficarei feliz em dar uma olhada rápida.

---

> [**alby128**](https://civitai.com/user/alby128)
[**@RestlessDiffusion**](https://civitai.com/user/RestlessDiffusion) Obrigado. Postei três tentativas aqui: https://civitai.com/posts/1025299 (a última iteração da janela do aquário, a primeira iteração do LoRa, a primeira iteração do ADetailer). Espero que você possa me ajudar a entender isso.

---

> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
[**@alby128**](https://civitai.com/user/alby128) O problema é que estou usando geração de números aleatórios por GPU e as imagens que você gerou estão usando geração de números por CPU. Você pode mudar isso indo para Configurações -> Stable Diffusion -> role para baixo até perto do final, e mude de CPU para GPU. Depois, Aplique Configurações e Recarregue a UI, e você deverá ser capaz de obter as mesmas imagens que eu gerei.
Espero que isso ajude, abraços.

---

> [**alby128**](https://civitai.com/user/alby128)
[**@RestlessDiffusion**](https://civitai.com/user/RestlessDiffusion) Esse era meu medo... o problema é que tenho o RNG configurado para GPU no Automatic1111 MAS também estou usando dois backends que provavelmente não suportam isso (MPS no Apple M2 e DirectML no AMD RT 5700 XT), enquanto eu acho que você usou CUDA em uma GPU Nvidia...
Infelizmente, não tenho uma GPU Nvidia disponível no momento. Acho que não preciso reproduzir exatamente seus resultados, era apenas para confirmação de que entendi os passos "corretamente". Mas, se é apenas uma questão de seed/RNG, ainda estou bem.
Obrigado novamente por este post e pelo seu tempo!

---

> [**g0t0s0da**](https://civitai.com/user/g0t0s0da)
Muito bom tutorial, eu dependo muito de Textural Inversions e não sabia que poderia dar peso a elas. Também estava usando o ADetailed de forma muito básica e você me mostrou como obter muito mais dele.

---

> [**feralun**](https://civitai.com/user/feralun)
Oi,
obrigado pelo tutorial!
>
> Comecei há cerca de um ano com stable diffusion, mas parei rapidamente depois de perceber que minhas fotos não eram o que eu queria, fiquei um pouco frustrado.
>
> Agora, um ano depois, quis tentar novamente, e seu tutorial me deu os últimos detalhes que eu precisava!
obrigado, cara!

>> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
Estou feliz que você achou útil!

---

> [**gogogo131347453**](https://civitai.com/user/gogogo131347453)
Este é um tutorial incrível. Muito mais valioso do que apenas assistir alguém mostrando no YouTube. Adoro os exemplos que você fez e como nos guiou por eles. Trabalho fantástico.
Se você está procurando mais coisas para fazer (desculpe por provocá-lo), adoraria ver um guia detalhado semelhante sobre inpainting e, mais ainda, sobre controlnets e openpose em particular.
Muito obrigado. Aprendi muitas coisas com as quais nem ousava mexer.

>> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
Sim, esses definitivamente estão na lista, mas esses guias levam muito tempo para serem feitos 😩

---

> [**rob79**](https://civitai.com/user/rob79)
Isso foi muito esclarecedor, estou tentando entender como tudo funciona, isso condensou muito do que eu teria que procurar na internet em pedaços e partes, muito apreciado!

---

> [**Axel0689**](https://civitai.com/user/Axel0689)
Guia essencial. Parabéns. Apenas uma coisa: é possível corrigir o link para LOWRA. Parece não estar funcionando, obrigado.

>> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
Parece que o autor original do Lowra o removeu do site. Deixe-me investigar.

>> [**pmg1**](https://civitai.com/user/pmg1)
procure por lowra e alguém o reuploadou!

>>> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
De fato, alguém fez! Obrigado amigo. Vou atualizar o link.

---

> [**pmg1**](https://civitai.com/user/pmg1)
Estou com um problema ao seguir o tutorial. Todos os meus prompts saem da mesma forma, mas por algum motivo eles são esbranquiçados e sem cores vibrantes? Especialmente no succubus e no azteca. Qualquer ajuda será muito apreciada.

>> [**pmg1**](https://civitai.com/user/pmg1)
Devo mencionar que é como um efeito de baixa saturação. Eu até peguei sua imagem para o azteca e coloquei no img2img e removi o prompt e ainda assim diminuiu a saturação.

>>> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
Olá amigo, o principal culpado das cores desbotadas é um VAE ausente. Por favor, siga as instruções para este modelo VAE. Espero que isso ajude! Sinta-se à vontade para entrar em contato se precisar de mais assistência. https://civitai.com/models/276082/vae-ft-mse-840000-ema-pruned-or-840000-or-840k-sd15-vae?modelVersionId=311162

>>>> [**pmg1**](https://civitai.com/user/pmg1)
Obrigado! Tive que mexer nas minhas configurações. Descobri que estava selecionando automaticamente um VAE e estava escolhendo nenhum em vez dos que eu já havia baixado.

---

> [**rickmurphy789626**](https://civitai.com/user/rickmurphy789626)
Muito bom tutorial, estruturado de forma abrangente e fácil de entender. Muito obrigado pelo tempo e esforço que você dedicou para ajudar todos nós a melhorarmos um pouco. Definitivamente aprendi algumas coisas que não sabia antes e fiz algumas conexões que provavelmente deveria ter feito anteriormente, então, novamente, muito obrigado. Por favor, continue com o ótimo trabalho e continue a compartilhar com a comunidade, todos nós realmente apreciamos.

>> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
Obrigado pelas palavras gentis, amigo. Comentários como esses são a razão pela qual eu faço isso.

---

> [**MrCee12**](https://civitai.com/user/MrCee12)
Obrigado por suas percepções e sabedoria. Sua atenção aos detalhes, clareza e simplicidade são apreciadas!

>> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
Obrigado amigo, espero que você tenha achado útil!

---

> [**Zylas**](https://civitai.com/user/Zylas)
Ótimo tutorial!

>> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
Obrigado!

---

> [**erfan2281592**](https://civitai.com/user/erfan2281592)
Você realmente conseguiu, eu estava procurando desesperadamente por um bom artigo ou fonte para entender melhor os prompts e não havia nada tão significativo.
Este é o melhor guia de prompts de TODOS.

---

> [**Elegon**](https://civitai.com/user/Elegon)
A comunidade tem sorte de ter pessoas como você!

---

> [**funcar291**](https://civitai.com/user/funcar291)
Muito obrigado pelo seu trabalho, comecei a me aprofundar em SD e afins há poucos dias e isso me ajudou muito <3
Uma atualização, acho que o Detail Tweaker mudou seu "nome" ou não sei como chamamos isso, não é mais <lora:more-details:1> mas sim <lora:add-detail:1>, ou pelo menos foi o que percebi quando tentei seguir seu guia :)

---

> [**FUAIfu**](https://civitai.com/user/FUAIfu)
Seu link para o guia de A13JM (Making Images Great Again!) não funciona para mim. Deveria ser https://civitai.com/articles/4990/making-images-great-again ?

>> [**RestlessDiffusion - OP**](https://civitai.com/user/RestlessDiffusion)
Sim, você está certo, atualizei o URL. Obrigado, amigo.

---

> [**stifffits**](https://civitai.com/user/stifffits)
Obrigado pelo guia útil! Algumas perguntas:
Sobre parênteses e pesos numéricos: eles se acumulam ou não? ((interior:1.9)) é mais pesado que (interior:1.9) ou não?
Para aqueles que não têm um computador poderoso o suficiente para rodar programas de stable diffusion baixados, qual guia para o gerador de imagens do próprio Civitai você recomendaria?
Junto com as palavras-chave de ativação vêm também os códigos dos recursos relevantes expressos assim: <[palavra-chave de ativação ou algo estendido]:[peso digital do recurso]>. Se este último for incluído no prompt, também é necessário adicionar a palavra-chave de ativação se a palavra-chave de ativação e o peso digital do recurso tiverem o mesmo peso?
Você usou os sinais '<' e '>' como parênteses para o termo fotográfico em seu exemplo. Esses parênteses devem ser omitidos no prompt final ou incluídos? O que os parênteses '<', '>', '[', ']', '{' e '}' fazem nos prompts?
Prompts compostos como ', old_factory,' e ', old factory,' funcionam de maneira diferente nos prompts?
Se eu quiser representar um personagem várias vezes (o original e um ou mais clones do original) e outro como uma única pessoa, como promptar isso? Por exemplo, três Flandre Scarlets e uma Remilia Scarlet, ao todo '4girls' em uma imagem. Eu tentei, mas falhei, em um prompt semelhante consegui apenas uma Flandre Scarlet.

>> [**FUAIfu**](https://civitai.com/user/FUAIfu)
Eu vi diferentes alegações de diferentes pessoas. Alguns dizem que parênteses são obsoletos e você não deve usar mais de um conjunto de (), em vez disso, focando apenas no peso numérico. Se bem me lembro, os conjuntos extras eram apenas peso exponencial: por exemplo, ((1.1)) = 1.1 * 1.1 = (1.21).
Só vi <> usados para definir recursos específicos. A coisa a entender aqui é que para aqueles que estão rodando algo como o A1111 em sua própria máquina, a interface é diferente do site. No site, você tem sliders para definir recursos, mas se uma interface não oferecer isso, a configuração é definida via prompt, onde a sintaxe é basicamente <Nome do Recurso:Peso do Recurso>. (A maioria dos recursos são LoRAs).
Por exemplo, digamos que você veja uma imagem que deseja remixar. Ela usou o prompt <3Danimation:0.8>. Se você encontrasse essa LoRA no site e a adicionasse ao seu trabalho, você teria um slider que poderia definir para 0,80. Você não precisaria do prompt <3Danimation:0.8> quando o slider estiver definido para 0,8 porque isso seria redundante. Não posso confirmar qual configuração ele usará se seu slider e seu prompt estiverem definidos para pesos diferentes.
\[] é algo que só vi usado para blending. A sintaxe é [De:Para:Quando], por exemplo, você pode tentar misturar características associadas a diferentes prompts, como raça, e especificar em que ponto nos passos deve mudar de desenhar o primeiro prompt para o segundo. [Asiático:Francês:0.5] se você tiver seu trabalho definido para 30 passos, desenha Asiático por 15 passos e Francês por 15 passos.
Não tive problemas para obter várias vistas de um personagem, usando prompts como "várias vistas," "vista frontal," "vista lateral," etc. No entanto, o gerador do site atualmente não oferece um meio de confinar prompts a regiões específicas. Não posso dar conselhos sobre cenas com vários personagens feitas no site além de verificar o que foi postado, encontrar algo semelhante ao que você deseja e verificar como foi feito.

>> [**FUAIfu**](https://civitai.com/user/FUAIfu)
palavras-chave de ativação são apenas prompts que um recurso foi treinado para usar. Por exemplo, uma LoRA treinada para desenhar um personagem específico pode sugerir que você use certos prompts. Existem vários personagens chamados Lulu e uma IA não pode saber qual você quer usando "Lulu" como um prompt solitário. Uma LoRA Lulu de FFX com palavras-chave de ativação como "batom roxo" é mais eficaz quando você inclui todas as palavras-chave de ativação relevantes como prompts normais. Elas não precisam de sintaxe especial a menos que você esteja tentando fazer algo especial com elas, como misturar dois personagens.

---

> [**Thenar_Eminence**](https://civitai.com/user/Thenar_Eminence)
Ótimo artigo, não sei como o ignorei antes. Incrível. Bom modelo também, auto-promoções são um pequeno preço a pagar.

---
