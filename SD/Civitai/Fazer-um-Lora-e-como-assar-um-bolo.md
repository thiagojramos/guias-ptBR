# [Fazer um Lora é como assar um bolo.](https://civitai.com/articles/138/making-a-lora-is-like-baking-a-cake)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/2ca7a840-f115-451d-b5d1-2dffb56c72a4/width=150/01749-3103463454-solo,%20standing,%20huge_breasts,%20masterpiece,%20best%20quality,%20detailed%20face,%20detailed%20eyes,%20highres,%20%20%20%20,%20tohsaka_rin_alt.jpeg)


criado: 19-06-2024
tags: `training guide` `lora training` `getting started` `lora` `howto`
autor: [knxo](https://civitai.com/user/knxo)

---

Primeiro de tudo, este guia foi originalmente criado por mim a pedido de um canal do Discord (ele mudou bastante desde então), também está postado no [rentry](https://rentry.org/42x68) (aquele está desatualizado). Este guia é relativamente pouco detalhado e é focado principalmente na criação de Lora/Lycoris de personagens, também é direcionado para treinamento em 512x512 e SD 1.5, minha placa de vídeo é muito fraca para treinar SDXL. Pergunte nos comentários se alguma parte precisar de esclarecimento. Tentarei responder (se eu souber a resposta :P) e adicionar ao guia. A criação de Lycoris segue todos os mesmos passos da criação de lora, mas difere na seleção dos parâmetros de treinamento, então as diferenças estarão na seção de "baking".

Edição (20240610): Experimentei com perda mascarada. Não é "melhor", mas parece ser uma ótima ferramenta para lidar com algumas situações irritantes como fundos repetitivos ou imagens bagunçadas. Infelizmente, você precisa de um certo tipo de imagem para que funcione melhor. Verifique a seção de "baking" para treinamento mascarado, se precisar, bem como a parte de criação de máscaras na seção de regularização do conjunto de dados.

## Fazer um Lora é como assar um bolo.

Muita preparação e depois deixar assar. Se você não fizer as preparações adequadas, provavelmente ficará intragável.

Tentei organizar o guia na mesma ordem dos passos necessários para treinar um LORA, mas se você estiver confuso sobre a ordem, tente a lista de verificação no final e depois procure os detalhes conforme necessário nas seções relevantes.  
Estou fazendo este guia para criação offline, você também pode usar o Google Colab, mas não tenho experiência com isso.

## Introdução

Uma boa maneira de visualizar o treinamento é como um conjunto de controles deslizantes ao criar um personagem em um jogo. Se movemos um controle, podemos aumentar o tamanho do nariz ou a redondeza dos olhos. Claro, eles são muito mais complexos; por exemplo, podemos supor que "olhos redondos" significa que os controles a=5, b=10 e c=15, então talvez olhos fechados signifiquem a=2, b=7, c=1 e d=8. Podemos considerar um modelo como apenas uma enorme coleção desses controles deslizantes.

Então, o que é um LORA? Quando pedimos algo "novo", o modelo apenas diz "O quê? Eu não tenho controles para isso!" É isso que um LORA é: nós apenas colamos alguns novos controles no modelo e ajustamos alguns dos já existentes para valores que preferimos.

Quando treinamos um lora, estamos basicamente gerando todos esses controles e seus valores. O que nossa legenda faz é dividir os controles resultantes entre cada uma das tags. O modelo faz isso por vários meios:

-   Experiência anterior: Por exemplo, se uma tag é um tipo de roupa usada por um humano na parte inferior do corpo, por exemplo, "saia", o modelo verificará o que sabe sobre saias e atribuirá o conjunto correspondente de controles a ela. Isso inclui cores, formas e localizações relativas.
    
-   Similaridade de palavras: Se a tag for, por exemplo, "terninho de saia", ele verificará os conceitos individuais e tentará interpolar. Esta é a principal causa de sobreposição de conceitos, na minha opinião.
    
-   Comparação: Se duas imagens são em grande parte iguais e uma tem um "objeto azul" e a outra não, e a imagem com o "objeto azul" tem uma tag extra, os controles extras serão atribuídos a ela. Então, agora a nova tag = "objeto azul".
    
-   Localização: Por exemplo, o padrão de um vestido ocupa o mesmo espaço físico na imagem que o conceito de vestido.
    
-   Ordem: As primeiras tags da esquerda para a direita receberão os controles primeiro. É aqui que as opções de ordenação de legendas entram (manter tokens, embaralhar legendas, descartar legendas e aquecimento de legendas).
    
-   Lembrete: Quaisquer controles que não forem ocupados por outras tags serão distribuídos uniformemente nas tags restantes (suspeito que isso seja feito em ordem crescente dependendo da quantidade atual de controles [que seria zero para novos gatilhos] e depois por localização), esperamos que nossos novos gatilhos (então lembre-se de sempre colocar seus gatilhos no início das legendas).
    
-   Magia: Quero dizer, os acima são os que discerni, pode usar muitos outros meios.
    

Algumas pessoas parecem ter a impressão de que o que você legenda não está sendo treinado, isso é completamente falso, é só que adicionar uma "luva vermelha" ao conceito de uma "luva vermelha" resulta em uma "luva vermelha". Para não ser treinado é para isso que servem as imagens de regularização. Por exemplo, pegue a "luva vermelha" de antes, o modelo diz "Eu sei o que é uma luva vermelha". As imagens de treinamento dizem "Eu quero mudar o que é uma luva vermelha!" e as imagens de regularização dizem "Eu concordo com o modelo!" Isso faz com que as mudanças nos controles que representam a luva vermelha não mudem muito.

Enfim! Agora que você tem uma imagem mental básica, apenas considere a legendagem como um diagrama de Venn. Você deve adicionar, remover e ajustar as tags para que os controles obtidos do treinamento vão para onde você quer. Você pode fazer todo tipo de truque, como duplicar conjuntos de dados e marcá-los de maneira diferente para que o SD entenda você melhor. Digamos que eu tenha uma imagem com uma garota com uma blusa vermelha e uma saia preta, e quero treinar ela e sua roupa. Mas! Eu só tenho uma imagem e quando a marco como character1, outfit1, ambos os gatilhos fazem exatamente a mesma coisa! Claro que sim, o SD não tem ideia do que você está falando, exceto por alguma interferência do modelo via similaridade de palavras.

O que fazer? Fácil! Duplique sua imagem e marque ambas as imagens com character1, então uma com outfit1 e a outra com as partes da roupa. Dessa forma, o SD deve entender que a parte que não é a roupa é seu personagem e que a roupa é igual às partes individuais. Igual a um diagrama de Venn!

Claro que isso começa a ficar complexo quanto maior for o conjunto de dados e as coisas que você deseja treinar, mas depois de alguma prática, você deve pegar o jeito, então continue e continue assando. Se você errar, seu lora vai te avisar!

### Sobre a Arte da arte de IA (reflexão)

Se eu fosse concordar com algo dito por alguns do grupo anti-IA furiosos, seria que a criação de imagens por IA é diferente de desenhar ou pintar. Se algo, é muito mais como culinária.

Há muitos paralelos, desde como um cozinheiro precisa provar o que está fazendo, até como ele precisa ajustar a receita conforme necessário dependendo da qualidade dos ingredientes disponíveis, até como pode ser tão técnico quanto qualquer ciência medindo quantidades ao micrograma. Algumas pessoas seguem uma receita, outras adicionam temperos conforme necessário. Algumas tentam fazer o resultado final o mais próximo possível de um ideal, algumas tentam fazer algo novo e algumas apenas tentam fazer uma refeição comestível. Mesmo os métodos mais frios e metódicos de fazer um modelo usando scraping automatizado dizem algo sobre o criador, mesmo que ele nunca tenha olhado para o conjunto de dados (Eles geralmente parecem médios).

Devo admitir um certo orgulho, não sei se mal colocado ou não, ao selecionar cada imagem no meu conjunto de dados para tentar alcançar um ideal, talvez da primeira vez não funcione, mas com ajustes e experimentos você pode chegar cada vez mais perto do seu objetivo. Mesmo que seu objetivo seja obter imagens bonitas de mulheres voluptuosas!

Então tenha orgulho de suas criações, mesmo que sejam falhas! Se você está se esforçando no seu conjunto de dados e configurações para alcançar seu ideal, isso não é arte?

===============================================================

## Preparações

Primeiro você precisa de algumas coisas indispensáveis:

1.  Uma placa de vídeo Nvidia com pelo menos 6GB, mas realisticamente 8GB de VRAM. Soluções para placas ATI existem, mas ainda não são maduras.
    
2.  Espaço suficiente em disco para armazenar as instalações
    
3.  Uma instalação funcionando do Automatic1111 [https://github.com/AUTOMATIC1111/stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) ou outra interface.
    
4.  Alguns modelos (para anime é recomendado usar a família NAI: NAI, AnythingV3.0, AnythingV5.0, AnythingV4.5) Eu normalmente uso AnythingV4.5 [~https://huggingface.co/andite/anything-v4.0/blob/main/anything-v4.5-pruned.safetensors~](https://huggingface.co/andite/anything-v4.0/blob/main/anything-v4.5-pruned.safetensors) (parece que foi despublicado). Pode ser encontrado aqui: [AnythingV4.5](https://civitai.com/models/4855?modelVersionId=5581), (sem relação com anything V3 ou V5). Use a versão pruned safetensor com a soma de verificação sha256: 6E430EB51421CE5BF18F04E2DBE90B2CAD437311948BE4EF8C33658A73C86B2A
    
5.  Um editor de tags/legendas como [stable-diffusion-webui-dataset-tag-editor](https://github.com/toshiaki1729/stable-diffusion-webui-dataset-tag-editor)
    
6.  Um upscaler para a inevitável imagem que é muito pequena, mas muito preciosa para deixar de fora do conjunto de dados. Recomendo [RealESRGAN_x4plus](https://openmodeldb.info/models/4x-realesrgan-x4plus) para fotorrealismo, [RealESRGAN_x4plusAnime](https://openmodeldb.info/models/4x-realesrgan-x4plus-anime-6b) para anime e [2x_MangaScaleV3](https://openmodeldb.info/models/2x-MangaScaleV3) para mangá.
    
7.  Uma coleção de imagens para o seu personagem. Quanto mais, melhor, e quanto mais variadas as poses e o nível de zoom, melhor. Se você está treinando roupas, obterá melhores resultados se tiver algumas fotos de costas e laterais, mesmo que o rosto do personagem não esteja claramente visível, no pior cenário, algumas fotos apenas da roupa podem servir, apenas lembre-se de não marcar seu personagem nessas imagens se ela não estiver visível.
    
8.  Scripts do Kohya, uma boa versão para Windows pode ser encontrada em [https://github.com/derrian-distro/LoRA_Easy_Training_Scripts](https://github.com/derrian-distro/LoRA_Easy_Training_Scripts), o método de instalação mudou, agora você deve clonar o repositório e clicar no arquivo install.bat. Ainda prefiro essa distribuição ao comando original de linha ou às completas interfaces webui, pois é uma boa mistura entre leveza e facilidade de uso.
    

===============================================================

## Coleta de Dataset

Isso é o suficiente para começar. Agora começa a parte tediosa da coleta e limpeza do dataset:

1. Primeiramente, reúna um dataset. Você pode pegar emprestado, roubar ou implorar por imagens. Muito provavelmente, você terá que raspar um booru, seja manualmente ou usando um script. Para coisas mais raras, você pode acabar revendo aquele anime antigo que você adorava quando era criança e capturando quadro a quadro. [mpc-hc](https://github.com/clsid2/mpc-hc/releases/) tem um recurso útil para salvar capturas de tela em png com um clique direito -> arquivo -> salvar imagem. Você também pode avançar e retroceder um quadro pressionando ctrl + seta esquerda ou direita. Para animes, este guia lista uma boa quantidade de lugares onde procurar imagens: [Useful online tools for Datasets, and where to find data](https://civitai.com/articles/75/useful-online-tools-for-datasets-and-where-to-find-data).
    
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/717fcfdb-fdcd-4345-bc8a-af22e2286812/width=525/717fcfdb-fdcd-4345-bc8a-af22e2286812.jpeg) Um programa de raspagem ok é o grabber, eu não gosto muito dele, mas temos que usar o que temos. Infelizmente, ele não gosta do sankaku complex e às vezes eles têm algumas imagens que não são encontradas em outros lugares.
    
    1. Primeiro, defina sua pasta de salvamento e convenção de salvamento de imagem e clique em salvar. No meu caso, usei a soma de verificação md5.ext, com ext sendo qualquer extensão original que a imagem tinha. Assim: %md5%.%ext%
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/82588b66-883a-46ab-b073-f3a94c42266b/width=525/82588b66-883a-46ab-b073-f3a94c42266b.jpeg)
        
    2. Em seguida, vá para ferramentas -> opções -> salvar -> separar arquivos de log -> adicionar um arquivo de log separado
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f82f168c-67e6-4b54-8fc6-099c80d2e0fe/width=525/f82f168c-67e6-4b54-8fc6-099c80d2e0fe.jpeg)
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/0aa769ab-395c-418f-b0c7-3dddbd6c5e56/width=525/0aa769ab-395c-418f-b0c7-3dddbd6c5e56.jpeg)
        
    3. Naquela janela, digite o nome como %md5%, a pasta como a mesma que você colocou no passo anterior, o nome do arquivo como %md5%.txt para coincidir com seus arquivos de imagem e, finalmente, %all:includenamespace,excludenamespace=general,unsafe,separator=|% como conteúdo do arquivo de texto. Agora, quando você baixar uma imagem, ela baixará as tags do booru. Você precisará processar o arquivo substituindo todos os " " por "_" (espaços por traços) e "|" por "," (o símbolo ou por uma vírgula). Você também pode querer remover a maioria das tags que contêm ":" e um prefixo. Mas, de outra forma, você obterá tags geradas por humanos de qualidade duvidosa.
        
    4. Para danbooru, não funciona direto. Para fazer funcionar, vá para Sources -> Danbooru -> options -> headers, digite "User-Agent" e "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:126.0) Gecko/20100101 Firefox/126.0" e clique em confirmar, isso deve permitir que você veja as imagens do danbooru.
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/3b1a2b3c-d5ea-431b-9edf-4b566953794a/width=525/3b1a2b3c-d5ea-431b-9edf-4b566953794a.jpeg)
        
    5. Para buscar imagens, basta clicar com o botão direito no cabeçalho e selecionar nova aba, depois faça como se estivesse criando uma legenda, mas sem vírgulas: absurdres highres characterA.
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/75c5d7bf-126e-488d-a3bf-cabd7fe1ca96/width=525/75c5d7bf-126e-488d-a3bf-cabd7fe1ca96.jpeg)
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/ac0a8214-80cd-466e-af21-fd84611629ad/width=525/ac0a8214-80cd-466e-af21-fd84611629ad.jpeg)
        
    6. Depois de obter suas imagens, você pode selecioná-las e clicar em salvar uma a uma ou clicar em obter todas e ir para a aba de downloads.
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/8eab9c49-9c9c-4046-a2db-66e76ca753c1/width=525/8eab9c49-9c9c-4046-a2db-66e76ca753c1.jpeg)
        
    7. Na página de downloads, simplesmente faça um ctrl+a para selecionar tudo, clique com o botão direito e selecione download. Ele começará a baixar tudo o que você adicionou. Cuidado, pois pode ser muito, então verifique cuidadosamente as fontes que você deseja. Se uma fonte falhar, simplesmente pule manualmente e selecione a próxima, clique com o botão direito e faça o download, e assim por diante.
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/44508cf1-7e7b-4b50-aa2b-15ebdbef75ff/width=525/44508cf1-7e7b-4b50-aa2b-15ebdbef75ff.jpeg)
        
2. Depois de reunir seu dataset, é bom remover imagens duplicadas. Um programa ok é o dupe guru [https://dupeguru.voltaicideas.net/](https://dupeguru.voltaicideas.net/), ele não vai pegar tudo e pode pegar variações de imagens em que apenas a expressão facial ou um único item de roupa muda.
    
    1. Para usar, primeiro clique no sinal de + e selecione sua pasta de imagens.
        
    2. Clique no modo imagens.
        
    3. Clique em scan.
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6f254f62-c904-45e4-a7c9-4c74927ce619/width=525/6f254f62-c904-45e4-a7c9-4c74927ce619.jpeg)
        
    4. Depois disso, ele dará uma página de resultados onde você pode ver as correspondências e a porcentagem de similaridade.
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/a9e0d833-cc0d-4493-9231-1db91444c16f/width=525/a9e0d833-cc0d-4493-9231-1db91444c16f.jpeg)
        
    5. A partir daí, você pode selecionar o filtro apenas duplicatas, marcar tudo no menu de marcas e então clicar com o botão direito para decidir se deseja deletá-las ou movê-las para outro lugar.
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/8e398028-6328-4539-9d06-ecf88bebc931/width=525/8e398028-6328-4539-9d06-ecf88bebc931.jpeg)
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/9ea11f33-1604-4640-981b-744356a017df/width=525/9ea11f33-1604-4640-981b-744356a017df.jpeg)
        
3. Converta todas as suas imagens para um formato útil, de preferência png, mas jpg pode servir. Você pode usar os [scripts do powershell que eu enviei para o civitai](https://civitai.com/models/82080?modelVersionId=87138) ou fazer você mesmo usando os passos na próxima entrada (3) ou o script no final.
    
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6d1fe289-60e2-44ca-b1fd-cc31a280d38b/width=525/6d1fe289-60e2-44ca-b1fd-cc31a280d38b.jpeg)
    
4. Conversão manual para PNG:
    
    - Para gif, eu uso um [divisor](https://github.com/adnanafzal565/GIF-Splitter) open source ou [ffmpeg](https://ffmpeg.org/download.html#build-windows). Simplesmente abra uma janela cmd na pasta de imagens e digite:
        
        ```
        for %f in (*.gif) do ffmpeg.exe -i "%f" "%~nf%04d.png"
        ```
        
    - Para webp, eu uso dwebp direto das [bibliotecas do google](https://storage.googleapis.com/downloads.webmproject.org/releases/webp/index.html), extraia dwebp do zip baixado na pasta de imagens, abra cmd lá e execute:
        
        ```
        for %f in (*.webp) do dwebp.exe "%f" -o "%~nf.png"
        ```
        
    - Para arquivos avif, obtenha a versão mais recente do [libavif](https://ci.appveyor.com/project/louquillio/libavif/history) (verifique a última compilação bem-sucedida e pegue o arquivo avifdec.exe na aba de artefatos), depois coloque-o na pasta e execute da mesma forma que para webp:
        
        ```
        for %f in (*.avif) do avifdec.exe "%f" "%~nf.png"
        ```
        

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/1a3ef543-0e8d-4f25-a2d0-56eea62d0745/width=525/1a3ef543-0e8d-4f25-a2d0-56eea62d0745.jpeg)

### Fontes de Imagens do Dataset

Nem todas as imagens são iguais! Além da resolução e do desfoque, a fonte realmente afeta o resultado final.

Portanto, temos três abordagens sobre o tipo de imagens que devemos priorizar dependendo do tipo de LORA que estamos fazendo:

- Personagens 2D não fotorrealistas. Para esses, você deve tentar coletar imagens na seguinte ordem:
    
    1. Fanart colorido/arte oficial de alta resolução: Fanart colorido normalmente é de qualidade superior à maioria das coisas. Isso é simplesmente material de primeira linha.
        
    2. Fanart monocromático/arte oficial de alta resolução: Isso inclui doujins, lineart e mangá original. Embora sobrecarregar o modelo com coisas monocromáticas possa torná-lo mais suscetível a produzir monocromático, o SD é excelente em aprender com monocromático e lineart, proporcionando mais detalhes, menos desfoque e, em geral, resultados superiores.
        
    3. Settei/arte conceitual: Estes são incríveis, pois muitas vezes oferecem rotações e variações de rostos e roupas. Procure-os no Google como "settei" e você geralmente encontrará alguns repositórios. Esses muitas vezes podem exigir alguma limpeza e/ou upscaling, pois frequentemente têm anotações de cores e formas de partes do corpo.
        
    4. Fanart monocromático/arte oficial de baixa resolução: a arte monocromática pode ser mais facilmente ampliada e perde menos detalhes quando você faz isso.
        
    5. Screencaps de alta resolução: Capturas de tela são o fundo do poço das boas fontes. Deixando de lado as restrições de orçamento dos animadores que às vezes economizam nos quadros intermediários, a homogeneidade das imagens pode ser um pouco destrutiva, o que é estranho quando comparado ao mangá, que parece elevar os resultados. **Atenção: animes dos anos 80 e 90 parecem treinar muito mais rápido, então ou acolchoe seu dataset com fanart ou reduza os passos para aproximadamente 1/3 do valor normal. Isso também se aplica a screencaps de baixa resolução.**
        
    6. Arte 3D: Cuidado se você estiver tentando fazer um modelo puramente 2D. Adicionar qualquer arte 3D o puxará para um estilo 2.5D ou 3D estilizado.
        
    7. Fanart colorido/arte oficial de baixa resolução: Este material quase sempre precisará de alguma limpeza, filtragem e talvez upscaling. Nos piores casos, precisará de img2img para corrigir.
        
    8. Imagens geradas por IA: Estas podem subir ou descer nesta lista dependendo da qualidade. Lembre-se de examiná-las cuidadosamente para deformidades e artefatos que não são facilmente vistos. Imagens de IA não são ruins, mas podem esconder coisas estranhas que um artista humano não faria, como uma mão extra sorrateira enquanto distraem você com "olhos" bonitos. Para artefatos mais complexos e "texturas", existem inúmeros filtros ESRGAN em [https://openmodeldb.info/](https://openmodeldb.info/) que podem ajudar. Alguns que eu usei são: 1x_JPEGDestroyerV2_96000G, 1x_NoiseToner-Poisson-Detailed_108000_G, 1x_GainRESV3_Aggro e 1x_DitherDeleterV3-Smooth-[32]_115000_G.
        
    9. Screencaps de baixa resolução: Estamos nos aproximando do território "Oh Deus, por quê?". Estas quase sempre precisarão de algum processamento extra para serem utilizáveis e, em geral, serão um peso na qualidade do modelo. Não se envergonhe de usá-las! Como a música diz "Fazemos o que devemos porque podemos!" Uma realidade de treinar LORAs é que na maioria das vezes você lidará com datasets limitados de alguma forma.
        
    10. Fotos de figuras de anime: Estas puxarão o modelo para um 2.5D ou 3D. Elas não são necessariamente ruins, mas cuidado com isso se esse não for seu objetivo.
        
    11. Imagens de lojas online de cosplay: Oh garoto, agora começa a piorar. Se você precisar fazer um traje, pode acabar cavando aqui. As imagens de amostra geralmente parecem horríveis. Lembre-se de limpá-las, filtrá-las e ampliá-las. Ah, elas também sempre têm marcas d'água, então limpe isso também. Certifique-se de marcá-las como manequim e sem_humanos e remova 1girl ou 1boy.
        
    12. Imagens de cosplay: Quase atingindo o fundo do poço. Eu não recomendo usar pessoas reais para coisas não fotorrealistas. Se for para um traje, eu recomendaria cortar a cabeça e marcá-la como cabeça_fora_do_quadro.
        
    13. Superdeformed/chibi: Estes são uma aposta se irão ajudar ou atrapalhar. Eu recomendo contra usá-los, a menos que sejam usados apenas para trajes, e se você usá-los, certifique-se de marcá-los adequadamente.
        
- Personagens 3D não fotorrealistas. Para esses, você deve fazer exatamente a mesma coisa que para 2D, mas usando principalmente coisas 3D, pois qualquer coisa 2D puxará o modelo para um estilo 2.5D.
    
- Fotorrealista: Não se preocupe, apenas obtenha as imagens de maior resolução que puder, que não sejam desfocadas ou tenham artefatos estranhos.
    

===============================================================

## Regularização do Dataset

O próximo passo divertido é a regularização das imagens. Imagens comuns de treinamento para SD são de 512x512 pixels. Isso significa que todas as suas imagens devem ser redimensionadas, ou pelo menos essa era a sabedoria comum na época em que comecei a treinar. Atualmente, a abordagem mais comum é o bucketing, que permite ao script de treinamento do LORA classificar automaticamente as imagens em grupos de tamanhos e redimensioná-las automaticamente. Também é consenso que muitos grupos podem causar uma qualidade ruim no treinamento. Minha sugestão? Redimensione tudo para a resolução desejada de treinamento ou escolha alguns tamanhos de bucket e redimensione tudo para o bucket mais próximo apropriado, manualmente ou usando um script. Na maioria das vezes, você não terá mais de 8 ou 9 buckets, esse número está bom e não tive problemas com isso.

As tarefas principais desta seção são filtrar, redimensionar, processar e classificar.

### Filtragem

1. A menos que você esteja criando um LORA enorme que considere o estilo, remova do seu dataset qualquer imagem que possa conflitar com as outras, por exemplo, versões chibi ou superdeformadas dos personagens. Isso pode ser contabilizado por marcações específicas, mas pode levar a uma inflação enorme do tempo necessário para preparar o LORA.
    
2. Exclua quaisquer imagens que tenham muitos elementos ou estejam desordenadas, por exemplo, fotos de grupo, cenas de gangbang onde muitas pessoas aparecem.
    
3. Exclua imagens com marcas d'água ou texto em locais complicados onde não possam ser removidos com Photoshop ou limpos via lama ou inpaint.
    
4. Exclua imagens com mãos, membros deformados ou poses que não fazem sentido à primeira vista.
    
5. Exclua imagens em que os rostos estão muito borrados; elas podem ser úteis para trajes se você recortar a cabeça.
    
6. Se você estiver criando um LORA de estilo anime, doujins, mangás e lineart são ótimas fontes de treinamento, pois o SD parece captar as características muito facilmente e claramente. Você precisará equilibrá-las com imagens coloridas ou ele sempre tentará gerar em monocromático (até 50% não deve causar problemas, consegui resultados confiáveis treinando com mais de 80%+ em preto e branco usando monocromático e grayscale nos prompts negativos).

### Redimensionamento

-   **Corte:** Se você estiver usando bucketing, lembre-se de cortar suas imagens para remover qualquer tipo de espaço vazio, pois cada pixel importa. Lembre-se de que com bucketing sua imagem será redimensionada até que sua Largura x Comprimento seja < 512x512 (262.144) pixels. Eu criei [um script em PowerShell](https://civitai.com/models/233193) que faz o downscale para o tamanho de bucket esperado. Use-o para verificar se alguma de suas imagens precisa ser cortada ou removida do dataset em caso de perda de muito detalhe durante o processo de downscale do script de treinamento.
    
    -   Antes de começar a fazer mais processamentos, é bom eliminar qualquer padding que suas imagens possam ter. Tive sucesso usando [ImageMagick](https://imagemagick.org/script/download.php). Ele verifica a cor dos cantos e aplica um nível de tolerância, removendo as linhas ou colunas dependendo da cor até falharem na verificação. Adicionei algumas imagens do antes e depois de executar o comando.
        
    -   Basta instalá-lo, selecionar um valor para as tolerâncias (chamado fuzz, no meu caso escolhi 20%), ir para a pasta onde estão suas imagens, abrir uma janela de cmd e digitar:
        

```
 for %f in (*.png) do magick "%f" -fuzz 20% -trim "Cropped_%~nf.png"
```

  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/82a9bea5-8b2e-4c0b-902a-c1cd49d4a493/width=525/82a9bea5-8b2e-4c0b-902a-c1cd49d4a493.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/5135a0f7-d9c7-4ee0-badc-383441f4252d/width=525/5135a0f7-d9c7-4ee0-badc-383441f4252d.jpeg)

-   **Downscaling** é principalmente supérfluo na era do bucketing, mas! Lembre-se de que as imagens serão redimensionadas de qualquer maneira! Portanto, esteja atento à perda de detalhes, especialmente em imagens de alta resolução de corpo inteiro:
    
    -   Se você optar por downscaling, uma técnica é passar as imagens por um script que simplesmente redimensionará, seja cortando ou redimensionando e preenchendo o espaço vazio com branco. Se você tiver mais de 250 imagens, esta é a melhor abordagem e não é um problema. Basta revisar as imagens e descartar as que não passaram. Isso pode ser feito no A1111 na aba Train->preprocess images.
        
    -   Se você, por outro lado, tem um orçamento limitado de imagens, fazer algum downscaling/corte pode ser benéfico, pois você pode obter subimagens a diferentes distâncias. Recomendo fazer isso manualmente. O Paint3D do Windows é adequado, se não uma boa opção. Basta ir à aba de canvas e mover os limites da sua imagem. Por que fazer isso? Porque você pode obter várias subimagens de uma única imagem, desde que seja de alta resolução. Por exemplo, suponha que você tenha uma imagem de corpo inteiro de alta resolução de um personagem. Se estiver usando resize to square, você pode redimensionar e preencher os espaços em branco para obter 1 imagem. Faça um corte na cintura e faça uma foto de retrato para uma 2ª imagem. Depois, faça um corte no pescoço para uma 3ª imagem com apenas um close-up. Se estiver usando bucketing, você pode cortar a original, cortar uma foto de meio corpo e um retrato, todas serão tratadas como imagens diferentes, pois serão redimensionadas de maneira diferente pelo algoritmo de bucketing.
        
        -   Se estiver usando resize to square:
            
            ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/abc95820-8b49-468c-b690-c3b2eeef056d/width=525/abc95820-8b49-468c-b690-c3b2eeef056d.jpeg)
            
        -   Se estiver usando bucketing:
            
            ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f3ae47b2-16f2-46c8-8887-e2395a7c9b61/width=525/f3ae47b2-16f2-46c8-8887-e2395a7c9b61.jpeg)
            
    -   Para simplificar as coisas, criei [um script](https://civitai.com/models/82080) (também está como texto no final do guia) que torna as imagens quadradas preenchendo-as. É útil para pré-processar imagens ruins (resolução abaixo de 512x512) antes de enviá-las para um upscaler (para aqueles que requerem uma proporção fixa). Basta copiar o código em um arquivo de texto e renomeá-lo para algo.ps1, depois clicar com o botão direito e executar com o PowerShell. **_Isso é principalmente desatualizado e era útil antes de Kohya implementar bucketing para Embeddings._**
        
-   **Upscaling:** Nem todas as imagens nascem em alta resolução, então isso é importante o suficiente para ter sua própria seção. Upscaling provavelmente deve ser o último processo a ser executado em suas imagens, então a seção está mais abaixo.
    

### Processamento e Limpeza de Imagens

-   Ok, então você tem algumas imagens mais ou menos limpas ou algumas não tão limpas que você não consegue descartar. A próxima parte "divertida" é a limpeza manual. Faça uma revisão imagem por imagem tentando cortar, pintar ou deletar quaisquer elementos extras. O objetivo é que apenas o personagem alvo permaneça na sua imagem (se o seu personagem estiver interagindo com outro, por exemplo, fazendo sexo, é melhor cortar o outro personagem da imagem). Tente deletar ou corrigir marcas d'água, balões de fala e texto de efeitos sonoros. Redimensione as imagens, passando imagens de baixa resolução por um upscaler (veja a próxima seção) ou img2img para ampliá-las. Percebi que borrar os rostos de outros personagens no estilo faceless_male ou faceless_female funciona maravilhosamente para reduzir a contaminação. Anedota aleatória: No meu Ranma-chan V1 LORA, se você invocar 1boy, basicamente obterá um Ranma masculino perfeito com cabelo avermelhado, pois todos os homens no dataset estão sem rosto.  
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/d7ad9ade-141e-4116-8ccb-e198c8f5466c/width=525/d7ad9ade-141e-4116-8ccb-e198c8f5466c.jpeg)
    
    Isso é apenas um exemplo de faceless, realisticamente, se você estivesse tentando treinar Misato, o que você quer é isso (embora eu não goste muito dessa imagem e não a adicionaria ao meu dataset, mas para fins de demonstração funciona):  
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/a3621f65-1b93-4abf-a901-322c12f19e6c/width=525/a3621f65-1b93-4abf-a901-322c12f19e6c.jpeg)
    
-   **Img2Img:** Não tenha medo de passar uma imagem extremamente ruim no img2img para tentar deixá-la menos borrada ou ruim. Datasets sintéticos são uma coisa, então não sinta vergonha de usar um parcialmente ou totalmente sintético. É assim que alguns LORAs de personagens originais são feitos. Lembre-se disso, se você já fez um LORA e ele acabou ruim... Use-o! Usar um LORA de um personagem para corrigir imagens ruins desse personagem é uma coisa e dá ótimos resultados. Apenas certifique-se de que a imagem resultante é o que você quer e lembre-se de que quaisquer defeitos serão treinados no seu próximo LORA (isso significa mãos, então use imagens onde elas estão escondidas ou você terá que corrigi-las manualmente). Quando estiver fazendo isso, certifique-se de interrogar a imagem com um bom tagger, deepdanboru é terrível quando as imagens estão borradas (ele sempre as detecta como censura em mosaico e pênis). Tente adicionar qualquer tag ausente manualmente e lembre-se de adicionar borrado e o que você não quer nos negativos. Recomendo manter o denoise baixo, em torno de 0.1~0.3, e iterar na imagem até se sentir confortável com ela. O objetivo é que ela fique clara, não bonita.
    
-   Para remover fundos, você pode usar [stable-diffusion-webui-rembg](https://github.com/AUTOMATIC1111/stable-diffusion-webui-rembg.git) instalando na aba de extensões, ele aparecerá na aba extra. **_Eu não gosto dele_**. Não tive nenhum sucesso com ele. Em vez disso, recomendo transparent-background, que é muito menos amigável, mas parece me dar melhores resultados. Recomendo reutilizar sua instalação do A1111 e colocá-la lá, pois já tem todos os requisitos. Basta abrir um PowerShell ou cmd em stable-diffusion-webui\venv\Scripts e executar Activate.ps1 ou Activate.bat, dependendo se você usou ps ou cmd. Então, instale usando: pip install transparent-background. Após instalado, execute digitando: transparent-background --source "D:\inputDir\sourceimage.png" --dest "D:\outputDir\" --type white
    
    Este comando específico preencherá o fundo com branco, você também pode usar rgb [255, 0, 0] e algumas opções extras, basta verificar a parte do wiki na página do GitHub [https://github.com/plemeri/transparent-background](https://github.com/plemeri/transparent-background).
    

### Limpeza de Imagens 2: Lama e a Arte Masoquista de Limpar Mangás, Fanart e Doujins

Como mencionei antes, mangás, doujins e lineart são simplesmente a mídia de treinamento superior, parece que a falta dessas cores irritantes faz o SD se deliciar. O SD parece ter uma inclinação perversa de funcionar melhor com coisas que são difíceis de obter ou limpar.

Como sempre, os elefantes na sala são três: SFX, balões de fala e marcas d'água. A menos que você esteja reaaaaaaaallly entediado. Você precisará de algo para semi-automatizar a tarefa, ou encontrará-se copiando e colando screentones para cobrir um SFX deletado (já estive lá, já fiz isso).

A solução? [https://huggingface.co/spaces/Sanster/Lama-Cleaner-lama](https://huggingface.co/spaces/Sanster/Lama-Cleaner-lama) o Lama cleaner, é ruim e lento, mas funcional. Vou adicionar os passos para reutilizar seu ambiente A1111 para instalá-lo localmente porque é mais rápido e menos provável que alguém esteja roubando seus doujins perversos.

Lembre-se de que para a versão A1111, eles recomendam uma força de denoise de 0.4. **Honestamente, para mim, a versão local stand-alone é mais rápida, me dá melhores resultados e é um pouco mais amigável.**

Instalação da extensão A1111:

1. Vá para a aba de extensões.
    
2. Instale o controlnet se ainda não o fez.
    
3. Clique em get all available extensions e selecione lama no filtro ou selecione get from URL e cole [https://github.com/light-and-ray/sd-webui-lama-cleaner-masked-content.git](https://github.com/light-and-ray/sd-webui-lama-cleaner-masked-content.git)
    
4. Instale e redefina sua UI.
    
5. Vá para inpaint em img2img e selecione lama cleaner e apenas masked, use o cursor para mascarar o que você quer remover e clique em generate. Eles parecem recomendar um denoise de 0.4
    
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/2959c167-3ec5-40ed-8de8-ca80378ff8cc/width=525/2959c167-3ec5-40ed-8de8-ca80378ff8cc.jpeg)
    

Instalação local "stand-alone":

1. Primeiro, ative seu venv A1111 clicando com o botão direito e selecionando abrir janela do PowerShell aqui em sua pasta stable-diffusion-webui\venv\Scripts e digitando ".\activate".  
    Alternativamente, crie um venv vazio digitando python -m venv c:\path\to\myenv em uma janela de comando, esteja ciente de que ele baixará torch e tudo mais.
    
2. Instale o Lama cleaner digitando pip install lama-cleaner
    
3. Isso deve bagunçar sua instalação do A1111, mas não se preocupe!
    
4. Digite: pip install -U Werkzeug
    
5. Digite: pip install -U transformers --pre
    
6. Digite: pip install -U diffusers --pre
    
7. Digite: pip install -U tokenizers --pre
    
8. Digite pip install -U flask --pre
    
9. Pronto, tudo deve voltar ao normal, ignore as reclamações sobre incompatibilidades no lama-cleaner, pois testei as últimas versões dessas dependências e elas funcionam bem juntas.
    
10. Inicie usando: lama-cleaner --model=lama --device=cuda --port=8080. Você também pode usar --device=cpu, é mais lento, mas não tanto.
    
11. Abra no porta 8080 ou o que você usou [http://127.0.0.1:8080](http://127.0.0.1:8080/)
    
12. Basta arrastar o cursor para pintar uma máscara, no momento em que soltar, ele começará a processar.
    
13. Tente manter a máscara o mais justa possível ao que você quer deletar, a menos que esteja cercado por um fundo homogêneo.
    

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/abdfa7e1-ac7f-4ee5-b239-7c495f651e8e/width=525/abdfa7e1-ac7f-4ee5-b239-7c495f651e8e.jpeg)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/a934d2c5-5301-4ea1-9292-bfcfa69b26bb/width=525/a934d2c5-5301-4ea1-9292-bfcfa69b26bb.jpeg)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/654cf107-c77f-44a1-974e-0c4a5fc7b794/width=525/654cf107-c77f-44a1-974e-0c4a5fc7b794.jpeg)

Agora a parte suculenta das Recomendações:

1. Pré-corte suas imagens para exibir apenas a parte esperada. Imagens menores diminuirão o tempo de processamento do lama e ajudarão a focar nos SFX, balões de fala ou marcas d'água que você deseja remover.
    
2. Cuidado com coisas nas bordas da área de mascaramento, pois elas serão usadas como parte do preenchimento. Então, se você tiver um pixel preto aleatório, ele pode se transformar em uma linha inteira. Recomendo denoising em sua imagem e talvez remover artefatos JPEG, pois isso evitará que a imagem tenha muitos pixels aleatórios que podem se tornar artefatos.
    
3. Esqueça balões de fala grandes, se você não puder dizer o que deveria estar atrás deles, o Lama também não conseguirá. Em vez disso, transforme os balões de

 fala em spoken_heart. O SD parece saber perfeitamente o que são e os ignorará perfeitamente se estiverem corretamente marcados durante a legendagem. A forma não importa, o SD parece aceitar balões de fala clássicos, quadrados, espinhosos ou de pensamento. A tag é "spoken_heart". Se você de alguma forma perdeu isso.  
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/61859c2e-9022-4b31-ac2a-0020953eee5a/width=525/61859c2e-9022-4b31-ac2a-0020953eee5a.jpeg)
    
4. Braços, pernas e cabelos são mais fáceis de recuperar, dedos, padrões de roupas ou sombras complexas não são, e você pode precisar corrigir manualmente ou cortar.
    
5. O Lama é ótimo para prever linhas, substitua um espaço vazio entre duas linhas aparentemente contínuas e ele preencherá o restante. Você também pode usar isso para restaurar linhas quebradas.
    
6. Você pode apagar coisas perto da área que está restaurando para que o Lama não siga o padrão dessa seção. Por exemplo, você pode precisar apagar o fundo perto do cabelo para que o Lama siga o padrão do cabelo em vez do fundo.
    
7. O Lama é ruim em recriar screentones adequadamente, se estiver perto o suficiente, esqueça. Nos meus testes, o SD não parece prestar muita atenção ao padrão do screentone, desde que o tom médio esteja correto.
    
8. Sobreposições semitransparentes ou grandes marcas d'água transparentes exigem ser restauradas em pequenos passos a partir da área descoberta e é, em geral, uma dor. Você pode querer corrigi-las manualmente.
    
9. Você fuçou e encontrou um modelo específico de mangá nas opções? Sim, esse parece ser ligeiramente falho.
    

### Transparência/Alfa e você

Neste ponto, você pode pensar "transparência? Ótimo! Agora eu não vou treinar aquele fundo inútil!" Errado! SD odeia transparência, no melhor dos casos nada acontecerá, nos piores casos, o kohya simplesmente ignorará as imagens ou você terá distorções estranhas nos fundos enquanto o SD tenta simular as bordas recortadas.

Então, o que fazer? [image magick](https://imagemagick.org/script/download.php) vem ao resgate mais uma vez.

1. Certifique-se de que o image magick está instalado
    
2. Reúna suas imagens com transparência (ou simplesmente execute isso em todas, apenas levará mais tempo).
    
3. Execute o seguinte comando:
    
    ```
    for %f in (*.png) do magick "%f"  -background white -alpha remove -alpha off "RemovedAlpha_%~nf.png"
    ```
    

### Criação de Máscaras

Então todas as imagens do seu conjunto de dados têm o mesmo fundo irritante ou muitos objetos que te incomodam? Bem, a solução para isso é o treinamento com perda mascarada. Basicamente, o treinamento ignorará um pouco a perda das áreas mascaradas de uma máscara fornecida. Ferramentas para criar máscaras automaticamente provavelmente se resumem a transparent-background ou REMBG. Eu não tenho muita confiança no REMBG, então vou adicionar um fluxo de trabalho para transparent-background.

Primeiro, devemos instalar o transparent-background, então crie um novo venv ou reutilize o ambiente do a1111 e instale o transparent-background usando pip

```
pip install transparent-background
```

Em seguida, ainda no seu venv, você deve executar o transparent-background da seguinte forma:

```
transparent-background --source "D:\dataset" --dest "D:\dataset_mask" --type map
```

Isso gerará a máscara de suas imagens com o sufixo "_map", a seguir devemos remover o sufixo, basta abrir o powershell nos arquivos de máscara e executar o seguinte comando:

```
get-childitem *.png | foreach { rename-item $_ $_.Name.Replace("_map", "") }
```

Agora todos os seus arquivos de máscara devem ter os mesmos nomes que seus originais. Por fim, antes de usar suas imagens mascaradas para fazer o treinamento com perda mascarada, verifique as máscaras; máscaras em escala de cinza nas quais você pode ver partes do seu personagem não são boas, assim como máscaras que são completamente pretas.

As seguintes máscaras são ruins:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/82a9efa4-cb7b-4fe0-a37e-958259a9c7b9/width=525/82a9efa4-cb7b-4fe0-a37e-958259a9c7b9.jpeg)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/3b6fceb4-4ea1-4fb3-a35a-fc304adb4c3f/width=525/3b6fceb4-4ea1-4fb3-a35a-fc304adb4c3f.jpeg)

