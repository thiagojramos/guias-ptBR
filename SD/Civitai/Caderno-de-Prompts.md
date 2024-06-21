# [Caderno de Prompts](https://civitai.com/articles/3160/prompt-notebook)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/3a2c1b6c-71d8-4e51-af9c-2f986bf79084/width=450/final_00003_.jpeg)


created: 13-06-2024
tags: `workflows` `guidelines` `prompting` `prompt engineer`
author: [taches](https://civitai.com/user/taches)

---

## Introdução

Olá. Aqui estão algumas Obs.s sobre como eu estruturo meus prompts. Este guia é o resultado de experiências pessoais, então nada é considerado verdade absoluta, é apenas o que está funcionando bem para mim!

Fui muito inspirado pelo [TianChimp](https://civitai.com/user/TianChimp) escrito no checkpoint [Hardcore - Hentai](https://civitai.com/models/136054?modelVersionId=232266). Confira meus [modelos favoritos](https://civitai.com/user/taches/collections), eles geralmente funcionam muito bem com a minha maneira de criar prompts. Se você quiser dar uma olhada no meu ambiente, veja meu artigo sobre o [ComfyUI Workflow](https://civitai.com/articles/5366/).

Eu vou atualizar este artigo de tempos em tempos. Não hesite em pegar, ajustar e compartilhar seus pensamentos!

## Filosofia

Eu tento seguir diretrizes filosóficas quando faço meus prompts. Recomendo ler o [Howto](https://danbooru.donmai.us/wiki_pages/howto:tag) do Danbooru, que são boas diretrizes.

-   **Marque o que você vê, não o que você sabe**. Não use tags de coisas que não estão visíveis na imagem final.
    
-   **Critérios Mínimos de Marcação**. Muitas tags causam ruído e levam a resultados menos precisos. Mantenha as coisas no estritamente necessário, coloque apenas o que você realmente quer ver. Por exemplo, por que colocar uma tag "nsfw" se sua cena já é explícita?
    

## Estrutura

Eu estruturo meus prompts principalmente em 5 categorias, com subcategorias dentro. Tento separar esses conceitos com linhas de quebra, isso me ajuda a fazer ajustes rapidamente. Recomendo dar uma olhada nos [grupos de tags](https://danbooru.donmai.us/wiki_pages/tag_groups) do Danbooru para encontrar o que você precisa.

Lembre-se de que é uma diretriz. Está ok misturar essa ordem, especialmente dentro de uma categoria.

**1\.** [**Composição**](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

1.  Qualidade
    
2.  Estilo (fotorrealístico, paleta de cores, etc)
    
3.  Ponto de vista
    
4.  Luz, Hora do dia (dia, noite, pôr do sol, …)
    

**2\. Sujeitos**

1.  Sujeito(s) principal(is) (menino, menina, animal, objeto, paisagem, …)
    

**3\. Ações**

1.  [Atos sexuais](https://danbooru.donmai.us/wiki_pages/tag_group%3Asex_acts)
    
2.  [Posições sexuais](https://danbooru.donmai.us/wiki_pages/tag_group%3Asexual_positions)
    

**4\.** [**Corpo**](https://danbooru.donmai.us/wiki_pages/tag_group%3Abody_parts)

1.  [Postura](https://danbooru.donmai.us/wiki_pages/tag_group%3Aposture)
    
2.  Características principais (tamanho, peso, tipo de corpo, gordura corporal, cor da pele, …)
    
3.  Elementos do corpo (seios, mamilos, bunda, …)
    
4.  Relacionados ao rosto (cor dos olhos, estilo de cabelo, …)
    
5.  Expressões (feliz, surpreso, sério, determinado, …)
    
6.  [Roupas](https://danbooru.donmai.us/wiki_pages/tag_group%3Aattire)
    

**5\. Fundo**

1.  Ambiente principal (interior, exterior, …)
    
2.  Clima (vento, chuva, neve, …)
    
3.  Objetos (móveis, veículos, …)
    

_Obs.: Me pergunto se Clima e Hora do dia não deveriam ir para Composição. Vou pensar sobre isso._

## Exemplo

Aqui está um exemplo seguindo a estrutura. Eu geralmente coloco algumas linhas de quebra para ter uma melhor visualização das principais áreas do prompt.

```
score_9, score_8_up, score_7_up, score_6_up,
lineart, warm tones,
portrait, from above,
sidelighting, light particles

1girl, ginger, solo,
hand in own hair, writing, holding pencil,
sitting, heads up,

pale skin, sweat, freckles, small breasts,
ginger hair, long hair, straight hair,
blue eyes, glowing eyes, glasses, looking at viewer,

smile, smirk, light frown,
white tank top,

indoor, library,
sunset, sunny, daylight,
desk, chair,
```

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/399adeb2-dde9-4496-b708-f67082a94bd9/width=525/399adeb2-dde9-4496-b708-f67082a94bd9.jpeg)
Como você pode ver, as linhas de quebra não dividem perfeitamente os 5 campos da estrutura. Dependendo do que você quer gerar, pode fazer mais sentido separar visualmente partes menores e maiores. Aqui, o _sujeito_, _ações_ e _postura_ do corpo estão agrupados: separá-los criaria uma bagunça visual mais do que qualquer outra coisa.

## Casos de Uso

Aqui estão alguns casos de uso e palavras-chave favoritas. Tento organizar por grupo lógico, do mais geral ao mais específico. Recomendo selecionar a dedo o que faz sentido para você, em vez de copiar e colar a linha toda.

Você também encontrará uma referência de onde eu coloco isso na estrutura. Usarei um ID com o formato [xx.yy], usando o acrônimo da categoria e sua subcategoria.

Lembre-se de que alguns deles também podem ir para cima ou para baixo na ordem de aparição (por exemplo, _de cabeça para baixo_ poderia ser colocado como [compos.pov] ou [body.posture]).

## Genérico

Aqui está o que eu uso em quase todos os meus prompts para influenciar a qualidade geral da imagem. Eu geralmente adiciono palavras-chave negativas adicionais se algo que eu não gosto aparecer, mas não antes. Novamente, gosto de manter as coisas curtas e simples.

A lista a seguir cobre alguns casos clássicos, detalhando tanto prompts _positivos_ quanto _negativos_.

-   **Qualidade (SD1.5)**  
    ([absurdres](https://danbooru.donmai.us/wiki_pages/absurdres), best quality, masterpiece:1.4),  
    (worst quality, low quality, [lowres](https://danbooru.donmai.us/wiki_pages/lowres), normal quality:1.4)
    
-   **Qualidade (Pony)**  
    score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up,  
    score_6, score_5, score_4,
    
-   **Detalhes**  
    detailed, detailed body, detailed face
    
-   **Negativo+**  
    text, logo, watermark, digit, [multiple views,](https://danbooru.donmai.us/wiki_pages/multiple_views) [monochrome,](https://danbooru.donmai.us/wiki_pages/monochrome)  
    [bad proportions,](https://danbooru.donmai.us/wiki_pages/bad_proportions) [anatomical nonsense,](https://danbooru.donmai.us/wiki_pages/anatomical_nonsense) [bad hands,](https://danbooru.donmai.us/wiki_pages/bad_hands) [bad face](https://danbooru.donmai.us/wiki_pages/bad_face)
    

_Obs.: Negativo+ é uma lista adicional onde eu seleciono conforme a situação. Na minha experiência, palavras-chave negativas relacionadas à anatomia nem sempre melhoram os resultados, então eu as mantenho de lado caso precise._

## Cenas

-   **Ações** [compos.style]: [motion lines](https://danbooru.donmai.us/wiki_pages/motion_lines), [emphasis lines](https://danbooru.donmai.us/wiki_pages/emphasis_lines), [speed lines,](https://safebooru.donmai.us/wiki_pages/speed_lines) [afterimage,](https://danbooru.donmai.us/wiki_pages/afterimage) [motion blur](https://danbooru.donmai.us/wiki_pages/motion_blur), [bouncing](https://danbooru.donmai.us/wiki_pages/bouncing)(+[hair](https://danbooru.donmai.us/wiki_pages/bouncing_hair), [breasts](https://danbooru.donmai.us/wiki_pages/bouncing_breasts), [ass](https://danbooru.donmai.us/wiki_pages/bouncing_ass), etc), [ass ripple](https://danbooru.donmai.us/wiki_pages/ass_ripple), [sweatdrop,](https://danbooru.donmai.us/wiki_pages/sweatdrop) [trembling,](https://danbooru.donmai.us/wiki_pages/trembling) [shaking,](https://danbooru.donmai.us/wiki_pages/shaking) [twitching,](https://danbooru.donmai.us/wiki_pages/twitching) sound effects, [sparkling sweat](https://danbooru.donmai.us/wiki_pages/sparkling_sweat)
    
-   **Iluminação** [compos.light]: [backlighting,](https://danbooru.donmai.us/wiki_pages/backlighting) dim light, rim light, [dusk,](https://danbooru.donmai.us/wiki_pages/dusk) [sunset,](https://danbooru.donmai.us/wiki_pages/sunset) [dawn,](https://danbooru.donmai.us/wiki_pages/dawn) [sunrise,](https://danbooru.donmai.us/wiki_pages/sunrise) [light particles,](https://danbooru.donmai.us/posts?tags=light_particles) [light rays,](https://danbooru.donmai.us/posts?tags=light_rays) [sunlight,](https://danbooru.donmai.us/wiki_pages/sunlight) [dappled sunlight,](https://danbooru.donmai.us/posts?tags=dappled_sunlight) [tree shade,](https://danbooru.donmai.us/wiki_pages/tree_shade) [crack of light](https://danbooru.donmai.us/posts?tags=crack_of_light)
    
-   **Quente** [body.posture]: [sweat,](https://danbooru.donmai.us/wiki_pages/sweat) sweating, [](https://safebooru.donmai.us/wiki_pages/sweat)[very sweaty,](https://danbooru.donmai.us/wiki_pages/very_sweaty) [](https://safebooru.donmai.us/wiki_pages/very_sweaty)[sweating profusely](https://danbooru.donmai.us/wiki_pages/sweating_profusely)[,](https://safebooru.donmai.us/wiki_pages/very_sweaty) [sweatdrop,](https://danbooru.donmai.us/wiki_pages/sweatdrop) [wet](https://danbooru.donmai.us/wiki_pages/wet) (+[clothes](https://danbooru.donmai.us/wiki_pages/wet_clothes),[hair](https://danbooru.donmai.us/wiki_pages/wet_hair), etc), [sweat clothes](https://danbooru.donmai.us/wiki_pages/sweaty_clothes), [hot](https://danbooru.donmai.us/wiki_pages/hot), [heat stroke](https://danbooru.donmai.us/wiki_pages/heat_stroke), [blush](https://danbooru.donmai.us/wiki_pages/blush), [full-face blush](https://danbooru.donmai.us/wiki_pages/full-face_blush), [body blush](https://danbooru.donmai.us/wiki_pages/body_blush), [breath](https://danbooru.donmai.us/wiki_pages/breath), [heavy breathing](https://danbooru.donmai.us/wiki_pages/heavy_breathing), [steaming body](https://danbooru.donmai.us/wiki_pages/steaming_body),
    
-   **Gangbang** [subject]: [multiple boys](https://danbooru.donmai.us/wiki_pages/multiple_boys)/[girls](https://danbooru.donmai.us/wiki_pages/multiple_girls), [group sex,](https://danbooru.donmai.us/wiki_pages/group_sex) [gangbang,](https://danbooru.donmai.us/posts?tags=gangbang) [orgy,](https://danbooru.donmai.us/wiki_pages/orgy) [dogpile,](https://danbooru.donmai.us/posts?tags=dogpile) [](https://danbooru.donmai.us/wiki_pages/group_sex)[love train,](https://danbooru.donmai.us/posts?tags=love_train) [threesome,](https://danbooru.donmai.us/wiki_pages/threesome)  
    _Obs.: precisa exata quantidade com_ **_x_**_boys/_**_x_**_girls._
    

## Sujeito

-   **Demônio** [subject],[body]: [demon](https://danbooru.donmai.us/wiki_pages/demon), [demon girl](https://danbooru.donmai.us/wiki_pages/demon_girl), [horns](https://danbooru.donmai.us/wiki_pages/horns) ([demon,](https://danbooru.donmai.us/wiki_pages/demon_horns) [dragon,](https://danbooru.donmai.us/wiki_pages/dragon_horns) [curled,](https://danbooru.donmai.us/wiki_pages/curled_horns) [cow,](https://danbooru.donmai.us/wiki_pages/cow_horns) etc), [tail](https://danbooru.donmai.us/wiki_pages/tail) ([demon,](https://danbooru.donmai.us/wiki_pages/demon_tail) [raised,](https://danbooru.donmai.us/wiki_pages/tail_raised) [](https://danbooru.donmai.us/wiki_pages/demon_tail)etc), [colored skin](https://danbooru.donmai.us/wiki_pages/colored_skin) ([red,](https://danbooru.donmai.us/wiki_pages/red_skin) [black,](https://danbooru.donmai.us/wiki_pages/black_skin) etc.)
    
-   **Goblin** [subject],[body]: [goblin](https://danbooru.donmai.us/wiki_pages/goblin), [female goblin](https://danbooru.donmai.us/wiki_pages/female_goblin), [green skin](https://danbooru.donmai.us/wiki_pages/green_skin), [pointy ears](https://danbooru.donmai.us/wiki_pages/pointy_ears), [fangs](https://danbooru.donmai.us/wiki_pages/fangs)
    
-   **Rosto com Sorriso de Lado** [body.express]**:** [smirk](https://danbooru.donmai.us/wiki_pages/smirk), [smug](https://danbooru.donmai.us/wiki_pages/smug), [frown](https://danbooru.donmai.us/wiki_pages/frown), [doyagao](https://danbooru.donmai.us/wiki_pages/doyagao),
    
-   **Yakuza** [subject],[body]: [yakuza](https://danbooru.donmai.us/wiki_pages/yakuza), [tattoo](https://danbooru.donmai.us/wiki_pages/tattoo) ([irezumi](https://danbooru.donmai.us/wiki_pages/irezumi), [full-body](https://danbooru.donmai.us/wiki_pages/full-body_tattoo), etc.), [piercing](https://danbooru.donmai.us/posts?tags=piercing) ([nose,](https://danbooru.donmai.us/wiki_pages/nose_piercing) [navel,](https://danbooru.donmai.us/wiki_pages/navel_piercing) [nipple](https://danbooru.donmai.us/wiki_pages/nipple_piercing), etc), [makeup](https://danbooru.donmai.us/posts?tags=makeup)

---

# Comentários
<p>
</p>

> [**metalife**](https://civitai.com/user/metalife)
Super útil, muito obrigado. Seria super incrível se houvesse um gerador de prompts decente com imagens de exemplo para cada composição, tema, corpo, pose, etc...

>> [**taches - OP**](https://civitai.com/user/taches)
Obrigado! Eu conheço alguns geradores de prompts online, mas não de acordo com as tags do Danbooru. Seria bom, de fato!

>> [**JayNailer**](https://civitai.com/user/JayNailer)
Confira o site perchance para geradores. Existem toneladas de geradores já existentes para praticamente todos os aspectos que você pode imaginar. Você também pode configurar seus próprios geradores de prompts muito (!) facilmente, por exemplo, conforme descrito acima.

---

> [**shargoths**](https://civitai.com/user/shargoths)
Algo que percebi ao trabalhar com múltiplos personagens é que a colocação importa, então você não pode seguir exatamente a sua estrutura (suposição, ainda não tentei isso, apenas com base no meu pouco conhecimento). Além disso, seria bem legal se você adicionasse alguns exemplos de prompts, não precisa ser super detalhado, mas nos daria um exemplo para seguir a estrutura e ver como ficaria na prática. Seguindo seu guia, acho que um exemplo poderia ser (este é um prompt ingênuo, dando-lhe uma liberdade decente)
>
> score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up, BREAK 1girl, moon BREAK dancing in the moonlight BREAK lean, tall, long hair, dress, BREAK forest

>> [**taches - OP**](https://civitai.com/user/taches)
Obrigado pelo feedback. Eu ainda não tentei com múltiplos personagens por enquanto. Acho que é difícil de conseguir sem inpainting, então a estrutura deve funcionar nesse caso? Vou fazer alguns testes em breve!
Boa ideia sobre os exemplos, vou trabalhar nisso e adicionar ao artigo. Estou usando o ComfyUI, então não acho que as palavras-chave BREAK funcionem automaticamente.

>>> [**shargoths**](https://civitai.com/user/shargoths)
Também não sei se BREAK funciona, gostaria de poder dizer; já vi prompts usando {} para separar as coisas também, mas estou ainda menos certo sobre como isso funciona. Ainda sou novo no ComfyUI e tentando (e falhando) adicionar um refiner ao regional prompting. Normalmente, tento fazer imagens complexas para ter uma ideia melhor de como o modelo é treinado e gosto do desafio de conseguir uma boa imagem fazendo o que quero. Espero que o exemplo de prompt que usei esteja na esfera de como você moldaria o seu.

---

> [**qinop**](https://civitai.com/user/qinop)
Você teria algum conselho para gerar de forma confiável um número específico de personagens?


>> [**taches - OP**](https://civitai.com/user/taches)
Com tags do Danbooru, você tem "multiple girls" ou "multiple boys", e pode ser um pouco mais preciso com 1girl, 2girls, 3girls / 1boy, 2boys, 3boys, etc. Eu nunca tentei fazer múltiplos personagens (...ainda!), mas acho que usaria inpainting para um melhor controle.

>>> [**shargoths**](https://civitai.com/user/shargoths)
Inpainting pode dar o controle que você deseja para múltiplos personagens, Regional prompting também pode fazer maravilhas. Quando você está trabalhando com múltiplos personagens, a IA normalmente confunde o que cada personagem deve estar fazendo.
>>> 
>>> Para um exemplo de prompt: 1girl, long hair, white hair, dark skin, handjob, cum on face, after sex BREAK 1girl, short hair, white skin, blowjob, cum from mouth BREAK 1boy, penis,
>>> 
>>> edit: porque não tenho certeza sobre o prompt acima mais, já faz muito tempo, uma versão melhor seria algo assim
>>> 
>>> `score_9, score_8_up, score_7_up, 1boy, 2girls, human male, human female, large breasts, small breasts, dark skin, white skin, BREAK dark skin, long hair, large breasts, handjob, black hair BREAK human female, white skin, long hair, white hair, small breasts, blowjob, mouth closed, BREAK human male, large cock,`
>>> 
>>> isso ainda tem o problema da confusão abaixo, mas é decente em teoria para mim.
>>> 
>>> (para mim) a imagem normalmente dará a ambas as garotas um rosto de sexo oral, o esperma normalmente estará nos rostos de ambas, e seus tons de cabelo/pele muitas vezes trocam. Estou escrevendo um artigo falando sobre múltiplos personagens e prompts complexos, mas ainda não estou onde quero estar nos meus próprios prompts.
>>> 
>>> Se você estiver tentando trabalhar com dois personagens diferentes usando LoRA's, o problema acima também acontecerá. Vai gerar eles nas roupas, penteados, cor da pele um do outro. Em geral, para mim, eu evito isso, mas estou prestes a tentar fazer funcionar. A IA é um jogo de números, especialmente se não houver aprendizado e atualização dos pesos.

>>>> [**jaimuh731354**](https://civitai.com/user/jaimuh731354)
tentei seu exemplo de prompt, mas não consigo 2 garotas. Não entendo porque às vezes é tão difícil conseguir 2 personagens, mesmo se eu colocar "1girl, 1boy" no começo do prompt

>>>>> [**shargoths**](https://civitai.com/user/shargoths)
Algumas coisas que quero realmente abordar porque o exemplo de prompt acima eu não testei e era um exemplo, já que no momento em que escrevi este post não tinha exemplos (ou estou pensando em outro post). Mas para responder sua pergunta especificamente, você está procurando por um dos múltiplos tokens. Para gerar múltiplas garotas, você pode usar os seguintes tokens, "#girls" (#=quantidade de número, então 2girls, 3girls, 4girls,) ou "multiple girls".
>>>>> 
>>>>> Ainda estou aprendendo sobre prompting, com tentativa e erro e olhando para as imagens de outros. Vou atualizar meu post original quando realmente tiver uma boa metodologia para prompts de múltiplas pessoas e cenas mais complexas.
>>>>> 
>>>>> Agora, gerando cenas mais complexas (embora não exatamente como acima, algo semelhante porque achei que lembrava meu prompt original e não lembrava e não estou mudando) o que estou olhando em termos de prompts é
>>>>> 
>>>>> score_9, score_8_up, score_7_up, 1boy, 2girls, human male, human female, large breasts, small breasts, dark skin, white skin, BREAK dark skin, long hair, large breasts, handjob, black hair BREAK human female, white skin, long hair, white hair, small breasts, blowjob, mouth closed, BREAK human male, large cock,
>>>>> 
>>>>> Minha teoria de trabalho é que "score_9, score_8_up, score_7_up, 1boy, 2girls, human male, human female, large breasts, small breasts, dark skin, white skin," diz à IA que você quer isso na imagem de alguma forma, usando BREAK eu digo à IA como eu especificamente quero que gere a imagem, embora nem sempre funcione assim. Vou tentar resolver os problemas com o tempo.

>>>>>> [**jaimuh731354**](https://civitai.com/user/jaimuh731354)
acho que o SD3 vai resolver todos esses problemas, mas obrigado pela resposta

---

> [**2018cfh**](https://civitai.com/user/2018cfh)
Este é um artigo fantástico e eu copiei o formato para o bloco de Obs.s para poder brincar adicionando meus próprios "preencher aqui", as tags vinculadas funcionam apenas com pôneis ou são bastante universais?

>> [**taches - OP**](https://civitai.com/user/taches)
Obrigado pelo seu comentário, que bom que gostou! As tags estão vinculadas ao site Danbooru, que muitos modelos diferentes usam para prompts, especialmente os focados em anime/irrealistas. Como regra geral, sempre verifique a descrição dos checkpoints e LoRA para ver os conselhos do autor sobre como usar o modelo deles :-)

>>> [**2018cfh**](https://civitai.com/user/2018cfh)
Obrigado! Tentei algumas tags para realismo e uma boa parte parece funcionar para roupas e posturas. Coisas obscuras nem tanto.🙃

---

> [**civilai003**](https://civitai.com/user/civilai003)
ah entendi

---
