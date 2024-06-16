# [**O que é score_9 e como usá-lo no Pony Diffusion**](https://civitai.com/articles/4248/what-is-score9-and-how-to-use-it-in-pony-diffusion)


![O que é score_9 e como usá-lo no Pony Diffusion](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/8b438cad-f16d-4d88-8247-7a0cb4b079b0/width=400/00082-1496646277.jpeg)


-Autor: [**PurpleSmartAI**](https://civitai.com/user/PurpleSmartAI)

---

<p></p>

**Interessado na próxima versão do Pony Diffusion? Leia a atualização aqui:**
[https://civitai.com/articles/5069/towards-pony-diffusion-v7](https://civitai.com/articles/5069/towards-pony-diffusion-v7)

Você pode ter visto `score_9` ou sua versão mais longa `score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up` usados em prompts para o Pony Diffusion, vamos explorar o que é essa tag, como ela surgiu e como usá-la corretamente para gerar imagens melhores.

## Por que?

O ciclo de vida (simplificado) de um modelo de IA consiste em duas etapas - Treinamento e Inferência.

Durante o Treinamento, um modelo que ou não sabe fazer nada (treinamento do zero) ou não tem conhecimento específico (finetuning) é repetidamente alimentado com pares de imagem-legenda, ensinando-lhe conceitos que fazem sentido da perspectiva dos humanos. Este é um processo longo e, para o Pony Diffusion V6, levou cerca de 3 meses em um hardware muito potente.

Quando o modelo está pronto, começamos a usá-lo para gerar imagens, isso é chamado de Inferência.

Existem vários desafios que precisamos superar para realmente conseguir gerar algo bonito.

Os computadores não entendem o conceito de "bonito" e as imagens geradas durante a Inferência geralmente correspondem (em qualidade) às observadas durante o Treinamento (pessoas inteligentes chamam isso de [GIGO](https://en.wikipedia.org/wiki/Garbage_in,_garbage_out)).

Uma opção óbvia (e infelizmente ingênua) é treinar modelos apenas com _dados bons_. Primeiro de tudo, muitos conceitos (por exemplo, personagens, objetos, ações) podem não ter dados bons suficientes para ajudar o modelo a aprender. Em segundo lugar, ainda não sabemos como separar _dados bons_ de _dados ruins_ em nosso banco de dados de imagens-fonte.

Se quisermos um modelo diversificado (em outras palavras, um modelo que conhece muitos personagens e outros conceitos), precisamos pegar o máximo de dados que pudermos. Mas também, não muito, pois com mais dados vem mais tempo de treinamento e mais dinheiro gasto.

Portanto, precisamos de uma maneira de encontrar o subconjunto de bons dados em todos os dados disponíveis para nós e depois classificar as imagens dentro desse conjunto de dados.

## Ensinando as máquinas a saber o que é bom

Felizmente para nós, temos maneiras de ensinar às máquinas o que é considerado bonito pelos humanos. Existem muitas maneiras de fazer isso, mas a PSAI está usando algo chamado "ranking estético baseado em CLIP".

[CLIP](https://en.wikipedia.org/wiki/DALL-E#:~:text=model.%5B24%5D-,Contrastive%20Language%2DImage%20Pre%2Dtraining%20(CLIP),-%5Bedit%5D) ou Contrastive Language-Image Pre-training é uma maneira de emparelhar imagens e legendas correspondentes. Simplificando, é outro modelo de IA treinado em um grande conjunto de dados de legendas criadas por humanos e é capaz de aceitar tanto imagens quanto textos, medindo o quanto eles se correlacionam entre si. Além de aprender sobre coisas como "cachorro" ou "gato", o CLIP também aprende sobre conceitos como "obra-prima", "melhor qualidade" ou "hd" (já que essas palavras são comuns em legendas de imagens criadas por humanos).

Se você já usou o Stable Diffusion com outros modelos, pode ter usado tais palavras-chave/tags para melhorar a qualidade de suas gerações.

Então, por que não pegar o CLIP e usá-lo em todos os lugares se ele é tão bom em medir quais imagens correspondem a "obra-prima" e outras tags semelhantes? Bem, novamente temos desafios a superar.

O CLIP foi treinado com um pouco de tudo e a qualidade das legendas usadas é um tanto limitada, o que significa que o CLIP não é muito bom em conteúdo não fotorrealista e um pouco menos popular, como pôneis ou personagens de desenhos animados furry (e funciona muito melhor em Anime).

Mas, ainda podemos usar o CLIP, pois em seu interior contém muitos sinais que podem não ter necessariamente um nome bom associado a eles, mas se pudermos revelá-los, podemos usá-los para separar boas imagens das ruins.

## Entrando no inferno da rotulagem de dados

Para implementar nosso plano, ainda precisamos de muitas imagens boas (mas também de muitas não tão boas e algumas muito ruins). Como podemos conseguir algumas? Bem, por uma vez, podemos olhar para várias pontuações/classificações atribuídas a elas em boorus populares para escolher algumas imagens.

Neste ponto, você pode dizer - "Ei, espera um minuto. Você já tem as pontuações! Basta usá-las para escolher boas imagens!" e você estará parcialmente certo. Alguns modelos (incluindo os primeiros do Pony Diffusion) usaram esses metadados de pontuação.

Infelizmente, usar pontuações introduz dois problemas - os usuários classificam as imagens com base tanto na qualidade quanto no conteúdo, e embora geralmente estejam correlacionados, há alguns vieses, como conteúdo NSFW sendo classificado mais alto ou personagens específicos recebendo tratamento preferencial independentemente da qualidade. Além disso, essas pontuações são afetadas pela idade da imagem e não correspondem entre diferentes fontes de metadados (por exemplo, uma pontuação 100 em um site pode ser o top 1%, enquanto em outro é uma pontuação média).

Então, pelo menos podemos usar as pontuações para escolher uma distribuição decente de imagens, agora vamos analisá-las e classificá-las manualmente em termos de qualidade. Eu pessoalmente decidi usar uma escala de 1 a 5 pontos. Ainda restam duas perguntas - quantas imagens precisamos e quem irá classificá-las.

Precisamos de muitas imagens, queremos ter um número decente de imagens de cada "tipo", algumas 3D, alguns esboços, algumas semi-realistas, etc... Se faltar algum estilo, o modelo não aprenderá a julgá-los corretamente.

No caso do V6, esse número foi de ~20k imagens rotuladas manualmente. Agora, precisamos de alguém que possa olhar para as imagens e usar suas habilidades de crítica de arte para julgá-las na escala que inventamos. E quem é essa pessoa imparcial, isenta e neutra, capaz de tomar decisões ou julgamentos com base em critérios objetivos em vez de sentimentos pessoais, interesses ou preconceitos? Sou eu, obviamente. Então, depois de passar semanas na caverna de rotulagem de dados classificando meticulosamente cada imagem, consegui gerar nosso conjunto de dados estéticos grande o suficiente para ser útil.

Agora podemos treinar um novo modelo que pegaria a representação de imagem do CLIP (que chamamos de embedding) e uma avaliação humana e aprenderia com eles como classificar novas imagens. Em seguida, usamos esse modelo nas embeddings de cada imagem que encontramos e obtemos uma classificação de 1 a 5 (que na verdade agora é uma classificação de 0 a 1, já que os computadores gostam mais desse intervalo).

Agora resolvemos dois grandes problemas, primeiro de tudo, podemos usar este novo modelo para selecionar apenas um conjunto de imagens para treinar o novo modelo e anotar as legendas com uma tag especial.

Então, as melhores imagens recebem uma legenda como `score_9, um pônei fofo` e imagens um pouco menos boas `score_8, talvez não tão fofo pônei`.

## É hora de Treinamento

Agora temos dados anotados e finalmente podemos treinar o Pony Diffusion real. Vamos continuar mostrando ao modelo imagens e nossas legendas contendo as tags de pontuação para que ele aprenda quais tags de pontuação correspondem a boas imagens, nos dando uma versão mais controlável de "obra-prima" e amigos.

Mas espere, acabou que eu errei um pouco! O que descrevi acima é como o PD V5.X costumava fazer as coisas, no V6 eu queria também poder dizer - "ei, me dê qualquer coisa 80% boa e acima". Mas a tag `score_8` só nos daria imagens na faixa de 80% a 90%. Talvez usar tanto `score_8` quanto `score_9` funcionasse, mas eu queria verificar isso, então mudei as etiquetas de simples `score_9` para algo mais verboso como `score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up` e `score_8` para `score_8, score_7_up, score_6_up, score_5_up, score_4_up`. Na realidade, me expus a uma variação do efeito

 [Hans Esperto](https://en.wikipedia.org/wiki/Clever_Hans), onde o modelo aprendeu que toda a longa string correlaciona com as imagens "bonitas", em vez de partes separadas dela. Infelizmente, quando percebi isso, já estávamos muito além do ponto médio do treinamento, então eu apenas continuei com isso (tentei usar tags mais curtas após a descoberta, mas devido à forma como treinamos, não teve um efeito tão forte).

Vou corrigir isso no V7.

tldr: _usamos um modelo treinado nas preferências humanas para rotular todos os dados com tags especiais e depois treinamos um modelo de texto para imagem nessas etiquetas, permitindo-nos pedir ao modelo por imagens "boas" via uso dessas tags_.

## Eu preciso me preocupar?

Talvez... em alguns casos.

Algumas ferramentas, como o bot do PSAI no Discord, adicionam automaticamente as tags `score_9, ...`, mas em outras interfaces você precisará adicionar a versão longa você mesmo. Em algumas interfaces (Auto1111) você pode salvar como um estilo, em outras basta copiar e colar no início de todos os seus prompts.

As tags de pontuação têm alguns vieses nelas, então se você estiver usando LoRAs de estilo/artista, às vezes faz sentido excluir as tags e ver como o modelo reage. Não tenho uma boa recomendação sobre usá-las no treinamento, pois não faço LoRAs por conta própria.

Ah, e uma nota importante - essas tags são menos úteis em negativos, pois você só pode ir até 4 e, idealmente, deveríamos treinar na qualidade até 1, mas isso torna o treinamento significativamente mais caro. Então, você ainda pode usar score\_4/5 no negativo, mas eles não vão te afastar das imagens _realmente ruins_.


## Comentários


>[**Rating_Agent**](https://civitai.com/user/Rating_Agent)
Obrigado, eu não entendi

>> [**NoobFromEgypt**](https://civitai.com/user/NoobFromEgypt)
    Acho que precisamos continuar digitando score_9, score_8_up, score_7_up, score_6_up, no nosso prompt 🤷🏻‍♂️ e score_4/5 não têm muito efeito nos negativos 🤷🏻‍♂️ ou estou completamente errado 🤷🏻‍♂️🤣

>>> [**ABSDE**](https://civitai.com/user/ABSDE)
        Você precisa de todos os prompts para o próprio pônei devido a um erro de treinamento. Eles estão planejando fazer um em que apenas Score_9_up seja suficiente. Não tenho certeza sobre outros derivados de Pônei.

>>> [**NoobFromEgypt**](https://civitai.com/user/NoobFromEgypt)
        Obrigado

>>> [**newtypezx177**](https://civitai.com/user/newtypezx177)
        Quais são? Tentando entender

>>>> [**ABSDE**](https://civitai.com/user/ABSDE)
            score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up,


>>> [**sci**](https://civitai.com/user/sci)
        Insígnia de Primeiro Aniversário
        Em resumo, o modelo aprendeu que as imagens ficam boas quando têm todas as tags de pontuação em uma ordem específica, em vez da intenção de "ter 1 dessas tags de pontuação".

>>>> [**felicityc**](https://civitai.com/user/felicityc)
            Isso é hilário

>>> [**StefanEtienne**](https://civitai.com/user/StefanEtienne)
        Sim, eu também não entendi muito bem. Quer dizer, entendi que você adiciona isso lá. Mas não entendo o "porquê".

---

>[**Ailight**](https://civitai.com/user/Ailight)
Obrigado pelo post! Uma pergunta, como o V7 vai corrigir isso? Eliminando o score_x_up e usando apenas score_x?

>> **@PurpleSmartAI (OP)**
    Sim, teremos apenas score_9, da mesma forma que V5 e V5.5 usavam. Também estou explorando opções de DPO, mas honestamente gosto mais da ideia de pontuação. Também há a possibilidade de que o controle de estilo do V7 torne as tags de pontuação geralmente irrelevantes, mas ainda não tenho certeza.

>>> [**MoonRide**](https://civitai.com/user/MoonRide)
        Por que você não faz alta qualidade (digamos 8+ de 10) a opção padrão sem precisar especificar prompts personalizados, e só fazer pontuação mais baixa opcional para quem quer qualidade de saída mais baixa? É assim que outros modelos geralmente funcionam, e tenho certeza de que a maioria das pessoas gostaria de ter imagens bonitas sem ter que conhecer e usar prompts personalizados específicos para um único modelo. Ou, alternativamente, apenas manter as tags de qualidade populares, como obra-prima, melhor qualidade, alta qualidade, etc.?

>>> [**Dokitai**](https://civitai.com/user/Dokitai)
        Pergunta total de novato aqui, o que aconteceria se você treinasse com duplicatas da mesma imagem, mas com tags diferentes? Isso permitiria acionar um resultado semelhante com tags diferentes?
        tipo você tenta com score_9 e 8 etc?

---

>[**AntUnderboot**](https://civitai.com/user/AntUnderboot)
O que aconteceria se invertêssemos a contagem regressiva, ou seja: score_4, score_5_up, score_6_up, score_7_up, score_8_up, score_9_up? Estou apenas tentando entender o significado, obrigado.

>> **@PurpleSmartAI] (OP)**
    Pode ter algum efeito, mas não tão forte quanto o que começa com score_9, pois o modelo aprendeu uma ordem específica das tags.

>>> [**AntUnderboot**](https://civitai.com/user/AntUnderboot)
        Eu tentei na noite passada e obtive quase o mesmo efeito. Então agora estou experimentando outras ordens de sequência como score_A, score_B_up, score_C_up, score_D_up, score_E_up, score_F_, ou score_19, score_18_up, score_17_up, score_16_up, score_15_up, score_14_, só para ver o que acontece.

>>>> [**alexclerick**](https://civitai.com/user/alexclerick)
            Eu não li como deveria ser lido, mas tenho quase certeza de que essas tags são apenas tags de qualidade como 8k etc. em outros modelos, então não haverá nenhuma informação no modelo para a tag "score_19".

---

>[**duskfallcrew**](https://civitai.com/user/duskfallcrew)
Ei, estou feliz que isso exista, finalmente li tudo pela quarta vez agora que usei o modelo finalmente - e é bom ver um modelo que pode realmente tolerar minha arte. Obrigado pelo seu trabalho árduo!

---

>[**Tetsuoo**](https://civitai.com/user/Tetsuoo)
Obrigado pelas explicações, está mais claro na minha mente agora. Tenho explorado o treinamento de Lora com Pony há alguns dias e, por fim, adicionei as pontuações nas legendas e não estou satisfeito com os resultados. Parece... menos interessante, muito limpo e não segue o estilo que estou treinando tanto quanto a tentativa anterior. Ou talvez usar subconjuntos não tenha sido uma boa ideia, não tenho certeza, em qualquer caso, vou tentar novamente sem as pontuações... será a 11ª tentativa xDD
Para aqueles interessados em mais estilos para Pony, postei uma versão final aqui: NeoArtCorE-Lora - v3C_PonyXL | Stable Diffusion LoRA | Civitai

>> [**PetroUA**](https://civitai.com/user/PetroUA)
    Por favor, escreva um artigo sobre o treinamento de Lora para Pony 🙏😿😱

---

>[**Papu_95**](https://civitai.com/user/Papu_95)
Uau! Obrigado por todas as explicações. É meio que o que acontece quando tentamos fazer coisas muito complexas e inteligentes 😅
Obrigado por aprender a lição por nós com sua experiência... e pela paciência de rotular todas aquelas imagens.

---

>[**utahmyers937**](https://civitai.com/user/utahmyers937)
Isso significa que devemos ter score_5, score_4 no negativo para um melhor resultado? Temos toda a string no positivo? É legal saber como isso foi bagunçado, mas eu adoraria saber como usar esse sistema para as melhores imagens.

>> [**Tetsuoo**](https://civitai.com/user/Tetsuoo)
    Não sei por que as pessoas fazem isso. Eu não tentei, é como não mencionar algo no prompt, mas adicionar no negativo, geralmente você faria isso quando está desesperadamente tentando remover algo específico. Mas pelo que eu entendo, pontuações mais baixas são para saídas de qualidade inferior, se você não as quiser, não as adicione, deve ser tão simples quanto isso, certo?

---

>[**g0t0s0da**](https://civitai.com/user/g0t0s0da)
Aquele momento estranho quando um modelo de checkpoint focado em furry (e extremamente bem-feito) entrou em cena e mudou todo o jogo.

>> [**fr34ky**](https://civitai.com/user/fr34ky)
    Provavelmente é para isso que a arte de IA foi inicialmente destinada, então faz total sentido para mim 🤣

---

>[**rare193919**](https://civitai.com/user/rare193919)
então é tipo score_9 a score_7 como melhor qualidade, alta qualidade e usado em boas amostras e então score 5, score4 são como pior qualidade, baixa qualidade e usados principalmente em ruins? (eu não li tudo 😂)

---

>[**moraesnil89**](https://civitai.com/user/moraesnil89)
Em resumo, ele escreveu um livro, mas não conseguiu explicar de maneira simples.

>> [**robot1me**](https://civitai.com/user/robot1me)
    A versão TL;DR é que, por enquanto, você tem que colocar toda a string score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up para melhor qualidade, e que para o próximo v7 funcionará usar as tags individualmente.

---

>[**null00303003**](https://civitai.com/user/null00303003)
Ok, mas algum dia teremos um post com aquela imagem matadora da Undyne que você usou no cabeçalho? 🥺💦

---

>[**mr_Jack**](https://civitai.com/user/mr_Jack)
Obrigado por me dar esse conhecimento, especialmente [Hans Esperto]

---

>[**AbstractPhila**](https://civitai.com/user/AbstractPhila)
Alguma atualização sobre o treinamento do V7?

---

>[**pheromon321284**](https://civitai.com/user/pheromon321284)
Obrigado pela explicação. Permita-me expressar minha opinião, você deve deixar como está agora. Isso dá aos usuários mais liberdade e possibilidades de criação de imagens flexíveis.

---

>[**A1322**](https://civitai.com/user/A1322)
O fato de que o Pony V6 sofreu um erro clerical tão grande durante o treinamento e ainda assim é muito mais responsivo e utilizável do que o SDXL original, a ponto de suplantá-lo, é tanto engraçado quanto deprimente.
>
> Tanto dinheiro em Big Tech, mas eles não são nada sem o trabalho não remunerado de seus usuários.

---

>[**Erquint**](https://civitai.com/user/Erquint)
> Aqui está minha interpretação:
>
> O autor tentou treinar o modelo com base em pontuações. Foi longe demais com uma ideia genuinamente boa de como melhorar isso e acabou prejudicando toda a compreensão de pontuação do modelo, fazendo-o produzir lixo, a menos que você comece seu prompt com, literalmente, a seguinte string inteira: "score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up", sem aspas.
Não, você não deve limitá-lo apenas aos números correspondentes às pontuações altas. Novamente, a pontuação não funciona nesta versão como pretendido e o autor admite que quebrou completamente. Se você tiver "score_9", mas não tiver "score_8_up, score_7_up, score_6_up, score_5_up, score_4_up" — o modelo provavelmente interpretará o prompt de forma errada e servirá lixo anormal.
Você deve ser capaz de controlar o limite superior da qualidade, se você quiser imagens não as melhores a serem produzidas, por exemplo: "score_8, score_7_up, score_6_up, score_5_up, score_4_up".
Não, "score_7_up" não transmite ao modelo uma faixa de pontuação de 7 a 9. Isso é o que o autor tentou fazer o modelo entender, mas não é assim que ele acabou aprendendo.
Não, não há pontuações abaixo de 4 em que o modelo foi treinado e você não deve usar pontuações de 1 a 3 no prompt negativo. O uso de qualquer pontuação no prompt negativo é duvidoso.
> 
> A conclusão mais importante é que você deve usar toda a string longa para as classificações.
Deixe-me ilustrar qual sequência de tags corresponde a cada pontuação que você deseja referir:
> 9. "score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up"
> 8. "score_8, score_7_up, score_6_up, score_5_up, score_4_up"
> 7. "score_7, score_6_up, score_5_up, score_4_up"
> 6. "score_6, score_5_up, score_4_up"
> 5. "score_5, score_4_up"
> 4. "score_4"
> 
> Essa foi minha interpretação do artigo.
E aqui estão minhas especulações extrapoladas dessa interpretação:
> 
> Como as tags "_up" se sobrepõem entre todas elas, e "score_4_up" é compartilhada por todas, exceto 4 — usar qualquer uma dessas tags no prompt negativo provavelmente levará a resultados duvidosos.
Eu suspeito, no entanto, que "score_4" funcionará no prompt negativo, pois não se sobrepõe a nada e não segue suas próprias tags "_up".
Quanto a "score_5" e acima no prompt negativo — tais tags não-"_up" poderiam talvez funcionar no negativo, já que não se sobrepõem, mas cada uma delas é tecnicamente falha sem suas tags "_up" subsequentes, então isso é um assunto nebuloso…
> 
> Eu também suspeito que combinar pontuações para permitir que o modelo tenha mais espaço para criatividade pode ser possível combinando tags de pontuação não-"_up" de uma dessas maneiras estranhas, por exemplo, de 8 a 9:
> - "score_9, score_8, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up"
> - "score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up, score_8"
> - "score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up, score_8, score_7_up, score_6_up, score_5_up, score_4_up"
Temo que todos esses terão alguma estranheza quanto à ordem da sequência não ser exatamente o esperado, então novamente: assunto nebuloso…
> 
> Essas foram minhas especulações extrapoladas dessa interpretação.
> 
> Eu já tentei PDXL antes de ler este artigo, limitando tags a 9 e 8, como faria sentido se a ideia de aprimoramento do autor realmente funcionasse em vez de dar errado para ele, e tive dificuldades horríveis. Deixei uma avaliação negativa do modelo com base na minha experiência de trabalho com ele.
> A explicação do grande defeito de design e o método de contorná-lo descrito no artigo faz sentido e vou tentar colocar esse conselho em prática mais tarde.
> 
> P.S.
> 
> Testei o prompt positivo "score_9, score_8_up, score_7_up, score_6_up, score_5_up, score_4_up" e ainda estou muito insatisfeito com a clareza das representações e a aderência ao prompt.
> 
> Testei o prompt negativo "score_4" e realmente resulta em elementos ligeiramente mais detalhados, ricos e confiantes na média.

> > [**AbstractPhila**](https://civitai.com/user/AbstractPhila)
> > Eu fiz um benchmark disso. Meus dados são um pouco ad hoc, então provavelmente não serão tão úteis quanto um conjunto de dados totalmente mapeado, mas definitivamente mostraram alguns vieses em relação ao score_7_up e score_8_up sendo os verdadeiros pilares. score_9 não é uma peça ruim, mas não tem realmente as informações necessárias nem os detalhes que são introduzidos com os valores de pontuação anteriores, que parecem todos produzir conceitos mais avançados para as peças detalhadas mais intricadas pretendidas, enquanto a estrutura geral da imagem de alta definição e alta qualidade é preenchida com as variações mais altas da pontuação.
> > 
> > Mais testes são definitivamente necessários. É mais provável que você obtenha roupas e acessórios se incluir valores de pontuação mais baixos, mas isso não garante que as cores correspondam.
> > 
> > Tenho experimentado com este prompt aqui;
> > 
> > rating_safe, score_9, score_8_up, score_7_up, score_6_up, score_5_up,
> > 
> > [(fundo gradiente, fundo preto, fundo vermelho:3):
> > 
> > {PROMPT}
> > 
> > :0.05]
> > 
> > Isso produziu resultados muito interessantes quando configurado para 20 etapas, e ocasionalmente você pode até mapear as cores da imagem inteira apenas definindo vários tipos de fundo e puxando o tapete debaixo deles e descontinuando sua utilidade. Usar tags como 2koma também produz resultados interessantes quando descontinuadas. Às vezes você pode preencher facilmente a saída de múltiplos contextos, apenas usando uma única etapa poderosa 2koma.
> > 
> > Além disso; frequentemente ao usar isso no final do prompt;
> > 
> > [: 3d, fotorrealístico :1.5]
> > 
> > Resultará em uma imagem de muito maior detalhamento. 1.5 é upscale na metade, 0.5 seria escala normal na metade. Use seu melhor julgamento quanto a onde deve rodar, mas definitivamente ajuda muito se rodar perto do final, mas não durante todo o sistema. Mais tarde é melhor, pois mais cedo tende a impactar negativamente o resultado de maneiras muito negativas e deformadoras.

> > > [**tensorboyz**](https://civitai.com/user/tensorboyz)
> > > como esse prompt deve funcionar?

---

>[**RisRis**](https://civitai.com/user/RisRis)
> Por alguma razão, meu stable diffusion produz artefatos e leva muito tempo para criar uma imagem... Eu não entendo o que pode estar errado.

---

>[**Cinnamondrops**](https://civitai.com/user/Cinnamondrops)
Obrigado por todo seu trabalho duro e dedicação. Seu modelo e suas variantes são incríveis.

---

>[**Sac_Poubelle**](https://civitai.com/user/Sac_Poubelle)
Estou impressionado e grato por toda a rotulagem manual. Obrigado pela explicação.

---

>[**Ash_Love**](https://civitai.com/user/Ash_Love)
muito obrigado por isso

---

>[**EechiZero**](https://civitai.com/user/EechiZero)
preciso de um guia para iniciantes, qual seria a indicação para usar o estilo "anime screencap"? já que só me dá resultados super detalhados

---

>[**Romazeo**](https://civitai.com/user/Romazeo)
Então, eu preciso usar nos negativos score_6_up ou score_6 e etc? Quero dizer, "up" também é negativo?

---

>[**fox955**](https://civitai.com/user/fox955)
Diga-me, qual modelo de movimento pode ser usado para este modelo neural?

---

>[**deleted**](https://civitai.com/user/deleted)
Muito obrigado por esses detalhes, não sendo fã de geração de imagens, realmente apreciei sua explicação e, acima de tudo, vida longa ao Poney ;)

---

>[**Clydius**](https://civitai.com/user/Clydius)
Obrigado pela explicação e pelo trabalho árduo. As tags de pontuação fazem mais sentido para mim agora.

---

>[**trytensor**](https://civitai.com/user/trytensor)
Neste exemplo https://civitai.com/images/10436914 o que significa gpo, tdj, eqg, wpt, aur, hnj, bry, bfu, cle, cwn, eqg, ewu, bwl, bse? É invocado via embedding bdsqlsz? Não entendi.

---

>[**tpcdaz**](https://civitai.com/user/tpcdaz)
Modelo terrível. Estou surpreso que eles até permitiram essa porcaria quebrada aqui, não está praticamente dominando o site com todos os complementos necessários para tirar algo dele.

>> [**trytensor**](https://civitai.com/user/trytensor)
    Ele disse que há uma correção futura no v7, mas a pergunta é, quantas horas de GPU? todos os complementos devem ser v7 também, estou certo?

---

>[**serget2**](https://civitai.com/user/serget2)
Então, por que não usar apenas score7_up? Quero dizer, se você quer os 30% melhores, por que eu colocaria score9, score8_up antes de score7_up? Se score7_up também me daria score8's e score9's? Ou estou vendo isso errado?

>> [**TeKett**](https://civitai.com/user/TeKett)
    Como a legenda completa gera a imagem. A maneira como ela aprende é pegando a legenda e depois tentando desfazer o ruído na imagem de treinamento, e a menos que tenha a legenda completa, não pode fazer isso, pois não foi treinada para fazer isso. Acho que é uma falha na maneira como a IA opera, onde é o caminho na rede neural que gera a imagem e não os próprios nós. Pense nisso como vetores. O ponto final pode ser o mesmo, mas o caminho que leva a esse local é diferente. É da mesma forma que nós humanos lembramos das coisas, por associação.

>>> [**trytensor**](https://civitai.com/user/trytensor)
        Você precisa de 1girl, score9 1girl, score8_up 1girl ou apenas (1girl) após as linhas score_up seguidas por BREAK enter?

---

>[**jezartography**](https://civitai.com/user/jezartography)
Obrigado por todo o seu trabalho e tempo investido nisso, e realmente obrigado pela ótima explicação! :)

---

>[**StefanEtienne**](https://civitai.com/user/StefanEtienne)
.... o que?

---

>[**thaysmail930**](https://civitai.com/user/thaysmail930)
Obrigado!!

---

>[**TeKett**](https://civitai.com/user/TeKett)
Legal, sempre me perguntei como funciona a legendagem, bom obter uma clarificação de que a legenda completa é o que gera a imagem, pelo menos a melhor. Isso não significaria que você deveria duplicar suas imagens por cada token e cada combinação de tokens? como "1girl", "1girl vestindo um vestido" e "Asuka Langley Soryu vestindo um vestido" deveriam todas ser capazes de gerar a mesma imagem. Talvez isso seja uma falha na forma como a IA generativa funciona.

>> [**trytensor**](https://civitai.com/user/trytensor)
    Você precisa de 1girl, score9 1girl, score8_up 1girl ou apenas (1girl) após as linhas score_up seguidas por BREAK enter?