É assim que uma boa máscara deve parecer:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/3dbedc06-5969-4bb3-88fb-02aeeeb28e22/width=525/3dbedc06-5969-4bb3-88fb-02aeeeb28e22.jpeg)

Lembre-se, você faz o treinamento com perda mascarada por pasta, então remova da sua pasta as imagens cujas máscaras não ficaram boas e simplesmente coloque-as em outra pasta com a perda mascarada desativada. (atualmente incerto, mas deve ser corrigido em algum momento.)

### Redimensionamento 2: A Volta do Upscaling

Existem 3 tipos comuns de Upscaling: Animado, Lineart e Fotorrealista. Como eu faço principalmente LORAs de personagens de anime, tenho mais experiência com os dois primeiros, pois cobrem anime, mangá, doujins e fanart.

Também recomendo passar suas imagens por este [Script](https://civitai.com/models/233193/bucketing-downscaler-downscales-and-sorts-images-to-expected-bucket-sizes?modelVersionId=381519) que irá classificar suas originais por bucket e criar uma pré-visualização em escala para a resolução esperada do bucket. Mais importante, ele dirá quais imagens precisam ser upscaladas para preencher corretamente seu bucket. Recomendo fazer isso no início do processo, pois ele classifica as imagens em subpastas por bucket.

- Anime:
    
    - Para upscaling de baixa resolução, meu escalador de anime preferido atualmente é [RealESRGAN_x4plus_anime_6B](https://github.com/xinntao/Real-ESRGAN/blob/master/docs/anime_model.md). Tive bons resultados para imagens de baixa resolução até 512. Basta colocar o modelo dentro da pasta A1111 model\ESRGAN e usá-lo na aba extras. Alternativamente, encontrei um bom escalador de anime que é uma aplicação pronta para Windows [waifu2x-caffe](https://github.com/lltcggie/waifu2x-caffe/releases). Basta baixar o arquivo zip e executar waifu2x-caffe.exe. Então você pode selecionar várias imagens e upscalá-las para 512x512. Para capturas de tela de baixa resolução ou imagens antigas, recomendo o modelo "Photography, Anime". Você pode aplicar a redução de ruído antes ou depois, dependendo da qualidade original da imagem.
        
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/330f6baf-51f4-4cb9-9e63-7827a8fa4f0e/width=525/330f6baf-51f4-4cb9-9e63-7827a8fa4f0e.jpeg)
        
    - Alguns escaladores e filtros extras podem ser encontrados na wiki. [openmodeldb.info](https://openmodeldb.info/) 1x_NoiseToner-Poisson-Detailed_108000_G funciona bem para reduzir alguns grãos e artefatos em imagens de baixa qualidade. Como o 1x indica, esses não são escaladores e provavelmente devem ser usados como um filtro secundário na aba extras ou sozinhos, sem upscaling.
        
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/996541ad-d7d2-4b1f-9177-fb8ae4159013/width=525/996541ad-d7d2-4b1f-9177-fb8ae4159013.jpeg)
        
    - Img2img: para upscaling de capturas de tela de anime usando img2img, o melhor que você pode fazer é usar uma LORA falhada do personagem em questão e usá-la no prompt com baixa intensidade ao fazer o upscale no SD. Não tem uma? Bem, então... se as imagens forem menos de 10, basta passá-las por um autotagger, limpar qualquer tag estranha, adicionar os positivos e negativos padrão e fazer img2img usando upscale no SD. Recomendo uma força de redução de ruído de 0.1. Não se preocupe com a forma da imagem ao usar o upscale no SD, a proporção das dimensões é mantida. Cuidado com os olhos e as mãos, pois você pode ter que fazer várias gerações para obter boas imagens sem muita distorção.
        
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/7da613d3-a710-4827-8dda-80df8db0f5a2/width=525/7da613d3-a710-4827-8dda-80df8db0f5a2.jpeg)
        
        Para img2img em massa, a menos que você planeje curar os prompts, simplesmente vá para batch em img2img, configure os diretórios e use as configurações acima e simplesmente use um prompt genérico como 1girl e deixe processar. Você pode configurar para fazer um lote de 16 e escolher as melhores gerações.
        
- Mangá:
    
    - Para mangá (especialmente mangá antigo) temos alguns inimigos extras
        
        - Compressão jpeg: Isso parece distorções perto das linhas.
            
        - Screentone/dithering: Mangás e doujins, como são mídia impressa, muitas vezes são coloridos usando padrões de tinta em vez de verdadeiro grayscale. Se você usar um escalador ruim, ele pode ou comer todo o screentone transformando em verdadeiro grayscale ou, pior, comer parcialmente deixando uma bagunça mista.
            
    - Para upscaling, tive a melhor sorte usando [2x_MangaScaleV3](https://openmodeldb.info/models/2x-MangaScaleV3). Sua única desvantagem é que pode mudar alguns grayscale em screentones. Se sua imagem quase não tem screentones, então o DAT_X4 (padrão no a1111) parece fazer um trabalho aceitável. Para alguns pré-filtros, caso as imagens sejam especialmente ruins, tive sorte filtrando usando [1x_JPEGDestroyerV2_96000G](https://openmodeldb.info/models/1x-JPEGDestroyer). Pode ser usado junto com [1x_NoiseToner-Poisson-Detailed_108000_G](https://openmodeldb.info/models/1x-NoiseToner-Poisson-Detailed). Parece ter feito seu trabalho sem comer muito o padrão de dithering.  
        
        Abaixo está um exemplo indo de inutilizável a um "talvez" usando 2x_mangascaleV3, a menor é a original.
        
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/84cab47c-ed37-4ee4-9193-1b2b4d796291/width=525/84cab47c-ed37-4ee4-9193-1b2b4d796291.jpeg)
        
- Fotorrealista ou 3D:
    
    - Não tenho muita experiência com isso, mas quando preciso de algo normalmente uso RealESRGAN_x4plus, que parece ser suficiente.
        

### Classificação

Então todas as suas imagens estão impecáveis e todas têm 512x512 ou mais de 262.144 pixels totais. Algumas são menores? Volte e faça o upscaling delas! Pronto? Bem, o próximo passo é classificar, simplesmente crie algumas pastas e literalmente classifique suas imagens. Normalmente, classifico por roupa e por qualidade.

Normalmente você vai querer dar menos repetições para imagens de baixa qualidade e precisará saber quantas imagens tem por roupa para ver se são suficientes para treinar, descartar em misc ou sair à caça de mais imagens debaixo de pedras nos ermos.

Agora que você tem uma certa ordem no seu conjunto de dados, pode continuar para a fase de planejamento.

===============================================================

## Planejamento

Algumas pessoas diriam que a fase de planejamento deve ser feita antes de coletar o conjunto de dados. Essas pessoas logo vão colidir de cara com a realidade. A realidade de fazer uma LORA depende inteiramente da existência de um conjunto de dados. Mesmo que você decida usar um conjunto de dados sintético, para fazer qualquer trabalho real, você precisa criar a coisa primeiro. De qualquer forma, você tem um conjunto de dados do que quer que seja que seu coração deseja. A maioria das LORAs se enquadra em 2 ou 3 arquétipos: Estilos, Conceitos ou Personagens. Um personagem é um tipo de conceito, mas é tão onipresente que pode ser uma categoria por si só. Agora, além dessas categorias, temos um tipo de LORA quase: o LECO.

### LORAS

Loras podem mudar devido ao estilo de marcação e isso reflete diretamente como o conceito é invocado e pode ser separado em 3 categorias principais, para esta explicação um conceito pode ser um personagem, uma situação, um objeto ou um estilo:

- Fluido: Nesta abordagem, você deixa todas as tags na legenda. Assim, tudo sobre o conceito é mutável e o gatilho atua como uma ponte entre os conceitos. O prompt, neste caso, usando um personagem como exemplo, será assim: CharaA, cabelo_longo, olhos_azuis, pernas_longas, coxas_grossas, camisa_vermelha, etc. O problema com essa abordagem é a reprodutibilidade, pois para fazer o personagem parecer o mais próximo possível, será necessário um prompt muito longo descrevendo-o em detalhes com tags. Este estilo, para mim, requer acesso ao resumo das tags do treinamento para ser mais útil. Surpreendentemente, para o usuário final, isso é perfeito para iniciantes ou para especialistas. Para iniciantes porque eles não se importam muito, desde que o personagem pareça semelhante, e para especialistas porque com o tempo você adquire uma noção das tags que podem estar faltando para reproduzir um personagem se não tiver acesso ao resumo das tags ou ao conjunto de dados.
    
- Semi-estático: Nesta abordagem, você poda parcialmente as legendas, de modo que o gatilho sempre reproduzirá o conceito desejado e o usuário final pode personalizar as tags restantes. Mais uma vez, este exemplo é de um personagem. O prompt será assim: CharaA, seios_grandes, coxas_grossas, Roupa1, saltos_altos. Nesse caso, características como estilo de cabelo, cor e cor dos olhos são excluídas ou podadas e incorporadas ao CharaA, deixando apenas coisas como tamanho dos seios e outras características do corpo editáveis. Para este exemplo, partes da roupa também são excluídas e incorporadas ao gatilho Roupa1, deixando apenas os sapatos editáveis. Este estilo é mais útil para usuários intermediários, pois eles podem se concentrar na composição geral sabendo que o personagem sempre será fiel até certo ponto.
    
    - Um exemplo de poda é assim:
        
        - Não podado: 1girl, ascot, boca_fechada, ascot_com_frufrus, saia_com_frufrus, frufrus, cabelo_verde, mangas_longas, olhando_para_o_observador, desafio_de_desenho_de_uma_hora, boca_aberta, xadrez, saia_xadrez, colete_xadrez, saia_vermelha, colete_vermelho, camisa, cabelo_curto, fundo_simples, saia, solo, guarda-chuva, colete, camisa_branca, fundo_amarelo
            
        - Podado: CharaA, Roupa1, boca_fechada, olhando_para_o_observador, boca_aberta, fundo_simples, solo, guarda-chuva, fundo_amarelo
            
    - A lógica é simples: CharaA absorverá conceitos como 1girl que representam o personagem, enquanto Roupa1 absorverá os conceitos da roupa. Lembre-se de que, para o modelo aprender a diferença entre CharaA e Roupa1, o gatilho deve aparecer em imagens diferentes para que o modelo aprenda que são, de fato, conceitos diferentes. Por exemplo, o personagem vestido com uma roupa diferente ensinará ao modelo que Roupa1 representa apenas a roupa, enquanto CharaA aparece em imagens que têm roupas diferentes.
        
    - Recomendações:
        
        - Para roupas de personagens, nunca incorpore tags de calçados (salto_alto, botas, calçados) nas roupas. Isso geralmente causa um viés para fotos de corpo inteiro, pois o modelo tenta mostrar o corpo inteiro, incluindo a parte superior e os sapatos, e também pode causar cabeças parcialmente cortadas.
            
        - Para personagens, nunca pode o tamanho dos seios. As pessoas ficam extremamente defensivas quando não podem mudar o tamanho dos seios.
            
        - Para estilos, lembre-se de podar coisas inerentes ao estilo, por exemplo, se for preto e branco, pode monochrome ou se a arte for muito "escura", pode tags como noite, sombra, pôr do sol, etc.
            
- Estático: Se você podar todas as tags relacionadas ao conceito, você obtém isso. Ele resistirá a qualquer mudança que você tentar fazer. Por exemplo, para um personagem, alguém vai te acusar de algo por não fazer uma determinada parte do corpo editável. Eu sempre faço os seios nas minhas loras editáveis e ainda recebo reclamações desse tipo. O prompt será assim: CharaA. Bem, não tão extremo, você ainda pode forçar algumas mudanças. O que você está fazendo é essencialmente transformar todo o personagem, roupa e tudo, em um único conceito e ele será reproduzido como tal.
    

### Personagens

Então você decidiu fazer um personagem. Espero que você já tenha decidido qual abordagem seguir e, espero, que tenha sido a semi-estática, ou se estiver com preguiça, a fluida servirá.

Como pode ser facilmente deduzido, apenas as primeiras duas rotas são úteis. A abordagem semi-estática requer muito mais experiência sobre o que podar e o que manter na legenda. Verifique a seção de legendagem em triggers avançados para mais detalhes sobre isso. Surpreendentemente, a maioria dos criadores parece seguir o estilo fluido, não tenho certeza se os usuários finais preferem, se é por preguiça ou se nunca sentiram a necessidade de experimentar a legendagem para fazer coisas mais complexas.

De qualquer forma, você escolheu seu veneno, agora vem as roupas.

- Se você está usando uma abordagem fluida, apenas certifique-se de ter uma representação boa o suficiente da roupa no conjunto de dados, qualquer coisa entre 8 a 50 imagens serve, apenas deixe-as lá. Depois de terminar o treinamento, tente recriar a roupa via prompt e anexe às notas quando publicar...
    
- Se você está usando uma abordagem semi-estática, pode querer ou precisar treinar roupas individuais como sub-conceitos. Eu diria que tento adicionar todas as roupas representativas, mas na realidade eu adiciono principalmente as sensuais. Aqui é onde você começa a fazer compromissos, pois inevitavelmente ficará sem imagens. Então... adicione as feias. Você sabe quais. Aquelas que não passaram no corte. Apenas dê uma ajeitada nelas com photoshop ou img2img. Eu honestamente acabei procurando em leilões de cosplay aquela imagem de baixa resolução de uma roupa em um manequim. Como mencionei no início, o conjunto de dados é tudo e qualquer planejamento depende do conjunto de dados.
    

De qualquer forma, como mencionei, ao selecionar uma roupa para treinar, você precisará de pelo menos 8 imagens com ângulos o mais variados possível. Não espere nada bom para uma roupa complexa a menos que você tenha mais de 20 imagens. 8 é o mínimo e você provavelmente precisará de ajuda do modelo, então, neste caso, é melhor construir os triggers da roupa da seguinte forma: Cor1_item2_Cor2_Item2...CorN_ItemN. É possível adicionar outros descritores no trigger como Kimono_amarelo_curto, neste caso sendo composto por dois tokens: amarelo e curto_kimono. Se fosse, por exemplo, sem mangas e com um obi, eu provavelmente colocaria como Kimono_amarelo_curto_Obi_Sem_Mangas. Não tenho certeza de quanto a ordem afeta a eficácia da contaminação do modelo (contribuição, neste caso), mas tento ordená-los em ordem decrescente de visibilidade, com modificadores que não podem ser diretamente anexados à roupa no final. Então eu colocaria sem_mangas, halterneck, highleg, mangas_destacadas, etc. no final.

### Pacotes de Personagens

Então você decidiu que quer fazer um pacote de personagens? Se você está usando um estilo de legendagem de personagem fluido, então é melhor rezar para não ter dois personagens loiros. A verdade simples é que fazer isso aumenta o vazamento. Se os rostos dos seus personagens forem semelhantes e suas cores e estilos de cabelo forem diferentes o suficiente, isso é viável, mas nunca vi um pacote de personagens excepcional feito dessa forma. Não me entenda mal, eles não parecem ruins ou errados, bem, quando são feitos corretamente. Eles parecem sem graça, pois os personagens extras puxam a LORA um pouco para um meio-termo feliz devido às tags compartilhadas. Mas se você gosta de um estilo "sem graça", isso é perfeitamente aceitável! Neste ponto, provavelmente existem centenas de LORAs no estilo "Genshin Impact" e eles estão prosperando! Como isso é possível? Não tenho a menor ideia. Se me permite pegar emprestado as palavras de alguns dos artistas que gostam de nos criticar, pessoas que usam IA, eu diria que são "bonitos, mas um pouco sem alma". Então, de novo, eu sou velho e já vi animes desenhados à mão dos anos 80.

Seguindo em frente... Se você está usando um estilo de legendagem semi-estático, basta basicamente fazer um conjunto de dados para cada personagem, juntá-los e treiná-los. Devido à poda, não deve haver sobreposição de conceitos, isso minimizará o vazamento. Parece fácil? É! O único problema é que é o mesmo trabalho que fazer duas LORAs diferentes e levará o dobro do tempo de treinamento.

Então agora você está se perguntando por que se incomodar? Bem, há duas razões principais:

- Para diminuir a quantidade de LORAs. Por que ter dezenas de LORAs se você pode ter uma por série! Honestamente, isso faz sentido do ponto de vista do usuário final, mas do ponto de vista do criador, só adiciona mais pontos de falha, porque você pode ter acertado 5 personagens, mas o sexto pode parecer ruim. Lembre-se! A menos que você esteja sendo pago por isso, facilitar a vida do usuário final à sua custa é completamente opcional, e se você estiver sendo pago, deve apenas facilitar "um pouco" para que continuem precisando dos seus serviços no futuro. :P Por outro lado, se você tem TOC e quer ter suas LORAs organizadas assim, então acho que é tempo bem gasto.
    
- A verdadeira razão, para você poder ter dois ou mais personagens na mesma imagem ao mesmo tempo! Isso pode ser feito com várias LORAs, mas elas inevitavelmente terão conflitos de estilo e peso. Isso só pode ser mitigado usando um modelo dreambooth ou uma LORA com ambos os personagens. Se você está pensando em NSFW, está completamente correto! Mas também uma simples caminhada de casal de mãos dadas (que algumas pessoas consideram a forma mais perversa de NSFW).
    
    Para isso, você necessariamente precisa que os dois personagens sejam legendados de forma semi-estática, caso contrário, quando você tentar promptar os personagens, o SD não saberá quais atributos atribuir a cada um. Por exemplo, se você tem "1boy, cabelo_vermelho, músculos" e "1girl, cabelo_preto, seios_grandes", o SD pode fazer o que quiser (ou fazer uma única pessoa ou distribuir os traços aleatoriamente). Por outro lado, se você tem "PersonagemA, PersonagemB", fica muito mais difícil para o SD errar. Uma consideração é que o SD não gerencia múltiplos personagens muito bem, então é essencial adicionar um terceiro grupo de imagens ao conjunto de dados com ambos os personagens na mesma imagem.
    
    Em outras palavras, para melhor resultado, o fluxo de trabalho deve ser o seguinte. Você precisa de 3 grupos distintos de imagens: Personagem A, Personagem B e Personagem A+B, cada um com seu respectivo gatilho. Eu recomendaria pelo menos 50+ imagens de cada. Em segundo lugar, você precisará fazer uma poda extensiva de tags para os grupos A e B no estilo semi-estático, como se estivesse fazendo 2 LORAs de personagens diferentes, certificando-se de que o SD saiba claramente quais atributos pertencem a quem. Depois disso, o grupo A+B deve ser marcado com os 3 gatilhos, certificando-se de podar qualquer tag que aluda ao número de pessoas. Basicamente, o que você estará fazendo é colocar duas LORAs de personagens e uma LORA de conceito (o conceito sendo duas pessoas) em uma única LORA. Então, no final, basta juntar os 3 conjuntos de dados, adicionar alguns ajustes para roupas extras (se houver) e pronto. Mesmo depois de tudo isso, a LORA pode exigir que você use 2girls, 2boys ou "1boy, 1girl" para fazer a quantidade correta de pessoas. SD é irritante assim.
    

