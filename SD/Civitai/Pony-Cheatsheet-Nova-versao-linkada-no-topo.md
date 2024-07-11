# [Pony Cheatsheet (Nova versão linkada no topo)](https://civitai.com/articles/4829/pony-cheatsheet-new-version-linked-at-top)

---

Criado em: 05 de julho de 2024 às 20:21 (UTC: -03:00)
Tags: []
Autor: 

---

## **Versão 2 do cheatsheet lançada. Pode ser encontrada aqui:** [**https://civitai.com/articles/5473**](https://civitai.com/articles/5473)

Tenho mexido com Pony há um tempo e percebi que muitas pessoas estão criando muitas imagens confusas e bagunçadas. Então, criei uma pequena base para vários estilos/fontes.

Por favor, leia as notas para cada tipo para obter informações extras além dos padrões.

Se você tiver alguma sugestão, melhoria ou pedido, sinta-se à vontade para perguntar. No entanto, tenha em mente que eu não faço estilos de artistas.

Cada imagem (a menos que declarado de outra forma nas notas) foi feita com as seguintes informações:  
Modelo: [https://civitai.com/models/288584?modelVersionId=324524](https://civitai.com/models/288584?modelVersionId=324524)

Prompt: Usei a base e "1girl, face" como prompt para manter as coisas parecidas.  
Método de amostragem: DPM++ 2M SDE Karras  
Passos de amostragem: 35  
Largura: 1024  
Altura: 1024  
Seed: 2242263340

- Atualização 19/04/24:
    

Um bom modelo alternativo é: [https://civitai.com/models/356456/matrix-hentai-pony](https://civitai.com/models/356456/matrix-hentai-pony)  
Descobri que este dá um pouco mais de poder aos estilos e personagens. Ainda estou testando com configurações e afins e atualizarei mais após os testes. Também encontrei alguns novos estilos que adicionarei.

## 2.5D:

### Notas:

Lora: https://civitai.com/models/297619  
Força entre 0.5-0.9 para obter mais ou menos do visual 2.5D

### Prompt:

```
(<lora:Realistic_2.5DAnime_Merge1:0.8>, score_9, score_8_up, score_7_up), BREAK
1girl, face
```

### Neg:

```
(score_3_up, score_4_up, score_5_up)
```

### Preview:

![](http://192.168.1.3:7860/file=/home/bk/documents/stable-diffusion-webui-old/output/txt2img-images/2024-04-05/00111-2242263340.png?1712360443.0399737)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/4e40fc2e-7e45-42c6-a9ce-3d290d4b89c5/width=525/4e40fc2e-7e45-42c6-a9ce-3d290d4b89c5.jpeg)

## Anime dos anos 80:

### Notas:

Lora: https://civitai.com/models/340679/80s-anime-woman  
A imagem é bem sem graça, mas eu não queria mudar o prompt para manter as coisas iguais. Porém, adicionar um fundo simples ao neg normalmente corrige os fundos.

### Prompt:

```
(score_9, score_8_up, score_7_up, 80swa, <lora:80s_Anime_Woman:1>), BREAK
1girl, face
```

### Neg:

```
(score_3_up, score_4_up, score_5_up)
```

### Preview:

![](http://192.168.1.3:7860/file=/home/bk/documents/stable-diffusion-webui-old/output/txt2img-images/2024-04-05/00112-2242263340.png?1712360545.678623)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/1aa20009-bd41-4272-9e9d-44dc63fbbe3f/width=525/1aa20009-bd41-4272-9e9d-44dc63fbbe3f.jpeg)

## Anime dos anos 90:

### Notas:

Lora: https://civitai.com/models/333041

### Prompt:

```
(score_9, score_8_up, score_7_up, Vintage, 1990s \\(style\\)), <lora:OldAnimeStyle_XL_v3:0.75>, BREAK
1girl, face
```

### Neg:

```
(score_3_up, score_4_up, score_5_up)
```

### Preview:

![](http://192.168.1.3:7860/file=/home/bk/documents/stable-diffusion-webui-old/output/txt2img-images/2024-04-05/00113-2242263340.png?1712360632.040301)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/9f82be42-570b-4e28-9d77-0c9b2502676d/width=525/9f82be42-570b-4e28-9d77-0c9b2502676d.jpeg)

## Pixel Art:

### Notas:

Lora: https://civitai.com/models/43820

### Prompt:

```
(<lora:pixel:1> , score_9, score_8_up, score_8, pixelart, pixel), BREAK
1girl, face
```

### Neg:

```
(score_6, score_5, score_4)
```

### Preview:

![](http://192.168.1.3:7860/file=/home/bk/documents/stable-diffusion-webui-old/output/txt2img-images/2024-04-05/00114-2242263340.png?1712360714.8985848)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/e1c16e6a-f86c-4394-a957-252d50a36786/width=525/e1c16e6a-f86c-4394-a957-252d50a36786.jpeg)

## Realista:

### Notas:

Modelo: https://civitai.com/models/365041?modelVersionId=407890  
É muito bom com corpos, os rostos ainda podem melhorar. Normalmente crio algo e depois uso adetailer para corrigir os rostos com um modelo realista SDXL normal.

### Prompt:

```
(score_9,score_8_up,score_7_up,score_6_up), BREAK
1girl, face
```

### Neg:

```
(score_5,score_4)
```

### Preview:

![](http://192.168.1.3:7860/file=/home/bk/documents/stable-diffusion-webui-old/output/txt2img-images/2024-04-05/00115-2242263340.png?1712360843.3477693)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/4df081f5-9dbf-48ac-8615-02a84606ab92/width=525/4df081f5-9dbf-48ac-8615-02a84606ab92.jpeg)

## 3D:

### Notas:

Lora: https://civitai.com/models/331812

### Prompt:

```
(score_9, score_8_up, score_7_up, 3d, <lora:RetouchXL_PonyV6_v2:1>), BREAK
1girl, face
```

### Neg:

```
(score_3_up, score_4_up, score_5_up)
```

### Preview:

![](http://192.168.1.3:7860/file=/home/bk/documents/stable-diffusion-webui-old/output/txt2img-images/2024-04-05/00116-2242263340.png?1712360972.9536543)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/abf21529-2777-49df-bfcd-7d141e81755f/width=525/abf21529-2777-49df-bfcd-7d141e81755f.jpeg)

## Source:

### Notas:

Lora: https://civitai.com/models/335860/x3d-style-pony  
Qual é a diferença entre este e o 3D?... Mais brilho?  
Os rostos não são ótimos, adetailer com o mesmo modelo normalmente corrige isso.

### Prompt:

```
(score_9, score_8_up, score_7_up, X3dCE, 3d, <lora:X3dCE_V2:0.6>), BREAK
1girl, face
```

### Neg:

### Preview:

![](http://192.168.1.3:7860/file=/home/bk/documents/stable-diffusion-webui-old/output/txt2img-images/2024-04-05/00117-2242263340.png?1712361088.1059287)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/8c272fa1-a150-46bd-9437-d67c3365e3e5/width=525/8c272fa1-a150-46bd-9437-d67c3365e3e5.jpeg)

## Anime:

### Notas:

Acho que isso poderia ser melhor. Vou trabalhar para melhorar.

### Prompt:

```
(source_anime, score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up), BREAK
1girl, face
```

### Neg:

```
(score_3_up, source_furry, source_pony, source_cartoon)
```

### Preview:

![](http://192.168.1.3:7860/file=/home/bk/documents/stable-diffusion-webui-old/output/txt2img-images/2024-04-05/00118-2242263340.png?1712361189.7412739)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/02bcbc6f-df79-40a7-bdda-3b8d648142bf/width=525/02bcbc6f-df79-40a7-bdda-3b8d648142bf.jpeg)

## Furry:

### Notas:

Sim, apenas orelhas de gato. Mas queria manter tudo igual ao resto.

### Prompt:

```
(source_furry, score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up), BREAK
1girl, face
```

### Neg:

```
(score_3_up)
```

### Preview:

![](http://192.168.1.3:7860/file=/home/bk/documents/stable-diffusion-webui-old/output/txt2img-images/2024-04-05/00119-2242263340.png?1712361274.4062858)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/ea5625b0-2e55-4c75-9efd-f20b462dbb6d/width=525/ea5625b0-2e55-4c75-9efd-f20b462dbb6d.jpeg)

## Cartoon:

### Notas:

### Prompt:

```
(source_cartoon, score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up), BREAK
1girl, face
```

### Neg:

```
(score_3_up, source_anime, source_pony, source_furry)
```

### Preview:

![](http://192.168.1.3:7860/file=/home/bk/documents/stable-diffusion-webui-old/output/txt2img-images/2024-04-05/00120-2242263340.png?1712361353.4378548)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6ad7f8be-b6c4-4c24-8569-4ef7b4c77a2f/width=525/6ad7f8be-b6c4-4c24-8569-4ef7b4c77a2f.jpeg)

## Pony:

### Notas:

### Prompt:

```
(source_pony, score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up), BREAK
1girl, face
```

### Neg:

```
(score_3_up, source_anime, source_furry, source_cartoon)
```

### Preview:

![](http://192.168.1.3:7860/file=/home/bk/documents/stable-diffusion-webui-old/output/txt2img-images/2024-04-05/00121-2242263340.png?1712361439.076223)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/789ee59b-73cd-43b8-a9f8-1dad9f0bb581/width=525/789ee59b-73cd-43b8-a9f8-1dad9f0bb581.jpeg)