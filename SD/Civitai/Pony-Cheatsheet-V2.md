# [Pony Cheatsheet V2](https://civitai.com/articles/5473)

---

Criado em: 05 de julho de 2024 às 20:21 (UTC: -03:00)
Tags: []
Autor:

---

Finalmente, após muitos testes, cerca de 30+ modelos lançados durante a atualização, e sabe-se lá quantos prompts e gerações. A versão 2 do meu cheatsheet está pronta. Eu a movi para um novo artigo para que o antigo ainda possa ser consultado, se as pessoas desejarem.

Obrigado a todos por esperarem, sei que demorou um pouco, espero que tenha valido a pena. Como sempre, se tiverem qualquer pedido ou sugestão, por favor deixem um comentário abaixo.

## Introdução:

Desta vez, adicionei algumas opções para diferentes modelos e novos estilos. Também optei por usar um personagem em vez de uma garota para facilitar a comparação. Assim como no meu cheatsheet anterior, usei o mesmo modelo, prompt, seed, etc., a menos que indicado de outra forma.

Com os modelos, cada um funciona melhor em diferentes situações, então deve oferecer opções para as pessoas. Quanto aos motivos pelos quais escolhi cada um, são por alguns motivos. Meus testes com eles atingiram alguns pontos:

Prompt mínimo - Não gosto de ter 15 linhas de negativos só para obter uma imagem bonita, então escolhi aqueles que precisam de pouco prompting para ficarem bons.

Qualidade - Aqui é claro, eles precisam ter uma qualidade incrível com pouco ou nenhum prompt.

Obediência - Este é outro grande ponto para mim, se eu disser para colocar uma pinta na bochecha esquerda, quero ver uma pinta na bochecha esquerda. Basicamente, apenas certificando-se de que o modelo obedece bem e entende as palavras faladas, em vez de apenas tags.

Corpos Inteiros - Verificando como a qualidade é ao mostrar um corpo inteiro, já que em muitos modelos o rosto fica muito ruim quando se mostra uma área maior. Nenhum se saiu extremamente bem, então ainda recomendaria adetailer, eles funcionaram razoavelmente bem, então adetailer não precisa de nenhuma configuração maluca.

Com isso, aqui estão minhas recomendações atuais:

Melhor Modelo para Anime: Hassaku [https://civitai.com/models/376031](https://civitai.com/models/376031)

Melhor Modelo para Realista: Fast Pony [https://civitai.com/models/378903](https://civitai.com/models/378903)

Melhor Modelo para Foto Realista: For Real [https://civitai.com/models/451528](https://civitai.com/models/451528)

Melhor Modelo para Anime 2.5D: Faetality [https://civitai.com/models/331116](https://civitai.com/models/331116)

Melhor Modelo para Uso Geral: Perfect Pony [https://civitai.com/models/439889](https://civitai.com/models/439889)

Menção Honrosa: Piano Mix, embora este não atinja realmente uma área, ele tem um estilo e qualidade muito bons. [https://civitai.com/models/400801](https://civitai.com/models/400801)

## Detalhes Base:

Estes são os detalhes com os quais todos os estilos são feitos, a menos que indicado de outra forma.

Modelo: Perfect Pony

Método de Amostragem: DPM++ 2M SDE Karras

Passos: 35

Largura: 1024

Altura: 1024

Escala CFG: 7

ClipSkip: 2

Seed: 2161800139

## Estilos:

## 2.5D:

### Notas:

Lora: https://civitai.com/models/297619
Alternativa: Detalhes do Modelo Faetality no final

### Prompt:
```
(<lora:Realistic_2.5DAnime_Merge1:0.8>), score_9, score_8_up, score_7_up, Tifa, portrait
```
### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/2ba777c0-93f0-41f0-bc8e-c820b64c51ea/width=525/2ba777c0-93f0-41f0-bc8e-c820b64c51ea.jpeg)

## 3D:

### Notas:

Lora: https://civitai.com/models/331812

### Prompt:
```
(<lora:RetouchXL_PonyV6_v2:1> 3d),score_9,score_8_up,score_7_up,Tifa,portrait
```
### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/7836a781-8a0b-4122-952b-bce1ed74a6e6/width=525/7836a781-8a0b-4122-952b-bce1ed74a6e6.jpeg)

## Anime dos Anos 80:

### Notas:

Lora: https://civitai.com/models/340679

### Prompt:
```
(<lora:80s_Anime_Woman:1> 80swa), score_9, score_8_up, score_7_up, Tifa, portrait
```

### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/5cedeac0-9f49-46e9-8d79-bb353079b04e/width=525/5cedeac0-9f49-46e9-8d79-bb353079b04e.jpeg)

## Mangá dos Anos 90:

### Notas:

Lora: https://civitai.com/models/409261

### Prompt:
```
(<lora:90s_manga_style:1> monochrome, grayscale), score_9, score_8_up, score_7_up, Tifa, portrait
```
### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/b0ada972-2c81-47ed-8ecb-46e79fffd17a/width=525/b0ada972-2c81-47ed-8ecb-46e79fffd17a.jpeg)

## Esboço:

### Notas:

Lora: https://civitai.com/models/451101

### Prompt:
```
(<lora:characters_sketch_PDXL:1> Stexsketch), score_9, score_8_up, score_7_up, Tifa, portrait
```
### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f9406d99-7ceb-48d9-8e0e-085ecf803881/width=525/f9406d99-7ceb-48d9-8e0e-085ecf803881.jpeg)

## Disney:

### Notas:

Lora: https://civitai.com/models/404277

### Prompt:
```
(<lora:DisneyStudios_style:1> DisneyStyleXLP,cartoon,cute,cinematic),score_9,score_8_up,score_7_up,Tifa,portrait
```
### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/0849c5b0-bb32-4eeb-aefd-973d0a18ec49/width=525/0849c5b0-bb32-4eeb-aefd-973d0a18ec49.jpeg)

## Family Guy:

### Notas:

Lora: https://civitai.com/models/413128
A lora tem recomendações sobre como fazer isso parecer mais com Family Guy, eu só quis manter próximo aos outros exemplos.

### Prompt:
```
(<lora:FamilyGuy_style_PDXL:1> familyguystyle), score_9, score_8_up, score_7_up, Tifa, portrait
```
### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/be991d1d-8754-461a-b0f4-5110bd019e9c/width=525/be991d1d-8754-461a-b0f4-5110bd019e9c.jpeg)

## Futurama:

### Notas:

Lora: https://civitai.com/models/448193

### Prompt:
```
(<lora:Futurama_Style:1> futuramastyle), score_9, score_8_up, score_7_up, Tifa, portrait
```
### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/dc93aeda-0b75-4f4e-a5cf-3da432627010/width=525/dc93aeda-0b75-4f4e-a5cf-3da432627010.jpeg)

## Ghibli:

### Notas:

Provavelmente voltarei a este, não estou satisfeito com os resultados e já obtive resultados muito melhores no passado.
Lora: https://civitai.com/models/433138

### Prompt:
```
    <lora:Ghibli_style_PDXL:1> Ghiblistyle, score_9, score_8_up, score_7_up, Tifa, portrait
```
### Neg Prompt:
```
    score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f1f04b82-375e-421a-86a3-22dc2eff8a98/width=525/f1f04b82-375e-421a-86a3-22dc2eff8a98.jpeg)

## Anime dos Anos 90:

### Notas:

Lora: https://civitai.com/models/333041

### Prompt:
```
(<lora:OldAnimeStyle_XL_v3:1> Vintage, 1990s \(style\)), score_9, score_8_up, score_7_up, Tifa, portrait
```
### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/88533519-c49c-43f6-9c86-b282e1216e1a/width=525/88533519-c49c-43f6-9c86-b282e1216e1a.jpeg)

## OSRS:

### Notas:

Por quê? Porque é engraçado
Lora: https://civitai.com/models/429004

### Prompt:
```
(<lora:osrs character lora v0.2:1>), score_9, score_8_up, score_7_up, Tifa, portrait
```
### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/7fae36bc-6002-406a-8fc7-cb454f145d68/width=525/7fae36bc-6002-406a-8fc7-cb454f145d68.jpeg)

## Pixel:

### Notas:

Lora: https://civitai.com/models/449085

### Prompt:
```
(<lora:Pixel_Art_Pony:1>), score_9, score_8_up, score_7_up, Tifa, portrait
```
### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/d3da8413-6c5e-4c68-bad5-95e33bd26d23/width=525/d3da8413-6c5e-4c68-bad5-95e33bd26d23.jpeg)

## Source:

### Notas:

Lora: https://civitai.com/models/335860

### Prompt:
```
<lora:X3dCE_V2:1> X3dCE,3d,score_9,score_8_up,score_7_up,Tifa,portrait
```
### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/785b97ec-0164-47d3-ad1c-a0f320069c67/width=525/785b97ec-0164-47d3-ad1c-a0f320069c67.jpeg)

## Robotização:

### Notas:

Lora: https://civitai.com/models/259585

### Prompt:
```
(<lora:Robotization_Pony:1> Robot girl, Mecha, Android, mechanical limbs, robot joints, metal skin, black sclera, no mouth, armor, glowing eyes, no face), score_9, score_8_up, score_7_up, Tifa, portrait
```
### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/19aed081-3697-4406-85ce-3b7b8ec21c37/width=525/19aed081-3697-4406-85ce-3b7b8ec21c37.jpeg)

## Realista:

### Notas:

Modelo Usado: Fast Pony
Detalhes no final

### Prompt:
```
hyperrealistic art of Tifa,portrait
```
### Neg Prompt:

### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/a291373a-65b2-4ca4-8401-60dda77222fe/width=525/a291373a-65b2-4ca4-8401-60dda77222fe.jpeg)  

## Foto Realista:

### Notas:

Modelo Usado: ForReal
Detalhes no final

### Prompt:
```
score_9,score_8_up,score_7_up,Tifa,portrait
```
### Neg Prompt:
```
score_7,score_6,score_5,score_4
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/cd6b28fe-84d2-475c-ae36-83de940d735f/width=525/cd6b28fe-84d2-475c-ae36-83de940d735f.jpeg)

## Modelos:

## Hassaku:

### Notas:

Modelo: https://civitai.com/models/376031

### Prompt:
```
score_9, score_8_up, score_7_up, Tifa,portrait
```
### Neg Prompt:
```
score_6, score_5, score_4, signature, source_pony, source_furry, source_cartoon, 3d, realistic, monochrome, mosaic, censor, white bar, black bar
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/7f12b217-4493-4a31-9636-b6112d79e9d1/width=525/7f12b217-4493-4a31-9636-b6112d79e9d1.jpeg)

## Fast Pony:

### Notas:

Modelo: https://civitai.com/models/378903

### Prompt:
```
hyperrealistic art of Tifa,portrait
```
### Neg Prompt:

### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/26537367-572f-47e1-875e-988b7254f672/width=525/26537367-572f-47e1-875e-988b7254f672.jpeg)

## For Real:

### Notas:

Modelo: https://civitai.com/models/451528

### Prompt:
```
score_9,score_8_up,score_7_up,Tifa,portrait
```
### Neg Prompt:
```
score_7,score_6,score_5,score_4
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/93a559f2-ed29-4d7d-a2c3-0121b3c3226f/width=525/93a559f2-ed29-4d7d-a2c3-0121b3c3226f.jpeg)

## Faetality:

### Notas:

Modelo: https://civitai.com/models/331116

### Prompt:
```
score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up, Tifa, portrait
```
### Neg Prompt:

### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/edaf9ef4-fab3-476f-bfdb-7d634491210a/width=525/edaf9ef4-fab3-476f-bfdb-7d634491210a.jpeg)

## Perfect Pony:

### Notas:

Modelo: https://civitai.com/models/439889

### Prompt:
```
score_9, score_8_up, score_7_up, Tifa, portrait
```
### Neg Prompt:
```
score_4,score_5,score_6
```
### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/cc1100b7-0993-4131-8b3e-cfb8c6610a28/width=525/cc1100b7-0993-4131-8b3e-cfb8c6610a28.jpeg)

## Piano:

### Notas:

Modelo: https://civitai.com/models/400801

### Prompt:

```
score_9, score_8_up, score_7_up, Tifa, portrait
```

### Neg Prompt:

```
score_6_up, score_4_up, score_5_up,bad anatomy, bad hands, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, quality bad, hands bad, eyes bad, face bad, normal quality, jpeg artifacts, (signature, watermark, text, username, artist name:1.2), blurry
```

### Pré-visualização:  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/b081942d-4442-45a0-9c2c-547c590a8e49/width=525/b081942d-4442-45a0-9c2c-547c590a8e49.jpeg)