### Estilos

Estilos é exatamente o que parece, o estilo de um artista. O que você tenta capturar é o ambiente geral, proporções do corpo dos personagens, arquitetura e estilo de desenho (se não for fotorrealista) de um artista. Por exemplo, se você verificar minhas LORAs ou pelo menos suas imagens de amostra, notará que eu prefiro um estilo mais sombrio, facções um pouco mais maduras e peitos grandes (não tem como evitar, sou cínico o suficiente para saber que sou um pervertido irredimível). Se você coletasse todos os meus conjuntos de dados e imagens de amostra e os mesclasse em uma LORA, você obteria um estilo KNXO, eu acho.

Eu realmente não gosto de treinar estilos, pois acho um pouco desrespeitoso, especialmente se o artista ainda estiver vivo. Eu só fiz uma LORA de estilo/personagem porque esse artista de doujin específico jurou parar de desenhar o personagem depois de ser pressionado pelo estúdio que agora possui os direitos da franquia. De qualquer forma, se você gosta tanto do estilo de um artista que não consegue se conter, apenas envie um kudos para o artista e faça o que quiser. Eu não sou sua mãe.

Teoricamente, o estilo é o tipo de LORA mais simples de treinar, basta usar um conjunto de dados grande, limpo e devidamente marcado. Se você está treinando um estilo, precisará de tantas imagens variadas quanto possível. Para este tipo de treinamento, as legendas são tratadas de forma diferente, o que você quer fazer é excluir todas as tags dos arquivos de legenda, opcionalmente deixando apenas o gatilho e deixar a LORA assumir quando for invocada. Uma abordagem mais difícil é adicionar o gatilho e apenas eliminar as tags associadas ao estilo (por exemplo, estilo retrô, um fundo específico, perspectiva e tags desse tipo) e deixar todas as tags particulares intactas (1girl, cabelo_azul, etc.). É mais simples apenas eliminar todas as tags e usá-lo apenas com o gatilho ou sem gatilho. Lembre-se e não posso enfatizar o suficiente, **limpe seu conjunto de dados o melhor possível**, por exemplo, se você usar mangá ou doujins, limpe os efeitos sonoros e outras coisas irritantes, pois eles afetarão o produto final (a menos que façam parte da estética que você deseja, é claro).

### Conceitos

Conceitos... Primeiro de tudo, você está ferrado. Treinar um conceito é 50% arte e 50% sorte. Primeiro, certifique-se de limpar suas imagens o melhor possível para remover a maioria dos elementos estranhos. Tente escolher imagens que sejam simples e óbvias sobre o que está acontecendo. Tente escolher imagens que sejam o mais diferentes possível e apenas compartilhem o conceito que você deseja. Para a marcação, você precisa adicionar seu gatilho e eliminar todas as tags que toquem no seu conceito, deixando as outras sozinhas.

Algumas pessoas dirão que adicionar um conceito é como treinar um personagem, isso é verdade até certo ponto para conceitos de objetos físicos. Em qualquer outro caso... Oh! como estão erradas! O SD foi treinado principalmente para desenhar pessoas e realmente sabe o que faz, dedos e mãos à parte. Ele também conhece objetos comuns, comum sendo a palavra-chave. Se o SD não tem ideia do que você está treinando, você terá que basicamente gravá-lo no modelo usando fogo profano. No outro extremo, se é uma variação de algo que ele já pode fazer, provavelmente queimará como pólvora. Aqui estão alguns exemplos

- Um bom exemplo para o primeiro caso seria um formigueiro, uma bactéria ou um padrão repetitivo aleatório. Alguém poderia pensar: "bem, só preciso de um monte de imagens de exemplo o mais variadas possível". Não, se o SD não entender, você precisará aumentar o LR, reps e epochs. No meu caso particular, acabei precisando de 14 epochs usando prodigy, então o LR estava provavelmente lá em cima. Tive que usar um conjunto de dados enorme com um monte de repetições. Acho que foram 186 imagens com 15 repetições por 14 epochs. Também usei keep token no meu gatilho e shuffle de legendas. Como disse, tive que basicamente gravá-lo na LORA. O resultado foi medíocre na minha opinião, embora as pessoas pareçam gostar, pois faz o que deveria fazer.
    
- No segundo caso, fiz uma variação de uma pose/combinação de roupas. (Não procure se você tem olhos virginais). O problema é que é uma variação de um conceito comum. É realmente possível fazer (com confiabilidade extremamente baixa) apenas usando prompts, também queria ser o mais neutro possível em termos de estilo para que fosse fácil de misturar. Resumindo, qualquer quantidade de repetições fazia com que ficasse ou overbaked ou muito afetado pelo estilo. A única solução que encontrei foi essas configurações: Dim 1 alpha 1 em 8 epoch 284 imagens únicas com 1 repetição. O dim e alpha baixos tornaram menos provável afetar o estilo, as imagens únicas atuaram como imagens de regularização puxando o estilo em muitas direções, cancelando-o um pouco e as repetições baixas mantiveram o overcooking sob controle. Acabei fazendo duas variações, uma com adamw e outra com Prodigy. A Prodigy foi mais consistente, mas mais pesada em estilo, apesar dos meus melhores esforços. A AdamW foi muito mais neutra, mas um pouco instável.
    

**Resumindo:** Use baixo alpha e dim para conceitos generalizados e poses que você deseja que sejam flexíveis. Use um conjunto de dados tão grande e variado quanto possível. Se o conceito for estranho ao SD, grave-o com fogo, e se já estiver gravado com fogo, toque-o com uma pena.


### LECO ou Sliders

LECO pode ser considerado um tipo de Demi LORA; de fato, eles compartilham a mesma estrutura de arquivos e podem ser invocados como um LORA normal. Apesar de serem idênticos ao LORA em muitos aspectos, os LECO são treinados de uma maneira completamente diferente, sem usar imagens. **_<u>Para treinar LECO você precisa de pelo menos 8GB de VRAM</u>_**. Atualmente, existem dois scripts capazes de treinar LECO:

-   [https://github.com/p1atdev/LECO](https://github.com/p1atdev/LECO): Implementação mais antiga, focada principalmente em adicionar ou remover conceitos.
    
-   [https://github.com/ostris/ai-toolkit](https://github.com/ostris/ai-toolkit): Implementação mais moderna, focada na criação de sliders.
    

Para o treinamento, usei o Ai Toolkit. Atenção, o projeto não é muito maduro, então está mudando constantemente. Vou adicionar instruções usando uma versão específica que sei que funciona e que precisará de alguns ajustes.

As seguintes instruções são para uma instalação no Windows usando CUDA 121. Certifique-se de ter o Git para Windows [https://gitforwindows.org/](https://gitforwindows.org/) instalado.

Primeiro, abra o PowerShell em uma pasta vazia e faça os seguintes passos:

```
git clone https://github.com/ostris/ai-toolkit.git
git checkout 561914d8e62c5f2502475ff36c064d0e0ec5a614
cd ai-toolkit
git submodule update --init --recursive
python -m venv venv
.\venv\Scripts\activate
```

Depois, baixe o arquivo zip do ai toolkit anexado à direita; ele tem uma cópia do meu ambiente de trabalho atual usando CUDA 121 e Torch 2.2.1. Substitua o arquivo de requisitos, bem como o arquivo dentro da pasta (ele tem uma correção de conversão para inteiro).

Então, execute:

```
pip install -r requirements.txt
```

Isso deve demorar um pouco enquanto tudo é baixado. Enquanto isso, baixe o arquivo train_slider.example.yml à direita e é hora de editá-lo.

Realisticamente, os únicos valores que você precisa editar são:

-   Nome: O nome do LECO
    
-   LR: A taxa de aprendizado recomendada era 2e-4, mas era muito alta; melhor **_configurá-la para 1e-4 ou um pouco menos._**
    
-   Hyperparameters: Otimizador, denoising, scheduler, todos funcionam bem, o LECO queimou bem antes dos 500 passos, então isso também está ok.
    
-   steps: Houve alguma estranheza arredondando para 500, a última época não foi gerada, então adicione 1 ao máximo só por precaução. Eu padronizei para 501.
    
-   dtype(Train): se sua placa de vídeo suportar, use bf16; caso contrário, use FP32, **_FP16 causa erros de perda NAN tornando o LECO inutilizável._**
    
-   dtype(Save): sempre deixe como float16 para compatibilidade.
    
-   name_or_path: O caminho para o seu modelo use "/" para a estrutura de pastas, se usar o outro ele travará.
    
-   modeltypes: configure V2 e Vpred se SD2.0, xl se for XL ou deixe falso se for SD1.5.
    
-   max_step_saves_to_keep: Esta é a quantidade de épocas que você deseja manter, se colocar menos que o total, as mais antigas serão excluídas.
    
-   prompts: a parte --m é a força do LECO, recomendo os seguintes valores para você ver o progresso: -2, -1,-.5,-.1,0,.1,.5,1,2. Eles devem parecer assim:
    
    ```
    - "1girl. skirt, standing --m -2"
    ```
    
-   resolutions: 512 para SD1.5, 768 para 2 e 1024 para XL.
    
-   target_class: O conceito que você deseja modificar, por exemplo [skirt](https://civitai.com/models/325863/skirt-length-slider-leco) ou 1girl.
    
-   positive: A extensão máxima do conceito, por exemplo, para uma saia seria long_skirt.
    
-   negative: A extensão mínima do conceito, para uma saia seria microskirt, mas no meu caso isso não foi suficiente, pois esse conceito não está muito bem treinado no meu modelo, então tive que ajudar incluindo outros conceitos relacionados, acabou sendo: "microskirt,lowleg_skirt,underbutt,thighs,panties,pantyshot,underwear".
    
-   metadata: Apenas coloque seu nome e endereço da web, você não quer que digam que pertencem a KNXO.
    

Então você terminou de preencher seu arquivo de configuração e instalar todos os requisitos? Então, no PowerShell com o venv aberto e na pasta ai-toolkit, digite:

```
python run.py path2ConfigFile/train_slider.example.yml 
```

Então, supondo que tudo tenha corrido bem, começará a treinar. LECO treina muito mais lentamente que LORA e requer mais VRAM; por outro lado, eles suportam retomar, você pode simplesmente parar o treinamento e iniciá-lo novamente conforme necessário. O AItools salva o progresso e a época na pasta de saída, então verifique o progresso lá.

Para saber se um LECO está pronto, verifique as imagens de amostra; se os extremos pararam de mudar, então está cozido demais. Por exemplo, as seguintes imagens são do passo 50 peso 2 e passo 100 peso 1. Não apenas é óbvio que ambos são iguais, mas o segundo tem um artefato azul no ombro esquerdo dela. Então, nesse caso, a época do passo 50 é a que eu quero (a LR recomendada 2e-4 foi realmente estupidamente alta).

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6edd2e3e-eaa0-447a-9540-4c1b8d963504/width=525/6edd2e3e-eaa0-447a-9540-4c1b8d963504.jpeg)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/52607a47-b203-412a-baf8-22657d803efa/width=525/52607a47-b203-412a-baf8-22657d803efa.jpeg)

Então você decidiu qual época parece melhor, então simplesmente teste como qualquer outro LORA. Parabéns, você fez seu primeiro pseudo LORA. A dificuldade do LECO vem principalmente de configurar o ambiente e encontrar boas tags para ambos os extremos do conceito.

### Treinamento com Máscara

Seu dataset está cheio de imagens com o mesmo ou um fundo problemático? Talvez muita desordem? Bem, o treinamento com máscara pode ou não ser uma solução! As vantagens dessa abordagem são que o treinamento é simplesmente mais concreto, reduzindo fatores externos. A desvantagem? Você tem que fazer máscaras, de preferência máscaras de boa qualidade. A menos que você seja masoquista e queira criar manualmente a máscara de cada uma das suas imagens, você precisará de algo para gerá-las e provavelmente será fundo transparente ou REMBG. Verifique a seção de criação de máscaras para saber como fazer com fundo transparente.

Pelo que sei, o treinamento com perda mascarada funciona da mesma forma que o treinamento normal, mas ao salvar os pesos, as zonas mascaradas são atenuadas ou ignoradas dependendo da opacidade da máscara. Em palavras simples, a parte preta na máscara é ignorada.

Em meus testes, o treinamento com máscara funciona um pouco melhor do que, por exemplo, treinar com um fundo branco, não é uma melhoria massiva de forma alguma, mas funciona bem o suficiente. Para ser verdadeiramente valioso, precisa de algumas condições:

1.  O dataset é amigável ao programa de criação de máscaras:
    
    -   As imagens devem ser o mais sólidas possível; imagens borradas, monocromáticas, lineart e qualquer coisa muito difusa provavelmente falharão em produzir uma boa máscara.
        
2.  O dataset tem algo que você "realmente" não deseja treinar e não pode ser recortado:
    
    -   Isso se aplica, por exemplo, se o dataset tiver fundos repetitivos ou problemáticos.
        
3.  Itens ou fundos problemáticos não estão devidamente etiquetados.
    
    -   Se forem ignorados, não importa muito se estiverem mal etiquetados.
        

Então, se seu dataset está devidamente recortado e etiquetado, os ganhos serão mínimos.

De qualquer forma, para fazer treinamento com máscara, basta criar as máscaras para suas imagens e certificar-se de que tenham o mesmo nome que os originais. Então, simplesmente ative a opção de usá-las e defina a pasta de máscaras, depois prossiga com o treinamento como de costume.

Não vi nenhuma atenuação particular na taxa de aprendizado nem sobrecarga de desempenho. Elas podem existir, mas são gerenciáveis. No geral, o treinamento com máscara é apenas outra ferramenta situacional para treinamento.

### Conclusão de Planejamento

**_<u>Finalmente, gerencie suas expectativas.</u>_** Um LORA de personagem é bom quando você pode fazer com que ele produza a semelhança do personagem que você deseja. Um LORA de conceito de objeto é o mesmo. Mas como medir o sucesso de uma pose? Pode-se dizer que, enquanto a pose for explicitamente a mesma (basicamente exagerar de propósito), então missão cumprida! Mas você também pode acrescentar que o LORA de pose funciona bem com outros LORAs, que seja neutro em estilo ou que tenha variações suficientes de ângulo de visão enquanto ainda é a mesma pose. Para mim, os LORAs de conceito que fiz foram projetos de várias semanas que me deixaram um pouco esgotado e um pouco desanimado, pois cada melhoria só destacava as deficiências do LORA. Então lembre-se disso, se você está ficando cansado e o LORA mais ou menos funciona, diga "que se dane!" e jogue-o na rede para que os usuários finais o usem e sofram. Esqueça, os usuários vão reclamar das deficiências, mas isso dará uma ideia do que realmente precisa ser corrigido. Então, simplesmente esqueça e volte a ele quando a sensação de fracasso tiver passado e você puder e quiser investir mais tempo nele, agora com um feedback real sobre o que corrigir.


===============================================================


Então, todas as suas imagens estão organizadas, 512x512 ou em alguns buckets. O próximo passo é a estrutura de pastas, as imagens devem estar dentro de uma pasta com o formato X_Nome onde X é a quantidade de vezes que a imagem será processada. Você terminará com um caminho como este: train\10_DBZStart onde dentro da pasta train está a pasta contendo as imagens. Imagens de regularização usam a mesma estrutura. Você pode ter muitas pastas, todas são tratadas da mesma forma e permitem manter as coisas organizadas se você estiver treinando múltiplos conceitos como diferentes roupas para um personagem. Também permite adicionar repetições de processamento mais altas para imagens de alta qualidade ou talvez uma roupa com poucas imagens. Por enquanto, basta definir tudo para 10 repetições, você precisará ajustar esses números após terminar de organizar suas imagens nas pastas.

No exemplo abaixo, marquei 6 roupas, a pasta misc tem roupas diversas sem imagens suficientes para serem viáveis. Ajustei as repetições dependendo da quantidade de imagens dentro de cada pasta para tentar mantê-las equilibradas. Confira a seção de Repetições e Épocas para ajustá-las.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/877c640a-5dff-4347-aa6f-ac4b8da6e38b/width=525/877c640a-5dff-4347-aa6f-ac4b8da6e38b.jpeg)

Então, depois de terminar a estrutura, é hora de classificar suas imagens nas pastas correspondentes. Recomendo que, se a foto estiver acima do coração, jogue-a em misc, pois não fornecerá muita informação para a roupa, essas roupas parciais em misc devem ser marcadas com suas partes visíveis parciais em vez da roupa a que pertencem. A menos que tenham uma parte especial não visível na parte inferior da roupa, neste caso deixe-as como estão e trate-as como parte normal da roupa e apenas tenha cuidado para não sobrecarregar a roupa com retratos. **Sugestão aleatória**: Para roupas, adicionar algumas fotos de corpo inteiro sem cabeça (para tornar o personagem irreconhecível) marcadas com "1girl, nome_da_roupa, etc" faz maravilhas para separar o conceito da roupa do personagem.

===============================================================

## Repetições e Épocas

Definir Repetições e Épocas pode ser uma questão de quem veio primeiro, o ovo ou a galinha. Os fatores mais importantes são o Dim, alpha, otimizador, a taxa de aprendizado e a quantidade de épocas e repetições. Cada um tem suas próprias receitas para ajuste fino. Algumas são melhores, outras são piores.

A minha é tão genérica quanto possível e normalmente dá bons resultados ao gerar com peso em torno de .7. Atualizei esta parte pois as opções de ip noise gamma e min snr gamma parecem diminuir consideravelmente a quantidade de passos necessários. Para prodigy, 2000 a 3000 passos totais para SD1.5 e 1500 a 2500 para SDXL.

Lembre-se que os scripts de treinamento baseados em kohya calculam a quantidade de passos dividindo-os pelo tamanho do lote. Quando falo de passos, estou falando dos totais sem dividir pelo tamanho do lote.

Agora, o que significa passos por época? É apenas a quantidade de repetições vezes a quantidade de imagens em uma pasta. Suponha que estou fazendo um **personagem** LORA com 3 roupas. Tenho 100 imagens de roupa1, 50 de roupa2, 10 de roupa3.

Eu definiria as repetições das pastas como:

- 3_roupa1 = 3 reps * 100 img = 300 passos por época
    
- 6_roupa2 = 6 reps * 50 img = 300 passos por época
    
- 30_roupa3 = 30 reps * 10 img = 300 passos por época
    

Lembre-se que você também está treinando o conceito principal ao fazer isso, no caso acima isso resulta no **personagem** sendo treinado 900 passos. Portanto, tenha cuidado para não "cozinhar demais" (ou sobreajustar). Quanto mais conceitos sobrepostos você adicionar, maior o risco de "cozinhar demais" o lora.

Isso pode ser mitigado removendo a relação entre o personagem e a roupa. Pegue por exemplo a roupa1 de cima, eu poderia pegar 50 das imagens e remover a tag do **personagem** e substituí-la pelas tags de descrição original (cor do cabelo, cor dos olhos, etc). Dessa forma, quando a roupa1 estiver sendo treinada, o **personagem** não estará. Outra alternativa que funciona um pouco é usar normalização de escala que "achata" valores que disparam muito além do resto, limitando um pouco o "cozinhar demais". Usar o otimizador Prodigy também torna as coisas menos propensas a cozinhar demais. Finalmente, quando tudo mais falhar, o uso de imagens de regularização pode ajudar a mitigar esse problema. Pegue o exemplo acima, você divide as imagens da roupa marcando metade com o gatilho do personagem e a outra metade com as tags individuais. Usar imagens de regularização com essas tags as retornará a um "estado base" do modelo, permitindo continuar treinando sem sobreajustar, assim efetivamente treinando apenas o gatilho da roupa.

**_<u>Aviso:</u>_** Não use normalização de escala com prodigy, pois eles não são compatíveis.

Para o conceito principal (**personagem** no exemplo) eu recomendaria manter abaixo de (6000 passos em adamW ou 3000 para prodigy) no total antes de precisar começar a ajustar o conjunto de dados para mantê-lo baixo e evitar queimar.

Lembre-se de que essas são todas aproximações e se você tiver 10 imagens a mais para uma roupa, pode deixar assim. Se seu Lora estiver um pouco "cozido demais", na maioria das vezes pode ser compensado reduzindo o peso ao gerar. Se seu LORA começar a "fritar" imagens com peso inferior a .5, eu definitivamente recomeçaria o treino, ainda será utilizável, mas a faixa de usabilidade se torna muito estreita. Há também um [script de rebase](https://civitai.com/articles/222) por aí para mudar o peso, então você poderia teoricamente definir .5 para se tornar 1.1, aumentando assim a faixa de usabilidade.

### Receita

Eu uso 8 épocas, Dim 32, Alpha 16 Prodigy com taxa de aprendizado de 1 com 300 passos por época por conceito e 100 passos por subconceito (normalmente roupas). Alternativamente, use Dim 32, AdamW com taxa de aprendizado de .0001 e dobre a quantidade de passos para prodigy. (Não adicione decimais, sempre arredonde para baixo). Ficou "cozido demais"? Reduza as repetições/taxa de aprendizado ou em adamw ative a normalização.

**_<u>AVISO:</u>_** Usar capturas de tela, embora não seja ruim em si, é mais propenso a "assar demais" devido à homogeneidade dos conjuntos de dados. **Eu encontrei esse problema especialmente com animes antigos, ao legendar você notará que normalmente recebe tags de retro, estilo dos anos 80 e 90. Nesse caso, para prodigy, eles parecem treinar absurdamente mais rápido e fazer um bom trabalho com 1000~2000 passos totais em vez dos comuns 2000~3000.**

### Tabela de repetições ridícula do KNXO

Abaixo, adiciono uma tabela ruim e difícil de ler, provavelmente a anexarei um pouco maior à direita nos downloads como um arquivo zip. Cada linha sendo um cenário e o valor NA significando que não é necessário. Por exemplo, a primeira linha é para um lora simples de um personagem com uma roupa. Isso é para dim 32 alpha 16.

***Por roupa simples quero dizer: camisa, calças, saias sem babados ou padrões estranhos.**

***A coluna de personagem representa uma pasta misc cheia de imagens aleatórias do personagem em roupas não treináveis (poucas imagens da roupa ou roupas treinadas faltando muitas de suas partes) e fotos do rosto (assim como nus). É bom para o LORA ter uma forma básica do personagem separada de suas roupas. Nesta pasta, as roupas devem ser marcadas naturalmente, ou seja, em peças, como o autotagger normalmente faz sem podar as partes individuais.**

***As colunas de roupa representam pastas com visualização completa e clara da roupa e marcadas como tal. Essas imagens podem ou não também estar marcadas com o gatilho do personagem, se aplicável (por exemplo: manequins vestindo a roupa não devem ser marcados com o gatilho do personagem, apenas com o gatilho da roupa).**

Se o seu cenário desejado não estiver explicitamente lá, então interpolar, por exemplo, se você tiver um lora de dois personagens com 3 roupas por personagem, basta aplicar o cenário 3 para ambos os personagens (ou seja, reduzir as repetições dos personagens para 400). A tabela basicamente explica mais ou menos as repetições necessárias para um LORA de 1 ou 2 personagens com até 4 roupas (2 complexas e 2 simples). Também permite a substituição de uma roupa base mais uma roupa derivada, por exemplo, uma roupa com uma variante com cachecol ou jaqueta ou algo simples assim.

**_<u>Cuidado, esta tabela é completamente empírica e sem base da minha parte, o fato de ter funcionado para mim é mera coincidência e ainda precisa de alguns ajustes em uma base por caso, mas eu normalmente sigo ela. Esses valores gravitam em torno do extremo superior, então você pode acabar escolhendo épocas antes da última. Se eu tivesse que adivinhar, sua melhor aposta seria a época 5 ou 6.</u>_**

**_<u>Cuidado 2: Animes antigos parecem precisar de aproximadamente 1/3 dos passos de treinamento que animes contemporâneos por "razões" (não tenho certeza exata, mas provavelmente devido ao conjunto de dados original do NAI).</u>_**

**_<u>Cuidado 3: </u>_ <u>opções de ip noise gamma e min snr gamma parecem diminuir consideravelmente a quantidade de passos necessários. Então tente multiplicar os valores por 3/5, como 500*3/5=300 passos.</u>**

**<u>Cuidado 4: para sdxl, tente multiplicar os valores por 1/2, 500*1/2=250 passos.</u>**

**_<u><img src="https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/e4d97eed-2860-4c24-9da2-58f08bddda07/width=525/e4d97eed-2860-4c24-9da2-58f08bddda07.jpeg"></u>_**

===============================================================

## Legendas

Então, todas as suas imagens estão limpas e no tamanho 512x512, o próximo passo é legendar. Legendar pode ser tão raso quanto uma poça ou tão profundo quanto a Fossa das Marianas. **Legendar** (adicionar tags) e **podar** (excluir tags) são a maneira como criamos **gatilhos,** um gatilho é uma tag personalizada que absorveu (na falta de uma palavra melhor) os conceitos das tags podadas. Para personagens de anime, é recomendável usar ~deepboru~ WD1.4 vit-v2 que usa a marcação no estilo [danbooru](https://danbooru.donmai.us/). A melhor maneira que encontrei é usar o diffusion-webui-dataset-tag-editor (procure por ele em extensões) para A1111, que inclui um gerenciador de tags e tagger de waifu diffusion.

- Vá para a guia A1111 do [stable-diffusion-webui-dataset-tag-editor](https://github.com/toshiaki1729/stable-diffusion-webui-dataset-tag-editor) e selecione um tagger nas configurações de carregamento do dataset, selecione usar tagger se estiver vazio. Em seguida, basta carregar o diretório das suas imagens e, após tudo terminar de marcar, basta clicar em salvar. Recomendo selecionar dois dos taggers WD1.4, ele criará tags duplicadas, mas então você pode ir para editar legendas em lote->remover->remover duplicados e obter a cobertura máxima de tags.
    
    - Alternativamente, vá para A1111 na guia Train->preprocess images e marque usar deepbooru para legendas e processe-as. **Eu desaconselho fortemente o uso do deepbooru padrão, sempre que encontra imagens borradas ou névoa ele vê pênis ou sexo, pois a desfocagem é uma forma comum de censura. Você pode notar facilmente quando o deepbooru enlouqueceu, pois os arquivos de legendas produzidos têm mais de 1kb. <u>Faça um favor a si mesmo e use apenas o editor de dataset.</u>**
        
-   ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/66d2d0a1-1ce1-4414-bac2-765cb767efd6/width=525/66d2d0a1-1ce1-4414-bac2-765cb767efd6.jpeg)![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/71a3265e-69df-4af0-8b42-1c8321b0ebb6/width=525/71a3265e-69df-4af0-8b42-1c8321b0ebb6.jpeg) Uma ferramenta ok que pode ajudar como um passo preliminar é o [DatasetHelpers](https://github.com/Particle1904/DatasetHelpers), honestamente ela não é muito boa, mas tem duas opções muito úteis, remoção de redundância e consolidação de tags, ambas na aba Process tags, basta selecionar sua pasta de dataset após marcar, verificar ambas as opções e clicar em processar. Isso consolidará algumas tags, por exemplo, camisa, camiseta_branca em apenas camiseta_branca e tags como vestido_branco e vestido_gola_alta em vestido_gola_alta_branco. Não é muito boa, mas reduzirá a quantidade de tags que você precisará verificar e podar. ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6cf8d2e8-6c7f-4ae5-8130-fbb8988799c9/width=525/6cf8d2e8-6c7f-4ae5-8130-fbb8988799c9.jpeg)  
    

### Estilos de Marcação

Ao criar seu dataset, você terá que responder a uma pergunta importante: Blip ou Danbooru?

-   Blip: Supostamente baseado em linguagem natural (Isso faz minha cabeça doer por algum motivo). A legenda no estilo Blip é o padrão no qual o SD foi treinado. Parece ser péssimo. Eu não gosto disso, minha cabeça não gosta disso por algum motivo. Blip é usado apenas em modelos fotorrealistas no SD1.5 e na maioria dos modelos no SDXL. Uma legenda Blip parece assim: "Uma mulher com olhos verdes e cabelo castanho caminhando na floresta usando um vestido verde comendo um burrito com (salsa)"
    
-   Danbooru: O estilo de legenda Danbooru é baseado no sistema de tags do Booru e implementado em todos os derivados e misturas do NAI, o que representa a maioria dos modelos não fotorrealistas do SD1.5. Geralmente aparece na seguinte forma "1girl, green_eyes, brown_hair, walking, forest, green_dress, eating, burrito, (sauce)". Este estilo de marcação é nomeado em homenagem ao site, como em [https://danbooru.donmai.us/](https://danbooru.donmai.us/). Sempre que você tiver dúvidas sobre o significado de uma tag, pode navegar até o Danbooru, procurar pela tag e abrir seu wiki.
    
    Tome como exemplo o seguinte: procure pela tag "road". Quando abrirmos seu wiki, veremos a definição exata, bem como tags derivadas como street, sidewalk ou alley, além da quantidade de vezes que a imagem foi usada (13K).
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/ce270b7b-df68-4442-86fc-29752d0e1943/width=525/ce270b7b-df68-4442-86fc-29752d0e1943.jpeg)
    
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c4beb670-9e95-4725-95bf-ca56d6885f17/width=525/c4beb670-9e95-4725-95bf-ca56d6885f17.jpeg)
    
    Na prática, isso significa que o conceito é treinado em algum grau nos modelos baseados em NAI e misturas. A quantidade de vezes que a tag aparece no Danbooru realmente se correlaciona com a força do treinamento (já que o NAI foi treinado diretamente nos dados do Danbooru). Então, qualquer conceito abaixo de 500 coincidências é um pouco duvidoso. Tenha isso em mente ao legendar, pois às vezes faz mais sentido usar uma tag genérica em vez da correta, por exemplo, "road" aparece 13k vezes enquanto "dirt_road" apenas 395 vezes. Neste caso específico, usar "dirt_road" não deve ser problemático, pois "dirt_road" contém "road" de qualquer maneira e o SD é capaz de ver a associação.
    
    Enfim, lembre-se de consultar o Danbooru quando precisar de ajuda para legendar algo ou podar tags. Outro exemplo ótimo é Tiara vs circlet. A maioria dos taggers vai gerar ambas. Mas! Uma Tiara vai no topo da cabeça e é mais parecida com uma coroa, enquanto um circlet vai na testa! Sailor Moon estava errada o tempo todo! Ela tinha um circlet de lua em vez de uma tiara de lua! Embora isso possa parecer um pouco inconsequente, cada erro de marcação afetará negativamente o LORA pouco a pouco, criando um efeito bola de neve.

### Limpeza

Limpeza de legendas: Antes de começar a seleção de Triggers, é melhor fazer uma limpeza das tags (certifique-se de ignorar tags que serão incorporadas nos triggers, pois essas provavelmente serão podadas). Tags supérfluas são melhor tratadas das seguintes maneiras:

-   Excluir: Para tags inúteis como meme, parody, sailor_moon_redraw_challenge_(meme), shiny_skin, uncensored (eu me esforço para sempre podar uncensored, pois quero que meus loras lembrem que este é o estado padrão). Também para identidades equivocadas no caso de seu personagem ser identificado como outro personagem ou um objeto ser identificado erroneamente.
    
-   Consolidar: Para tags genéricas, por exemplo, "bow" é melhor substituído por seu equivalente de cor + parte como black_back_bow ou red_bowtie e excluir as tags individuais associadas. Isso se aplica principalmente a cabelo, roupas, cenários e a tag "holding".
    
-   Dividir: Também para tags genéricas, por exemplo, "armor" é melhor dividido em pauldron, gorget, breastplate, etc. Joias, maquiagem e roupas íntimas são infratores comuns.
    
-   Sinônimos: Uma versão deve ser escolhida e a outra consolidada, por exemplo, "circlet" e "tiara", a maioria dos taggers vai pegar ambas.
    
-   Avaliar: Essas podem aumentar ou corromper o conceito de treinamento. Por exemplo, se você estiver treinando um personagem e ele for reconhecido. Se a resposta do modelo for branda ao gerar uma imagem dele, então pode ser usado como trigger do personagem para melhorá-lo. Se, por outro lado, ele já estiver fortemente treinado, provavelmente fará seu LORA "overcook". Então, use-os como triggers ou exclua-os. Você não precisa se preocupar se eles apenas existirem em seu dataset e você não estiver treinando para eles (por exemplo, se você estiver treinando a arquitetura da cidade de Gotham e ele reconhecer o Batman).

### Seleção de Triggers

Agora você deve decidir quais tags vai usar como trigger para seu LORA. Existem 4 tipos de "contaminação" que seu trigger pode receber do modelo:

-   Contaminação negativa: Por exemplo, você deseja fazer um Lora para Bulma em seu traje DBZ. Então você escolhe a tag "Bulma_DBZ". Errado! Se seu personagem for desconhecido, não há problema, mas se você escolher um personagem famoso como Bulma, você obterá contaminação de estilo da palavra "Bulma" e da palavra "DBZ". No caso de Bulma, seu estilo é tão profundamente treinado na maioria dos modelos de anime que provavelmente vai "overcook" seu LORA simplesmente por estar associado a ele. Lembre-se de que underscores, hifens e traços são equivalentes a espaços na notação Danbooru e mesmo que parcialmente, você pode obter algum vazamento devido a invocar tangencialmente seus conceitos.
    
-   Ruído: Se quando você passar seu trigger pelo modelo ele produzir algo diferente a cada vez, isso significa que o trigger está "livre" ou não treinado no modelo e é perfeito para usar em seu Lora se você quiser minimizar a interferência externa.
    
-   Contaminação positiva: Por outro lado, essa contaminação pode ser benéfica especialmente para roupas. Tome como exemplo o seguinte trigger: Green_Turtleneck_Shirt_Blue_Skirt. Como não está completamente concatenado, ele obterá um pouco de contaminação de cada uma das palavras que o formam. Isso pode ser muito útil para melhorar triggers de roupas nas quais você tem apenas algumas imagens. Apenas certifique-se de passá-lo pelo seu modelo e verificar se ele produz algo semelhante ao que você está tentando treinar.
    
-   Contaminação deliberada: Esta é uma técnica que encontrei para lidar com roupas semelhantes. Parece funcionar melhor com o otimizador prodigy, mas pode ser usado com adamw com menor sucesso. Consiste em usar conceitos semelhantes para construir sobre outros mais complexos e semelhantes. Um bom exemplo é um uniforme escolar com variação sazonal ou os uniformes de sailor senshi que têm pequenas mudanças nas mangas e broches. Vamos tomar como exemplo o uniforme escolar, a variante de verão é uma camisa branca, uma saia cinza e uma gravata borboleta. A de outono é a mesma mais um colete de suéter. A de inverno usa um cardigã. E finalmente, uma versão formal tem um blazer cinza. Se você tentar treinar todas as quatro variantes como estão, você terá um "overcooked". Minha solução? Use a contaminação de uma versão mais simples para a próxima roupa. Nesse caso, faça uma roupa "base": School_uniform_White_shirt_Grey_skirt_bowtie. Então use a contaminação dessa para fazer as outras: School_uniform_White_shirt_Grey_skirt_bowtie_Sweater_vest, School_uniform_White_shirt_Grey_skirt_bowtie_Cardigan e School_uniform_White_shirt_Grey_skirt_bowtie_Grey_blazer. Lembre-se de reduzir as repetições, se normalmente você usaria 500 passos para cada um dos 4 triggers, é uma boa ideia dividir pela metade se estiver compartilhando para dois triggers ou dividir por 3 se estiver fazendo mais. Nesse caso, com um pai e 3 filhos, acho que usei 200 repetições por época para 8 épocas para cada um dos quatro conceitos. Você pode ver o Lora resultante (Orihime Inoue de bleach) [aqui](https://civitai.com/models/175586/orihime-inoue-fanart-bleach?modelVersionId=197152).

Em resumo: **_<u>antes de atribuir uma tag para um trigger, passe pelo A1111 e verifique se retorna ruído ou se aumenta ligeiramente suas necessidades</u>_**. No exemplo de contaminação negativa, eu poderia concatenar para BulmaDBZ ou o que fiz, que foi usar a grafia romanji Buruma. Uma maneira alternativa de reduzir esse problema é o uso de imagens de regularização, mas falarei sobre elas mais tarde.

### Poda

A próxima parte é a poda de tags, use o editor de tags ou vá manualmente para a pasta na qual o tagger A1111 etiquetou suas imagens. Você deve remover quaisquer tags em que seu personagem foi reconhecido se não estiver usando isso como um trigger. Para facilitar o uso, recomendo podar todas as tags de características do personagem (cabelo longo, batom, cor do cabelo, cor dos olhos, estilo de cabelo) exceto o tamanho dos seios (Se você deixar os seios dela estáticos, as pessoas vão reclamar que são grandes demais ou pequenos demais, acredite, isso é uma coisa). O LORA será mais rígido, mas muito mais fácil de usar, pois não exigirá tags auxiliares para produzir seu personagem. Eu faço a poda usando a função de substituição do tagger, remoção em massa ou manualmente usando a opção de pesquisa->encontrar em arquivos do notepad++ e fazendo uma substituição, por exemplo, "Bulma, " em troca de "". Lembre-se de limpar tags errôneas ou supérfluas. **Usando o editor de tags, essa tarefa é fácil, basta dar uma olhada na lista de tags e verificar as mais absurdas e clicar nelas, a aba filtrará as imagens e mostrará as ofensivas, então você pode excluir ou mudar a tag para uma apropriada**.
Abaixo adiciono um exemplo de como podar usando remoção em massa, neste caso estou trabalhando em uma pasta que já separei por conceito (uma roupa) e estou fazendo dois triggers, um trigger de personagem e um trigger de roupa. O trigger de personagem existe em outras pastas, então posso podar livremente suas tags como 1girl ou cabelo longo. Eu também podo partes individuais de sua roupa para que sejam absorvidas pelo trigger de roupa, como uniform, military, detached sleeves, etc.
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/485f5bd9-e0b1-46a6-8f09-1a4635e58575/width=525/485f5bd9-e0b1-46a6-8f09-1a4635e58575.jpeg)
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/d853513a-1fe2-4a0d-ae6c-8493686c91af/width=525/d853513a-1fe2-4a0d-ae6c-8493686c91af.jpeg)

### Implementação de Trigger

A implementação de um trigger é tão simples quanto adicionar uma nova tag às legendas. Claro, com sua simplicidade vem um monte de complicações. E os triggers podem ser separados em níveis pelo esforço requerido:

-   Nível 0 Sem trigger: Apenas não faça nada. Isso resultará em um lora no qual você precisa inserir cada característica do que está tentando exibir. Será extremamente flexível, mas pouco confiável. Este é parte do estilo fluido LORA e exigirá acesso ao dataset e ao resumo de tags para realmente saber do que o lora é capaz. **Resumo: simples de fazer, difícil de usar.**
    
-   Nível 1 Triggers fluidos, também conhecido como a rota preguiçosa: simplesmente não pode nenhuma das tags e apenas adicione a tag de trigger a todas as imagens. O benefício da rota preguiçosa é que o usuário poderá mudar praticamente qualquer coisa na aparência do personagem. A desvantagem é que se o usuário usar apenas o trigger, o personagem apenas vagamente se parecerá consigo mesmo (pois o trigger só absorveu uma pequena parte de todos os outros conceitos) e exigirá tags de suporte extras como cor dos olhos e estilo de cabelo para combinar totalmente com seu eu correto. **Resumo: simples de fazer, pouco confiável de usar.**
    
-   Nível 2 Triggers estáticos, também conhecido como a maneira "rígida": é adicionar a tag de trigger a todas as imagens e depois podar todas as características intrínsecas do personagem como cor dos olhos, estilo de cabelo (rabo de cavalo, franja, etc.), cor da pele, características físicas notáveis, talvez alguns ornamentos de cabelo ou tatuagens. O benefício é que o personagem aparecerá praticamente como esperado ao usar o trigger. Por outro lado, lutará contra os usuários se eles quiserem mudar a cor do cabelo ou dos olhos. **Resumo: dificuldade normal de fazer e fácil de usar, mas muito rígido.**
    
-   Nível 3 Triggers semi estáticos, também conhecido como o jeito personalizado ("chique"): depende de saber exatamente o que podar e requer mais do que familiaridade passageira com o personagem ou conceito em questão. Por exemplo, se um personagem pode mudar a cor do cabelo, então não pode isso. Se os penteados do personagem são icônicos, então adicione um trigger para cada um! O personagem usa um estilo particular para um vestido particular? Apenas pode esse penteado sempre que aparecer em combinação com a roupa para incorporar o penteado nela. Além disso, no meu caso, nunca podo o tamanho dos seios, pois as pessoas começarão a reclamar: "Por que os peitos da Misato são tão grandes?" Ao que você inevitavelmente terá que responder "apenas peça ela com small_breasts!" ou "Peitos grandes são grandes porque estão cheios dos sonhos e esperanças da humanidade!" Este último método é obviamente o melhor. **Resumo: dificuldade moderada, relativamente fácil de usar e flexível. IE. isso é o que você quer.**
    
-   Nível 4 Multi trigger: igual ao acima, mas com mais diversão e diagramas de Venn!

Os seguintes são os tipos mais comuns de triggers:

-   Personagem: Para estes você deve remover cor dos olhos, cor do cabelo, maquiagem (e batom se for comum no personagem), partes específicas da anatomia (por exemplo, Rikka Takarada de ssss.gridman é conhecida por suas coxas grossas). **Nunca pode o tamanho dos seios.** As pessoas realmente ficam irritadas por não poderem mudar o tamanho dos seios.
    
-   Roupas: Para marcar combinações específicas de guarda-roupa, um penteado específico ou arma, você vai querer remover todas as tags para as partes individuais do traje ou item em questão. Por exemplo, se algum personagem usa um vestido vermelho, saltos altos vermelhos e um colar amarelo. Você deve excluir essas tags individuais e substituí-las todas por uma tag personalizada “OutfitName”.
    
-   Poses: Estas são mais comuns como seu próprio tipo de LORA. Pode qualquer coisa relacionada à sua pose. Se o WD não pegou nada que possa ser podado, você terá que se apoiar em comparação e repetição para fazer o SD entender sua pose. O que quero dizer? Você precisa de tantas imagens quanto possível onde a única diferença seja sua pose existente ou não, de preferência o mais simples possível para que sua pose seja proeminente e clara.
    
-   Situações: Igual a poses e inclui cenários (como uma escola). Simplesmente exclua qualquer coisa relacionada à situação captada pelo WD. Treinar uma situação depende principalmente da repetição. Por exemplo, lembro-me de alguém fazendo um lora de traição. Basicamente um casal se beijando e um homem entrando na sala surpreso. O que você faria? Bateria em ambos? Hm não... embora catártico, isso o levaria para a cadeia e você nunca terminaria seu lora! Primeiro, vamos analisar o que compõe a cena: 3 pessoas, 2 se beijando, 1 surpreso e é isso. Então, exclua 2boys, multiple_boys, kissing, 1girl, door, breaking_and_entering... etc. Você entendeu a ideia. Então, divida a situação que você quer em sua forma mais simples e pode isso. Depois, confie na repetição e comparação para que seu lora aprenda o que é e o que não é sua situação.
    
-   Objetos: Para objetos desconhecidos (Desconhecidos para o SD) você tem que confiar inteiramente na comparação. Suas imagens devem ser o mais simples possível e, se possível, ter o objeto em isolamento. Basicamente para que o SD diga que conhece tudo, exceto aquela coisa! Tenho uma tag vazia, então deve significar que aquela coisa é aquela tag! Tome como exemplo o keystaff (basicamente um grande bastão em forma de chave) de Elhazard ou Sailor Moon. Como você o treinaria? Simples! adicione algumas imagens com o personagem com o bastão, algumas sem e algumas do próprio bastão. Teoricamente, se você tiver sorte, o SD deve aprender exatamente o que é um keystaff e até um pouco sobre como ele é usado.

Nas seções seguintes, focaremos no nível 4, pois se você conseguir entendê-los, deve ser capaz de criar qualquer um dos níveis inferiores.

### Adicionando um trigger

Enfim, depois de excluir quaisquer tags problemáticas, é hora de inserir seus triggers. Para fazer isso no editor de tags, vá para batch edit captions -> search and replace, certifique-se de que a caixa de texto superior esteja vazia e coloque sua tag na caixa de texto abaixo e clique em aplicar. Isso adicionará a tag a todos os arquivos de legenda. **Você deve marcar a caixa de seleção prepend additional tags para garantir que as tags sejam adicionadas no início.**

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/e7d4610e-a5d5-4ec0-b40b-27d9ab15bdc2/width=525/e7d4610e-a5d5-4ec0-b40b-27d9ab15bdc2.jpeg)  
Depois, você precisa classificar em ordem decrescente. **Cuidado que seu trigger pode não acabar como o primeiro, pois após a frequência, os triggers são adicionados por ordem alfabética. Isso pode ser um problema se você estiver usando caption drop, warmup ou shuffle durante o processo de treinamento. Nesse caso, primeiro classifique e depois adicione o trigger.**  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/e398bd91-2838-4fb0-be9a-5a4727684681/width=525/e398bd91-2838-4fb0-be9a-5a4727684681.jpeg)  
Uma alternativa nativa do Windows para fazer isso é selecionar a pasta com as legendas e pressionar ctl+shift+clique direito e selecionar abrir uma janela do PowerShell aqui. Lá você pode executar o seguinte comando:

```
foreach ($File in Get-ChildItem *.txt) {"Tag1, tag2, " + (Get-Content $File.fullname) | Set-Content $File.fullname}
```

No comando, você precisa alterar "Tag1, tag2, " para sua tag ou tags de trigger. O comando inserirá as novas tags no início de cada arquivo de legenda. Eu prefiro essa abordagem mesmo quando uso o editor de tags, pois ela inserirá meu trigger indubitavelmente como a primeira tag em todos os arquivos, pode ser superstição (ou talvez não), mas gosto assim.  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/2f15d12a-3154-4e61-860c-54c0014dc806/width=525/2f15d12a-3154-4e61-860c-54c0014dc806.jpeg)

### LORAs Multi Trigger ou como aprendi a parar de me preocupar e amar os diagramas de Venn

LORAs Multi Trigger são basicamente diagramas de Venn nos quais você deve combinar as combinações de triggers, tags e poda para fazer o SD entender o que cada um dos seus triggers significa.

Exemplo 1 e 2:

Você tem um grupo de imagens de um personagem. Infelizmente, seu personagem está usando a mesma roupa em todas as imagens.

Suponha que você queira um trigger para CharacterDress e character. Como todas as imagens têm a roupa, você poda as características do personagem e exclui a tag da roupa, depois adiciona ambos os triggers a todas as imagens. O resultado é que ambos os triggers fazem exatamente a mesma coisa. Veja o primeiro exemplo na imagem abaixo. Este resultado é inferior, pois os 3 conceitos podados são divididos entre os 2 novos triggers e a única salvação seria que o trigger CharacterDress contém a palavra "dress", o que o empurraria para o papel de roupa.

**Nota: A única maneira adequada de marcar uma única roupa é: se você tiver uma boa quantidade de imagens, 80+, pegue aquelas em que a roupa é melhor representada, separe-as e marque o trigger da roupa, podando as tags relacionadas, as imagens restantes deixe sem poda, adicionando o trigger do personagem a ambas, isso provavelmente acabará fortemente tendencioso para fazer aquela roupa, mas deve ser um pouco controlável. Se, por outro lado, você não tiver imagens suficientes para dividi-las, então simplesmente duplique-as, marque ambas com o trigger do personagem, pode um conjunto e adicione o trigger da roupa e deixe o outro sem poda e sem o trigger da roupa.**

O próximo exemplo seria se você tivesse pelo menos 2 roupas aparecendo no seu dataset, dividindo-o em dois grupos (esquerda e direita), neste caso devemos garantir a sobreposição, por exemplo, se ambas as roupas têm um laço, é provável que o trigger do personagem obtenha o conceito de laço em vez das roupas individuais, então seria melhor não podar o laço. Enfim, é um jogo de matemática, se A é o trigger do personagem e B e C são roupas. Primeiro atribuímos os triggers de roupas ao grupo correspondente, então adicionamos B ao grupo esquerdo e C ao grupo direito. Ambos os grupos compartilham o trigger do personagem "A", então esse é adicionado a ambos.

Agora, o SD sabe que qualquer coisa podada no grupo "direito" que não seja podada no grupo "esquerdo" deve pertencer ao conceito C. O oposto também é verdadeiro, qualquer coisa podada no grupo "esquerdo" que não seja podada no grupo "direito" deve pertencer ao conceito B. Pela mesma lógica, o conceito "A" existe em ambos os grupos, então os conceitos podados em ambos os grupos pertencem a "A".

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/aa026d90-9328-4465-bb2e-d37cd9ae1b59/width=525/aa026d90-9328-4465-bb2e-d37cd9ae1b59.jpeg)

Exemplo 3:

Neste caso, você tem um dataset maior, o grupo 1 de imagens tem o personagem e um vestido vermelho específico, o grupo 2 contém o mesmo personagem e um vestido rosa, o grupo 3 contém também o mesmo personagem, mas com muitos vestidos de cores diferentes com poucas imagens individualmente para formar seu próprio grupo.

Suponha que você queira um trigger para Outfit1, Outfit2 e personagem. Como todas as imagens têm um vestido, você poda as características do personagem e exclui as tags de vestido nos grupos 1 e 2, depois adiciona o trigger do personagem a todas as imagens e as tags de roupa aos seus respectivos grupos. O resultado é que, como o grupo 3 não tem sua tag de vestido podada, o Lora sabe que vestido não faz parte do trigger do personagem, quanto aos triggers de roupa, não há problema com eles, pois não há sobreposição.  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/18e32561-01e3-44f4-af5f-5178c55c0f04/width=525/18e32561-01e3-44f4-af5f-5178c55c0f04.jpeg)

Exemplo 4:

Neste caso, temos 3 grupos de imagens, todos do mesmo personagem, o grupo 1 tem um vestido rosa e uma katana, o grupo 2 um vestido azul e a mesma katana e o grupo 3 um vestido vermelho.

Suponha que você queira um trigger para ChDress1, ChDress2, ChDress3, CharacterSword e personagem. Todas as imagens têm um vestido, então você poda as características do personagem e exclui todas as tags de vestido, depois adiciona o trigger do personagem a todas as imagens e as tags de vestido do personagem aos seus respectivos grupos. Você também adiciona o trigger da espada às imagens em que se aplica. O resultado é que, como todos os grupos tiveram suas tags de vestido podadas, o Lora pensa que vestido faz parte do trigger do personagem, os triggers de ChDress serão diluídos e podem não acionar tão fortemente quanto deveriam, felizmente a cor e outras tags mais ocultas também farão a diferença, mas o resultado será um pouco diluído em comparação com a separação de conceitos perfeita. O personagem também terá uma tendência a aparecer em um vestido, independentemente da cor. Quanto ao trigger CharacterSword, como aparece em um subconjunto bem definido de imagens, deve acionar corretamente. Uma solução para essa situação é comumente um grupo misc de imagens com tags não podadas para ensinar claramente ao lora que esses laços, vestidos, acessórios, etc., não fazem parte do trigger do personagem.  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/ec1afe9a-5168-4cf3-96c3-8c5eddfc3671/width=525/ec1afe9a-5168-4cf3-96c3-8c5eddfc3671.jpeg)

LORAs Multi Trigger podem rapidamente escalar em complexidade dependendo da quantidade de triggers que você está criando, também requer um dataset maior com limites claramente definidos. Além disso, pode exigir alguns ajustes de repetições (Veja a seção de Pastas) para aumentar o treinamento de triggers com menos imagens. Por exemplo, se você tem 20 imagens de outfitA, 10 de outfitB e 5 de outfitC, seria melhor classificá-las em pastas como 10_OutfitA, 20_OutfitB e 40_Out

fitC, dessa forma todas terão aproximadamente o mesmo peso no treinamento (20*10=10*20=5*40).

===============================================================

## Imagens de Regularização

Então, agora que você tem todas as suas imagens bem organizadas, você se pergunta o que diabos são as imagens de regularização. Bem, imagens de regularização são como prompts negativos, mas não exatamente; elas trazem seu modelo de volta para um estado "neutro". De qualquer forma, a menos que você realmente precise delas, ignore-as. Normalmente, elas não são usadas em LORAs, pois não há necessidade de restaurar o modelo, já que você pode simplesmente diminuir o peso do LORA ou desativá-lo. Existem alguns métodos para criá-las, assim como alguns usos teóricos listados abaixo.

### O que são imagens de Reg

Até onde eu sei, as imagens de regularização são tratadas internamente pelos scripts de treinamento da mesma maneira que as imagens normais de treinamento, mas sem penalidades de perda. Ou seja, as ferramentas de treinamento estão simplesmente removendo filtros e confiando que você sabe o que está fazendo. É como dizer que 1x1=1. Em outras palavras, se suas imagens de regularização foram feitas pelo modelo, qualquer informação extra deve ser mínima e introduzida pela semente aleatória.

Então, por exemplo, queremos ensinar ao nosso modelo o conceito **A** e temos 2 imagens, uma com os conceitos **A** e **B** e uma imagem de regularização com o conceito **B**. A cada repetição da primeira imagem, o modelo aprende um pouco de **A** e o conceito **B** é modificado levemente. A cada repetição da imagem de reg, o novo conceito **A** no modelo permanece intocado e o conceito **B** modificado é atualizado com o conceito **B** da imagem de reg, teoricamente retornando-o ao mais próximo do original possível. Abaixo tento explicar isso de forma mais abstrata.

Até onde eu entendo, considere o modelo como M, L é o LORA. L consiste de NC e MC onde NC são novos conceitos e MC são conceitos modificados do modelo. Finalmente, temos R das imagens de regularização, R é parte do Modelo como foi criado por imagens inerentes ao modelo. Se feito corretamente, R é também, esperançosamente, a parte de M que está sendo sobrescrita pela parte MC do LORA.

Sem imagens de regularização

L = M + NC + MC - M = NC + MC

Com imagens de regularização

L = M + (NC + MC) - M - R

mas tentamos fazer R equivalente a MC, assim

L = M + NC - M = NC

Agora um exemplo mais concreto. Temos um conjunto de dados que ensina **1girl**, **red_hair** e character1, o modelo já conhece red_hair e 1girl, mas elas são diferentes do conjunto de dados, então não estão em negrito. As imagens de regularização contêm 1girl e red_hair do modelo.

M = 1girl, red_hair, etc.

NC = character1

MC = **1girl, red_hair**

R = 1girl, red_hair

Sem imagens de regularização

L = 1girl, red_hair, etc. + character1 + **1girl, red_hair** - 1girl, red_hair, etc.

L = character1 + **1girl, red_hair**

Com imagens de regularização

L = 1girl, red_hair, etc. + character1 + **1girl, red_hair** - 1girl, red_hair, etc. - 1girl, red_hair

L = character1 + **1girl, red_hair** - 1girl, red_hair

L = character1 + restos de (**1girl, red_hair** - 1girl, red_hair)

Depende da sorte até que ponto "**1girl, red_hair** - 1girl, red_hair" se cancelam ou se misturam, mas deve estar mais próximo dos originais do modelo, independentemente.

Obviamente, há uma maneira matemática verdadeira de descrever isso; isso é apenas uma maneira de tentar simplificar.

### Usos de imagens de Reg

Para mim, elas têm apenas três usos realistas:

1. O uso principal é para neutralização de estilo, suponha que você queira treinar uma pose. Você pega todas as suas imagens bonitinhas e as treina. O conceito é muito bem representado e funciona bem, exceto que tudo parece estar em estilo cyberpunk. Afinal, você usou apenas imagens cyberpunk para treiná-lo. É para isso que servem as imagens de reg; se as imagens de reg forem feitas e marcadas corretamente, elas forçarão todos os conceitos, exceto seus gatilhos, para mais perto do modelo original, esperançosamente se livrando do estilo.

2. Mitigar a contaminação do seu gatilho. Suponha que eu queira treinar um personagem chamado Mona_Taco. O resultado será contaminado com imagens da Monalisa e tacos. Então você pode ir ao A1111 e gerar um monte de imagens com o prompt Taco e Mona e jogá-las na sua pasta de regularização com suas legendas apropriadas. Agora, seu Lora saberá que Mona_Taco não tem nada a ver com a Mona Lisa nem com tacos. Alternativamente, basta usar uma tag diferente ou concatená-la, MonaTaco provavelmente funcionará bem por si só, sem os passos extras. Ainda recomendaria usar uma palavra sem sentido que retorne ruído.

3. Para dreambooth, elas são simplesmente uma necessidade; caso contrário, seu modelo, em vez de ser ajustado finamente e aprender coisas novas, se transformará em um modelo diferente. Isso não é ruim por si só, mas normalmente não é o objetivo de um modelo dreambooth.

### Como fazer imagens de Reg

Até onde eu sei, existem várias opiniões sobre como criar imagens de reg. Vou listar apenas 2: o método dreambooth e minhas próprias divagações estranhas.

- Dreambooth: modelos dreambooth são frequentemente "legendados" simplesmente usando uma classe e uma subclasse na estrutura da pasta, por exemplo, 10_SailorMoon_1girl. Como esperado, 10 são as repetições, SailorMoon é o gatilho e 1girl é a classe. Para criar as imagens de reg necessárias, você deve gerar a mesma quantidade de imagens que há dentro da pasta; elas devem ser geradas pelo modelo usado para treinar e criadas com a classe, ou seja, 1girl.

  Então, se você tem 100 imagens na pasta Sailor Moon, você precisa fazer 100 imagens com o prompt: 1girl

- KNXO adhoc: Este é o método que uso para fazer minhas imagens de reg caso eu precise delas.

  - Primeiro de tudo, renomeie suas imagens por número. Aqui está um renomeador em lote chamado Power Rename  que faz parte do programa [PowerToys](https://github.com/microsoft/PowerToys/releases), feito pela Microsoft.
    
    - Primeiro, selecione todas as suas imagens e clique com o botão direito selecionando Power Rename no menu.
      
      ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/91fcbe8b-97ab-41ed-b8bf-f0f3433444d8/width=525/91fcbe8b-97ab-41ed-b8bf-f0f3433444d8.jpeg)
      
    - Essas configurações devem ajustar seu conjunto de dados para 3 dígitos (000.png ... 999.png)
      
      ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/359a5b49-78d1-4855-a67f-a94fdc3954fe/width=525/359a5b49-78d1-4855-a67f-a94fdc3954fe.jpeg)
      
  - Legende suas imagens como gatilhos normais e tudo mais.
    
  - Abra uma janela cmd e navegue até a pasta onde suas imagens e legendas estão armazenadas.
    
  - Execute:
    
    ```
    FOR %f IN (*.txt) DO type %f >> newfile.txt & echo. >> newfile.txt
    ```
    
  - Você deve obter um arquivo grande com um prompt em cada linha.
    
  - Faça um substituir tudo para remover seus gatilhos ou conceitos que você não quer que sejam regulados, por exemplo, "Trigger1, " para ""
    
  - Selecione o modelo que você usará para treinar no A1111 e selecione prompts de arquivo ou caixa de texto na seção de script.
    
  - Copie e cole o conteúdo do seu arquivo no A1111, você também pode clicar no botão de upload e procurá-lo.
    
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/fcc026af-0c5a-4b5b-ae96-eec81cc9b292/width=525/fcc026af-0c5a-4b5b-ae96-eec81cc9b292.jpeg)
    
  - Gere as imagens. Elas devem estar na mesma ordem que suas imagens originais devido à conversão de nome.
    
  - Copie suas imagens de reg para uma pasta e renomeie-as da mesma forma que acima, esperançosamente a ordem será mantida.
    
  - Faça uma cópia de seus arquivos de legenda originais e cole-os nas pastas de suas imagens de reg.
    
  - Remova seus gatilhos e tags não reguladas das legendas das imagens de reg. Recomendo usar o editor de tags de dataset no A1111 ou a função de busca em arquivos do notepad++
    
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/b47e2669-0a98-4bfb-adee-83a49ec10078/width=525/b47e2669-0a98-4bfb-adee-83a49ec10078.jpeg)
    
  - Finalmente, revise as imagens de reg para aberrações e reexecute manualmente as que não passaram no critério. Lembre-se de que as imagens de reg são treinadas como imagens normais, então mãos ruins e corrupção também serão treinadas de volta no modelo.
        

===============================================================

## Assando o LORA/LyCORIS

Agora o passo que você estava esperando: a assadura. Honestamente, a opção padrão funcionará bem 99% das vezes. Talvez você queira diminuir a taxa de aprendizado para estilo. Mas de qualquer forma, para um Lora, você deve abrir o run.bat dos LoRA_Easy_Training_Scripts. Recomendo nunca iniciar o treinamento imediatamente, mas salvar o arquivo TOML do treinamento e revisá-lo primeiro. **A versão pop-up do script parece ter sido substituída por uma interface adequada, então faça um pull se você ainda tiver o run_popup.bat.**

1.  Argumentos Gerais:
    
    -   Selecione um modelo base: Coloque o modelo com o qual você deseja treinar. Recomendo um da família NAI (o original ou um dos "Any" ou "AOM" originais ou misturas) para Anime. Eu gosto do [AnythingV4.5](https://civitai.com/models/4855?modelVersionId=5581) (sem relação com o Anything V3 ou V5). A versão pruned safetensor com o checksum 6E430EB51421CE5BF18F04E2DBE90B2CAD437311948BE4EF8C33658A73C86B2A. Houve muito drama porque o autor usou o esquema de nomes dos outros modelos Anything. Para ser honesto, eu simplesmente gosto do estilo quase 2.5D (mais próximo do 2D do que do 2.5D), acho melhor que o V3 ou V5 e tem melhor suporte NSFW.
        
    -   VAE Externo: caso o VAE do seu modelo seja ruim, corrompido ou de baixa qualidade. Supostamente alguns VAEs oferecem pequenos ganhos em cor e clareza, mas não é realmente necessário e os ganhos parecem ser marginais, a menos que o seu modelo de treinamento esteja comprometido de alguma forma. Só é usado no início do treinamento para transformar suas imagens de treinamento em latentes (e talvez para as imagens de amostra). Se você estiver usando um VAE externo, recomendo usar um neutro como clearvae.
        
    -   Modelo SD2: Não. A família NAI é baseada no 1.5.
        
    -   Checkpoint de Gradiente: Medida de economia de VRAM aumentará o tempo por iteração entre 30~100%, então espere que o tempo de treinamento demore o dobro do esperado. Atualmente, parece ser a única maneira de treinar modelos DORA com 8 GB de VRAM.
        
    -   Acumulação de Gradiente: Simula tamanhos maiores de batch para amortecer a Taxa de Aprendizado (para tentar aprender mais detalhes).
        
    -   Seed: Apenas coloque seu número da sorte.
        
    -   Pular Clip: tem a ver com a camada do codificador de texto, se bem me lembro, a maioria dos modelos de anime usa 2. Algumas pessoas realmente chamam isso de "a maldição do NAI" porque originou-se do modelo deles. A maioria dos modelos fotorrealistas usa 1.
        
    -   Peso da Perda Prioritária: sem ideia, apenas deixe em 1.
        
    -   Precisão do Treinamento: escolha fp16, deve ser o mais compatível.
        
    -   Tamanho do Batch: A quantidade de imagens por batch depende da sua vram, com 8 GB você pode fazer 2 ou 1 se estiver usando espelhamento de imagem, então selecione apenas 1, a menos que seu personagem seja assimétrico (um rabo de cavalo, tapa-olho, rabo de cavalo lateral, etc.). O otimizador Prodigy usa mais VRAM do que o AdamW, então cuidado, você pode precisar diminuir o tamanho do batch.
        
    -   Comprimento do Token: é literalmente o tamanho máximo da palavra para as tags, eu vi pessoas usando strings estilo clip ("Um burrito gigante comendo um humano em um ambiente urbano") em marcação estilo danbooru, **não seja essa pessoa**. Recomendo gatilhos longos apenas quando você precisar tocar na contaminação positiva do modelo base, especialmente para roupas complexas que não fazem muito sentido ou o modelo tem dificuldade com cores ou partes dela, como: red_skirt_blue_sweater_gray_thighhighs_green_highheels. (Fazer isso deve ajudar a estabilizar a saída, se você usasse um gatilho genérico, é uma aposta se o modelo escolherá as cores corretas para a roupa).
        
    -   Tempo Máximo de Treinamento: depende do seu dataset. Normalmente uso 8 épocas para 10 repetições (X_Name torna-se 10_Name) para 100 a 400 imagens. Ou entre 8000 e 32000 passos (Isso não é para múltiplos conceitos). Isso é para adamW, para prodigy apenas corte pela metade. Para um único conceito você precisa entre 1500 e 3000 passos para prodigy e o dobro para adamw.
        
    -   Xformers: Para usar a biblioteca xformers para reduzir o consumo de VRAM. Eu realmente tive algum aumento na velocidade de treinamento com a última versão do xformers, para instalar, entre no venv e faça "pip install -U --pre xformers".
        
    -   SDPA: Alternativa ao xformers, funciona mais ou menos da mesma forma para mim.
        
    -   Cache de latentes: armazene as imagens na vram para manter o uso da vram estável.
        
    -   Para disco: Armazena os latentes das imagens processadas no disco para economizar vram (pode desacelerar as coisas).
        
    -   Manter separador de tokens: alternativa à opção manter tokens na seção de subconjuntos de dados, em vez de selecionar um número de tags, elas são separadas por um caractere especial. Parece que o sugerido é |||. Então uma legenda como "CharaA, OutfitB, ||| red_eyes, black_hair, blue_dress" manterá "CharaA, OutfitB," estáticas e aplicará opções selecionadas como drop out de legenda ou shuffle para as outras. Acho que o aquecimento de tokens agiria sobre "CharaA, OutfitB,".
        
    -   Comentários: Lembre-se de colocar seus Triggers no campo de comentários. Se alguém encontrar seu LORA por aí, poderá verificar os metadados e usá-lo. **_<u>Não seja aquela pessoa que deixa seus LORAS órfãos por aí sem ninguém poder usá-los.</u>_**
        
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c65e97ec-e856-4a63-930f-c83418baa4de/width=525/c65e97ec-e856-4a63-930f-c83418baa4de.jpeg)  
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/be0a448f-aafd-4eda-bd9b-117329e87ef4/width=525/be0a448f-aafd-4eda-bd9b-117329e87ef4.jpeg)
        
2.  Subconjuntos de Dados:
    
    -   Dir da pasta de imagens: Selecione sua pasta de imagens, aquelas com o formato X_name, o número de repetições deve ser preenchido automaticamente. Para adicionar mais pastas, clique em adicionar subconjunto de dados no topo.
        
    -   flip augment: Se seu personagem for simétrico, lembre-se de habilitar o flip augment.
        
    -   Manter token: ele torna a quantidade definida de tokens (tags delimitadas por vírgula contadas da esquerda para a direita) estática para que não sejam afetadas por shuffle captions, caption drop e tenho certeza de que também warmup. Resumo: Ative se você adicionou qualquer outra opção de "legendas". Lembre-se de que as primeiras tags no arquivo são processadas primeiro e absorvem conceitos primeiro. Por exemplo, se você definir para duas e usar a seguinte legenda "1girl, 1boy, long_hair, huge_breasts, from_behind", os dois primeiros parâmetros "1girl, 1boy" permanecerão estáticos e o resto será embaralhado, descartado ou adicionado lentamente.
        
    -   shuffle captions: Faz o que diz na lata, é útil, pois as legendas absorvem conceitos na ordem da esquerda para a direita, então se você embaralhar, teoricamente as legendas aprenderão coisas ligeiramente diferentes em cada repetição. Não é realmente necessário a menos que você tenha um dataset extremamente homogêneo.
        
    -   Extensão da legenda: o padrão é armazená-lo como arquivos txt comuns. Ainda não vi um diferente.
        
    -   Imagens de regularização: Expliquei acima, a resposta comum é não usá-las, mas se você usar, ative a pasta como uma pasta de regularização aqui.
        
    -   Crop aleatório: Este é antigo, acho que é uma alternativa ao bucketing e faz um mosaico de uma imagem maior para processá-la. Não tenho certeza se aplica a legenda igualmente a cada mosaico. Mutuamente exclusivo com cache de latentes.
        
    -   augment de cor: Acho que este ajusta os valores de saturação para amostrar melhor as imagens, não me cite nisso. Mutuamente exclusivo com cache de latentes.
        
    -   Crop de rosto: Pelo que eu sei, atua como um augment fazendo um corte focado no rosto do personagem. Costumava ser mutuamente exclusivo com cache de latentes (não tenho certeza agora). Não tenho certeza sobre sua confiabilidade.
        
    -   Drop out de legenda: Acho que este começa a descartar as legendas da direita para a esquerda. Pode ser útil para estilo para evitar que tags individuais sejam queimadas, pois tudo é lentamente concentrado na primeira tag (mais à esquerda), que deve ser o gatilho. Deve ser usado com manter token.
        
    -   Dir de imagens mascaradas: Esta opção só é ativada ao selecionar treinamento mascarado na aba de argumentos do otimizador. As imagens dentro desta pasta devem corresponder ao nome e quantidade das imagens na pasta acima.
        
    -   Aquecimento de tokens: oposto do drop out de legenda. Começa treinando mais e mais tags à medida que o tempo passa. Este, eu acho, está na ordem nos arquivos de legenda da esquerda para a direita.  
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/abb50f59-e417-47b3-8267-75dad4527107/width=525/abb50f59-e417-47b3-8267-75dad4527107.jpeg)  
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/88f984ae-79b3-436e-bc15-c475d2aa505d/width=525/88f984ae-79b3-436e-bc15-c475d2aa505d.jpeg)
        
3.  Argumentos da Rede:
    
    -   Tipo: aqui você pode escolher o tipo de LORA. LyCORIS requer alguns scripts extras e demoram mais para treinar. Escolha lora, a menos que seu cartão seja rápido e tenha os scripts necessários para usar Lycoris. Aqui estão alguns detalhes que eu sei sobre LORa e de lycoris (posso estar errado):
        
        -   Lora: O normal que todos conhecemos e amamos.
            
        -   Dora: Opção para dividir a direção e magnitude nos vetores durante o treinamento. Parece dar resultados ligeiramente melhores do que LOCON, mas requer pelo menos cerca de 40% mais vram. para 1.5, se você estiver treinando usando 8GB de Vram, você precisará ativar **Checkpoint de Gradiente** com a penalidade de velocidade associada. Para SDXL, 12GB pode não ser suficiente, mas ainda não confirmei.  
            Dora é aplicável a LOCON, LOHa e LOKR e está atualmente disponível nas ramificações de desenvolvimento dos scripts Easy Training do derrians e do Kohya ss do bmaltais.  
            ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/14797dcd-88a3-449f-991a-2078279896cc/width=525/14797dcd-88a3-449f-991a-2078279896cc.jpeg)
            
        -   Locon (Lycoris): pega mais detalhes e isso pode ser uma coisa boa em objetos intrincados, mas lembre-se da qualidade do seu dataset, pois também captará ruídos e artefatos mais fortemente. Tem uma ligeira vantagem em loras de múltiplos trajes, pois os detalhes extras ajudam a diferenciar os trajes, limitando um pouco a mistura (melhoria muito leve).
            
        -   Locon (Kohya): Implementação mais antiga do LOCON. Eu esperaria que fosse um pouco pior, mas não tentei.
            
        -   Loha: tamanhos de arquivo menores, parece produzir alguma variabilidade no estilo (mais nítido? gótico?) que algumas pessoas gostam.
            
        -   Lokr: semelhante ao Loha em tamanhos menores, usa um algoritmo diferente.
            
        -   IA3: tamanhos menores, treinamento mais rápido e captura de estilo extrema. Como treina apenas um subconjunto dos valores que um lora faz, é pequeno e rápido de treinar. Funciona bem com metade dos passos de um lora, tornando-o 200~300 passos por época para Prodigy e 500 para adamW (eu só testei para prodigy). Testei a compatibilidade com outros modelos e não é tão ruim quanto afirmado. No geral, eu não gosto dele como produto final, mas para prototipagem e depuração do dataset parece ser uma ótima opção devido à rapidez com que é treinado. Aqui estão meus resultados usando prodigy [https://civitai.com/models/155849/iori-yoshizuki-ia3-experiment-version-is](https://civitai.com/models/155849/iori-yoshizuki-ia3-experiment-version-is). Como pode ser visto, ele captou fortemente o estilo do dataset sendo principalmente em preto e branco e doujins coloridos. Honestamente, gosto mais dos resultados do LOCON e do LORA, pois absorvem mais do modelo base, preenchendo quaisquer lacunas.
            
        -   Dylora: o LORA dinâmico é o mesmo que o LORA normal, mas treina vários níveis de dim e alpha. Deve ser mais lento para treinar, mas o LORA final deve permitir que você use o lora como se tivesse treinado o mesmo lora várias vezes com parâmetros diferentes em vez de sólidos. Assim, permitindo que você escolha a combinação perfeita.
            
        -   Diag-OFT: Um novo "tipo" de LORA. A qualidade está entre um IA3 e um LORA normal. O treinamento leva cerca de 1,5 vezes o tempo por iteração de um LORA normal, mas só precisa de cerca de 170 passos por época por conceito para 8 épocas para treinar, tornando-o comparável ao IA3. Sua fama é sua robustez contra a queima, o que se mantém. Eu o treinei demais em 3 épocas inteiras sem efeitos negativos. Aqui estão os resultados do meu teste: [https://civitai.com/models/277342/iori-yoshizuki-diag-oft-experiment-version-is](https://civitai.com/models/277342/iori-yoshizuki-diag-oft-experiment-version-is)
            
        -   **_<u>Recomendações Atuais:</u>_**
            
            -   LoRA: Dim 32 alpha 16.
                
            -   LoCON: Dim 32 alpha 16 conv dim 32 e conv alpha 16 OU Dim 32 alpha 16 conv dim 16 e conv alpha 8. Não vá além de conv dim 64 e conv alpha 32.
                
            -   LoHA: Dim 32 alpha 16 deve funcionar? Não vá além de dim 32.
                
            -   LoKR: Muito semelhante ao LOHA Dim 32 alpha 16 deve funcionar? Não vá além de dim 32. De acordo com os repositórios, pode ser necessário ajustar a taxa de aprendizado, então tente entre 5e-5 a 8e-5 (.00005 a .00009).
                
            -   IA3: Dim 32 alpha 16 deve funcionar. Necessita de uma taxa de aprendizado mais alta, atualmente recomendado é 5e-3 a 1e-2 (.0005 a .001) com adamw. Prodigy funciona bem em LR=1 (testado).
                
            -   Dylora: Para Dylora, quanto maior, melhor (eles devem sempre ser divisíveis por 4), mas também aumenta o tempo de treinamento. Então dim=64, alpha=32 parece um bom compromisso em termos de velocidade. Os passos são configuráveis no valor da unidade dylora, o valor comum é 4 dim/alpha, então após o treinamento você pode gerar 64/32, 60/32, 64/28...4/4. Obviamente, Dylora leva muito mais tempo para treinar ou todos estariam usando para a flexibilidade extra.
                
            -   Diag-OFT: Dim 2 ou 1, defina os valores de alpha e conv iguais, pois não devem ser usados de qualquer maneira.
                
    -   Dimensão da Rede: Tem a ver com a quantidade de informações incluídas no LORA. Pelo que sei, 32 é o padrão atual, normalmente aumento para um máximo de 128, dependendo da quantidade de personagens e gatilhos de traje nos meus loras. Para um único personagem LORA, 32 deve ser ok.
        
    -   Alpha da Rede: Deve ter algo a ver com a variabilidade (não tenho certeza), **<u>regra geral, use metade do valor da Dimensão.</u>**
        
    -   Treinar em: Ambos, quase sempre escolha ambos. Unet apenas, treina apenas nas imagens, enquanto text encoder treina apenas nas tags de texto.
        
    -   Configurações de conv: conv é para LOCON, recomendaria os mesmos valores que dim e alpha, desde que dim seja igual ou inferior a 64, se você estiver usando valores mais altos, o máximo que deve definir conv dim e conv alpha é 64 e 32.
        
    -   Unidade Dylora: é a divisão do dim e alpha disponíveis. Se você usar dim 16 e unidade 4, poderá ter um lora que pode produzir imagens como se tivesse loras dim16, dim12, dim8 e dim4.
        
    -   Dropouts: Apenas para descartar parâmetros aleatoriamente para aumentar a variabilidade.
        
        -   Dropout da Rede: Supostamente torna a rede mais resiliente a dados desconhecidos e inesperados. Infelizmente, no uso normal, apenas deu resultados ligeiramente piores. Talvez funcionasse melhor com um parente mais distante do modelo de treinamento?
            
    -   Ip Noise Gamma: Supostamente bom para ser definido em .1, teoricamente acelera a convergência e a qualidade da imagem. Treinei [dois modelos iguais](https://civitai.com/models/257562/reimu-hakurei-older-fanart-touhou) com apenas essa configuração alterada e eles parecem bastante semelhantes para mim. Eu esperava que aquele com ING ativado queimasse devido à convergência mais cedo, mas não. Então, talvez ativar? Na pior das hipóteses, parece não fazer nada.
        
    -   LoRA FA: Pelo que entendi, ele pega algumas lições do IA3, reduzindo o número de parâmetros e congelando alguns pesos para torná-lo menor em termos de memória e computação. Vale a pena? Não foi para o IA3. Até agora, não vi nenhuma afirmação de que diminui o tempo de treinamento e aumenta a qualidade do produto final, a maioria do que li diz que é "quase" tão bom quanto um LORA normal, então não testei, pois simplesmente não acho que vale a pena.
        
    -   Pesos dos Blocos: Para se você quiser adicionar mais granularidade às fases de treinamento. Não tenho a menor ideia de qual seria uma configuração ótima para melhor qualidade (se é que existe uma), pois o dataset tem um impacto enorme. [Aqui está um guia](https://civitai.com/articles/76/bdsqlsz-lora-training-advanced-tutorial1lora-block-training) de bdsqlsz, é o único que conheço para treinamento de blocos.  
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/47a4d5a9-63d1-4058-a050-be5cfa3d5e23/width=525/47a4d5a9-63d1-4058-a050-be5cfa3d5e23.jpeg)
        
4.  Configurações do Otimizador:
    
    -   Otimizador: Atualmente, recomendo Prodigy, AdamW ou adamW8bit. Se o seu Lora não estiver em risco de queimar, recomendo ficar com AdamW. Se, por outro lado, você estiver no limite devido a problemas no dataset, Prodigy é o caminho para limitar o overbaking. Para Prodigy, recomendaria manter as repetições totais do traje em um máximo de 500 passos (ou seja, 50 imagens 10 repetições ou 100 imagens 5 repetições) por época, pois usa uma taxa de aprendizado mais agressiva. Os níveis de qualidade entre AdamW e Prodigy [parecem iguais](https://civitai.com/images/2170018?period=AllTime&periodMode=published&sort=Newest&view=categories&modelVersionId=146784&modelId=12976&postId=530064), na imagem vinculada eu comparo Lora AdamW vs Locon AdamW vs Lora Prodigy vs Locon Prodigy e tenho dificuldade em discernir se um deles é objetivamente melhor. Portanto, praticamente mudei para o Prodigy em tempo integral, mesmo que demore cerca de 25% mais por passo, isso é compensado por exigir apenas metade dos passos por época do que AdamW, o que realmente produz ganhos consideráveis no tempo de treinamento. Talvez você queira verificar o exemplo de TOML do Scheduler Prodigy se planeja usá-lo.
        
        -   AdamW: Treinamento padrão e padrão de ouro. Funciona bem em cerca de 1000 passos por época por gatilho de conceito.
            
        -   Prodigy: Melhor otimizador adaptativo, mais lento por passo do que Adam, mas requer apenas metade dos passos por época (500), na verdade economizando algum tempo de treinamento. É como DAdapt, mas parece realmente entregar. É um otimizador adaptativo, tornando desnecessário ajustar a taxa de aprendizado. Inicialmente experimentei devido a um lora que estava treinando que estava recebendo muita contaminação do modelo, fazendo com que queimasse. Experimentei isso vs normal vs normal com taxa de treinamento reduzida vs normal com repetições reduzidas vs usando normalização. Obtive o melhor resultado com Prodigy, seguido por AdamW usando normalização e AdamW simples na parte inferior. Então, acho que o hype é real. **_<u>O Prodigy requer adicionar argumentos extras do otimizador na aba2, lembre-se de primeiro clicar em adicionar argumento do otimizador. </u>_** Estes são os argumentos recomendados:
            
            -   weight_decay = "0.01"
                
            -   decouple = "True"
                
            -   use_bias_correction = "True"
                
            -   safeguard_warmup = "True"
                
            -   d_coef = "2" pode ser definido como 1 para um treinamento menos agressivo, mas requerendo mais passos de treinamento.
                
            -   Scheduler: cosine annealed
                
            
            Deve parecer como abaixo:
            
            ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/3efc8592-9b99-4905-bb6d-42d2c6136970/width=525/3efc8592-9b99-4905-bb6d-42d2c6136970.jpeg)  
            ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/05022fa6-442e-40eb-8889-e1284d333777/width=525/05022fa6-442e-40eb-8889-e1284d333777.jpeg)
            
        -   Dadapt: Esses otimizadores tentam calcular a taxa de aprendizado ideal. Fiz alguns testes "mal-sucedidos" com DAdaptAdam, funcionou, mas simplesmente não gostei dos resultados. Isso pode mudar no futuro quando mais testes forem feitos. Esses otimizadores usam taxas de aprendizado muito altas que são calculadas na hora. Para meus testes, usei as recomendações do repositório: taxa de aprendizado=1 com scheduler constante e weight decay=1. Algumas outras pessoas recomendam LR=.5 e WD= .1 a .8. Este otimizador também demorou 25% mais tempo de treinamento. Então, não recomendo... ainda. A ideia de não precisar ajustar a taxa de aprendizado é atraente, então, espero que funcionem melhor no futuro com alguns ajustes. **_<u>DAdaptAdam também requer adicionar um argumento extra do otimizador na aba2, lembre-se de primeiro clicar em adicionar argumento do otimizador. A primeira entrada deve ser "decouple" e o valor deve ser definido como "True"</u>_**
            
    -   Taxa de Aprendizado: Para Adam, está ok no padrão (.0001), diminua para estilo (.00001-.00009), para Prodigy deve ser 1.
        
    -   Taxas de aprendizado do codificador de texto e do Unet: isso é se você não quiser uma global. Acho que se o Unet for muito alto, você obtém deep fried e se o TE for muito alto, você obtém gibberish (deformado).
        
    -   Schedulers de Taxa de Aprendizado: Tecnicamente importante, pois manipulam a taxa de aprendizado. Na prática? Apenas selecione "cosine with restart" para AdamW e para Prodigy "annealed cosine with warmup restarts" deu-me bons resultados. Vi algumas comparações e esses produzem resultados satisfatórios.
        
    -   Tipo de Perda:
        
        -   L2 Loss é a perda padrão que todos conhecemos e amamos, tende a descer à medida que o modelo é treinado, tendo alguns vales que muitas vezes indicam as melhores épocas.
            
        -   Huber loss, por outro lado, permanece principalmente estática durante o processo de treinamento, atua como um SRN gamma mínimo alto, comendo alguns detalhes e, em troca, tornando a saída muito mais estável. Embora não seja mutuamente exclusivo, eu recomendaria usar um ou outro.  
            **Recomendo usar Huber loss apenas para iniciantes, pois torna o processo de treinamento muito mais tolerante** enquanto produz resultados ligeiramente inferiores aos que um dataset de alta qualidade limpo produziria com L2. Em outras palavras, funciona melhor do que L2 com um dataset de baixa qualidade e previne o overfitting enquanto faz as coisas parecerem ligeiramente insossas. Pelo que sei, Huber loss implementa L2 Loss no início e no final do treinamento e L1 smooth loss nos extremos, também vem em 3 variedades:
            
            -   SNR: que come detalhes dependendo de quão ruidoso é o dataset. Supostamente este funciona melhor.
                
            -   Exponential: Aumenta o que come ao se aproximar da convergência, permitindo mais detalhes no início do treinamento.
                
            -   Constant: bem, constante.
                
        -   Smooth L1: Esta perda tenta "média", assim captando ligeiramente menos detalhes.
            
            **Resumo:: Huber loss pode ser melhor para novos usuários, datasets ruins e provavelmente fotorrealistas (já que imagens fotorrealistas normalmente têm menos variação).**
            
    -   Número de reinicializações: Defina como 1 reinicialização para cosine with restarts. Algumas pessoas recomendam 3. YMMV.
        
    -   Taxa de aquecimento: para se você quiser um aumento lento na taxa de aprendizado no início. Eu não uso, e pode ser incompatível com alguns schedulers.
        
    -   Gamma mínimo de SNR: parece filtrar algum ruído. Eu realmente pareço obter imagens um pouco menos ruidosas ao usar. Se usar, defina como 5. A imagem começará a perder detalhes em níveis mais altos, o máximo recomendado é 10.
        
    -   Peso da escala: Pelo que sei, tenta nivelar os valores do novo peso introduzido pelo lora para seu valor médio, reduzindo picos e vales. Não testei, mas pode ser bom para reduzir estilo, também pode matar traços especiais. Provavelmente deve ser definido como 1. Isso funciona, eu não gosto, mas funciona, melhor usado em combinação com uma taxa de aprendizado reduzida ou menos repetições. **_<u>NÃO USE COM PRODIGY</u>_**
        
    -   Weight decay e beta: Pelo que entendi, weight decay atenua a força de um conceito para normalizá-lo, enquanto beta é o valor de normalização esperado. Algumas coisas que li mencionam diminuir weight decay em datasets grandes e aumentá-lo em pequenos. Mas eu sempre deixo como está. Weight decay para prodigy deve ser menor, .01 funciona bem.
        
    -   Perda mascarada: Permite carregar máscaras para as imagens do dataset, fazendo com que o treinamento ignore a perda nas áreas mascaradas. Isso ajuda com fundos repetitivos ou objetos extrínsecos. Ativar esta opção permite inserir um diretório de imagens mascaradas na seção de dataset.  
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/98256b68-656f-4614-829c-dd433939ba44/width=525/98256b68-656f-4614-829c-dd433939ba44.jpeg)  
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f222578b-7290-47ca-a8d3-2f196dc03bc0/width=525/f222578b-7290-47ca-a8d3-2f196dc03bc0.jpeg)
        
5.  Argumentos de Salvamento: Apenas faça uma nova pasta e coloque suas coisas lá
    
    -   pasta de saída: onde você quer salvar suas coisas
        
    -   Nome da Saída: **_<u>Atualmente trava se você não habilitar e dar um nome.</u>_**
        
    -   Precisão de Salvamento: defina como fp16, pois é o mais padrão
        
    -   Salvar como: safetensors, realmente essa opção nem deveria estar disponível a esta altura.
        
    -   Frequência de Salvamento: depende da quantidade de épocas que você está treinando. Normalmente treino em 8 épocas com repetição média, então cada época está bem, terei 8 arquivos. Se, por outro lado, você treinar épocas altas com baixa repetição, então deve mudar para cada 2 ou 10 ou o que precisar.
        
    -   Razão de Salvamento: Acho que é o número máximo de épocas salvas permitidas.
        
    -   salvar apenas o último: o mesmo que o anterior, apenas no caso de você temer que seu treinamento seja interrompido e queira manter apenas algumas épocas anteriores.
        
    -   Salvar estado: literalmente salva um despejo de memória do processo de treinamento para que possa ser retomado mais tarde, útil em casos de desastre como uma queda de energia ou falha de hardware/software. Ou talvez um gato travesso digitando ctl+c ou alt+f4.
        
    -   Retomar estado: o caminho do estado de salvamento do qual você está retomando o treinamento.
        
    -   Salvar ocorrência de tag: SIM! Isso é útil para quando você está criando coisas para ter uma ideia das tags disponíveis para seu personagem naquele lora em particular.
        
    -   Salvar arquivo TOML: Sim, sempre recomendo dar uma olhada antes de treinar para ver se você não fez besteira.  
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/fb949dee-35c3-4bbf-a11d-43c931728704/width=525/fb949dee-35c3-4bbf-a11d-43c931728704.jpeg)
        
6.  Bucketing: Pelo que sei, quanto menos buckets melhor, por exemplo, se você tiver um mínimo de 256 e um máximo de 1024 e 64 passos entre eles, pode ter no máximo 12 buckets ((1024-256)/64=12) por lado, com os tamanhos do lado complementar que não excedem o total máximo de pixels da resolução de treinamento 262144 (512*512), resultando em 47 buckets potenciais no total. Na imagem abaixo estão as combinações válidas para treinamento 512, sua imagem será alocada no maior bucket que couber após ser redimensionada. Por exemplo, uma imagem 1920x1080 será reduzida até caber no maior bucket com uma proporção de aspecto 16:9, então provavelmente será redimensionada para 640x360 (1920/3 e 1080/3) e alocada no bucket 640x384, pois é um bom ajuste. Eu também [criei um script](https://civitai.com/models/233193) que redimensiona da mesma forma que o bucketing faz, use-o para ver se alguma de suas imagens precisa ser cortada ou descartada.  
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/adf6c9e6-7cbc-4683-a994-b9cf3357a9c4/width=525/adf6c9e6-7cbc-4683-a994-b9cf3357a9c4.jpeg)  
    É altamente recomendado escolher 4 ou 5 buckets e redimensionar suas imagens para essas resoluções, pois ter muitos buckets tem sido associado a obter imagens borradas.
    
    Como você pode ver, o treinamento aceita imagens que ultrapassam a resolução máxima declarada de um lado. O algoritmo de bucketing parece fazer alguma mágica de matriz para processar todos os pixels desde que a contagem total esteja abaixo do máximo de 262144 (para treinamento 512), em vez de, digamos, fazer o maior lado 512 e encolher ainda mais o lado menor.
    
    -   Resolução Mínima do Bucket: resolução mínima do lado permitida para uma imagem.
        
    -   Resolução Máxima do Bucket: não tenho certeza se é a resolução máxima ou se é a resolução máxima do lado menor. Provavelmente é a primeira.
        
    -   passos de resolução do bucket: tamanho dos aumentos entre os buckets.
        
    -   Não aumentar o tamanho das imagens: faz o que diz na lata, não aumentará o tamanho da imagem para o bucket mais próximo e, em vez disso, preencherá com branco ou Alpha (transparência).  
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/adb6bfde-a9f5-4dad-b2fd-fe151513211b/width=525/adb6bfde-a9f5-4dad-b2fd-fe151513211b.jpeg)
        
7.  Deslocamento de Ruído: Literalmente, apenas adiciona ruído caso as imagens do dataset sejam muito semelhantes. Pode aumentar a qualidade do treinamento ou... adicionar ruído.
    
    -   Tipo: Ruído homogêneo normal ou piramidal (começa baixo, aumenta e desce)
        
    -   Valor de deslocamento de ruído: Quantidade de ruído a ser adicionada. O padrão parece ser .1. Normalmente não adiciono ruído.
        
    -   Iterações de pirâmide: Acho que é um padrão de lâmina de serra de x iterações.
        
    -   Desconto de pirâmide: Acho que é a inclinação da pirâmide.  
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/60d0dfdf-fa77-4881-b8b5-962b77e4e768/width=525/60d0dfdf-fa77-4881-b8b5-962b77e4e768.jpeg)
        
8.  Argumentos do Amostrador: parâmetros para imagens de teste que serão produzidas a cada época. Normalmente não habilito, pois isso desacelerará um pouco as coisas.
    
    -   Amostra, passos e prompt: Se você não sabe, por que leu até este ponto do guia? Se você não sabe, vá verificar um guia básico de geração de SD.  
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/039a2938-3d00-4cc0-bfc5-e85d7db990bf/width=525/039a2938-3d00-4cc0-bfc5-e85d7db990bf.jpeg)
        
9.  Argumentos de Registro: Para analisar o treinamento com algumas ferramentas. Honestamente, a essa altura, suspeito que se você cometeu um erro, seria melhor verificar seu dataset e tags do que gastar tempo pesquisando e sabendo que precisa diminuir seu alpha em .000001. Útil para tentar encontrar melhores combinações de parâmetros, não tanto para solucionar aquele lora que continua saindo feio.
    
    -   Configurações: praticamente estilo de registro e pastas onde salvar.
        
    -   Tensorboard é instalado por padrão com os scripts de treinamento fácil, então você pode executá-lo a partir do venv lá.
        
    -   Há [um guia de Jiweji no Civitai](https://civitai.com/articles/83/using-tensorboard-to-analyze-training-data-and-create-better-models) para uma explicação mais profunda.  
        ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c01b2cbb-e1e2-4013-bb28-fee51350b9f8/width=525/c01b2cbb-e1e2-4013-bb28-fee51350b9f8.jpeg)
        
10.  Treinamento em Batch: Você precisa salvar os arquivos TOML individuais, depois carregar um por um, dar um nome a cada um e clicar em adicionar à fila. Quando adicionar todos os treinamentos, basta clicar em iniciar treinamento.  
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6cc2448d-1f61-4003-970b-f76ba56ecd41/width=525/6cc2448d-1f61-4003-970b-f76ba56ecd41.jpeg)
    

Anexei um arquivo TOML de um dos meus treinamentos, você pode carregá-lo e apenas editar as pastas se quiser. Lembre-se de ligar ou desligar o flip augment conforme necessário.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/27a4bdcf-f4d7-4b5c-8949-715e4216d451/width=525/27a4bdcf-f4d7-4b5c-8949-715e4216d451.jpeg)

Finalmente, deixe cozinhar. É como um bolo, se você espiar, ele vai murchar. Se você usar muito o computador, pode misteriosamente diminuir a velocidade e levar o dobro do tempo. Então, apenas afaste-se, vá tocar grama, encare o sol diretamente. Grite com os filhos dos vizinhos para saírem do seu gramado.

Finalmente, seu Lora terminou de assar. Tente com peso 1 ou faça um gráfico xyz com vários pesos. Se falhar muito cedo, vá para uma época anterior. Parabéns, você terminou ou estragou tudo.

===============================================================

## Solução de Problemas e FAQ

1.  Meu lora parece um Picasso!: Ou você está treinando um Picasso ou, mais provavelmente, treinou demais seu lora. Se você estiver testando com força 1, diminua o peso até parecer normal. Na maioria dos casos, a relação entre época e peso é entre linear e logarítmica, então se parecer ok com peso .5, eu tentaria uma época entre .5 e .75 das épocas totais que você treinou. Por exemplo, se você treinou 8 épocas, eu tentaria 5 ou 6. **Se estiver usando tensorboard, a época recomendada parece ser a última com perda constante descendente antes de subir.** Obviamente, a outra alternativa é diminuir as repetições de treinamento.
    
2.  Meu personagem parece um personagem genérico de Genshin Impact!: Ou você está treinando um personagem de Genshin Impact ou, mais provavelmente, usou poucas repetições, aumente o peso do lora ou faça um retraining com repetições aumentadas.
    
3.  Dois dos meus Trajes parecem iguais!: isso devido à mistura entre conceitos. É por isso que recomendo criar os gatilhos de trajes como Color1_Item1_Color2_item2 e assim por diante para que o SD aprenda a relação cor/peça do traje. Além disso, criar um LOCON em vez de um LORA e usar o otimizador prodigy ajudam na separação de conceitos.
    
4.  Criei um lora e nada muda!: Se o treinamento terminou rapidamente, é melhor verificar se o script de treinamento está realmente apontando para as pastas de imagens corretas. Se demorou, o próximo seria verificar se o tagger criou os arquivos txt de legendas corretamente e eles têm tags dentro. Em seguida, seria comparar com o lora ativado vs desativado com o mesmo prompt. Se forem exatamente iguais, então algo está errado com o arquivo LORA ou o script usado para treinar, verifique se o a1111 ou o que estiver sendo usado para testar o lora está dando um erro para o lora. Se as saídas forem apenas extremamente semelhantes, então provavelmente ou o gatilho não foi aplicado corretamente ou a taxa de aprendizado ou outro parâmetro foi configurado incorretamente.
    
5.  O rosto do meu personagem está borrado ou distorcido!: Já vi esse caso para muitos novos criadores de lora. Lembre-se de que mesmo que você use uma bela imagem de corpo inteiro 4k, ela será reduzida até que seus pixels totais correspondam a 512x512, então se for uma imagem de corpo inteiro, provavelmente perderá a maioria dos detalhes do rosto. solução? Corte e use como uma imagem de corpo inteiro e um retrato, na verdade, recomendaria transformá-la em 4 imagens diferentes: uma de corpo inteiro, uma plano americano, uma acima do peito e um retrato de rosto.
    
6.  Todos os trajes parecem com o trajeA!: Bleedover. Infelizmente, às vezes um traje é tão prevalente em um dataset que se torna parte do personagem. A única solução seria buscar mais imagens neutras e diminuir a repetição para imagens contendo aquele traje. Lembre-se de que você pode usar seu novo lora um pouco ruim para aumentar o tamanho do seu dataset e fazer algumas imagens com trajes diferentes, mesmo que seja difícil.
    
7.  Estou ficando louco, meu personagem não parece certo! Meu lora de pose contamina o estilo e meu lora de estilo tem marcas d'água!: Primeiro, respire. Infelizmente, a resposta para tudo isso é aumentar o tamanho do dataset e limpá-lo corretamente. Para contaminação de estilo, a outra rota seria imagens de regularização, mas elas inflarão o tempo de treinamento. Então, trabalhe duro. Como mencionei em algum lugar acima, se estiver quase ok, apenas publique e deixe o usuário final sofrer, eles darão algum feedback, mesmo que seja apenas para reclamar. Às vezes, a única coisa necessária para fazer um lora funcionar corretamente é um negativo específico que, quando identificado, pode dizer o que remover ou ajustar do dataset.
    

===============================================================

## Lista de Verificação

1.  Prepare seu ambiente de treinamento.
    
2.  Reúna o dataset.
    
3.  Remova duplicatas.
    
4.  Filtre o dataset (Remova de baixa qualidade, má anatomia, estilo conflitante e coisas que você simplesmente não gosta)
    
5.  Classifique aproximadamente o dataset pelos gatilhos esperados (Por exemplo, por trajes) e o restante em uma pasta geral.
    
6.  Procure mais imagens para categorias com poucas imagens (Você pode precisar consertar algumas das descartadas no passo 2 ou até mesmo fazer as suas próprias) ou desistir e descartar a categoria, jogando-a na sua pasta geral.
    
7.  Regularize seu dataset (Opcionalmente, use um filtro para remover artefatos JPG).
    
8.  Pré-corte as imagens removendo elementos extras ou fundo excessivo.
    
9.  Limpe as imagens removendo marcas d'água, efeitos sonoros e objetos aleatórios que não adicionam nada útil.
    
10.  (Opcional) Faça um corte final, criando uma subimagem duplicada a partir de imagens de alta resolução para aumentar seu dataset. Por exemplo, cortar o rosto de uma imagem de corpo inteiro de alta resolução para adicionar ao dataset.
    
11.  (Opcional) Crie imagens extras para aumentar seu dataset (seja extra crítico com imagens geradas).
    
12.  Remova alfa/transparência das suas imagens.
    
13.  Aumente o tamanho do seu dataset até que todas as imagens tenham mais pixels do que TrainingRes^2.
    
14.  Classifique as imagens em suas pastas finais.
    
15.  Legende as imagens.
    
16.  Pré-processo de legendas (remova duplicatas/ use consolidação automática de tags)
    
17.  Filtre legendas para legendas erradas e tags ruins.
    
18.  Adicione gatilhos.
    
19.  Pode legendas para consolidar os gatilhos.
    
20.  Balanceie as repetições de treinamento editando o nome das pastas.
    
21.  Defina seus parâmetros de treinamento.
    
22.  Asse o LORA.
    
23.  Teste as últimas épocas usando um gráfico XYZ.
    
24.  Escolha a época que você mais gostou.
    
25.  Parabenize-se ou volte ao passo 3.
    

===============================================================

## Glossário

-   AdamW: Otimizador.
    
-   Otimizador Adaptativo: Um otimizador que calcula automaticamente a taxa de aprendizado.
    
-   Assando: Ativando o processo de treinamento que, na maioria das vezes, significa esperar pelo resultado.
    
-   Tamanho do Batch: quantidade de imagens que serão treinadas em cada iteração. tamanhos maiores de batch amortecem a taxa de aprendizado e médias o que é aprendido das imagens, suavizando um pouco os resultados. O tamanho máximo do batch depende da quantidade de VRAM que você tem. É uma boa prática defini-lo o mais alto que sua placa permitir para aumentar a eficiência do treinamento. O amortecimento do LR NÃO é linear, então você não dobra o LR quando dobra o tamanho do batch. Eu recomendaria multiplicar sua taxa de aprendizado por 1.2 toda vez que dobrar o tamanho do batch, mas você precisa calibrá-lo você mesmo, pois há muitos fatores que podem afetá-lo.
    
-   Bmaltais: Mantenedor da kohya UI [https://github.com/bmaltais/kohya_ss](https://github.com/bmaltais/kohya_ss)
    
-   Bucket: O bucketing é um algoritmo que permite treinamento com imagens não quadradas.
    
-   Legendar: Adicionar texto a um arquivo de texto com o mesmo nome que sua imagem correspondente, descrevendo a imagem.
    
-   Legendas: Texto descrevendo uma imagem.
    
-   Collab: implementação de rede de um script de treinamento. Nomeado após o google collab.
    
-   Convergência: ponto em que a perda se estabiliza ou não se obtêm mais ganhos no treinamento. Em palavras mais simples, o ponto em que a qualidade da saída para de subir e começa a diminuir.
    
-   Danbooru: Um site de image board que possui um conjunto bem estabelecido de tags para descrever imagens. O modelo NAI do SD1.5 foi treinado em um dataset de danbooru.
    
-   Decay: Taxa de mudança descendente do LR.
    
-   Derrian distro: Mantenedor dos scripts Easy Training: [https://github.com/derrian-distro/LoRA_Easy_Training_Scripts](https://github.com/derrian-distro/LoRA_Easy_Training_Scripts)
    
-   Easy training scripts: UI de treinamento comum por Derrian Distro implementando os scripts de Kohya e Lycoris.
    
-   Época: Quantidade arbitrária de passos em que você faz um instantâneo do modelo para teste. Normalmente os otimizadores fazem pelo menos uma revolução de seu algoritmo de agendamento por época.
    
-   Refinamento: Treinamento extra para um modelo pré-treinado para adicionar nova funcionalidade ou alterar uma parte dele conforme desejado.
    
-   Hiperparâmetros: Parâmetros de treinamento como taxa de aprendizado, Dim, alpha, etc.
    
-   Image Magick: Programa de edição de imagens em lote de código aberto [https://imagemagick.org/index.php](https://imagemagick.org/index.php)
    
-   Kohya: Scripts de treinamento mais comumente usados, eles alimentam a UI Kohya_ss do Bmaltais, bem como os scripts Easy Training do Derrian Distro.
    
-   Kohya_ss: UI de treinamento comum por Bmaltais implementando os scripts de Kohya e Lycoris.
    
-   Latente: Representação extremamente reduzida de uma imagem, é decodificada pelo VAE.
    
-   Taxa de Aprendizado: A taxa de aprendizado ou LR é a taxa em que o modelo é treinado, uma taxa de treinamento muito alta causa erros de perda NaN, reduz a quantidade de detalhes aprendidos e aumenta a chance de o modelo ser excessivamente treinado.
    
-   LORA: pequeno modelo auxiliar que adiciona ou edita pesos para um modelo principal. Basicamente permite adicionar ou alterar uma parte do modelo principal sem precisar refiná-lo.
    
-   Perda: Relacionada à precisão, é uma medida de quão bem seu modelo representa o dataset. A perda normalmente diminui com o tempo à medida que o modelo é treinado até atingir um mínimo e depois aumenta à medida que o modelo sofre overfitting. Não confie em uma perda extremamente baixa, normalmente significa um problema com o dataset ou algo estranho acontecendo. Além disso, você nem sempre quer o ponto mais baixo, pois isso pode tornar o modelo inflexível. SD normalmente usa L2 loss, outros tipos incluem huber loss e huber loss agendada.
    
-   Lycoris: Biblioteca que implementa alguns "Loras" mais exóticos como Locon, Dora, Ia^3, OFT, etc.
    
-   Magic: Ou image magick ou algo que não tenho habilidade para explicar.
    
-   Máscara: imagem em escala de cinza com a área branca indicando uma zona válida e a área preta indicando que deve ser ignorada.
    
-   NAI: Basicamente o modelo fundamental para todos os modelos SD1.5 de anime.
    
-   Perda NaN: Erro comum devido a um problema com os hiperparâmetros ou o dataset. Resulta em um LORA inutilizável.
    
-   Otimizador: algoritmo que manipula a Taxa de Aprendizado.
    
-   Overbaking: overfitting.
    
-   Overfitting: Status em que o modelo tenta recriar os dados de treinamento à custa do prompt. Um sintoma de overfitting é a caricaturização, na qual os principais traços parecem ser exagerados. Por exemplo, um conceito de batom excessivamente treinado parecerá maior, mais vermelho e deformado.
    
-   Pony: modelo SDXL treinado em arte de anime e furry, capaz de usar tags estilo booru.
    
-   Prodigy: Um otimizador adaptativo.
    
-   Pruning: remover uma tag de uma legenda.
    
-   Imagens de regularização: Imagens usadas para amortecer a taxa de aprendizado do conceito específico que elas contêm.
    
-   Repetições: Número de vezes que uma imagem é processada por época.
    
-   Safetensor: Formato de saída comum para LORA. A maioria dos outros formatos foi descontinuada devido a preocupações de segurança.
    
-   SD: stable diffusion.
    
-   Scheduler: Algoritmo para manipular a LR do otimizador para tentar alcançar o nível ótimo.
    
-   SNR: Relação sinal-ruído. Como o nome indica, dá a relação entre o nível desejado de um sinal e o ruído de fundo. Para fins de SD, pense nisso como o que você deseja treinar vs. detalhes indesejados da imagem.
    
-   Passos: Vezes que uma imagem é processada. Algumas ferramentas de treinamento dividem a quantidade REAL de passos pelo tamanho do batch, não faça isso, é confuso.
    
-   Marcação: Legendar.
    
-   Tensorboard: Biblioteca de registro para verificar o comportamento da LR/Perda durante o treinamento.
    
-   Codificador de texto: O TE contém um modelo clip que interage com o Unet para transformar seu prompt em uma imagem real.
    
-   Trigger: Palavra definida pelo usuário para representar o que está sendo treinado e que é adicionada durante o treinamento ao codificador de texto.
    
-   Underfitting: o oposto de overfitting. Tecnicamente, a maioria dos modelos úteis são ligeiramente subajustados, mas parecerão borrados ou não representarão o prompt se estiverem subajustados em grande grau.
    
-   Unet: A rede usada pelo SD, que é composta por vários blocos, cada um representando algumas características de uma imagem. Ele pega o ruído e a entrada do TE e gera um Latente.
    
-   VAE: Autoencoder Variacional, é um modelo usado para codificar e decodificar latentes em imagens.
    
-   Gráfico XYZ: Script comum no a1111 para criar uma grade de imagens variando vários valores.
    

===============================================================

## Script utilitário

Abaixo estão alguns [scripts powershell](https://civitai.com/models/82080?modelVersionId=87138) (também os carreguei no civitai em sua forma .ps1) úteis para alterar as extensões de arquivo para png e para quadrar e preencher o espaço vazio com branco. O script pode ser editado para alterar apenas o formato. Lembre-se de colá-lo em um arquivo com a extensão .ps1 e executá-lo clicando com o botão esquerdo.

Se você baixou e colocou ffmpeg.exe, dwebp.exe e avifdec.exe na pasta de imagens, pode adicionar as seguintes linhas no início do script abaixo para suportar também esses tipos de arquivo.

```
#change gif to png
Get-ChildItem -Recurse -Include *.gif | Foreach-Object{

$newName=($_.FullName -replace '.gif',"%04d_from_gif.png")  
 .\ffmpeg -i $_.FullName $newName 2&gt;&amp;1 | out-null

}

#change webp to png
Get-ChildItem -Recurse -Include *.webp | Foreach-Object{

$newName=($_.FullName -replace '.webp',"_from_webp.png")
  
 .\dwebp.exe $_.FullName $newName
}

#change avif to png
Get-ChildItem -Recurse -Include *.avif | Foreach-Object{

$newName=($_.FullName -replace '.avif',"_from_avif.png")
 
 .\avifdec.exe $_.FullName

 $newName
}
```

O script powershell abaixo converte imagens em arquivos png e as torna quadradas adicionando preenchimento branco. Elas podem ser alimentadas para um upscaler ou outro redimensionador para torná-las na resolução correta.

**_<u>Nota: você pode excluir tudo abaixo de "#Daqui para baixo é para quadrar e preencher as imagens" e usar o script apenas para alterar o formato dos arquivos de imagem.</u>_**

```
#change jpg to png
Get-ChildItem -Recurse -Include *.jpg | Foreach-Object{

$newName=($_.FullName -replace '.jpg',"_from_jpg.png")  
[void][System.Reflection.Assembly]::LoadWithPartialName("System.Drawing")
$bmp = new-object System.Drawing.Bitmap($_.FullName)
$bmp.Save($newName, "png")

}

#change jpeg to png
Get-ChildItem -Recurse -Include *.jpeg | Foreach-Object{

$newName=($_.FullName -replace '.jpeg',"_from_jpeg.png")  
[void][System.Reflection.Assembly]::LoadWithPartialName("System.Drawing")
$bmp = new-object System.Drawing.Bitmap($_.FullName)
$bmp.Save($newName, "png")

}

#change bmp to png
Get-ChildItem -Recurse -Include *.bmp | Foreach-Object{

$newName=($_.FullName -replace '.bmp',"_from_bmp.png")  
[void][System.Reflection.Assembly]::LoadWithPartialName("System.Drawing")
$bmp = new-object System.Drawing.Bitmap($_.FullName)
$bmp.Save($newName, "png")

}


#Daqui para baixo é para quadrar e preencher as imagens.  
$cnt=0
Get-ChildItem -Recurse -Include *.png | Foreach-Object{

$newName=$PSScriptRoot+"\resized"+$cnt.ToString().PadLeft(6,'0')+".png"
[void][System.Reflection.Assembly]::LoadWithPartialName("System.Drawing")
$bmp = [System.Drawing.Image]::FromFile($_.FullName)



if($bmp.Width -le $bmp.Height)
{
$canvasWidth = $bmp.Height
$canvasHeight = $bmp.Height
$OffsetX= [int] ($canvasWidth/2 - $bmp.Width/2)
$OffsetY=0
}
else
{
$canvasWidth = $bmp.Width
$canvasHeight = $bmp.Width
$OffsetX=0
$OffsetY=[int] ($canvasWidth/2 - $bmp.Height/2)
}



#Encoder parameter for image quality
$myEncoder = [System.Drawing.Imaging.Encoder]::Quality
$encoderParams = New-Object System.Drawing.Imaging.EncoderParameters(1)
$encoderParams.Param[0] = New-Object System.Drawing.Imaging.EncoderParameter($myEncoder, 100)
# get codec
$myImageCodecInfo = [System.Drawing.Imaging.ImageCodecInfo]::GetImageEncoders()|where {$_.MimeType -eq 'image/jpeg'}


#create resized bitmap

$bmpResized = New-Object System.Drawing.Bitmap($canvasWidth, $canvasHeight)
$graph = [System.Drawing.Graphics]::FromImage($bmpResized)

$graph.Clear([System.Drawing.Color]::White)
$graph.DrawImage($bmp,$OffsetX,$OffsetY , $bmp.Width, $bmp.Height)

#save to file
$bmpResized.Save($newName,$myImageCodecInfo, $($encoderParams))
$graph.Dispose()
$bmpResized.Dispose()
$bmp.Dispose()

$cnt++

   }

```
---

# Comentários
<p>
</p>

> [**DreamAngel**](https://civitai.com/user/DreamAngel)
Valeu, massa!
É pra você https://www.birme.net/

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@DreamAngel**](https://civitai.com/user/DreamAngel) Obrigado pela sugestão, mas há um problema com esse site, as imagens resultantes são em jpg e webp. Reconvertê-las para jpg provavelmente adicionará mais artefatos, e o webp (se forem do tipo sem perdas) ainda precisa ser convertido para png, adicionando passos extras.

---

> [**vgt13**](https://civitai.com/user/vgt13)
Oi, uma pequena dúvida, se eu quiser treinar um Lora com vários personagens, posso fazer isso apenas adicionando várias pastas como 10_Caracter1, 10_Caracter2?

---

> [**vgt13**](https://civitai.com/user/vgt13)
[**@knxo**](https://civitai.com/user/knxo) se você definir a qualidade JPEG para 100%, isso não deve ser um grande problema. Birme => "Formato e Qualidade da Imagem"

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@vgt13**](https://civitai.com/user/vgt13) As pastas por si só não fazem nada, são apenas para organização e para definir as repetições (quantas vezes a imagem será processada em cada época). O que é realmente importante é que o gatilho do respectivo personagem esteja nas suas imagens correspondentes e, se você quiser que seja mais estável, que você podar as características definidoras dele. Em outras palavras, organize cada personagem em sua própria pasta para não se confundir e certifique-se de que todos os arquivos de legendas das imagens tenham seu gatilho correspondente e tags podadas.

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@vgt13**](https://civitai.com/user/vgt13) Deve ficar ok, eu só prefiro trabalhar principalmente com PNGs e o cropper e resizer do A1111 funcionam bem o suficiente.

---

> [**magenta**](https://civitai.com/user/magenta)
Muita informação útil, obrigado!

---

> [**sbnoob7**](https://civitai.com/user/sbnoob7)
E aí, primeiro de tudo, muito obrigado pelo guia! Algumas perguntas, eu vi que na maioria dos LoRa que baixei, eles têm um monte de tags sob "ss_tag_frequency" e estou começando a me perguntar se meus resultados ruins são devido ao meu listar apenas uma tag para isso. Tem algum passo que estou perdendo ou isso importa? Outra é pedir uma dica para treinar características não humanas. Estou tentando fazer meu próprio personagem de jogo como no meu avatar, onde as orelhas são incomuns.

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@sbnoob7**](https://civitai.com/user/sbnoob7) Você usou um auto tagger? Se sim, certifique-se de que ele de fato gerou o arquivo txt acompanhante para cada uma das suas imagens, também certifique-se de que os arquivos tenham tags. Normalmente, para anime, são usadas tags no estilo danbooru (1girl, red_eyes, etc.) para foto realista é usado o Clip ("red eyes woman standing in a hill"), então se você usou Clip, isso é provavelmente o comportamento esperado. Eu não recomendaria Clip para nada que não seja fotorealista. De qualquer forma, está correto que você deve ter a frequência ss_tag seguida pela pasta seguida pelas tags e suas contagens, assim:
"ss_tag_frequency": { "10_SailorMoonOutfit": { "smusagitsukino": 51,
Para características não humanas, o que você pode fazer é, se tiver imagens suficientes, simplesmente podar tags relacionadas como orelhas de animal e treinar um lora somente para as orelhas com um gatilho personalizado. Lembre-se de que você pode misturar e combinar vários LORAs ao mesmo tempo. Se, por outro lado, você quiser garantir que o personagem sempre tenha essa característica, simplesmente pode todas as tags que fazem referência a ela para que o gatilho do personagem absorva o conceito das orelhas.

---

> [**sbnoob7**](https://civitai.com/user/sbnoob7)
[**@knxo**](https://civitai.com/user/knxo) É, tem algo errado com minha marcação então haha, já que eu só tenho isso "ss_tag_frequency": { "15_maiwol": { "maiwol": 109 }. Estou usando o "BooruDatasetTagManager" para me ajudar a marcar e ele criou o arquivo "txt" como esperado com o mesmo nome, mas de alguma forma só listou isso (comparado às tags no seu Usagi LoRa). Parece que meu primeiro LoRa publicado foi um golpe de sorte. Procurando online, não consigo encontrar nenhuma informação sobre isso. Meio que espero que este site tenha uma função de DM, já que não acho que seja algo que possa ser resolvido com um comentário XD. Me avise se for ok entrar em contato diretamente ou talvez algum grupo no Discord que você recomende para perguntar sobre isso.

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@sbnoob7**](https://civitai.com/user/sbnoob7) Estou no Discord da civitai como Knyght, sinta-se à vontade para perguntar se me vir por lá. O Discord da Civitai também tem um canal de ajuda. Teoricamente, como está agora, seu LORA é um Lora de estilo com o gatilho maiwol.

---

> [**[deleted]**](https://civitai.com/user/[deleted])
Cara, você tem que tentar o conversor de arquivos. Economiza muito tempo
https://github.com/Tichau/FileConverter/?from=about
além disso, tenho que dar um alô para meu camarada Cupscale, para fazer um upscale rápido para obter imagens de resolução média um pouco maiores. Obviamente não é tão alta qualidade quanto img2img, mas é 10x mais simples.
https://github.com/n00mkrad/cupscale

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@dustystu**](https://civitai.com/user/dustystu) Soluções sólidas. O conversor de arquivos parece muito bom, mas não parece estar muito ativo, realmente seria ótimo se suportasse avif e divisão de gif por padrão. O Cupscale também parece sólido, mas raramente preciso usar um upscaler fora do A1111 e basicamente tem a mesma funcionalidade que a guia Extra.

---

> [**[deleted]**](https://civitai.com/user/[deleted])
Há muita diferença entre usar imagens jpg e png?


>> [**hansolocambo**](https://civitai.com/user/hansolocambo)
JPG comprime pixels = PERDA de qualidade.
PNG comprime dados = SEM PERDA de qualidade.
JPG não tem alfa = imagem retangular, não importa o que.
PNG tem alfa gradiente = uma imagem retangular pode ter qualquer forma imaginável (uma folha, um único fio de cabelo, um bokeh, etc.) uma vez usada em qualquer mídia digital.
Se jpg ainda é usado, é apenas para economizar espaço e largura de banda na web.
Para qualquer coisa relacionada a arte digital (3D, IA, etc), sempre e somente use PNG.
Exceto se você gosta de perder informações de cor de pixels ou se tem HDs minúsculos.

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@lilgatito123931**](https://civitai.com/user/lilgatito123931) Se elas estão prontas para o treinamento, não deve ser um problema. Se, por outro lado, você planeja pré-processá-las, redimensioná-las ou fazer qualquer coisa que envolva salvá-las novamente, nesse caso, você estará sujeito a obter artefatos extras do processo de compressão. Então, se você não vai mexer nelas, deixe como estão. Se não, então é melhor convertê-las e usar PNGs no restante do seu fluxo de trabalho.

---

> [**[deleted]**](https://civitai.com/user/[deleted])
Você precisa de arquivos extras para treinar Lycos no kohya webui?

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@dustystu**](https://civitai.com/user/dustystu) Não tenho certeza, pois uso LoRA_Easy_Training_Scripts que por sua vez usa o repositório principal do lycoris https://github.com/KohakuBlueleaf/LyCORIS Sei que o Kohya tem uma implementação LOCON nativa, mas é uma versão mais antiga. Se por kohya webui você quis dizer kohya_ss, acho que o repositório lycoris é suportado (ou pelo menos diz o git), mas não tenho certeza se é suportado nativamente ou se precisa ser instalado via pip.

---

> [**ploro**](https://civitai.com/user/ploro)
Guia incrível! obrigado por compartilhar :) Você conhece algum detalhado assim para treinar estilos?

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@ploro**](https://civitai.com/user/ploro) Não creio, pois relativamente poucas pessoas fazem isso, já que são muito mais sensíveis. Se você vai tentar, as únicas recomendações que posso te dar são:
-Limpe suas imagens o melhor que puder, pois será muito mais sensível a marcas d'água e outras sujeiras e você não poderá marcá-las como normalmente faria.
-Se você quiser fazer totalmente sem gatilho, certifique-se de criar os arquivos de legenda vazios (.txt), caso contrário, ele tomará o nome da pasta como o gatilho (acho que isso é integrado no kohya).
-Se você optar por um único gatilho, apenas certifique-se de excluir todas as outras tags para que sejam devidamente absorvidas pelo gatilho. Lembre-se de verificar se o seu gatilho retorna ruído ou um conceito muito fraco gerando algumas imagens no modelo que você vai usar para treinar.
-Se você souber exatamente o que seu estilo implica, pode optar por uma abordagem com tags, mas precisará ser muito cuidadoso com o que marcar (isso se assemelha mais ao treinamento de conceito).
-Reduza sua taxa de aprendizado, eu tentaria com .00001~.00005
-Talvez queira habilitar a normalização de peso da escala nas configurações do otimizador para evitar que se fixe nos elementos mais fortes das suas imagens.
-Recomendo pelo menos 150 imagens, idealmente acima de 250. Também recomendo que faça épocas menores, talvez 1 ou 2 repetições e faça muitas delas com a geração de imagens habilitada para que possa verificar o progresso. Talvez tente algo como 100~150 épocas com 2 repetições por imagem (parece razoável?). Se puder, também recomendo habilitar a opção de salvar estados no caso horrível de seu lora acabar subtreinado.

---

> [**Asscancer**](https://civitai.com/user/Asscancer)
Sem dúvida, o melhor guia que existe, mas eu tenho uma pergunta:
Como você treina um Lora para ser flexível o suficiente para mostrar nudez se todas as imagens em que ele é treinado não têm nudez e a maioria das imagens são de close-up ou da parte superior do corpo?
Eu fiz capturas de tela de um monte de imagens do personagem, e o Lora produz imagens realmente boas e de alta qualidade da parte superior do corpo, mas realmente tem dificuldades com imagens de corpo inteiro (o rosto fica distorcido/não se parece com o personagem) e não consegue fazer nudez de jeito nenhum, a menos que eu reduza o peso, ponto em que não se parece mais com o personagem.
Simplesmente não sei como conseguir essas duas coisas sem comprometer o conjunto de dados com fanart de baixa qualidade.

---

> [**[deleted]**](https://civitai.com/user/[deleted])
[**@Asscancer**](https://civitai.com/user/Asscancer) Na minha opinião, marcar e podar só faz até certo ponto. Você precisa de exemplos de corpo inteiro NSFW no treinamento se quiser gerar NSFW de corpo inteiro. Você poderia tentar marcar seus dados como 'closeup, head only' e colocar 'full body, barefoot, navel, nude, etc.' no prompt, mas duvido que você tenha muita sorte

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@Asscancer**](https://civitai.com/user/Asscancer) Existem várias opções. A mais fácil seria usar o LORA com um modelo mais NSFW para "preencher as lacunas" no conhecimento. Eu normalmente uso o AnythingV4.5 porque é um pouco mais amigável ao NSFW do que, por exemplo, rev ou até mesmo o original AnythingV3 ou V5. Quanto a corrigir o LORA em si, capturas de tela realmente não são uma fonte tão boa, pois sua homogeneidade na verdade é um pouco contraproducente (você pode "cozinhar" o LORA mais rápido). Você provavelmente obterá melhores resultados adicionando algumas fanarts que parecem "certas" para você para adicionar alguma variedade. De qualquer forma, se não quiser adicionar nada fanart, você pode usar seu LORA atual e alguns dos poses LORAs para preencher seu conjunto de dados com algumas poses extras e nudez. Para ser honesto, além do benefício óbvio, adicionar imagens nuas é a melhor maneira de ensinar o LORA a diferenciar entre um personagem e suas roupas clássicas. Lembre-se, não há vergonha em usar um conjunto de dados totalmente ou parcialmente sintético. Você também pode tentar img2img em alguma fanart nua com seu LORA atual, isso deve arrastar o estilo para o original se você usá-lo com baixo denoise. Se a fanart for de baixa resolução, você também pode fazer upscale usando seu LORA anterior. Fazer um V2 é muito mais fácil quando você já tem um V1, mesmo que não seja tão bom.
Edit:
Para obter imagens de corpo inteiro, você pode pegar uma de suas imagens de retrato, redimensionar para o que você considera necessário para torná-la de corpo inteiro (adicionar espaço em branco abaixo), fazer uma cópia, preencher a parte branca com preto e deletar a parte superior. Em seguida, use inpaint upload com a imagem copiada como máscara. Passe sua original por um interrogador (WD1.4) e use esse prompt editado + fullbody + seu LORA V1 e gere as pernas até que algumas fiquem boas.

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@dustystu**](https://civitai.com/user/dustystu) Eu achei "completely_nude" a tag mais confiável para nudez completa.

---

> [**Asscancer**](https://civitai.com/user/Asscancer)
[**@knxo**](https://civitai.com/user/knxo) [**@dustystu**](https://civitai.com/user/dustystu) Okok, parece que terei que usar fanart e/ou gerar algumas imagens com o lora em baixa intensidade e tentar usar img2img para tentar fazê-las parecerem boas com o lora ativo em alta intensidade.
Outra pergunta sobre capturas de tela e fanart/imagens geradas. Qual seria a proporção recomendada no conjunto de dados de treinamento? Sei que você disse que ter todas como capturas de tela pode levar a maus resultados, então vou me livrar de algumas, mas não sei quantas. Atualmente tenho cerca de 50 capturas de tela, sendo:
> - 35 imagens da parte superior do corpo
> - 10 imagens de corpo inteiro
> - 5 retratos
> 
> Minha proporção está errada desde o início? E quanto às imagens de nudez que vou adicionar, quantas devo tentar adicionar ao conjunto de dados na pasta 2 (outfit 2)?
Desculpe por fazer muitas perguntas, só quero preencher as lacunas do meu conhecimento.

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@Asscancer**](https://civitai.com/user/Asscancer) Não está errado, por assim dizer. Você precisa olhar para as imagens, quero dizer, realmente olhar para elas porque os animadores adoram reutilizar coisas e pular nos detalhes quando conveniente. Se os ângulos de visão forem deslocados, eles provavelmente estão ok. Eu recomendaria preencher um pouco os retratos, se possível, adicionar alguns perfis. Conseguir imagens de costas é de grande ajuda para roupas e estabilizar penteados, mas quase impossível de conseguir. Enquanto cada imagem for diferente o suficiente, o lora aprenderá algo. A triste verdade é que não há uma resposta universal. O processo normalmente é: pegue tudo o que puder e depois descarte. Em outras palavras, se você gosta de uma imagem e ela é única o suficiente? então ela entra, você odeia, mas ela ensina algo importante? então ela entra. Seu conjunto de dados ficou muito grande e está queimando? Diminua a LR/repetições e volte a assá-lo. O tempo de treinamento aumentou? Agora esse é o verdadeiro problema.
O ideal seria uma proporção 1:1:1:1 (retrato, parte superior do corpo, metade da coxa e corpo inteiro) todos com fotos de frente, perfil, ângulos e costas. Se precisar priorizar uma distância, sugiro da metade da coxa até a cabeça, então um pouco maior do que a parte superior do corpo.
Para roupas extras, sim, estar nu conta como uma. Recomendo manter o equilíbrio entre as roupas. Normalmente recomendo entre 500-1000 passos por roupa por época, a menos que seja extremamente básica ou comum, caso em que provavelmente pode fazer com muito menos e/ou usar contaminação do modelo para ajudar (por exemplo, para a roupa do Naruto, você poderia usar Jumpsuit_Orange_Jacket_Orange_Pants dando ao SD um ponto de partida) e fazer com muito poucas imagens. Então sim, esse é outro ponto a considerar se a roupa for simples 200 passos podem bastar, se for estupidamente detalhada, pode exigir muito aprendizado extra e até quase queimar o LORA. Então acho que você pode dizer complexidade alta 1000 passos por época para complexidade baixa 200 passos por época.
Agora me sinto burro... Que tipo de bucketing você está usando? Se não estiver redimensionando nada e estiver obtendo muitos buckets, esse pode ser o seu problema. Se todas as suas imagens são de alta resolução, você pode querer classificá-las por proporção e redimensionar cada grupo para um tamanho comum para colocá-las todas em o mínimo de buckets possível. No meu caso, treino exclusivamente em 500x500, então obtenho um único bucket.

---

> [**Asscancer**](https://civitai.com/user/Asscancer)
[**@knxo**](https://civitai.com/user/knxo) É um personagem 3D, então os detalhes permanecem consistentes, mas sim, entendo o que você quer dizer. Provavelmente devo me livrar de algumas imagens onde há um pouco de desfoque de movimento nesse caso. Além disso, algumas das imagens estão a alguns quadros de distância, então vou me livrar dessas também, já que são muito semelhantes e o lora não aprenderá muito ou reforçará demais esse ângulo. Bom saber.
Redimensionei as capturas de tela para 512x512 usando Brime com o personagem sendo o foco central. Para as novas imagens geradas, todas estão em 1024x1536. Então, estou assumindo que isso significa que tenho apenas dois tamanhos de bucket?
Vou treinar amanhã, depois de reduzir as imagens para uma proporção mais equilibrada. Estou visando 2 pastas, uma roupa padrão com talvez 40 imagens (20-25 repetições) e uma pasta com imagens nuas, talvez 15 imagens (40 repetições).
Mais uma pergunta (lol): Se estou usando a tag completamente_nua, isso não seria ruim, já que o modelo de treinamento já tem associações com essa tag?

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@Asscancer**](https://civitai.com/user/Asscancer) Sim e não, seria problemático se você estivesse treinando um modelo em vez de um LORA, pois você estaria contaminando ou treinando ainda mais esse conceito (dependendo se o resultado é benéfico ou não), como este é um lora, não importa o quanto você estrague o modelo, tudo desaparece quando você remove o LORA. Pode ser problemático se você estiver usando dois loros de diferentes personagens com diferentes conceitos de nudez, caso em que eles podem se misturar ou fritar, mas esse é sempre um risco ao usar vários LORAs.
No seu caso particular, você estaria se beneficiando do entendimento do modelo sobre a nudez, enquanto esse conceito ganha um pouco mais da forma corporal do seu personagem. É um pouco de dar e receber. Você poderia tentar impor sua versão de nudez, mas precisaria gastar mais passos, mas ao fazer isso pode acabar tocando no conceito original de qualquer maneira. Bleedover é apenas um fato da vida.
Quanto aos buckets, isso está correto, se você tiver apenas esses dois tamanhos, terá apenas dois buckets.

---

> [**gaydiffusion**](https://civitai.com/user/gaydiffusion)
Um excelente e exaustivo guia, muito obrigado por montar isso

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@e0972951006**](https://civitai.com/user/e0972951006) Desculpe, não aceito pedidos. Como este é um hobby, faço apenas o que tenho vontade no momento e como não comercializo minhas coisas, não assumo nenhuma responsabilidade por elas também. :P

---

> [**JB2023**](https://civitai.com/user/JB2023)
Olá, obrigado pelo guia, estou me perguntando se é possível treinar estilo + vários personagens em um único lora?

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@JB2023**](https://civitai.com/user/JB2023) Não deve ser um problema, mas você deve ter em mente que a probabilidade de bleedover aumenta se houver sobreposições de conceitos (por exemplo, se você tentar treinar roupas semelhantes em cores diferentes). Portanto, é algo que se torna mais difícil de gerenciar à medida que o escopo do lora aumenta. Além disso, você vai querer ter um conjunto de dados bastante grande. Para estilo, é recomendado um mínimo absoluto de 100 imagens a 1000. Isso é além das imagens necessárias para treinar os personagens em si.
Então, essas são minhas recomendações:
30-100 imagens por personagem e pelo menos 20 por roupa: adicione gatilhos e pode tags necessárias para consolidar personagem e roupas e tags redundantes.
300+ imagens variadas para estilo: marque tudo e não pode a menos que a tag tenha sido identificada erroneamente, verifique se há tags ausentes e adicione-as, se estiver usando um marcador, talvez reduza o limite para obter mais tags. Você pode literalmente duplicar as imagens dos personagens e adicioná-las novamente ao conjunto de dados para a parte do estilo, mas desta vez com marcação exaustiva para preencher seu conjunto de dados (isso pode não funcionar tão bem).
Para o otimizador, prodigy dará uma melhor separação e menos bleeding, mas não tenho certeza de quão bom seria para o estilo (pode ser necessário aumentar o weight_decay e aumentar o número de épocas). Por outro lado, AdamW é mais comumente usado para treinar estilo, então você precisaria definir sua LR para .00001 e aumentar o número de épocas.

---

> [**JB2023**](https://civitai.com/user/JB2023)
[**@knxo**](https://civitai.com/user/knxo) Obrigado pela sua resposta detalhada. Só para confirmar, você está sugerindo que eu crie dois LORAs separados, um para personagens e outro para estilo?

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@JB2023**](https://civitai.com/user/JB2023) Não, mas seria muito mais simples. Quanto maior o escopo (gatilhos que você deseja treinar), maior a probabilidade de algo dar errado. Não é realmente um grande problema, pois quando você tentar, provavelmente terá uma ideia do que deu errado. Mas é provável que você precise fazer alguns treinos extras depois de verificar e ver como os personagens ficaram e como o estilo parece. O que você precisa fazer é essencialmente criar os conjuntos de dados individuais, juntá-los e corrigir problemas de compatibilidade. Então, a carga de trabalho será muito semelhante. A única diferença de fazer tudo de uma vez ou separado é a facilidade de uso para o usuário final e quão problemático é solucionar problemas para você.

---

> [**JB2023**](https://civitai.com/user/JB2023)
[**@knxo**](https://civitai.com/user/knxo) Entendi. Agradeço muito a sua ajuda. Acho que criar LORA é uma questão de tentativa e erro. Um dia, vou conseguir fazer funcionar, lol. Além disso, se não se importar, gostaria que explicasse um pouco mais sobre bucketing. Ainda estou um pouco confuso sobre como funciona. Por exemplo, digamos que tenho uma imagem com tamanho de 1920x1080 e uso bucketing. Ele redimensiona automaticamente para um tamanho menor com base na resolução do bucket? E como funcionaria se eu definir a resolução de treinamento para 512x512 e algumas das minhas imagens tiverem resoluções e tamanhos diferentes (512x640, 512x768)? Obrigado novamente

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@JB2023**](https://civitai.com/user/JB2023) Para bucketing, você deve prestar atenção à configuração que marca uma resolução máxima e mínima (essas determinam o quão distorcida a resolução pode ser 3:4, 16:9, etc.) e os passos de resolução do bucket, acho que o padrão é 64 pixels, o que determina o tamanho máximo de um bucket. Se você tiver bucketing habilitado, uma imagem de 1920x1080 será reduzida até caber no maior bucket com uma proporção de 16:9, então provavelmente será redimensionada para 640x384 ou 640x320. Os buckets aumentam em 64 no x e no y, então as resoluções que você obtém são 512x512, 576x448, 640x384... também alguns mais distorcidos como 640x320, 640x256 e, claro, todos eles, mas com o x e y invertidos 448x576... 384x640... etc. A regra geral parece ser ter o mínimo de buckets possível, então tente manter suas imagens em proporções comuns 1:1, 4:3, 16:9 e 16:10.

---

> [**JB2023**](https://civitai.com/user/JB2023)
[**@knxo**](https://civitai.com/user/knxo) Obrigado pela explicação. Acho que bucketing é um conceito que poucas pessoas falam. Vou precisar pesquisar sobre isso. Se você pudesse considerar adicionar uma seção extra no futuro, explicando como bucketing funciona com exemplos, seria ótimo. Ainda estou um pouco confuso sobre como as imagens como 4:3, 16:9 e 16:10 são processadas durante o treinamento do Lora quando você define a resolução de treinamento para 512x512 (1:1). Obrigado novamente

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@JB2023**](https://civitai.com/user/JB2023) Pelo que sei, elas são reduzidas até que o número total de pixels seja menor que 512*512 (262.144 pixels). Acho que faz alguma mágica de matriz para processar todos os pixels em vez de, digamos, fazer o maior lado 512 e reduzir ainda mais o lado menor. Pelo menos, pelo que entendi dos repositórios git.

---

> [**vladiyudi**](https://civitai.com/user/vladiyudi)
Alguém já tentou treinar Lora com um objeto, como um skate? Por favor, compartilhe qualquer conselho se algo vier à mente. Obrigado antecipadamente

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@vladiyudi**](https://civitai.com/user/vladiyudi) Objetos são iguais a personagens. Então você deve simplesmente seguir regras semelhantes. Adicione o máximo de variedades de skates que puder encontrar em tantos ângulos quanto possível (de preferência apenas o skate). Além disso, adicione imagens dele sendo usado ou segurado para que o SD entenda sua forma, suas possíveis variações, sua relação de tamanho com pessoas e outros objetos e seu uso. No exemplo do skate, o que eu faria é usar um daqueles programas de renderização 3D e obter o máximo de ângulos possíveis, depois preencher o conjunto de dados com skates de diferentes cores sendo usados de várias maneiras.

---

> [**Van4lyf**](https://civitai.com/user/Van4lyf)
Ei [**@knxo**](https://civitai.com/user/knxo) você é extremamente habilidoso nisso. Você pode ser a pessoa que eu estava procurando. Ouça, tenho uma proposta para você. Estou procurando o parceiro perfeito para transformar minha visão em realidade.
Você pode me enviar seu e-mail ou outro contato? Ou enviar um e-mail para admin[**@metaroids**](https://civitai.com/user/metaroids).com ou evan[**@neoblush**](https://civitai.com/user/neoblush).com

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@Van4lyf**](https://civitai.com/user/Van4lyf) Parece muito suspeito. Você não estaria vendendo "suporte técnico" de um "legítimo" campus da Microsoft na Índia, estaria?

---

> [**Van4lyf**](https://civitai.com/user/Van4lyf)
[**@knxo**](https://civitai.com/user/knxo) Oh, Deus, não. Garanto que não é nada disso. Sou apenas um empreendedor tentando se destacar no espaço da IA. Já vendi um curso de Stable Diffusion em junho passado. Mas agora, quero levar as coisas para o próximo nível. Minhas habilidades estão longe das suas, por isso tenho uma proposta para você. Já tenho um processo funcionando para vender o curso de SD. Tudo o que preciso é de alguém para melhorar nosso curso existente e depois criar outro. Se isso é algo que te interessa, adoraria me conectar.

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@Van4lyf**](https://civitai.com/user/Van4lyf) Não estou realmente interessado, pois não sou qualificado para ensinar. A maior parte deste guia é uma mistura de boatos, meu entendimento falho e minhas experiências treinando LORAs. De qualquer forma, é sempre bom espalhar conhecimento, então você pode se sentir à vontade para usar partes dele se quiser, apenas faça uma verificação dos fatos antes, pois tenho certeza de que estou mentindo sem querer em algumas partes.

---

> [**FallKiller**](https://civitai.com/user/FallKiller)
Obrigado pelo tutorial resumido, e verifiquei sua página inicial para ver que você gosta de mulheres com seios grandes tanto quanto eu

---

> [**kaffita**](https://civitai.com/user/kaffita)
Usando o gui kohya que você linkou, bem como o exemplo toml fornecido, resultou em um lora que não afeta realmente a saída do meu prompt. Acaba a mesma imagem, quer o lora esteja incluído no prompt ou não. Você sabe o que pode causar isso?

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@kaffita**](https://civitai.com/user/kaffita) hmm precisaria de mais informações para dizer algo definitivo. Isso exigiria que você abrisse seu lora com a1111, clicasse no ícone "!" e copiasse os metadados. Caso contrário, as coisas que podem ter afetado são problemas com as repetições ou as pastas de imagens, se terminou muito rapidamente, você pode estar usando poucas imagens ou ter as repetições muito baixas. Mas isso ainda afetaria a saída de alguma forma. Se o SD produzir exatamente a mesma imagem com ou sem o LORA, o arquivo pode ter acabado corrompido ou algo assim, e o A1111 ou o que você está usando não está carregando-o corretamente. Para isso, você precisará verificar o console quando estiver gerando para ver se ele diz algo sobre o arquivo.

---

> [**x_mkiv**](https://civitai.com/user/x_mkiv)
https://fancaps.net/ é um ótimo lugar para imagens, assim você não precisa assistir o anime todo de novo. A menos, claro, que você queira, lol.

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@x_mkiv**](https://civitai.com/user/x_mkiv) Eu já usei, mas infelizmente muitas vezes falta séries mais antigas. A única outra reclamação é que você normalmente só obtém uma captura de tela por cena. Muitas vezes, você pode obter muitas boas imagens dos keyframes em uma animação, especialmente quando um personagem gira.

---

> [**wongkentiers**](https://civitai.com/user/wongkentiers)
Oi, como cortar imagens automaticamente para 512x512? Ou fazer manualmente uma por uma?

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@wongkentiers**](https://civitai.com/user/wongkentiers) Realmente depende do que você planeja usar suas imagens. Se for para um lora, recomendo que faça manualmente, não para torná-las quadradas, mas para cortar o excesso e o espaço vazio. O objetivo é maximizar a área com conteúdo útil. Em seguida, deixe o bucketing assumir o redimensionamento. Outra alternativa é usar meu script de quadratura, ele irá preencher a imagem com branco, então o bucketing redimensionará automaticamente. Ou, se for para fazer um TI, você pode usar a opção de pré-processamento na guia de treinamento do A1111 e ele redimensionará para você. A última opção é usar o corte de foco automático, novamente na guia de pré-processamento do conjunto de dados do A1111, mas esteja preparado para perder algumas imagens, pois não é particularmente eficaz em escolher corretamente o que cortar.

---

> [**wongkentiers**](https://civitai.com/user/wongkentiers)
[**@knxo**](https://civitai.com/user/knxo) Então, que ferramentas você recomenda para cortar imagens? Pode fornecer alguns exemplos, talvez 5 ou 10, de imagens que podem conseguir um bom lora?

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@wongkentiers**](https://civitai.com/user/wongkentiers) Para manualmente, honestamente recomendo o Paint 3D do Windows, é extremamente leve e rápido para salvar. Como muitas vezes tenho que redimensionar manualmente até 200 imagens, essa enquanto é uma porcaria simplesmente funciona. você também pode pressionar ctrl enquanto redimensiona para mover as duas bordas (esquerda/direita, cima/baixo) ao mesmo tempo. Para cortar automaticamente, use as ferramentas de treinamento do A1111. Uma boa imagem de amostra seria esta https://danbooru.donmai.us/posts/5657295. Também pode ser cortada na altura do quadril e um pouco abaixo do final do cabelo dela para obter um total de 3 boas sub-imagens, todas dando uma boa representação do rosto e das roupas dela. Claro, você também precisaria de alguns perfis e fotos de costas.

---

> [**wongkentiers**](https://civitai.com/user/wongkentiers)
[**@knxo**](https://civitai.com/user/knxo) e quanto a imagem de regulação? E quantas imagens devem ser uma regulação?

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@wongkentiers**](https://civitai.com/user/wongkentiers) Pelo que sei, você deve usar exatamente a mesma quantidade de imagens e repetições que seu conjunto de dados original. Se você estiver treinando um LORA e não um modelo dreambooth, eu recomendaria que não as use. No dreambooth, elas são usadas para retornar conceitos tangencialmente afetados no modelo ao seu estado original. Ao usar um lora, você pode simplesmente removê-lo do prompt e o modelo voltará ao normal. Por exemplo, se você fizer um modelo dreambooth e adicionar um personagem feminino através do treinamento, sem imagens de regulação, você descobrirá que usar 1girl começará a parecer com seu novo personagem. Você usa as imagens de regulação para que isso não aconteça e 1girl produza a mesma coisa que sempre produziu. Por outro lado, se você estiver usando um lora de personagem, espera-se que você queira que esse personagem apareça, então se 1girl se parece com o que você quer, qual é o problema?

---

> [**wongkentiers**](https://civitai.com/user/wongkentiers)
[**@knxo**](https://civitai.com/user/knxo) posso te mandar mensagem diretamente? discord talvez? você explica bem as coisas

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@wongkentiers**](https://civitai.com/user/wongkentiers) Eu fico de vez em quando no Discord da Civitai, lá eu me chamo Knyght.

---

> [**wongkentiers**](https://civitai.com/user/wongkentiers)
[**@knxo**](https://civitai.com/user/knxo) pode me convidar? link expirado

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@wongkentiers**](https://civitai.com/user/wongkentiers) Você quer dizer o Discord da civitai? Deve estar no rodapé da página, é https://discord.com/invite/civitai

---

> [**wongkentiers**](https://civitai.com/user/wongkentiers)
[**@knxo**](https://civitai.com/user/knxo) sim, eu sei, mas o link expirou, não consegui entrar

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@wongkentiers**](https://civitai.com/user/wongkentiers) não faço ideia do que pode estar acontecendo, funciona para mim.

---

> [**TomLucidor**](https://civitai.com/user/TomLucidor)
P: qual é a diferença entre LoRAs de múltiplos trajes e pacotes de personagens? Eles são diferentes apenas na variação? Ou há coisas que precisam ser alteradas no pipeline de dados?

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@TomLucidor**](https://civitai.com/user/TomLucidor) Roupas, desde que sejam usadas pelo "personagem principal" no LORA, não atuam como um normalizador. O que quero dizer é que a maioria dos personagens em pacotes de personagens parece um pouco "sem graça", isso porque eles se tornam ligeiramente médios durante o treinamento, pois o modelo é puxado em diferentes direções por cada personagem que você treina.
Além disso, normalmente roupas (desde que sejam relativamente simples) precisam de menos repetições do que personagens. Fora isso, são praticamente iguais. A única coisa preocupante é que o SD tem mais dificuldade em diferenciar roupas do que personagens. Então, você pode obter mais bleedover se tiver roupas semelhantes no seu LORA. Nesses casos, se as roupas estiverem relacionadas, como uma tendo uma peça extra, recomendo construí-las uma sobre a outra, caso contrário, se forem variações de cores, recomendo se apoiar no modelo usando a contaminação a seu favor. Um exemplo de construir uma sobre a outra é usar os gatilhos assim: Blue_shirt e Blue_shirt_Red_skirt dando a maioria das repetições ao primeiro gatilho e uma porcentagem ao segundo. Se, por outro lado, você tiver, por exemplo, 3 vestidos de cores diferentes, você poderia fazer Green_dress_Pink_bowtie, Blue_dress_Blue_back_bow, Red_dress_Red_shoes concatenando as partes e cores da roupa para tornar os gatilhos mais exatos. Recomendo manter as repetições das roupas o mais equilibradas possível para que o LORA não se fixe em uma única roupa.

---

> [**FlynnDork56ish**](https://civitai.com/user/FlynnDork56ish)
Eu fiz isso e o lora resultante não fez nada. Obtenho as mesmas imagens do modelo base. Também estou percebendo que os metadados estão mostrando os nomes das pastas como as tags e não parece estar lendo as tags dos arquivos .txt. opa.

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@FlynnDork56ish**](https://civitai.com/user/FlynnDork56ish) Eu precisaria de mais detalhes para te dizer qualquer coisa. Se terminou rapidamente, eu verificaria se está realmente apontando para as pastas de imagens corretas. Se demorou, eu verificaria se o tagger criou os arquivos de legendas txt corretamente e se eles têm tags dentro. Em seguida, compararia com o lora ativado vs desativado com o mesmo prompt. Se forem exatamente iguais, então algo está errado com o arquivo LORA ou o script que você usou para treinar. Verifique se a1111 ou o que você está usando deu algum erro para o lora. Se for apenas extremamente semelhante, então provavelmente você errou na taxa de aprendizado ou em outro parâmetro.

---

> [**PartialBreakfast**](https://civitai.com/user/PartialBreakfast)
Guia 10/10, obrigado

---

> [**Bakunyuu_Beast**](https://civitai.com/user/Bakunyuu_Beast)
Tenho algumas perguntas relacionadas a criar suas próprias imagens personalizadas para regularização. Estou fazendo loros baseados em pessoas reais, então estou usando SD.15 como o modelo base para treinamento porque é muito versátil. Presumo que devo gerar as imagens de regularização usando este como meu checkpoint, mas as imagens que este checkpoint cria são notoriamente ruins (comparadas a todo o resto). Você diz para revisar as imagens em busca de aberrações e fazer novas se não passarem no teste. Meu problema é que este checkpoint só sabe realmente criar aberrações, então coisas como mãos ruins e corpos mutados são a produção padrão. Essas renderizações ruins do SD 1.5 são o que eu deveria usar, já que este é o modelo base em que será treinado?
Além disso, quantos passos de amostragem e qual escala de CFG devo usar para gerar imagens de regularização? Por fim, se a imagem que estou usando tem uma pessoa de costas para o observador e ela está virando a cabeça olhando por cima do ombro, tenho essas ações marcadas apropriadamente. No entanto, quando tentei gerar imagens de regulação com algumas dessas poses mais complexas, às vezes a renderização coloca a pessoa em uma pose diferente (em pé quando há uma tag de sentado, virada para a frente quando a tag diz olhando para trás, etc.). Eu uso apenas o que for produzido com as tags do prompt (independentemente de ser uma pose diferente da tag), ou continuo gerando uma nova imagem até finalmente obter uma pessoa em uma pose semelhante à imagem de treinamento a que corresponde?

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@Bakunyuu_Beast**](https://civitai.com/user/Bakunyuu_Beast) SD1.5 é normalmente para fotorealismo. Se você também estiver estritamente treinando fotorealismo, pode usar qualquer foto de pessoas reais de qualidade decente e variedade suficiente. Lembro que há um pacote de imagens de regulação "oficial" para SD1.5 circulando (não poderia dizer onde conseguir, mas vi no reddit ou discord, provavelmente não este, mas pode ajudar https://huggingface.co/datasets/ProGamerGov/StableDiffusion-v1-5-Regularization-Images). Quanto a poses, você deve lembrar de NÃO regularizar o que deseja treinar. Por exemplo, se deseja treinar uma pessoa, você quer regularizar roupas, fundos e poses. Se quiser treinar roupas, você quer regularizar rostos, tipos de corpo, fundo e talvez até a cor das roupas. Você adiciona as imagens de regulação para mitigar o resultado do seu treinamento em conceitos relacionados que não deseja afetar. Portanto, sim, para "vida real" fotorealista, recomendo encontrar aquele conjunto de dados de imagens reais curadas. Usar o mesmo modelo para suas imagens de regulação é para modelos que já têm um estilo embutido que você deseja manter, como o NAI de anime ou semirrealista como o chilloutmix.
No caso de realmente precisar fazer suas imagens de regulação, normalmente o cfg 7 funciona bem o suficiente. Se você estiver obtendo saídas ruins, recomendo adicionar alguns negativos leves, entendendo que eles também serão reaplicados ao modelo (o que pode ser um positivo no caso de remover malformações). Geralmente, você deseja que suas imagens de regulação sejam perfeitas, mas alinhadas ao modelo para que diminuam os erros e não mudem nada. Se suas imagens não refletirem o prompt, elas apenas adicionarão ruído, então tente descartá-las. É por isso que é muito importante cortar e limpar seu conjunto de dados o máximo possível, pois isso diminui a quantidade de conceitos em uma imagem e também torna as imagens de regulação resultantes mais fáceis de interpretar e descartar, se necessário. Portanto, infelizmente, é melhor continuar gerando até que algo apropriado apareça.

---

> [**Bakunyuu_Beast**](https://civitai.com/user/Bakunyuu_Beast)
[**@knxo**](https://civitai.com/user/knxo) Obrigado pelo insight. Vi ideias conflitantes em relação ao número de imagens de regularização a serem usadas. Vi alguns guias que dizem para igualar o número de imagens de regulação ao número de imagens de treinamento. Perguntei a outro criador e ele disse que usa uma pasta de imagens de regulação com cerca de 5000 imagens. Se eu usar as imagens de regularização do link que você forneceu acima, devo colocar todos os arquivos na pasta de regulação ou apenas arquivos suficientes para igualar o número de imagens de treinamento? Mais uma pergunta antes de sair do seu cabelo... devo anexar essas imagens de regulação com arquivos de texto de tags ou apenas as imagens são suficientes?

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@Bakunyuu_Beast**](https://civitai.com/user/Bakunyuu_Beast) Eu focaria mais nas repetições totais, se seu conjunto de dados tiver 50 imagens em 10 repetições, eu preferiria usar 500 imagens de regulação em 1 repetição para evitar que elas criem um viés. Teoricamente, como as imagens de regulação não devem afetar nada, desde que reg > conjunto de dados, deve estar bem. Claro, as imagens de regulação são processadas quase da mesma forma que as imagens de treinamento, então o tempo de treinamento aumentará de acordo, então depende de quantas delas você tolera desperdiçar tempo. Quanto a esse repositório, acho que eles são codificados por classe, o que significa que, como não têm arquivos de legenda, o nome da pasta é sua legenda. Então você pode deixá-los assim, desde que use o nome da pasta correta e o kohya inferirá. Para ser honesto, não gosto de fazer isso, então preferiria passá-los pelo WD1.4 ou Blip, dependendo do estilo de marcação que você está usando.

---

> [**wiz_**](https://civitai.com/user/wiz_)
há este projeto: https://huggingface.co/BangumiBase onde eles fazem torrent de um anime e depois usam um script para salvar todas as imagens dos personagens e outro script para separar os personagens com base na aparência, as ferramentas deles estão disponíveis no github, mas eles disseram que você precisa de conhecimento em python para usar, o que eu não tenho, lol, há alguma maneira de você talvez fazer um tutorial sobre isso?

---

> [**knxo - OP**](https://civitai.com/user/knxo)
[**@wiz_**](https://civitai.com/user/wiz_) Parece um bom conjunto de ferramentas, mas o git só parece ter as ferramentas individuais, não o script pré-fabricado. Meu palpite é que eles baixam o arquivo, dividem em quadros, usam a detecção de rosto para descartar quadros sem personagens, depois usam a detecção de diferença para isolar personagens individuais. Em seguida, fazem o recorte e limpeza. Honestamente, o processo parece horrivelmente caro em termos de computação ou tempo, em troca de ser completamente automatizado. Para ser honesto, para um usuário comum, faz mais sentido fazer um pedido a eles e baixar um conjunto de dados.

---

> [**squrhmkiljoepxtimx**](https://civitai.com/user/squrhmkiljoepxtimx)
Ótimo tutorial, que ajudou a criar ótimos resultados!
O maior salto na qualidade, ultimamente, foi mudar para treinamento de fine-tuning e usar o recurso "Extract LoRA" para obter o modelo.

---

> [**PALE_RIDER**](https://civitai.com/user/PALE_RIDER)
Ótimo artigo. Obrigado!

---

> [**rigkv**](https://civitai.com/user/rigkv)
P: Olá senhor, ainda sou novato, o que é NAI? Como treinar lora com isso?


>> [**knxo - OP**](https://civitai.com/user/knxo)
NAI ou novel AI é uma empresa de IA. A questão é que eles foram hackeados no ano passado e seu modelo de anime treinado no danbooru com tags no estilo danbooru (1girl, Red_hair, large_breasts) vazou e se tornou a base da maioria dos modelos modernos de anime. Para obtê-lo, você precisará procurar um pouco, pois não é tecnicamente legal. Alguns modelos de segunda geração ok-ish que você poderia usar caso não encontre NAI seriam os modelos Anything V3 ou v4.5 (não gosto do V5), os modelos AOM (abyss orange mix?). Algumas pessoas gostam do Anylora devido à sua falta de estilo (supostamente estilo neutro, eu não gosto também). De qualquer forma, o NAI e esses modelos de segunda geração são amplamente compatíveis a jusante, mas a esta altura tudo está tão remixado que você pode simplesmente treinar com qualquer modelo que gostar e obter resultados transferíveis relativamente bons.


>>> [**rigkv**](https://civitai.com/user/rigkv)
obrigado pela explicação ♥️


>>> [**TomLucidor**](https://civitai.com/user/TomLucidor)
Há uma boa substituição de anime para NAIv3 (que é mais SDXL)?


>>>> [**knxo - OP**](https://civitai.com/user/knxo)
Não faço ideia, ouvi dizer que as pessoas têm tido boa sorte usando o modelo furry "Pony", só tome cuidado com o óbvio. Caso contrário, o outro projeto de que estou ciente é dos caras do WD touhou, mas acho que a beta deles não foi tão boa.

>>>>> [**TomLucidor**](https://civitai.com/user/TomLucidor)
Preciso verificar isso, NAI precisa ser humilhado

---

> [**Pippinio**](https://civitai.com/user/Pippinio)
Olá, obrigado por este guia!
Tenho uma pergunta, não tenho certeza se isso foi mencionado em algum lugar aqui. No Discord da Civitai, algumas pessoas me disseram que usar boas imagens geradas por IA como parte do seu conjunto de dados para criar um novo Lora está certo.
Então, primeiro: você concorda com essa opinião?
E, se sim, a segunda pergunta seria: onde você as colocaria no seu "ranking de imagens" de 1 a 12?
Obrigado antecipadamente!


>> [**knxo - OP**](https://civitai.com/user/knxo)
Usar imagens geradas é absolutamente válido, eu as colocaria abaixo da arte de baixa resolução e acima das capturas de tela de baixa resolução, cerca de 7,5 na lista (supondo que menos é melhor). O problema com imagens geradas é que nossos cérebros parecem felizes em ignorar defeitos. Um artista não enlouquecerá de repente e sutilmente adicionará um terceiro braço enquanto você está distraído pela cor dos olhos. Então o problema não são as imagens em si ou o conteúdo, mas os erros que não "vemos" devido à distração. Em outras palavras, verifique cuidadosamente a anatomia, braços, dedos, olhos e detalhes das roupas, como botões, se tudo estiver ok, então está ok. Caso contrário, conserte ou corte. Algumas pessoas se preocupam com alguma granulação característica que algumas imagens do SD têm, essas podem ser suavizadas por alguns modelos ESRGAN, uma boa parte deles está em https://openmodeldb.info/. Há removedores de alias, removedores de dithering, removedores de artefatos de jpeg e removedores de artefatos diversos. Alguns que posso recomendar são 1x_JPEGDestroyerV2_96000G, 1x_NoiseToner-Poisson-Detailed_108000_G, 1x_GainRESV3_Aggro e 1x_DitherDeleterV3-Smooth-[32]_115000_G. Você pode ver que são filtros e não upscalers pelo prefixo 1x.


>>> [**Pippinio**](https://civitai.com/user/Pippinio)
Olá, obrigado pela resposta rápida, está bem claro! Vou tentar da maneira que você descreveu!
A propósito, então, em teoria, poderíamos fazer um novo Lora com o conjunto de dados sendo cheio de imagens geradas e obter um bom resultado, assumindo que as imagens geradas são boas? Ou é mais como "último recurso" porque você não conseguiu melhores?


>>>> [**knxo - OP**](https://civitai.com/user/knxo)
Não, é perfeitamente válido, a maioria dos LORAs de personagens originais são feitos dessa maneira, geralmente fazendo um LORA de primeira geração com uma única imagem e usando-o com diferentes modelos, Loras de pose e controlnet para criar o conjunto de dados para um produto final. Apenas lembre-se de ser extra crítico com as imagens produzidas e realmente examiná-las em busca de erros.


>>>>> [**Pippinio**](https://civitai.com/user/Pippinio)
Então, basicamente, qualquer imagem que tenha um dedo extra ou anatomia estranha, como dedos suficientes, mas em forma realmente estranha, todas estão erradas, certo?


>>>>>> [**knxo - OP**](https://civitai.com/user/knxo)
Sim, embora lembre-se de que você sempre pode tentar corrigir com inpaint ou simplesmente cortá-los, ou se tiver talento suficiente em photoshop, pode fazer você mesmo.


>>>>>>> [**Pippinio**](https://civitai.com/user/Pippinio)
Não estou familiarizado com photoshop, então tentarei inpainting, tentei um pouco, mas não é super eficiente, então talvez eu gere mais para obter bons seeds, acho?
>>>>>>>
>>>>>>> Obrigado pelas respostas novamente!

---

> [**fckmylife**](https://civitai.com/user/fckmylife)
Belo guia! Mas por que tudo sobre maldito anime? Estou de saco cheio dessa merda.


>> [**knxo - OP**](https://civitai.com/user/knxo)
É do que eu gosto e no que me especializo. Então é isso. Não há um significado mais profundo.

---

> [**sp00ns**](https://civitai.com/user/sp00ns)
Ao fazer upscale, é só para que atinjam o limite de resolução recomendado em altura e largura? Então, para 1.5 são imagens com 512x512 ou mais e para SDXL, é com 1024x1024 ou mais, ou há uma resolução específica que estou tentando atingir? Por exemplo, cortei minha imagem para mostrar a parte importante para não ter muita informação ou cortei uma marca d'água, mas isso reduziu as dimensões abaixo de 1024x1024, então faço upscale usando img2img para que ultrapasse 1024x1024? (E o mesmo para 1.5, mas pelo menos 512x512.)


>> [**knxo - OP**](https://civitai.com/user/knxo)
Sim, basta fazer o upscale delas além do tamanho 512 ou 1024, teoricamente você faria o upscale para exatamente caber no seu bucket, por exemplo 448x576 se você tiver uma imagem com esse formato, mas se você exagerar, o algoritmo de bucketing redimensionará para esse valor de qualquer forma, então não vale a pena se preocupar com isso. Eu recomendaria simplesmente não se preocupar e fazer o upscale pelo valor Xs do upscaler. A maioria é 2X ou 4X e tende a produzir um pouco menos de artefatos quando usados para fazer o upscale para o valor para o qual foram treinados. Se estiver usando A1111, você também pode querer usar a guia extras em vez do img2img. Dessa forma, você pode usar ESRGAN puro para fazer o upscale. Usar img2img, embora capaz de corrigir imperfeições, também pode adicionar novas (então você tem que ficar mais atento a elas). Então, se sua imagem só precisa de upscale, use ESRGAN na guia extras e se precisar de correção, então é melhor usar o upscale do img2img.

---

> [**ahmadnjadm232**](https://civitai.com/user/ahmadnjadm232)
Ótimo!🫂

---

> [**AIlurus**](https://civitai.com/user/AIlurus)
Que tutorial incrível! Muito obrigado, sendo um iniciante, ajudou-me a entender muitas coisas.
Estou em um projeto (muito) grande e preciso entregá-lo com alto nível de qualidade. Se alguns de vocês puderem verificar isso e me dizer se estou errado para que eu possa corrigir, ficaria muito grato. :)
Quero fazer uma coleção de raças de cães, cada uma sendo 100% fiel aos padrões oficiais caninos. Mas aqui está o truque... ao usar você deve ser capaz de definir a idade, sexo, estatura do corpo, cor(es) e padrão do tipo de pelagem, atitude e ação. Se isso não fosse muito já... mais tarde gostaria de complicar adicionando atividades específicas de cães, como esportes caninos e outras interações com humanos. Por essa razão, minha primeira lógica é usar um LORA por raça (e mais tarde por atividade) com SD1.5 para não ficar muito pesado para os usuários combinarem.
Os termos de prompt precisam ser os usados no mundo canino, então olhar para as definições oficiais de qualquer padrão deve ajudar no prompt.
O problema é que os padrões caninos estão cheios de dois dos piores inimigos para treinamento:
> - Termos muito gerais que podem desencadear muita confusão, como vermelho, castanho, flame...
> - Termos muito específicos que o SD pode não saber nada sobre, como merle, agouti, etc.
Graças a este tutorial, entendi que o primeiro problema pede uma boa regularização com imagens incluindo todas as possíveis confusões, e para o segundo problema um enorme número de imagens de treinamento. Esta tarefa é insana... ^^'
>
> Agora não consigo contar quantas horas já passei na primeira raça apenas aprendendo seu padrão, coletando o máximo de fotos que pude (até tirando um monte eu mesmo em parques caninos para detalhes que não encontrei online), selecionando as melhores, limpando com photoshop para que haja apenas cães para treinar, angulando para obter o máximo de cada imagem de tamanho otimizado, fazendo upscale 1:1 para adicionar alguns detalhes sem mudar nada no sujeito enquanto mantenho as fontes de tamanho pequeno... e estou longe de terminar essa preparação. ^^'
Então, se alguns de vocês puderem me ajudar a economizar tempo apontando erros na minha abordagem, dar dicas para este treinamento específico, dar qualquer conselho útil, ficarei abençoado!


>> [**knxo - OP**](https://civitai.com/user/knxo)
Primeiro de tudo, deixe-me dizer que a tarefa que você encontrou é bastante assustadora. Teoricamente, a melhor abordagem seria fazer um fine-tuning de um modelo. Mas isso seria mais difícil e não tenho certeza de quão fácil é generalizar coisas como idade de cães, padrões de pelagem e tal. Então, como você mencionou, um LORA por cachorro faz mais sentido, especialmente porque você pode ter progresso incremental e se falhar em alguma parte, não arruinará todo o esforço. No final, desde que você seja coerente com as tags, deve ser capaz de combinar os conjuntos de dados para treinar um modelo generalizado posteriormente.
Então, como em todo grande projeto, você precisa dividi-lo em partes gerenciáveis, então foque em uma única raça.
Para pelagens, não tenho certeza se um tipo de pelagem aparece semelhante em todas as raças, se sim, então você poderia criar um LORA de tipo de pelagem, se não, seria melhor fazer cada variação de pelagem parte do LORA da raça do cão.
Para sexo, sei que dachshunds são muito diferentes dependendo do sexo, outros nem tanto, então a menos que você esteja falando estritamente sobre pênis, se começar a confundir machos e fêmeas, você pode querer dividir cada LORA de raça de cão por sexo (então lora fêmea dachshund e um macho), em vez de simplesmente adicionar uma tag FemaleDog/MaleDog ao geral.
Quanto ao tipo de raça, eu tentaria ensinar ao LORA o que cada padrão de pelagem significa, como é um cão velho, de meia-idade e filhote da raça específica. Você terá que verificar antes se algum dos padrões de pelagem já aciona algo no modelo e pode querer trocar por uma tag inventada que não seja treinada.
Essencialmente, para cada raça, legendas assim:
Dogbreed1, FemaleDog, FurcoatA, puppy, actions, background_objects
Dogbreed1, MaleDog, FurcoatA, middle_aged_dog, actions, background_objects
Dogbreed1, MaleDog, FurcoatC, old_aged_dog, actions, background_objects
Para coisas mais detalhadas como idade numérica ou estatura. Você precisaria modificar fortemente o conjunto de dados e o SD1.5 pode não ser o melhor. Por exemplo, se eu quisesse um cão de 45 cm de altura, precisaria ensinar ao modelo como comparar a altura de um cão com um objeto conhecido. Então isso provavelmente significaria fazer um lora estilo foto de identificação com diferentes cães e uma régua ou padrão de grade indicando sua altura. Provavelmente precisará de uma quantidade considerável de imagens para que o modelo aprenda o que é uma régua e cada altura individual. SD às vezes generaliza bem e às vezes não, então não seria capaz de dar um tamanho de conjunto de dados definitivo. Mas provavelmente tentar pelo menos um cão a cada centímetro ou dois cães a cada centímetro. Claro, isso ensinará ao modelo que quando você insere a altura, a régua ou grade inevitavelmente aparecerá. Então isso é mais uma abordagem para fins técnicos, felizmente esta pode provavelmente ser feita usando photoshop para editar a escala, desde que não haja outros objetos na imagem (ou estejam em escala). Se você quisesse uma comparação mais solta, acho que enquanto você souber as alturas, pode usar uma quantidade razoável de imagens e deixar o SD classificar as alturas relativas contra objetos no fundo (certificando-se de que esses objetos estão corretamente marcados e sempre inserir a altura do cão).
Para idade numérica, o problema é semelhante, você precisaria aumentar o tamanho do conjunto de dados até que o SD entenda que um cão com uma determinada tag de idade parece de determinada forma. Também não acho que isso seja generalizável, então é provável que isso também tenha que ser um problema por raça e por sexo.
Absurdamente, o sobre atividades com humanos é o mais simples, pois o SD é principalmente treinado em torno de humanos fazendo coisas. :P

---

> [**\_E_**](https://civitai.com/user/_E_)
Olá, ótimo artigo!
Tenho algumas perguntas:
Alguma vez vale a pena assar um Lora base de 512x512 com imagens de 1024x1024?
Ouvi dizer que usar imagens de treinamento em HD dá um pouco mais de detalhes. O lado negativo é que desacelera a velocidade de treinamento, claro.


>> [**knxo - OP**](https://civitai.com/user/knxo)
Normalmente, o SD1.5 começa a agir de maneira estranha quando treinado em resoluções maiores que 768. Treinado em sendo a palavra-chave. Se você usar imagens maiores e tiver o bucketing habilitado, elas serão redimensionadas automaticamente até que o total de pixels seja igual a trainRez^2. Se você, por outro lado, aumentar a resolução de treinamento, ganha mais detalhes, mas aumenta o risco de distorções e algumas deformidades. Eu só recomendaria usar 768 em fotorealismo, especificamente pessoas, pois acho que é o único caso de uso em que a troca pode valer a pena, principalmente para obter detalhes mais finos do rosto. Para qualquer coisa acima disso, é melhor simplesmente treinar um lora SDXL. AFAIK, devido a algumas mudanças na arquitetura, o SDXL é mais robusto quando treinado com várias resoluções.

---

> [**\_E_**](https://civitai.com/user/_E_)
Também quero treinar um Lora de estilo. Digamos, gosto da maneira como este artista desenha personagens.
Tenho muitas imagens do conjunto de dados, vestidas, nuas, close-ups, etc...
Se eu erradicar todas as tags (como sugerido na instrução para treinar lora de estilo) e treinar com imagens de personagens vestidos e nus, qual seria o resultado final desse Lora?
Imagens sem tags serão aprendidas como "estado padrão", certo?

>> [**knxo - OP**](https://civitai.com/user/knxo)
De qualquer forma, você tem 4 maneiras de fazer isso:
1 Não marque nada e treine apenas o unet. Isso deve lhe dar uma semelhança ou tendência para a mesma composição de suas imagens ou basicamente um estilo. Será ativado e controlado via peso.
2 Usando um único gatilho sem legendas, semelhante ao anterior, mas será ativado quando você usar o gatilho. Afetará um pouco via peso, mas será controlado principalmente pelo peso do gatilho.
3 Legende tudo, este tenta separar o estilo dos objetos individuais. Como é sem gatilho, é controlado pelo peso do lora e parecerá mais com suas imagens quanto mais objetos do seu conjunto de dados você adicionar ao prompt. Por exemplo, se você treinar um estilo batman e batman aparecer muito no conjunto de dados, adicionar batman ao prompt aumentará um pouco o estilo.
4 Legende tudo + gatilho. Isso é basicamente uma combinação de 2 e 3. Os objetos devem ser separados do estilo, o estilo se consolidando no gatilho. Basicamente, cada tag de objeto absorve seu contraparte na imagem e as coisas que restam, como padrões de cores, iluminação, composição, etc., que não fazem parte dos objetos individuais, acabam sobrando e são adicionadas principalmente ao seu gatilho, já que este deve estar livre (ter poucos pesos associados). Por exemplo, tenho uma imagem de uma pêra e uma maçã desenhadas com giz de cera usando cores pastéis. Se eu marcar pêra, maçã, estiloA, a pêra receberá a pêra, a maçã receberá a maçã e o estiloA receberá o restante, neste caso feito com giz de cera em cores pastéis. Não é tão claro como mencionei e o conceito de maçã e pêra será ligeiramente modificado para se tornar mais pastel e crayolaish, mas em geral é assim que funciona. (Este também é um exemplo perfeito de quando adicionar imagens de regulação, pois você pode regularizar maçã e pêra retornando-as ao estado base, treinando apenas o estiloA.)
De qualquer forma, acho que o método mais popular é o 4 seguido pelo 2 e depois pelo 1. Acho que a melhor abordagem depende principalmente do tipo de coisas que o estilo pretende fazer, por exemplo, se um objeto é integral ao estilo, marcá-lo pode não ser a melhor abordagem, a menos que você planeje sempre adicioná-lo ao prompt. Essencialmente, tudo se resume a como você quer promptar seu lora na inferência ao fazer as imagens. Infelizmente, o SD é principalmente sobre encontrar maneiras estranhas de fazer o modelo entender você e produzir o que você deseja, então provavelmente você precisará experimentar para descobrir qual das quatro opções ou uma mistura delas resulta em permitir que você controle o lora da maneira que deseja.

>> [**knxo - OP**](https://civitai.com/user/knxo)
Quanto ao resultado, realmente depende do conjunto de dados, se a mistura for equilibrada, você pode obter uma nudez de vez em quando, mas o SD tem um viés para pessoas vestidas, então será menos de 50%. Quanto à forma como as coisas se misturam quando não são legendadas, bem, isso é em parte sorte, mas fica "parecido em aparência" à medida que você não está treinando explicitamente um objeto ou objetos definidos, mas toda a composição.

---

> [**Madafada1991**](https://civitai.com/user/Madafada1991)
ótimo guia!

---

> [**Technoyote**](https://civitai.com/user/Technoyote)
Olá, obrigado pelo guia! Já encaminhei muitas pessoas para cá.
Queria compartilhar uma informação sobre o parâmetro d_coef no Prodigy/DAdapt. Na verdade, não precisamos apenas configurá-lo para 2.
O ponto do d_coef é controlar a taxa de aprendizado. Quanto maior esse valor, mais rápido o Prodigy treinará. Os autores recomendam deixar no valor padrão de 1.0, então é o que eu faço. Mas é algo que deve ser experimentado. Você pode deixar o LR fixo em 1.0.
https://github.com/konstmish/prodigy?tab=readme-ov-file#scheduler

>> [**knxo - OP**](https://civitai.com/user/knxo)
Eu vi pessoas que o mantêm em 10, mas isso é um pouco agressivo demais para o meu gosto. Provavelmente configurá-lo para 1 deve ser melhor para estilos, pois seria mais gentil. Não fiz nenhum teste com valores sub 1.0, mas provavelmente tenderá a subestimar o LR ideal em vez de ultrapassá-lo um pouco e estabilizar nele. No geral, eu o manteria entre 1 e 2.

---

> [**eduardoreymr792**](https://civitai.com/user/eduardoreymr792)
muito bom

---

> [**doomsaga**](https://civitai.com/user/doomsaga)
Isso respondeu a muitas perguntas minhas, obrigado. Uma pergunta que eu tenho, já que atualmente uso este aplicativo para treinar localmente, existe uma configuração que me permitiria treinar Prodigy em 1024 x 1024 com uma placa de 12 GB para SDXL? Sinto que estou quase sem VRAM.

>> [**knxo - OP**](https://civitai.com/user/knxo)
Se você já tiver o Gradient checkpointing habilitado e usar xformers ou sdpa. A única opção restante seria treinar apenas o unet. O codificador de texto do SDXL é capaz de compensar por não ser treinado, então ainda deve funcionar. Na verdade, a maioria das pessoas não recomenda treinar o TE no SDXL com o prodigy de qualquer maneira, porque pode causar overtraining, pois é mais sensível do que o do SD1.5.

>>> [**doomsaga**](https://civitai.com/user/doomsaga)
ótimo, vou verificar. Vou dizer que até agora estou impressionado com os resultados usando 768x768, então se alguém estiver tendo problemas com uma placa de 8 GB no SDXL, talvez considere tentar isso com o AdamW?

>>>> [**knxo - OP**](https://civitai.com/user/knxo)
Deve ser possível, embora um pouco lento, mas nesse ponto pode ser melhor aceitar a realidade e usar um colab ou treinador civit.

>>>>> [**doomsaga**](https://civitai.com/user/doomsaga)
acho que o pior caso seria usar o AdamW. posso treinar bem com 1024x1024, é só o prodigy especificamente.

>>>>>> [**knxo - OP**](https://civitai.com/user/knxo)
Supostamente, o otimizador Came com o agendador rex também dá bons resultados e deve ser ainda mais leve que o AdamW na vram, então isso também é uma possibilidade.

---

> [**parkhwabong119**](https://civitai.com/user/parkhwabong119)
omg legal ~~~@!@

---

> [**Cocodee**](https://civitai.com/user/Cocodee)
Primeiro, obrigado por este incrível tutorial! Planejo treinar meu lora usando pony. Na verdade, eu tinha uma pergunta antes de começar, no meu caso, quero gerar imagens com vários personagens. Então, é uma boa ideia fazer um lora tudo-em-um (com mais de 6 personagens, usando suas próprias tags) ou 6 loros diferentes e tentar marcar também no prompt? Obrigado novamente por este artigo, tenha um bom dia :)

>> [**knxo - OP**](https://civitai.com/user/knxo)
Existem cerca de 3 caminhos possíveis, felizmente eles são um tanto incrementais:
1 Se você fizer 6 LORAs diferentes, provavelmente precisará do regional prompter para fazer dois dos personagens aparecerem juntos.
2 Se você treiná-los como um pacote de personagens, é mais provável que funcione imediatamente ao promptar duas pessoas e ainda pode usar o regional prompter (além de evitar possíveis conflitos entre o peso do LORA).
3 A melhor abordagem é adicionar imagens extras onde mais de um dos personagens apareça e treinar ainda mais conceitos de CharA+B para cada par, mas isso começa a ficar confuso quanto mais você adiciona.
Como você pode ver, à medida que a facilidade de treinamento diminui, a facilidade de uso aumenta (o SD é perverso assim). Pacotes de personagens precisam de um pouco mais de ajuste para equilibrá-los para que os personagens sejam treinados em um nível semelhante, e a última abordagem precisa ser um pacote de personagens em primeiro lugar e precisa de um conjunto de dados maior para treinar as combinações específicas de personagens como conceitos.
Minha sugestão é simplesmente fazer os seis LORAs individuais, você precisará dos conjuntos de dados limpos e marcados de qualquer maneira se quiser avançar para um pacote de personagens, pois isso exigiria apenas juntar os conjuntos de dados e equilibrar as repetições. A partir daí, passar para o exemplo 3 simplesmente exigiria imagens extras contendo vários personagens e marcadas adequadamente.
Para a marcação, se você quiser usar mais de 1 personagem por vez, absolutamente precisa podar o máximo de tags auxiliares (cor de cabelo, cor dos olhos, etc.) possível e incorporá-las em seus gatilhos de personagens. O SD simplesmente tem dificuldade em atribuir as tags ao personagem correto nas imagens, então tende a misturar e combinar. Portanto, tente podar o máximo que puder para que seu gatilho represente seu personagem o melhor possível, sem precisar das tags extras. Dessa forma, se por exemplo red_hair faz parte do personagemA, o SD terá mais dificuldade em dar isso ao personagemB.

>>> [**Cocodee**](https://civitai.com/user/Cocodee)
Perfeito, muito obrigado pela resposta! Obrigado pelo tempo que você leva para responder a todos :)

---

> [**dreamerdragon91**](https://civitai.com/user/dreamerdragon91)
Praticamente um novato no SD (cerca de 3 meses de uso) e fiz alguns LORAs até agora. Sempre tentando aprender coisas novas e este guia tem sido extremamente útil. Estou tentando especificamente fazer LORAs com 2 personagens (um casal, menino e menina) com sucesso misto. Definitivamente vi muito bleed nas imagens do casal, mas acho que suas diretrizes ajudarão muito (como podar tags específicas de personagens).
Estou usando civit para fazer loras, bem como treinamento local no sdui com esta extensão https://github.com/hako-mikan/sd-webui-traintrain
Só fiz um lora no civit com as configurações de treinamento padrão e o lora saiu muito bem.
Infelizmente, não tenho muitas imagens, pois um personagem é um OC e o outro é de um jogo com muito pouca arte.
Meus Loras têm funcionado bem, mas sempre tento melhorar!
meu próximo objetivo é tentar treiná-los juntos em um lora com várias roupas para cada personagem.
Estou me perguntando como dividir meu conjunto de dados fazendo isso. Seria assim:
>1 grupo para cada roupa (personagem solo)
>1 grupo para cada personagem (personagem solo)
>1 grupo para personagens juntos
ou eu teria 2 grupos para cada um, podados e não podados? ou apenas 2 podados vs não podados para cada roupa?
Acho que entendo, mas ainda estou um pouco confuso, você poderia, por favor, explicar como eu faria a poda se eu quiser fazer um lora assim?
EDIT: Também devo mencionar que algumas referências dos meus personagens são arte 3D. existe alguma maneira de minimizar o quanto isso influencia o estilo do lora (sem remover as imagens, já que essas são minhas únicas referências de vistas traseiras e laterais)
Muito obrigado! Quero continuar melhorando e aprendendo e seu guia definitivamente me ensinou muito. <3


>> [**knxo - OP**](https://civitai.com/user/knxo)
Primeiro de tudo, o personagem OC. A melhor maneira de lidar com isso é fazendo um LORA próprio. Pode não sair muito bom, mas usando-o você deve conseguir fazer imagens mais variadas para treinar um LORA de segunda geração melhor ou para o seu combinado. Certifique-se de ser extra crítico sobre as imagens que saem, especialmente mãos e olhos. Além disso, certifique-se de fazer poses e roupas diferentes.
Para imagens de duas pessoas, você pode usar a abordagem de colagem, simplesmente usar fundo transparente ou rembg para remover os fundos (ou criar as imagens com fundo branco para começar) e literalmente colá-las juntas.
Para a roupa, é aqui que fica complicado. Não há uma maneira confiável de garantir que a roupa vá para o alvo correto. Para fazer isso, você teria que podar a roupa que deseja e incorporar ao gatilho do personagem. Caso contrário, é apenas sorte do gacha.
Então, a estrutura das pastas deve ser assim:
CharaA
CharaB
CharaAOutfit1
CharaAOutfit2
CharaBOutfit1
CharaAPLUSCharaB
Onde os dois primeiros têm close-ups e roupas com poucas imagens para treinar, as pastas de roupas com suas respectivas roupas e a pasta combinada deve ter gatilhos CharaA, CharaB e é bom incorporar qualquer coisa que implique 2 pessoas em um gatilho combinado CharaAPLUSCharaB.
Quanto ao 3D, certifique-se de que o modelo que você planeja treinar produz 3D quando você o prompta e certifique-se de que suas imagens em 3D têm a tag. Se seu modelo já for capaz de entender o que é 3D, não deve ser um problema muito grande.


>>> [**dreamerdragon91**](https://civitai.com/user/dreamerdragon91)
Uau, muito obrigado pela resposta detalhada!! Você é incrível. Eu realmente aprecio você e o tempo que você dedicou a este guia e seu comentário para mim.
Obrigado!!! É por causa de pessoas como você que adoro aprender coisas novas no Stable Diffusion. :D

---

> [**JB2023**](https://civitai.com/user/JB2023)
Olá knxo, você sabe como posso atualizar o LoRA_Easy_Training_Scripts para que a nova opção - Huber loss (sob args do otimizador) apareça na interface gráfica?

>> [**knxo - OP**](https://civitai.com/user/knxo)
Você precisa fazer um git pull no branch dev. Mas derrian fez várias mudanças dividindo o back end para que possa ser usado em colabs e runpods enquanto é controlado localmente pela UI. Portanto, eu recomendaria deixar a instalação antiga em paz por enquanto e clonar o ambiente dev em outra pasta. Em seguida, execute o instalador e instale-o como se fosse novo. Se bem me lembro, as novas coisas no dev são Dora, Huber Loss, suporte para treinamento de TI e o otimizador Came com o agendador Rex. Acho que você também pode baixá-lo diretamente em https://github.com/derrian-distro/LoRA_Easy_Training_Scripts/archive/refs/heads/dev.zip


>>> [**JB2023**](https://civitai.com/user/JB2023)
Ok, agradeço sua ajuda. Você poderia explicar o que é o "otimizador came com o agendador Rex"? É um novo otimizador para treinamento?

>>>> [**knxo - OP**](https://civitai.com/user/knxo)
Sim, não testei pessoalmente, mas o CAME supostamente funciona de maneira muito semelhante ao AdamW, enquanto é mais eficiente em VRAM e um pouco mais rápido. O agendador Rex atua como um intermediário entre constante e cosseno. Constante sendo mais adequado para um alto número de etapas (com menor LR) e cosseno para menos etapas (com maior LR). Pelo menos é o que me lembro de ler nos artigos.

>>>>> [**JB2023**](https://civitai.com/user/JB2023)
Entendi. Obrigado pela explicação. Se você pudesse experimentar e nos contar o que achou, seria ótimo. Ao mesmo tempo, vou experimentar o otimizador CAME para ver se é bom.

---

> [**iocuevg**](https://civitai.com/user/iocuevg)
amei!

---