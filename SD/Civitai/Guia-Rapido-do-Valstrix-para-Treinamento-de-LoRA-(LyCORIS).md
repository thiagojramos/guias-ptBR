# [**Guia Rápido do Valstrix para Treinamento de LoRA (& LyCORIS)**](https://civitai.com/articles/3522/valstrixs-crash-course-guide-to-lora-and-lycoris-training)

![Guia Rápido do Valstrix para Treinamento de LoRA (& LyCORIS)](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/dea27b4a-6037-4a06-aa01-2db153f5e444/width=300/dea27b4a-6037-4a06-aa01-2db153f5e444.jpeg)

-Tags: `#training guide`, `#lycoris/locon`, `#lora training`, `#sdxl`, `#sd1.5`, `#lora guide`
-Autor: [**Valstrix**](https://civitai.com/user/Valstrix)

---
<p></p>

Originalmente um guia copiado e colado do Discord que escrevi, essas são minhas impressões para o pessoal do Civit. Com tantos guias desatualizados e/ou com informações incorretas, espero que isso seja útil tanto para aspirantes quanto para treinadores atuais.

Lembre-se de que isso é baseado no meu próprio fluxo de trabalho _pessoal_ e experiências! Provavelmente não será de forma alguma perfeito, e não pretendo fazer disso uma enciclopédia absoluta. Apenas um bom e velho curso rápido para você começar -   embora tenha evoluído para algo parecido com um manual agora.

Também vou assumir que você tem um sistema suficientemente capaz para treinamento local. (Embora isso _teoricamente_ deva ser aplicável principalmente para treinamentos em outros ambientes.)

Dito isso, este guia assumirá que você ainda não reuniu um conjunto de dados, então vamos mergulhar nisso!

**Aviso:** Jogue fora tudo o que você aprendeu daquele vídeo do YouTube do Sr. Genérico AI postado há 3 meses -   YouTube está _sempre_ desatualizado, não se confunda.

## Parte 1 | Conjuntos de Dados: Coleta e Noções Básicas

Seu conjunto de dados é **O ASPECTO MAIS IMPORTANTE** do seu LoRA, sem dúvida. **<u>Um conjunto de dados ruim produzirá um LoRA ruim todas as vezes, independentemente de suas configurações.</u>** Dados ruins entram, dados ruins saem!

Idealmente, treinar um bom LoRA usará um número decente de imagens: Para SD1.5, recomendo ~30 imagens, com um mínimo de ~15: Mas já usei apenas 8 e obtive um resultado 'decente'.

Para SDXL, tive a melhor sorte com ~20 imagens, com um mínimo de ~12. Algumas pessoas encontraram maneiras razoavelmente boas de fazer treinamentos de imagem única no XL, mas ainda não tentei isso.

Dito isso, você pode usar mais ou menos do que os números recomendados por mim, mas, de modo geral, esses são alguns bons valores para seguir -   mas não sobrecarregue seu conjunto de dados! Ter **muitas** imagens desnecessariamente vai atrasar seu treinamento para retornos rapidamente decrescentes, e dependendo do conteúdo pode te dar mais problemas do que vale a pena. Se você estiver tendo dificuldades para obter um conjunto de dados grande o suficiente, não se preocupe muito, pois há um método para expandir artificialmente seu conjunto de dados, mas abordaremos isso mais tarde.

Ao montar suas imagens, certifique-se de priorizar a qualidade em vez da quantidade. Um conjunto bem curado de 30 imagens pode facilmente superar um conjunto de 100 imagens ruins e medianas. Especialmente em conjuntos de dados menores, uma única imagem "ruim" pode desestabilizar todo o modelo de uma forma terrível. Dito isso, imagens ruins _PODEM_ ser usadas para preencher um conjunto de dados, mas devem ser devidamente marcadas (como com "esboço colorido", que será abordado mais tarde).

Você também deve garantir que suas imagens sejam variadas. Muitas imagens de um estilo similar/igual tentarão incorporar esse estilo no seu conceito, tornando a mudança de estilo excepcionalmente difícil, e fazendo qualquer mudança de estilo tendenciosa. Especialmente ao lidar com muitas capturas de tela e renderizações, você deve ser cuidadoso. Se você tiver uma quantidade significativa, marcá-las com o nome do artista que as fez, como uma renderização, etc., pode ajudar a vincular o estilo a outra tag e reduzir o impacto. Por outro lado, estilos incrivelmente distintos/poderosos podem influenciar o estilo aprendido geral, mesmo em pequenas quantidades, então recomendo marcá-los também.

Eu também recomendaria evitar imagens com temas fetichistas ao trabalhar com personagens (a menos que você queira isso no seu LoRA), pois, mesmo quando marcadas, sua anatomia frequentemente extrema pode distorcer seu modelo de uma forma pior do que se não estivessem presentes. Você _pode_, claro, usá-las para expandir seu conjunto de dados se realmente precisar, mas certifique-se de que estejam **minuciosamente** marcadas.

Pessoalmente, reúno meus dados de uma variedade de sites: e621, furaffinity, deviantart, pixiv e a wiki do monster hunter (e wikis de jogos em geral) são minhas fontes comuns. Novamente, certifique-se de evitar pegar muito do mesmo artista e estilos semelhantes. A busca de imagens no Google também vale a pena se você precisar de mais dados, pois pode frequentemente encontrar instâncias isoladas do Reddit, feeds da comunidade Steam e outros sites que você pode não ter pensado em procurar.

Pixiv é uma dádiva para monster hunter e outras franquias orientais como um site de arte japonês, mas encontrar específicos pode ser difícil às vezes, pois você precisa usar texto em japonês para garantir sua busca. Felizmente, várias wikis também incluem nomes em japonês, se aplicável. _Nota: A partir de 25/04/24, o Pixiv bloqueou 'conteúdo sensível' nos EUA e no Reino Unido -   Isso pode ser contornado definindo a região da sua conta para qualquer lugar fora desses locais, caso você se importe em usar esse conteúdo para treinamento._

À medida que você coleta suas imagens, deve considerar o que deseja treinar: Um personagem? Um estilo? Um objeto? Algumas roupas?

Para a maioria desses, você deve procurar principalmente imagens solo do sujeito em questão. Duos/Trios também funcionam, mas você deve pegá-los apenas se o seu assunto principal estiver amplamente desobstruído. Alternativamente, indivíduos extras podem ser facilmente removidos ou cortados. Se você incluir imagens de vários personagens, certifique-se de que estejam devidamente e minuciosamente marcadas. Incluir duos/trios/etc _pode_ ser benéfico para usar seu LoRA em gerações de vários personagens, mas não é necessário de forma alguma.

Se você planeja treinar um estilo, saiba que esses são mais avançados e são mais adequados para LyCORIS do que LoRA. Este guia ainda será amplamente aplicável, mas confira as partes posteriores para detalhes específicos sobre eles.

Depois de ter suas imagens, coloque-as em uma pasta para preparação na próxima parte. (Por exemplo, "Pastas de Treinamento/Pasta de Conceito/Bruto")

## Parte 2 | Conjuntos de Dados: Preparação

Depois de ter suas imagens brutas da parte 1, você pode começar a pré-processá-las para prepará-las para o treinamento. Você precisará de um programa editor de fotos: Recomendo o Photopea como uma alternativa web gratuita ao Photoshop. [Paint.net](http://paint.net/) e Krita também são opções válidas.

Pessoalmente, separo minhas imagens em dois grupos: Imagens que estão ok por conta própria e imagens que precisam de algum tipo de edição antes do uso. As que atendem aos critérios abaixo são movidas para outra pasta e editadas conforme necessário.

Dê uma olhada na extensão de suas imagens. Imagens **.webp** (geralmente retiradas de wikis) são incompatíveis com os treinadores atuais e **devem ser convertidas para PNG ou JPEG**. Ao fazer isso, observe que **imagens com fundos transparentes** também causam problemas. Estas devem ser levadas ao seu editor de imagens de escolha e devem receber um fundo. Recomendo usar várias cores sólidas, se tiver mais de uma -   a variação de fundo pode ser incrivelmente útil. Alternar entre branco/preto/azul/verde/vermelho/etc e marcá-los como tal pode ajudar seu treinamento se os fundos estiverem causando problemas, e de forma geral.

Em seguida, considere a resolução do seu treinamento. Resoluções mais altas permitem obter mais detalhes de uma imagem, mas vão desacelerar o tempo de treinamento. Usando SD1.5, a maioria das pessoas treina em **512 ou 768**, mas resoluções intermediárias também são aplicáveis, como treinar em 704 se você não conseguir 768.

Se você estiver usando SDXL, sua resolução _mínima_ deve ser de pelo menos 1024, que quase todos treinam.

**Qualquer imagem que seja maior que sua resolução será redimensionada automaticamente.** A resolução será detalhada mais adiante.

Depois de ter uma ideia da sua resolução, observe seu conjunto de dados. Lembre-se de que imagens não quadradas serão redimensionadas para manter suas proporções (com base na resolução do bucket), então ter **muito fundo vazio** pode ser prejudicial para obter detalhes. Imagens largas e altas com muito fundo vazio podem ser **recortadas para focar no sujeito**.

Além disso, se você tiver **imagens de alta resolução com várias representações** do seu sujeito (como uma folha de referência), você pode **recortar a imagem em várias partes** para que ela seja treinada em várias imagens em vez de uma única super comprimida. Essas imagens também podem ser treinadas sem cortes e recortes, apenas certifique-se de marcá-las com "várias vistas" e "folha de referência" mais tarde.

Imagens com pessoas além do sujeito devem estar focadas no sujeito se você planeja usá-las como estão, ou se planeja transformá-las em imagens solo, remova os outros sujeitos se possível, seja via recorte ou remoção.

Embora não seja necessário, elementos sobrepostos como texto, balões de fala e linhas de movimento podem ser removidos. Você deve lembrar que a IA aprende com repetição, e o mesmo elemento no mesmo lugar em várias imagens será algo que ela tentará manter. Está tudo bem se algumas poucas imagens tiverem isso, mas idealmente você quer o mínimo de repetição possível entre elas, e aquelas que não podem ser removidas mas repetem com frequência devem ser marcadas. Como a repetição é fundamental, outliers (geralmente) têm menos probabilidade de permanecer. A ferramenta de borracha mágica é muito útil para qualquer um desses casos que não estejam em um fundo de cor plana.

Se você tiver imagens menores que sua resolução de treinamento, considere ampliá-las. Ampliadores como 4x_Ultrasharp são ótimos para isso. Pessoalmente, tenho usado 4x_foolhardy_Romacri.

Se você tiver imagens **maiores que 3k pixels, reduza-as para 3k ou menos.** Aparentemente, o treinador Kohya tem alguns pequenos problemas ao lidar com imagens muito grandes -   não tenho certeza do porquê, mas reduzir imagens superdimensionadas no meu conjunto de dados mostrou alguma melhoria.

Por fim, se você tiver um sujeito com detalhes assimétricos (como uma marca, logo, braço robótico único, etc), certifique-se de que está virado para o mesmo lado em cada imagem. Imagens orientadas incorretamente devem ser invertidas para consistência. Se este for o caso, certifique-se de _não_ ativar a "aumentação de inversão", detalhada mais adiante.

Depois de fazer o seguinte, coloque _todas_ as suas imagens que você editou, e as imagens que você não editou, em uma nova pasta assim: "Pastas de Treinamento/Pasta de Conceito/X_NomeDoConceito" o 'NomeDoConceito' será seu token de instância (o que você vai usar no prompt), e o 'X' será o número de repetições da pasta por época, que será detalhado mais adiante. Deve parecer algo como "1_Hamburger".

## Parte 2.5 | Conjuntos de Dados: Curando Venenos

Como o Nightshade especialmente está ganhando muita tração agora, acho que vou colocar uma seção aqui cobrindo imagens "envenenadas". Você não encontrará essas com muita frequência, mas é bastante possível à medida que aumentam em popularidade.

O propósito de ferramentas de envenenamento de imagens como Glaze e Nightshade é adicionar "ruído adversarial" a uma imagem, o que interrompe o processo de aprendizado, efetivamente adicionando outliers insanos e obscurecendo os dados originais que ela treinaria. Como tal, incluir uma imagem envenenada no seu conjunto de dados pode resultar em anomalias estranhas, seja variações de cor, anatomia distorcida, etc. Quanto mais veneno você tiver, piores serão os efeitos. Idealmente, você não tem nenhum -   mas você PODE ainda usá-las.

Esses "venenos" têm uma fraqueza hilária -   o próprio ruído que estão introduzindo. Simplesmente pegando a imagem desejada e passando por um ampliador de IA bom em remover ruído (como com artefatos jpeg ou algo do tipo), ou até mesmo um ampliador geral, você pode simplesmente... remover o veneno. É fácil assim, geralmente. As pessoas ainda estão experimentando com os "melhores" métodos de remoção, mas francamente, especialmente com o Nightshade, praticamente qualquer método pode limpar a imagem para um estado utilizável.

Ampliadores de "suavização" ou "anti-artefato" funcionam melhor para o trabalho, usados com um dos dois métodos a seguir:  
A: Basta ampliá-lo. 2x geralmente é suficiente.  
B: Reduza para metade ou 3/4 do tamanho e, em seguida, amplie com IA. Funciona melhor com imagens já grandes, imagens de resolução pequena perderiam muito detalhe.

Alternativamente, "limpador adverso" pode fazer um trabalho decente e existe como uma [extensão para A1111](https://github.com/gogodr/AdverseCleanerExtension) ou como um [HF Space](https://huggingface.co/spaces/p1atdev/AdverseCleaner). Combinado com os métodos de ampliação acima, você pode neutralizar efetivamente o "veneno" completamente.

"Mas como eu reconheço uma imagem envenenada?"

Depende de quão agressivamente o trabalho foi envenenado -   Se parece ruim porque parece que uma criança de 3 anos colocou um filtro de câmera bobo nela, ou tem alguns artefatos bem óbvios, ou parece que a imagem inteira está coberta de um artefato de compressão Jpeg -   É 9/10 vezes envenenada. Venenos menos agressivos são mais difíceis de detectar, mas têm menos impacto no seu treinamento. Se você não tiver certeza, dê uma olhada de perto em um editor de fotos e/ou apenas passe pelos métodos de limpeza antes para estar seguro.

Como uma nota geral, os indivíduos que colocam venenos hiperagressivos em seu trabalho geralmente são delirantes o suficiente para que sua arte nem valha a pena ser usada em primeiro lugar -   Artistas que se respeitam geralmente mantêm o veneno mínimo para não afetar visualmente seu trabalho de maneira significativa, ou simplesmente não usam. Se você não quiser lidar com veneno, tire seus dados de imagens mais antigas se quiser estar totalmente seguro, ou simplesmente aprenda a identificar venenos e passe por cima deles.

## Parte 3 | Conjuntos de Dados: Marcação

Quase terminamos com o conjunto de dados! Estamos na etapa final agora, a marcação. Isso definirá seu token de instância e determinará como seu LoRA será usado.

Existem várias maneiras de fazer sua marcação, e uma infinidade de programas para auxiliar na marcação ou fazer a marcação automática. No entanto, na minha opinião _pessoal_, você não deve usar marcação automática (_especialmente_ em designs e temas específicos), pois isso dá mais trabalho do que ajuda. (No entanto, os marcadores automáticos estão melhorando rapidamente.)

Pessoalmente, uso o [Booru Dataset Tag Manager](https://github.com/starik222/BooruDatasetTagManager) e marco todas as minhas imagens manualmente. Você PODERIA marcar sem um programa, mas simplesmente... **não faça isso.** Criar, nomear e preencher manualmente um .txt para cada imagem **não** é o que você quer fazer com seu tempo.

Felizmente, o BDTM tem uma opção bacana para adicionar uma tag a todas as imagens do seu conjunto de dados de uma vez, o que torna o início do processo **muito** **mais fácil**.

Antes de marcar, você precisa escolher um modelo para treinar! Para fins de compatibilidade, sugiro que você treine em um **Modelo Base**, que é qualquer coisa como um finetune que **NÃO seja uma mistura de outros modelos**. Treinar em uma mistura ainda é viável, mas na minha experiência torna os resultados menos compatíveis com qualquer coisa que não seja aquele modelo. Se você só quer usar seu LoRA naquela mistura específica, você está perfeitamente bem para treinar nela, no entanto.

Agora, para a marcação em si. Antes de fazer _qualquer coisa_, descubra que tipo de tags você usará:

-   Atualmente, existem três tipos de estilos de prompt, como segue: Natural Language Prompting, (dan)Booru Tag Prompting, e e6 Tag Prompting, que você deve usar com base na "ascendência" do seu modelo.
      
-   O SD1.5 Base, SDXL Base e (atualmente) a maioria dos modelos SDXL usam Natural Prompting, ex: "Um cachorro marrom dormindo à noite, ele é muito fofinho." Às vezes funciona em outros modelos, mas _não é recomendado_.
      
-   A grande maioria dos modelos de Anime que você vê usa Booru prompting, especificamente usando a lista de tags do Danbooru, um image board de anime. Ouvi dizer que Anything v4.5 é uma boa escolha para 1.5.
      
-   Modelos com ascendência baseada em modelos furry usam e6 prompting, usando a lista de tags do e621. Fluffyrock ou BB95 são boas escolhas aqui para 1.5. PonyXL é a melhor escolha para suas opções XL.
      

Depois de saber qual modelo e tags você está usando, pode começar a marcar.

**Sua PRIMEIRA tag em TODAS as imagens deve ser seu token de instância**, ou seja, o que você nomeou X_ConceptName, neste caso "conceptname". Se o seu modelo já tiver seu assunto minimamente treinado para aquela tag, considere mudar seu token de instância para uma string que ele não teria. Por exemplo, "hamburger" poderia ser "hmbrgrlora". Isso nem sempre é necessário, mas se você vir resultados estranhos que derivam da interpretação original do modelo, pode querer fazer isso.

**Sua SEGUNDA tag em TODAS as imagens deve ser seu token de classe**, um "container" geral para seu token de instância. Isso diz à IA o que seu assunto é, geralmente, para ajudar no treinamento. Por exemplo, uma espada é uma "arma", Lola Bunny é uma "antro" e assim por diante. Nem todas as imagens precisam ter o mesmo token de classe, no entanto! Muitas vezes eu uso conjuntos de dados mistos, e ter várias classes para um token de instância é perfeitamente ok -   desde que seu modelo possa fazer sentido disso.

Meu processo funciona mais ou menos assim:

-   Adicionar a todas as imagens de uma vez: token de instância (geralmente espécie), token de classe (antro/feral/humano), gênero (se aplicável, ferals não especificados), elementos controláveis (ou seja, uma roupa específica do personagem), nudez, outros controláveis comuns (como a cor dos olhos mais comum).
      
-   Mover para a primeira imagem; Remover se necessário: elementos controláveis. Alterar se necessário: nudez (para tag(s) de roupa geral), cor dos olhos, etc.
      
-   Adicionar tags que você consideraria "elementos-chave" para a imagem: Meios específicos (como aquarela), composições (por exemplo, retrato de três quartos), etc.
      
-   Adicionar tags para descrever aspectos desviados: seios enormes/hiper, chifres/escamas/pele de cor variada, etc.
      
-   Repetir para cada imagem.
      

Dito isso, não exagere nas tags. Se usar muitas, você "sobrecarregará" o treinador e obterá resultados menos precisos, pois ele está tentando treinar em muitas tags. É geralmente melhor prática marcar apenas os itens que você consideraria um "elemento-chave" da imagem. **Marcar de menos é melhor do que marcar demais, então em caso de dúvida mantenha o mínimo**. Eu costumo ter ~5-20 tags por imagem, dependendo de sua complexidade.

Fundos e poses muitas vezes podem ser ignorados, mas se você tiver tipos específicos de locais/poses/fundos em um número significativo de suas imagens, você _deve_ marcá-los para evitar viés.

Por exemplo, se você tiver muitos fundos brancos, deve marcar "fundo branco". Se após um treinamento você vir uma pose específica sendo padronizada, deve encontrar todas as instâncias no seu conjunto de dados usando aquela pose e marcá-la.

Você também deve ter cuidado com tags "implícitas". Essas são tags que implicam outras tags apenas por sua presença. Tendo uma tag implícita, você não deve usar a(s) tag(s) que ela implica junto com ela. Por exemplo, "pernas abertas" implica "pernas", "pastor alemão" implica "cachorro", e assim por diante. Ter as tags que são implicadas por outra espalha o treinamento entre elas, enfraquecendo o efeito do seu treinamento. Em grandes quantidades, isso pode ser bastante prejudicial para seus resultados finais.

**Marcando imagens de baixa qualidade:** Às vezes, você simplesmente não tem escolha a não ser usar dados ruins. Esboços grosseiros, capturas de tela de baixa resolução, anatomia ruim, e outros caem nessa categoria.

-   Esboços geralmente podem ser marcados como "esboço colorido" ou "esboço", o que geralmente é tudo o que você precisa fazer. Se não estiver colorido, "monocromático" e "tons de cinza" geralmente são boas adições também.
      
-   Imagens de baixa resolução devem ser ampliadas com um ampliador apropriado, se possível, como um dos muitos feitos para ampliar capturas de tela de desenhos antigos, por exemplo. Se você não conseguir uma boa ampliação, use a tag apropriada para seu modelo para denotar a qualidade da resolução, mas às vezes é melhor deixá-las de fora.
      
-   A anatomia ruim deve ser marcada conforme você a vê, ou cortada do quadro se possível. Imagens com desvios significativos que não podem ser cortadas ou editadas, como o pescoço/cabeça/ombros estarem fora do centro, geralmente é melhor deixá-las fora do conjunto de dados completamente.
      

Depois de marcar todas as suas imagens, certifique-se de que salvou tudo e estará pronto para a próxima etapa.

## Parte 3.5 | Conjuntos de Dados: Preservação Prévia (Regularização)

Embora completamente opcional, outro método de combater o viés de estilo e a atribuição inadequada de tags é o uso de um conjunto de dados de Preservação Prévia. Isso atuará como um conjunto de dados separado, mas generalizado, para uso junto com seu conjunto de dados de treinamento, e geralmente pode ser usado entre várias sessões de treinamento. Eu recomendaria criar uma nova pasta para eles assim: "Pastas de Treinamento/Regularização/RegConceptA/1_RegConceptName".

"Mas como exatamente eu faço e uso isso?"

Você pode começar nomeando sua pasta com um token -   seu token de classe geralmente é uma boa escolha.

Criar um conjunto de dados para isso é na verdade _incrivelmente_ fácil -   nenhuma marcação é necessária. Dentro da pasta que você criou para a tag, você simplesmente precisa colocar um número de imagens aleatórias, únicas e variadas que se enquadrem no domínio dessa tag. **Não inclua imagens de nada do que você vai treinar.** A partir dos meus próprios testes, pessoalmente recomendo um número aproximadamente igual ao número de imagens no seu conjunto de dados principal para treinamento, mas mantenha uma pasta maior com ~50-100 imagens para puxar se você treinar com mais dados no futuro, em vez de expandir seu conjunto de regularização toda vez que tiver mais dados do que antes.

Dito isso, seu conjunto de dados de regularização não precisa explicitamente ser variado. Embora a variação seja boa para uso geral, digamos que você tenha muitas capturas de tela no seu conjunto de dados principal, ou muitas imagens do mesmo ou similares artistas. Em ambos os casos, o viés estilístico pode ser difícil de remover. Embora marcar os estilos das imagens possa ajudar, nem sempre é _suficiente_ para separar completamente aquele estilo. Nesse caso, você pode criar um conjunto de dados de regularização do estilo especificamente: Basta colocar um monte de obras do artista em uma pasta, tirar um monte de capturas de tela, etc., e então nomear a pasta de regularização com a tag apropriada.

Durante o treinamento, o treinador alternará entre treinar no seu conjunto de dados principal e no conjunto de regularização -   isso exigirá que você tenha um treinamento mais longo para atingir a mesma quantidade de aprendizado, mas reduzirá muito potentemente o viés.

Você também pode usar vários conjuntos de dados de regularização diferentes no mesmo treinamento, basta colocar ambas as pastas no diretório de regularização que você configurou durante o treinamento. Lembre-se de que repetições de pastas importarão aqui -   você sempre pode executar o treinamento novamente com mais repetições da regularização se ela não for poderosa o suficiente, mas cuidado para não aumentar demais a contagem de etapas.

Outra coisa a notar é que você pode influenciar diretamente a força do seu aprendizado de regularização sem apenas adicionar repetições ou mais imagens ao conjunto de dados. Na guia "avançado" das configurações da GUI, a configuração "Peso da perda de regularização" controla isso. Por padrão, está em 1: Quanto mais próximo esse número estiver de 0, mais forte será o efeito -   Lembre-se apenas de que você precisará de mais etapas de aprendizado para compensar. Se definido muito alto, você pode arruinar completamente sua execução de treinamento aprendendo muito pouco para ter qualquer efeito.

## Parte 3.6 | Marcação: Exemplos

Como exemplos geralmente são bastante úteis, vou colocar alguns exemplos dos meus próprios conjuntos de dados aqui para sua referência. **Lembre-se: Costumo treinar em fluffyrock e Pony Diffusion, modelos que usam marcação e6. Outros modelos devem trocar tags para suas próprias variantes, onde necessário. (ex: side view (e6) > from side (booru))**

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/fefc051a-fd52-4644-ba70-f8cd3308dfc6/width=525/fefc051a-fd52-4644-ba70-f8cd3308dfc6.jpeg)

mizutsune, feral, blue eyes, no sclera, bubbles, soap, side view, action pose, open mouth, realistic, twisted torso, looking back, hand on ground, white background

-   Fundos brancos eram mais prevalentes neste conjunto de dados, então o fundo foi marcado.
      

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/4ce8f39e-375d-4e63-8a6a-a22954d3c244/width=525/4ce8f39e-375d-4e63-8a6a-a22954d3c244.jpeg)

arzarmorm, human, male, black hair, brown eyes, dark skin, three-quarter view, full-length portrait, asymmetrical armwear, skirt, pouches, armband, pants

-   Neste caso, o modelo não estava cooperando apenas com o token de instância, então as tags "asymmetrical armwear, skirt, pouches, armband, pants" foram usadas como reforço, o que também as separou do conceito principal, permitindo que fossem controladas individualmente.
      
-   Este LoRA também tinha muito poucas instâncias de fundos brancos, então deixar sem marcação não foi um problema.
      

## Parte 4 | Treinamento: Noções Básicas

Agora que você tem seu conjunto de dados, é hora de realmente treiná-lo, o que requer um script de treinamento. O script mais comumente usado, que eu também utilizo, são os Kohya Scripts. Pessoalmente, uso o [Kohya-SS GUI](https://github.com/bmaltais/kohya_ss#windows), um fork do [SD-Scripts](https://github.com/kohya-ss/sd-scripts) trainer de linha de comando. Geralmente está um pouco desatualizado, mas é perfeitamente utilizável. Ambos são opções válidas, e outras opções existem, mas por questões de compatibilidade vou me manter com o Kohya GUI como referência. A maioria das configurações deve funcionar bem em outros trainers também.

Depois de instalá-lo e abri-lo (a instalação é realmente bem fácil), certifique-se de navegar até a guia LoRA no topo (o padrão é dreambooth, um método mais antigo).

Existem muitas coisas que podem ser ajustadas e alteradas no Kohya, então vamos com calma. Presuma que qualquer coisa que eu não mencione aqui pode ser deixada como está.

Texto amarelo como este denota configurações alternativas, semi-experimentais que estou testando. Sinta-se à vontade para dar feedback se usá-las, mas se você está procurando algo estável, ignore-as. Essas configurações mudarão frequentemente conforme eu as testo e treino com elas. Quando eu estiver satisfeito com uma configuração estável que as incorpore, elas serão adotadas nas configurações principais.

Texto verde denotará configurações para meu setup SDXL. Se você estiver usando SD1.5, ignore estas.

Vamos percorrer verticalmente, aba por aba.

```
Accelerate Launch 
```

Esta aba é onde estão suas configurações de multi-GPU, se você as tiver. Caso contrário, pule esta aba completamente, pois os padrões são perfeitamente adequados.

```
Model
```

Esta aba, como você provavelmente já adivinhou, é onde você define seu modelo para treinamento, seleciona seu conjunto de dados, etc.

-   Nome ou caminho do modelo pré-treinado:
      
      -   Insira o caminho completo do arquivo para o modelo que você usará para treinar.
          
-   Nome de saída do modelo treinado:
      
      -   Será o nome do seu arquivo de saída. Nomeie como quiser.
          
-   Pasta de imagens (contendo subpastas de imagens de treinamento):
      
      -   Deve ser o caminho completo para a pasta de treinamento, mas não a que tem o X_. Você deve definir o caminho para a pasta que essa pasta está dentro. Ex: "C:/Pastas de Treinamento/Pasta de Conceito/".
          
-   Abaixo disso, há 3 caixas de seleção:
      
      -   v2: Marque se você estiver usando um modelo SD 2.X.
          
      -   v_parameterization: Marque se seu modelo suportar V-Prediction (VPred).
          
      -   Modelo SDXL: Marque se você estiver usando alguma forma de SDXL, obviamente.
          
-   Salvar modelo treinado como:
      
      -   Pode ficar como "safetensors". "ckpt" é um formato mais antigo e menos seguro. A menos que você esteja usando propositalmente uma versão antiga pré-safetensor de algo, ckpt nunca deve ser usado.
          
-   Precisão de salvamento:
      
      -   "fp16" tem dados de maior precisão, mas internamente tem valores máximos menores. "bf16" mantém dados menos precisos, mas pode usar valores maiores e parece ser mais rápido para treinar em placas não-consumidoras (se você tiver uma). Escolha com base em suas necessidades, mas eu fico com fp16, pois a maior precisão é geralmente melhor para designs mais complexos. "float" salva seu LoRA no formato fp32, o que dá um tamanho de arquivo exagerado. Uso específico.
          

```
Metadata
```

Uma seção para informações meta. Isso é totalmente opcional, mas pode ajudar as pessoas a descobrir como usar o LoRA (ou quem o fez) se o encontrarem fora do site. Recomendo colocar pelo menos seu nome de usuário no campo de autor.

```
Folders
```

Tão simples quanto pode ser: Defina suas pastas de saída/regularização aqui e o diretório de registro, se quiser.

-   Pasta de saída:
      
      -   Onde seus modelos acabarão quando forem salvos durante/após o treinamento. Defina isso para onde quiser.
          
-   Diretório de regularização:
      
      -   Deve ser deixado vazio, a menos que você planeje usar um conjunto de dados de Preservação Prévia da seção 3.5, seguindo um caminho semelhante ao da pasta de imagens. Ex: "C:/Pastas de Treinamento/Regularização/RegConceptA/".
          

```
Parameters
```

O essencial do treinamento. Quase tudo que vamos configurar está nesta seção: Não se preocupe com os presets, na maioria das vezes.

-   Tipo de Lora: Padrão
      
      -   Alt: LyCORIS/LoCon
          
      -   LoCons, após uma quantidade decente de treinamentos e testes, parecem ajustar-se mais facilmente do que um LoRA padrão, então tome cuidado com isso ao usá-los.
          
          -   Usado junto com DoRA, não encontrei problemas de ajuste excessivo com LoCons como antes, o que é promissor.
              
-   Preset LyCORIS: Completo
      
-   Tamanho do Lote de Treinamento:
      
      -   Quantas imagens serão treinadas simultaneamente. Isso pode acelerar seu treinamento, mas pode causar resultados menos precisos/mais generalizados e nem sempre é benéfico. Eu geralmente mantenho isso em 1 ou 2, mas nunca vou além de 4. Isso também aumentará substancialmente o uso de vram e ter um tamanho de lote muito grande pode desacelerar você, em vez de acelerar.
          
      -   Há algumas discussões sobre valores exponenciais serem os melhores para usar (1/2/4/8/16/etc), mas na maioria dos casos você terá dificuldades para ajustar valores maiores na maioria das GPUs de consumidor.
          
      -   Atualmente, estou usando um lote de 2.
          
-   Época:
      
      -   Uma única época em etapas é o número de imagens que você tem, multiplicado pelo número "X_".
          
      -   O que você define como esse valor depende do seu conjunto de dados, mas como regra geral, começo com um número que treina cada imagem 100 vezes.
          
          -   Ex: "1_Hamburger" tem 30 imagens. A repetição da pasta (1_) é um valor de 1: Para atingir 900 etapas, rodaríamos por 30 épocas.
              
          -   Ex: "10_Hamburger" tem 30 imagens. A repetição da pasta (10_) é um valor de 10: Para o treinador, isso é visto como _300_ imagens. Para atingir 900 etapas, rodaríamos por 3 épocas.
              
          -   Ambos os métodos treinam a mesma quantidade: Como você faz isso é puramente preferência pessoal.
              
      -   Para SD 1.5, geralmente treino por 100 épocas (10 com método alternativo).
          
      -   Para SDXL, geralmente treino por 50 épocas (5 com método alternativo).
          
      -   Embora **não seja perfeito**, esses são bons valores iniciais para testar seu conjunto de dados.
          
-   Máxima época de treinamento:
      
      -   Opcional. Força seu treinamento a parar em X épocas, útil em alguns cenários. Substituído pelo próximo valor.
          
-   Máximos passos de treinamento:
      
      -   Opcional. Força o treinamento a parar na contagem exata de etapas fornecida, substituindo épocas. Útil se você quiser parar em exatos 2000 passos ou similar.
          
-   Salvar a cada n épocas:
      
      -   Opcional. Salva um LoRA antes de terminar a cada X número de épocas que você definir. Isso pode ser útil para voltar e ver onde seu ponto ideal pode estar.
          
      -   Eu geralmente mantenho isso em 1, salvando a cada época. Se a época final estiver sobrecarregada, volto para encontrar a melhor versão. Recomendo valores maiores para contagens de épocas altas.
          
      -   Sua época final será **sempre** salva, então definir isso para um número ímpar pode ser útil, como salvar a cada 3 épocas com um treinamento de 10 épocas lhe dará épocas 3, 6, 9 e 10, dando uma opção de backup no final se começar a exagerar.
          
-   Cache latents _e_ Cache latents to disk:
      
      -   Estes afetam onde seus dados são carregados durante o treinamento. Se você tiver uma placa de vídeo recente, "cache latents" é a melhor e mais rápida escolha, que mantém seus dados carregados na placa enquanto treina. Se estiver com falta de VRAM, a versão "to disk" é mais lenta, mas não consome sua VRAM para isso.
          
      -   Fazer cache para disco (to disk), no entanto, evita a necessidade de recarregar os dados se você rodar várias vezes, desde que não haja alterações neles. Útil para ajustar configurações do treinador.
          
-   LR Scheduler:
      
      -   Ao usar Prodigy/DAdapt, use apenas Cosine. Ao usar um Adam opt, Cosine With Restarts é geralmente o melhor. Outros agendadores podem funcionar, mas afetam como a IA aprende de maneiras bastante drásticas, então não mexa com eles até entender melhor.
          
-   Otimizador:
      
      -   Existem várias opções para escolher, mas as quatro que valem a pena usar, na minha opinião, são **Prodigy, DAdaptAdam, AdamW, e AdamW8bit**.
          
      -   Prodigy é o mais novo, mais fácil de usar e produz resultados excepcionais.
          
      -   Os otimizadores AdamW são bastante antigos, mas com ajustes finos podem produzir resultados melhores que o Prodigy em um tempo mais rápido.
          
      -   Para os propósitos deste guia, usaremos Prodigy e AdamW.
          
      -   DAdaptAdam é muito semelhante ao Prodigy, e essas configurações devem ser amplamente aplicáveis a ele também. Tem um método de aprendizado menos agressivo, então se você estiver tendo problemas com o Prodigy, experimente este.
          
-   Argumentos extras do otimizador:
      
      -   Prodigy/DAdapt: Defina para "decouple=True weight_decay=0.1 betas=0.9,0.99".
          
      -   AdamW: weight_decay=0.12 betas=0.9,0.99
          
-   "Taxa de Aprendizado" [Learning Rate]:
      
      -   Ao usar Prodigy/DAdapt, defina isso para 1. Prodigy e DAdapt são adaptativos e ajustam isso automaticamente conforme treinam.
          
          -   Como nota geral, as caixas específicas de "text encoder" e "unet" mais abaixo substituirão a caixa principal, se valores forem definidos nelas.
              
-   Aquecimento de LR [LR warmup ] (% do total de passos):
      
      -   Opcional. 10% é um bom valor na maioria dos cenários. Para conceitos mais simples que um modelo já está ciente (como personagens de anime), 5% parece ser uma escolha decente também.
          
      -   SDXL: 20%
          
-   "Número de ciclos de LR" [LR # cycles]:
      
      -   Se estiver usando um opt Adam, defina isso para 3. Só afeta agendadores específicos.
          
-   "Resolução Máxima":
      
      -   Para a maioria dos modelos, você vai querer isso definido para 768,768.
          
      -   Modelos que permitem geração nativa maior (como SDXL, por exemplo) podem usar valores maiores como 1024,1024.
          
      -   Você não deve definir isso para ser maior do que seu modelo pode gerar nativamente. Placas menos poderosas podem treinar em 512,512, mas terão qualidade reduzida. Alternativamente, muitos modelos baseados no antigo vazamento NAI (a maioria dos modelos de anime SD1.5), podem ser treinados em 640,640.
          
      -   SDXL: Não defina abaixo de 1024.
          
-   Ativar buckets: Verdade.
      
      -   Isso agrupa imagens de tamanhos semelhantes durante o treinamento. Isso é destinado ao treinamento em lote, mas não faz mal mantê-lo ativado.
          
-   "Resolução máxima do bucket":
      
      -   Deve ser maior que sua resolução de treinamento, eu pessoalmente uso 960 quando em resolução de treinamento 768. Qualquer imagem maior que este tamanho será redimensionada e/ou cortada para caber nele em sua maior largura, tentando preservar sua proporção.
          
      -   SDXL: 2048
          
-   "Taxa de Aprendizado do Text Encoder e Unet" [Text Encoder & Unet learning rate]:
      
      -   SD1.5 AdamW: Tenho isso definido para 0.00005 e 0.0001, respectivamente.
          
      -   SDXL: 0.0001 e 0.0003
          
-   "Parâmetros Específicos do SDXL":
      
      -   Esta subcategoria só aparece se você marcou a caixa SDXL antes.
          
          -   Cache de saídas do codificador de texto: Pode reduzir o uso de VRAM, mas é incompatível com embaralhamento/eliminação/etc de legendas. Bom para usar se você não estiver usando nada incompatível com isso.
              
          -   Sem meia VAE: Deve sempre ser Verdade, na minha opinião, apenas para evitar dores de cabeça.
              
-   "LyCORIS":
      
      -   Esta subcategoria só aparece se você estiver usando um tipo de lora LyCORIS.
          
          -   Decomposição de Peso DoRA: Esta configuração permite usar o novo método de treinamento DoRA. Eu ainda estou testando isso, mas até agora obtive resultados positivos.
              
              -   DoRA altera significativamente a forma como um LoRA aprende, treinando-o de maneira semelhante a um finetune completo do que um LoRA padrão -   Isso resulta em tempos de treinamento mais longos, mas maior qualidade e coerência, especialmente com pequenos detalhes.
                  
-   Rank da Rede e Alpha da Rede [Network Rank & Network Alpha:]:
      
      -   Estes afetam o tamanho do seu arquivo e a quantidade de dados que ele pode armazenar. O que você definir dependerá do seu assunto.
          
      -   Se você está treinando algo semelhante ao que seu modelo já conhece (como uma garota de anime em um modelo de anime) um Rank/Alpha de 8/8 provavelmente funcionará. Na maioria dos casos, 32/32 é um bom ponto de partida. Embora você possa ir até 128/128, isso é um exagero absoluto que só aumenta o tamanho do arquivo e em alguns casos pode piorar os resultados do treinamento. Geralmente, você não deve precisar ir além de 64. Seu Alpha deve ser mantido no mesmo número que seu Rank, na maioria dos cenários. **Otimizadores adaptativos como Prodigy e DAdapt devem definir seu alpha para 1.**
          
      -   Ao não usar otimizadores adaptativos, há alguma conversa sobre usar um alpha que na verdade é muito maior que seu rank, seguindo a equação "(net alpha * sqrt(net dim))", o que deve preservar melhor as taxas de aprendizado. Valores comuns usando isso seriam 64/512, 32/181.02, 16/64 e 8/22.63, como rank/alpha, respectivamente.
          
          -   Testar isso mostrou alguns resultados interessantes, mas optei por não usar.
              
      -   SDXL: 16/16
          
      -   Usando DoRA, parece que você pode facilmente dividir seus valores pela metade (incluindo valores de Conv), enquanto mantém a qualidade.
          
-   Rank e Alpha da Convolução [Convolution Rank & Alpha]:
      
      -   Rank de 16 com um alpha de 1. Ir além de 16 parece dar retornos decrescentes, e pode realmente prejudicar os resultados.
          
-   Normas de peso da escala [Scale weight norms]:
      
      -   Isso ajuda seu LoRA a funcionar bem com outros LoRAs em conjunto, mas _pode_ ser semi-destrutivo para sua saída.
          
      -   Pessoalmente, recomendo usar um conjunto de dados de regularização em vez de usar isso.
          
          -   Se você planeja usar seu LoRA com outros LoRAs, defina este valor para 1.
              
          -   Se seu LoRA provavelmente só será usado sozinho, deixe em 0.
              
          -   Dependendo do seu conceito, seus pesos que ficam muito "pesados" são reduzidos, reduzindo seu impacto. Isso permite que vários LoRAs funcionem em conjunto sem competir por valores, mas em alguns casos PODE afetar negativamente seus resultados finais.
              
          -   Definir valores superiores a 1 reduzirá o impacto, mas também reduzirá a compatibilidade cruzada.
              
          -   A escala parece ter um impacto significativamente menor na formação do LyCORIS, dado que o aprendizado é distribuído em mais pesos. Pode geralmente ser mantido em 1 sem preocupações.
              
          -   Atualmente, optei por manter isso em 0 a partir de agora.
              
-   Dropout da Rede:
      
      -   Recomendado, mas opcional. Um valor de 0,1 é um bom valor universal. Ajuda a evitar o ajuste excessivo na maioria dos cenários.
          

```
Advanced (Subtab)
```

Não vamos mexer muito aqui, pois a maioria dos valores tem propósitos específicos.

-   Peso da perda prévia [Prior loss weight]:
      
      -   Especificamente para conjuntos de dados de regularização, 1 é um bom valor.
          
-   Parâmetros adicionais:
      
      -   Se seu modelo suportar zSNR, use "--zero_terminal_snr".
          
-   Manter n tokens:
      
      -   Para uso com embaralhamento de legendas, para evitar que os primeiros X tokens sejam embaralhados, **incluindo as vírgulas usadas para separá-los**. Geralmente mantenho isso definido para 4 ou 6, para manter os primeiros 2-3 tokens sem serem embaralhados.
          
-   Pular camada clip [Clip skip]:
      
      -   Deve ser definido para o valor de pular clipe do seu modelo. A maioria dos modelos de anime e SDXL usa 2, a maioria dos outros usa 1. Se você não tiver certeza, a maioria dos modelos do civit nota o valor usado em sua página.
          
-   Checkpointing de Gradiente:
      
      -   Marque para salvar VRAM com um leve custo de velocidade. Não tem efeito na qualidade de saída.
          
-   Embaralhamento de legenda [Shuffle Caption]:
      
      -   Opcional. Se verdadeiro, isso embaralha as tags (fora dos primeiros X mantidos no lugar por "manter n tokens") toda vez que a imagem é treinada, o que ajuda com a flexibilidade geral.
          
      -   Alguns consideram isso inútil ou um "cope", mas sua utilidade varia com seu conjunto de dados. Também adiciona randomização ao seu treinamento, e com isso ativado, rodar o mesmo treinamento duas vezes pode dar dois LoRAs ligeiramente diferentes no final.
          
-   Carregador de Dados Persistente [Persistent Data Loader]:
      
      -   Opcional. Esta opção mantém suas imagens carregadas entre as épocas. **Isso consome MUITA VRAM**, mas acelerará o treinamento. Se você pode _se dar ao luxo_ de usá-lo, use-o.
          
-   CrossAttention:
      
      -   xformers, sempre.
          
-   Aumentação de cor [Color augmentation]:
      
      -   **_Não._**
          
-   Aumentação de inversão [Flip Augmentation]:
      
      -   Opcional. Isso permite que você essencialmente dobre seu conjunto de dados espelhando aleatoriamente suas imagens horizontalmente durante o treinamento. Isso pode ser especialmente útil se você tiver poucas imagens, mas **NÃO** use isso se tiver detalhes assimétricos que deseja preservar.
          
-   Mínimo SNR Gamma:
      
      -   5 é um valor conhecido como bom. Acelera ligeiramente o treinamento.
          
-   Perda de Estimativa Desviada [Debiased Estimation Loss]:
      
      -   Verdade. Ajuda com desvio de cor e supostamente faz o treinamento precisar de menos etapas.
          
-   Tipo de offset de ruído [Noise offset type]:
      
      -   Original
          
-   Offset de ruído [Noise offset]:
      
      -   0
          

E isso é tudo! Role para cima, abra o menu suspenso "configuração" e salve suas configurações com o nome que quiser. Depois de fazer isso, clique em "iniciar treinamento" na parte inferior e espere! Dependendo da sua placa, configurações e contagem de imagens, isso pode levar um bom tempo.

## Parte 5 | Perguntas e Respostas

Esta seção é reservada para dicas, truques e outras coisas que acho útil saber, mas que não se encaixam em outro lugar. Tentarei atualizar isso periodicamente.

**P:** Vejo em muitos guias a recomendação de treinar por 2000 passos ou algo parecido, mas você fala em épocas. Por quê?

**R:** Devido à inconsistência no tamanho e na qualidade dos conjuntos de dados, os passos acabam sendo uma medida completamente arbitrária. Épocas, pelo menos, contam repetições completas das pastas, sendo uma melhor forma de medir a quantidade de treinamento feito nos seus dados. Muitos desses guias também estão treinando conceitos muito fáceis, que o treinamento capta mais rápido do que outros. Não se preocupe se ultrapassar bastante essa contagem de passos, mas 2000 ainda é um bom número para se almejar.

**P:** Vejo outros guias dizendo para definir o Network Alpha como metade do Rank, por que você não faz isso?

**R:** Essa é uma concepção bastante antiga que ainda é muito disseminada. O Alpha funciona como um meio de alterar suas taxas de aprendizado: ser metade do Rank é metade da taxa de aprendizado. Não há problema em ter isso na metade ou até menor, mas provavelmente você precisará de um treinamento mais longo.

**P:** Meu script de treinamento está mostrando um valor de perda que continua mudando conforme o treinamento avança, o que é isso?

**R:** Na maioria dos casos, você não precisa se preocupar com a perda, nem deve se preocupar com valores ou intervalos específicos. A única vez que você deve prestar atenção é se vê-lo em um certo intervalo durante a maior parte do treinamento, apenas para ver uma mudança maciça mais tarde. Isso é um sinal de que algo pode ter dado errado ou começou a sobrecarregar.

**P:** Como posso saber se meu LoRA está sub/sobrecarregado?

**R:** Ambos devem ser bastante óbvios, mesmo para olhos não treinados. Se estiver subcarregado, você provavelmente verá detalhes "borrados" ou incompletos, ou uma adesão muito baixa aos detalhes. Se estiver sobrecarregado, você pode ter cores estranhas, super saturadas, viés de estilo, viés de pose, etc. Isso varia dependendo do seu conjunto de dados, então fique atento.

**P:** Você mencionou brevemente fp16 e bf16, mas o que são as versões "completas" que estou vendo?

**R:** O fp/bf16 "padrão" usa precisão mista, enquanto as versões "completas" não usam. É enganoso, mas as versões completas mantêm dados menos precisos, mas podem ser incrivelmente rápidas para treinar. Tenho certeza de que elas têm seus usos, mas na maioria dos casos, você está perfeitamente bem usando precisão mista.

**P:** Continuo vendo menções a "Vpred", o que exatamente é isso?

**R:** Vpred, ou V-Prediction, ou V-Parameterization, são todas a mesma coisa. Embora eu não entenda totalmente em um nível técnico, pelo que sei, é uma otimização para os agendadores de ruído que "prevê" resultados durante a geração de imagens, permitindo que um resultado final seja gerado em menos etapas.

**P:** O que é Min SNR? zSNR? Zero Terminal SNR? São a mesma coisa ou diferentes?

**R:** Não, embora sejam semelhantes, fazem coisas bastante diferentes. Para simplificar, zSNR (Zero Terminal SNR) é uma técnica que permite que a IA gere usando um espaço de cores mais amplo, incluindo pretos perfeitos. Pense nisso como a diferença entre um monitor normal e um monitor OLED HDR. Min SNR é um método de convergência de treinamento acelerada, que permite que modelos sejam treinados em menos etapas.

**P:** Posso treinar em uma resolução mais alta do que meu modelo de treinamento pode fazer?

**R:** Você pode? Sim. Você deve? Não. Enquanto normalmente resoluções mais altas são uma troca de qualidade por velocidade, neste caso, você estaria trocando velocidade por piores resultados. Sem entrar em detalhes técnicos, treinar maior do que seu modelo pode suportar não é bom para seus resultados.

**P:** Você mencionou não "exagerar nas tags" nas suas imagens, mas quantas são muitas?

**R:** Isso realmente depende do seu conjunto de dados e das configurações de treinamento. Treinamentos mais longos podem ajudar com o excesso de tags, mas aumentam o risco de sobrecarregar. Geralmente, tente manter o total por imagem em 20 ou menos, em média, mas ter exceções com mais não é o pior. Tente evitar tags que não são importantes para a imagem (a menos que você esteja descobrindo que os resultados estão se apegando a algo demais, nesse caso, tague), e tags que seu modelo tem pouco ou nenhum conhecimento. Tags vazias são vistas como alvos de treinamento e tentarão ser preenchidas. Se preenchidas com os dados errados, você pode acabar precisando de tags aparentemente aleatórias para obter o resultado desejado.

**P:** Qual a diferença entre um LoRA e um LyCORIS? Eles são diferentes?

**R:** Todo LyCORIS é um LoRA, mas nem todo LoRA é um LyCORIS. LyCORIS refere-se especificamente a um subconjunto de novos métodos de treinamento LoRA (LoCon, LoHa, LoKR, DyLoRA, etc.). Todos esses ainda são LoRAs, mas suas novas metodologias os tornam estruturalmente diferentes o suficiente para terem sua própria designação. Agora que a maioria das GUIs tem suporte embutido para eles, para um usuário final, eles funcionam de maneira funcionalmente idêntica. LoRA por si só se refere apenas ao método original.

**P:** Meu LoRA meio que funciona, mas às vezes tem uma anatomia muito estranha e distorcida. O que aconteceu?

**R:** Na maioria das vezes, a anatomia distorcida origina-se do seu conjunto de dados. Verifique se há imagens que são semelhantes às distorções que você está vendo. Poses incomuns, ângulos de câmera estranhos, imagens de duplas/grupos mal tagueadas e outros outliers podem ser causas prováveis. Tente taguear o que for aplicável, mas geralmente é melhor remover a imagem completamente ou cortar as partes que estão causando problemas, se possível.

**P:** Ouvi um pouco sobre treinamento de tag única, o que é isso?

**R:** Treinar com uma única tag é um método muito antigo, comumente usado por iniciantes que não querem gastar tempo tagueando. Ao treinar para uma única tag, a IA "homogeneiza" **_tudo_** que aprende de uma imagem na tag, resultando em resultados altamente generalizados. Isso só começará a funcionar se cada imagem for de um sujeito específico (como um personagem) e tem uma alta probabilidade de se apegar a fundos específicos, poses e outras variáveis indesejadas. Se usado com qualquer outra coisa que não seja repetitiva, você acabará com o que é efetivamente uma bagunça digital. Eu não recomendaria isso para nenhuma aplicação.

**P:** Vi outras pessoas dizendo para taguear suas imagens de uma maneira diferente da sua, sem tags para descrever o sujeito fora de seu token principal. Isso é melhor? Pior?

**R:** Nem melhor, nem pior! É apenas um método diferente de taguear: no entanto, é muito menos flexível. Se você pegar um personagem, por exemplo, taguear o personagem apenas como o personagem pode dificultar a alteração da cor dos olhos, roupa ou outros detalhes. Se você não se importa com isso, é perfeitamente válido! Alternativamente, meu método é muito mais flexível, mas conseguir a semelhança exata do personagem exigirá várias tags.

## Parte 6 | Treinamento Avançado: LoRA de Múltiplos Conceitos

Então você já se familiarizou e quer um desafio maior? Ou talvez tenha um personagem com muitos trajes? Armaduras específicas de gênero? É aqui que entra o treinamento de múltiplos conceitos.

As configurações de treinamento para estes são quase exatamente as mesmas em comparação com os LoRAs normais, com algumas ressalvas:

-   Não use um tamanho de lote maior que 1. Se imagens de múltiplos conceitos forem carregadas, elas se generalizarão em uma bagunça, ou uma dominará a outra.
      
      -   Possivelmente não é mais o caso, mas incerto neste momento.
          
-   Tenha cuidado ao usar aumentação de inversão, pois ela se aplicará a todas as imagens, não apenas a um conceito.
      
-   Dependendo de quantos conceitos você está treinando e quão complexos eles são, você pode querer aumentar seus valores de Rank e Alpha. Recomendo tentar 32 primeiro e ver como funciona.
      

Agora, reúna suas imagens da mesma forma que detalhei antes, mas separe-as com base em seus conceitos (trajes, armaduras, etc). Qualquer edição também deve ser feita como antes.

Depois de preparar totalmente seus dados, descubra qual conceito tem mais dados e, em sua pasta de conceito, crie uma pasta 1_nome_do_conceito para ele.

Agora, faça o mesmo com seus outros conceitos, obviamente substituindo "nome_do_conceito" pelo token de instância deles.

Depois de ter suas pastas nomeadas e preenchidas, faça o seguinte:

-   Pegue o número de imagens em sua maior pasta, então multiplique pelo "X_" para obter sua contagem total de passos. (imagens*repetições de pasta) = passos
      
      -   Por exemplo, a Pasta A tem 51 imagens, a Pasta B tem 43 imagens. A Pasta A seria usada. Supondo 10 repetições de pasta, isso nos dá 510 passos para a Pasta A. (51*10)
          
-   Agora, divida a contagem de passos pelo número de imagens em sua segunda maior pasta. O número resultante, arredondado para o mais próximo inteiro, é o número que o "X_" dessa pasta deve ser alterado.
      
      -   Então, a Pasta B tem 43 imagens. (510/43) = 11.86, que arredondamos para 12. Agora temos 10_PastaA e 12_PastaB.
          
-   Repita isso para cada pasta aplicável.
      
      -   A Pasta C tem 32 imagens, então comparamos com a Pasta A, como antes. (510/32) = 15.93, que arredondamos para 16.
          
-   Em nosso exemplo, agora temos três pastas balanceadas juntas. Elas poderiam ser deixadas como estão ou, como todas são divisíveis por dois, você pode reduzir cada X_ pela metade para obter 6_, 8_ e 5_ respectivamente. Lembre-se de que você multiplicará isso por suas épocas!
      

Por que fazer isso, você pergunta?

Fazemos isso para balancear o conjunto de dados. Se você mantiver tudo igual, a pasta com mais imagens dominará o treinamento, deixando os outros conceitos com uma fração. Balanceamos o conjunto de dados para garantir que cada conceito receba tempo de treinamento igual, o que impede que um domine e os outros conceitos sejam subcarregados.

Você deve ter em mente, no entanto, que se tiver muito poucas imagens em uma pasta de conceito, esse conceito individual pode ser supertreinado, mesmo que o resto do LoRA esteja bem. Esse é um problema maior quanto maior for a discrepância entre ela e a maior pasta.

Agora que suas pastas estão balanceadas, devemos olhar como nomeá-las e qual será sua tag de ativação para cada uma.

Se você estiver treinando um personagem com vários trajes, nomeie suas pastas como "1_tag_do_personagem, tag_do_traje". Suas duas primeiras tags devem ser essas, nessa ordem.

Se você estiver treinando algo não ligado a um personagem, como armadura de gênero, geralmente crio uma tag para cada versão. Por exemplo, "tagarmaduram" e "tagarmaduraf" para homens e mulheres, respectivamente. Assim como antes, essas devem ser a primeira tag em suas respectivas imagens.

Agora que seus nomes e tags de ativação estão definidos, você pode começar a taguear! Isso pode ser feito como um lora normal, você só tem um monte de imagens para passar.

E é isso! Uma vez tagueado, você pode treiná-lo como antes. Você provavelmente terá tempos de treinamento muito mais longos, dada a quantidade aumentada de imagens, mas no final você terá múltiplos conceitos em um único LoRA para usar como quiser.

## Parte 7 | Treinamento Avançado: LyCORIS e Seus Muitos Métodos

LyCORIS fica mais avançado a cada dia e, à medida que se torna mais comum, acho melhor ter uma seção falando sobre isso. Isso será um pouco mais técnico do que o resto, mas tentarei manter no "precisa saber".

**Tipos de LyCORIS:**

-   **LoCON:** Um LoRA que também afeta as camadas de convolução do modelo base, permitindo saídas mais dinâmicas.

-   **LoHa & LoKR:** Um LoRA que essencialmente é duas versões diferentes de si mesmo, que são combinadas/médias pelo Produto de Hadamard e Produto de Kronecker, respectivamente. Eles levam mais tempo para treinar e são mais orientados para treinamento generalizado.

-   **DyLoRA:** Abreviação de Dynamic LoRA, esta é uma implementação de LoRA que permite que o Rank mude dinamicamente, mas é um LoRA normal fora isso.

-   **GLoRA:** Abreviação de Generalized LoRA, esta é uma implementação feita para generalizar conjuntos de dados diversos de maneira flexível e capaz.

-   **iA3:** Em vez de afetar o rank como a maioria dos LoRA, o iA3 afeta os vetores aprendidos, resultando em um método de treinamento muito eficiente. Similar (aparentemente um pouco melhor?) a um LoRA normal, em um pacote muito menor.

-   **Diag-OFT:** Esta implementação "preserva a energia hiperesférica treinando transformações ortogonais que se aplicam às saídas de cada camada". Em suma, esse tipo é melhor para preservar o entendimento original do modelo base sobre itens coincidentais ao treinamento (como fundos e poses). Isso também aparentemente converge (treina) mais rápido do que um LoRA padrão.

-   **Afinamento Nativo:** Também conhecido como dreambooth, no qual não estamos focando e vamos ignorar para este guia. A implementação do LyCORIS permite que seja usado como um LoRA, mas produz arquivos **muito** grandes.


"Então, o que devo usar?"

Eu diria pessoalmente que cada um tem seus próprios usos, então eu os categorizei de forma semi-geral. Ainda não sou super conhecedor das suas complexidades, mas **me baseei principalmente nas notas de implementação _oficiais_ e na documentação.** O que você escolhe depende de você e é inteiramente baseado nas suas necessidades.

-   **Uso Geral:**
    
    -   LoCON, DyLoRA, iA3, Diag-OFT

-   **Múltiplos Conceitos:**
    
    -   LoCON, LoHa, LoKR

-   **Conceitos:**
    
    -   LoCON, LoHa, LoKR, GLoRA

-   **Estilos:**
    
    -   LoCON, GLoRA, iA3


**Benefícios, Desvantagens e Notas de Uso:**

-   **LoCON:**
    -   Amplamente Aplicável

    -   Afeta Mais Camadas do Modelo

    -   Arquivos Ligeiramente Maiores

    -   Basicamente Apenas um LoRA, Mas Melhor

      -   Dim <= 64 Máximo, 32 Recomendado

      -   Alpha >= 0.01, Metade Recomendado (Quando não estiver usando um otimizador adaptativo)


-   **LoHa & LoKR:**

    -   Bom Para Treinamento de Múltiplos Conceitos

    -   Bom Para Generalização

    -   Tempos de Treinamento Mais Longos

    -   Ruim Para Conceitos Altamente Detalhados

    -   Pode Ser Difícil de Transferir

      -   LoHa

        -   Dim <= 32

        -   Alpha >= 0.01, Metade Recomendado (Quando não estiver usando um otimizador adaptativo)

      -   LoKR

        -   Pequeno: Fator = -1

        -   Grande: Fator = ~8

-   **DyLoRA:**

    -   Encontra Automaticamente o Rank Ótimo

    -   Tempos de Treinamento Mais Longos

    -   Fora isso, Apenas um LoRA

      -   Use com Dim grande (~128), Alpha pela metade (Quando não estiver usando um otimizador adaptativo)

      -   Use Acumulação de Gradiente

      -   Tamanho de Lote de 1 Máximo

-   **GLoRA:**

    -   Muito Bom em Generalização (Estilos e Conceitos)

    -   Tempos de Treinamento Mais Curtos (?, A Testar)

    -   Não Muito Bom em Treinar Assuntos Não Generalizados

-   **iA3:**

    -   Tamanhos de Arquivo Muito Pequenos

    -   Geralmente Aplicável

    -   Geralmente Desempenha Melhor que LoRA

    -   Bom com Estilos

    -   Pode Ser Difícil de Transferir
    
      -   Use com Alta LR (Quando não estiver usando um otimizador adaptativo), a implementação oficial recomenda 5e-3 (0.005) ~ 1e-2 (0.01)

-   **Diag-OFT:**

    -   Tempo de Treinamento Mais Rápido

    -   Melhor Preserva Coincidentes

    -   Geralmente Aplicável
    

## Parte 8 | Treinamento Avançado: Estilos e Temas

Então, você quer treinar um estilo de algum tipo. Independentemente do que seja, para conceitos mais amplos, um LyCORIS é a ferramenta certa para o trabalho, mas, ao contrário de um LoRA, existem vários tipos de LyCORIS para escolher. Se você pulou a Parte 7, recomendo um LoCON, GLoRA ou iA3.

Depois de escolher seu tipo, certifique-se de que seu rank esteja definido para 32 ou menos. LyCORIS parece ter alguns problemas acima de certos pontos (embora você possa ir até 64, acredito), mas 32 é o máximo geralmente aceito antes de começar a ter problemas.

Agora que isso está fora do caminho, você deve começar a construir um conjunto de dados, assim como antes. No entanto, treinamentos de estilo se beneficiam muito mais de conjuntos de dados maiores, então, em vez da faixa de 15-50 de antes, procure ter entre 50-200, na minha experiência 125-150 é um bom lugar para estar.

Depois de obter suas imagens, comece a taguear. Você pode geralmente taguear da mesma maneira que antes, mas lembre-se de que você quer o estilo, não um personagem ou artigo de roupa. Você deve especialmente certificar-se de taguear fundos, roupas e qualquer outro elemento-chave.

Depois de taguear, você está pronto para começar o treinamento. Na minha experiência, esses geralmente levam menos épocas para treinar em comparação com um LoRA: Enquanto recomendo ~100 repetições para um LoRA, esses geralmente estão ok com ~30-40 repetições, mas seus resultados podem variar, dado o tamanho e composição do seu conjunto de dados.

## Parte 9 | Pós-Treinamento: Redimensionamento de LoRA (SDXL)

Como muitas pessoas notarão, os loras do SDXL são _grandes_. Eles ocupam muito mais espaço do que os loras 1.5, consumindo aqueles preciosos gigabytes do seu armazenamento. Felizmente, temos uma solução: Redimensionamento.

Embora esse processo seja totalmente opcional, você _pode_ reduzir o tamanho do arquivo _significativamente_ e _sem perda_, também!

Embora os scripts do Kohya tenham uma maneira de fazer isso, eles são _lentos e otimizados_, então usaremos um novo projeto de um membro do FD, Gaeros. Atualmente, este projeto só suporta SDXL **LoRAs e LoCONs** **apenas**, mas o suporte para outros métodos pode vir no futuro. Baixe o projeto de seu [Github](https://github.com/elias-gaeros/resize_lora/tree/main), e certifique-se de ter suas dependências instaladas.

Depois de instalado, abra uma janela do powershell/cmd na pasta instalada e insira o seguinte após ajustá-lo:

```
python resize_lora.py /caminho/para/o/checkpoint.safetensors /caminho/para/o/lora.safetensors -o /caminho/para/a/pasta/de/saída -r spn_ckpt=1,thr=-1.4
```

Que deve parecer algo assim, quando preenchido corretamente:

```
python resize_lora.py G:\stable-diffusion-webui\models\Stable-diffusion\ponyDiffusionV6XL.safetensors G:\LoRA_Output\MH_Dodogama_a1_8_PonyXL.safetensors -o G:\LoRA_Output -r spn_ckpt=1,thr=-1.4
```

Desde que você tenha todas as dependências felizes e apontadas para os arquivos corretos, o programa, na primeira vez, armazenará em cache valores chave do checkpoint selecionado, que ele pode reutilizar quantas vezes forem necessárias ao redimensionar outro lora no mesmo modelo. Esse primeiro cacheamento será o mais longo que você já esperou, pois redimensionamentos futuros levarão segundos literais.

Essas configurações específicas são as minhas preferidas para um redimensionamento seguro e sem perdas, que encontrei funcionar bem em todas as instâncias que testei. Se você estiver com vontade de mexer, pode optar por redimensionamentos mais agressivos, mas a qualidade do seu modelo pode cair, em alguns casos consideravelmente.

## Changelog

12/06/24

-   Parte 4:

    -   Corrigidos alguns pequenos erros gramaticais.

    -   Alteradas as LRs recomendadas para minhas configurações de SDXL.

    -   Adicionadas algumas configurações anteriormente ignoradas.

    -   Adicionadas seções referindo-se ao uso de DoRA.

-   Parte 9:

    -   Criado, cobrindo o redimensionamento de LoRA com minhas próprias configurações.


28/05/24

-   Introdução:

    -   Ajustada um pouco.

-   Parte 1:

    -   Feitas distinções entre recomendações 1.5 e novas XL.

    -   Ajustada alguma linguagem minoritariamente.

    -   Adicionada nota de aviso à minha recomendação do Pixiv.

-   Parte 2:

    -   Ajustes menores para corrigir erros gramaticais que notei.

    -   Adicionada uma pequena quantidade de explicação adicional em algumas seções.

    -   Adicionado um upscaler recomendado.

-   Parte 3:

    -   Adicionadas algumas distinções entre SD1.5 e XL.

    -   Ajustado alguns textos para adicionar ênfase em certas partes.

    -   Ajustada alguma redação na parte 3.5.

    -   Exemplos de tags ligeiramente atualizados na parte 3.6.

-   Parte 4:

    -   bmaltis mudou o layout da GUI, então agora tenho que reescrever as instruções >:(

      -   Reformatada a maior parte da seção e adotadas algumas configurações experimentais na configuração padrão.

      -   Também adicionadas especificidades para minhas configurações de SDXL.


24/03/24

-   Adicionadas taxas de aprendizado faltantes para minha configuração AdamW. Opa.

-   Parte 3.5 ligeiramente expandida.


04/03/24

-   Adicionada seção cobrindo tokens de classe à Parte 3.

-   Parte 3.5 (Exemplos de tags) mudada para 3.6.

-   Adicionada nova Parte 3.5, cobrindo Preservação de Prioridade.

    -   Adicionada configuração de Pasta de Regularização à Parte 4.

-   Configurações experimentais atualizadas.

-   Ajustadas algumas descrições de configurações.

-   Adicionadas algumas clarificações em áreas diversas.


29/02/24

-   Uma reestruturação extensa, mas no geral menor, implementando críticas de outros treinadores, principalmente ArgentVASIMR.

    -   Parte 1:

      -   Expandido sobre imagens de Duo/Trio/Grupo.

    -   Parte 2:

      -   Expandido/editado vários parágrafos para fornecer mais alternativas e informações mais claras.

    -   Parte 3:

      -   Edições menores, terminologia mudada para ser mais apropriada.

    -   Parte 4:

      -   Ajustadas algumas configurações, principalmente em relação ao uso do Adam opt.

      -   Adicionada um pouco mais de descrição para algumas configurações.

      -   Adicionados parâmetros para embaralhamento de legendas.

      -   Adicionada seção experimental sobre escalonamento alfa quando não estiver usando otimizadores adaptativos.

    -   Parte 5:

      -   Ajustada alguma redação para aderir melhor aos termos adequados.

      -   Ajustada Q&A sobre treinamento fp16 completo.

    -   Parte 6:

      -   Ajustada alguma terminologia, de novo.

      -   Ligeiramente alterada e fornecido um exemplo para a fórmula de balanceamento de conjunto de dados.


26/02/24

-   Configurações experimentais atualizadas.

-   Ajustadas algumas explicações ligeiramente em relação ao LyCORIS.


09/02/24

-   Configurações experimentais atualizadas.

-   Adicionados mais detalhes à parte 4.

-   Adicionada uma breve seção sobre algumas novas descobertas à parte 2.


01/02/24

-   Adicionada parte 2.5, uma subseção sobre Nightshade e outros "venenos" de IA.


07/01/24

-   Parte 7 movida para parte 8 e explicação do LyCORIS removida.

-   Adicionada (nova) parte 7, aprofundando mais no LyCORIS.

-   Ajustadas algumas configurações experimentais.


05/01/24

-   Configurações experimentais ajustadas e adicionadas algumas explicações para alguns valores.

-   Adicionadas perguntas de Q&A.

-   Expandido sobre o valor "normas de peso da escala" na parte 4.

-   Corrigidas seções sobre minsnr e zsnr para diferenciá-las corretamente.

-   Ajustados "parâmetros adicionais": Valor não é mais necessário.


30/12/23

-   Adicionadas configurações experimentais à parte 4.

-   Título mudado para incluir LyCORIS.

-   Adicionada pergunta de Q&A.


28/12/23

-   Correção de mais erros gramaticais.

-   Parte 1 e 2 ligeiramente expandidas.

-   Adicionada seção sobre tags implícitas à Parte 3.

-   Adicionadas pequenas elaborações em algumas áreas.


27/12/23

-   Correção de pequenos erros gramaticais nas partes 3 e 4.

-   Adicionadas novas perguntas de Q&A.

-   Adicionadas partes 6 e 7, cobrindo Treinamento de Múltiplos Conceitos e Estilos, respectivamente.

-   Adicionada parte 3.5 para exemplos de tags, adicionados dois para começar.


26/12/23

-   Guia Criado

<p>
</p>


# Comentários

> **[deleted]**
Muito bom.\
Conhece alguma boa fonte que explique o que diabos é Vpred?

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
@DUser Não sou muito versado sobre o que ele realmente faz, mas pelo que
entendo, ele "prevê" a trajetória da geração de imagem conforme ela
avança, permitindo menos passos necessários para chegar ao resultado
final. Além disso, não tenho certeza, a não ser que ele está afetando o
agendador de ruído para ser melhor no geral.

---

> [**danadere742**](https://civitai.com/user/danadere742)
Pode dar mais informações sobre o treinamento de estilo, quais são suas
características na marcação e no treinamento? E quantas tags por imagem
significariam que está excessivamente marcado?

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@danadere742](https://civitai.com/user/danadere742) Planejo adicionar uma seção sobre isso mais tarde, mas como um
resumo breve, isso é muito mais adequado para um LyCORIS do que para um LoRA. Na
minha experiência, ter mais imagens (50+) é melhor. A marcação é praticamente a
mesma, mas você deve ser mais minucioso, especialmente com os fundos. Meu Lycoris
de estilo "mecanizado" que fiz recentemente foi marcado de forma bem mínima,
mas usei ~150 imagens, e era mais um estilo conceitual do que um estilo de
artista. Também precisei de menos treinamento, acho que treinei apenas por 30-40
épocas. Mas independentemente do que você está treinando, é melhor marcar os
elementos-chave e não muito mais do que isso.

---

> [**danadere742**](https://civitai.com/user/danadere742)
[@Valstrix](https://civitai.com/user/Valstrix) Então, cerca de 20 tags por imagem é demais? Não preciso
descrever as características dos personagens para o estilo (exceto pela espécie
e pose)? Vou aguardar essa seção.

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@danadere742](https://civitai.com/user/danadere742) Minhas marcações usuais são ~5-20 tags por imagem. Se você
tem mais de 20 por imagem, eu diria que depende de quantas dessas são
compartilhadas. Se 10 das 20 são compartilhadas entre mais de uma imagem, você
está adicionando funcionalmente apenas as extras que são únicas para uma imagem.
O que causa um colapso/falha no treinamento de estilo é a IA atribuir o estilo em
si a múltiplas tags. Se as tags extras são algo que a IA já conhece, como
shorts, por exemplo, ela deve ser capaz de identificar esse item e não atribuir
o estilo. Se é algo que ela não conhece ou tem uma compreensão muito fraca,
no entanto, pode potencialmente causar problemas se você tiver muitas. Infelizmente,
não temos uma boa maneira de determinar os resultados sem realmente treinar, então
muitas vezes você faz múltiplas tentativas para ajustar na categoria "exata". Acabei
de terminar de treinar a sexta tentativa para um LoRA hoje, e depois da segunda
tentativa, tudo o que tenho ajustado é uma tag não cooperativa, lol. Vou trabalhar
em uma seção escrita adequada para isso em breve. :)

---

> [**Kazrik**](https://civitai.com/user/Kazrik)
Ótimo guia, obrigado por criá-lo! Tenho apenas algumas perguntas:\
Prefiro marcar no estilo booru mesmo para 3D realista/alto polígono. O
modelo base 1.5 ainda funcionaria bem ou há algo mais que eu deveria
usar?\
Você tem exemplos de como você marca seu conjunto de dados que estaria
disposto a compartilhar? Algumas imagens de exemplo ou o conjunto de
dados completo?\
E para treinar no modelo base 1.5, há algum inconveniente em usar 1024x1024
como resolução de treinamento além do impacto no desempenho?

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@Kazrik](https://civitai.com/user/Kazrik) Embora treinar o modelo base 1.5 com tags booru funcione na
teoria, o modelo em si não tem conhecimento prévio dessas tags como tags reais,
apenas palavras. Se você usar tags booru para treinar, deve usar um ajuste fino
que utilizou as tags booru em seu treinamento. Se o modelo já sabe o que algo
é, seu LoRA não precisa treiná-lo do zero, que é essencialmente o que você está
fazendo para cada tag que o modelo base não entende.\
Ao treinar em qualquer modelo, não apenas no base 1.5, você nunca deve
treinar em uma resolução superior à sua capacidade nativa de geração. Treinar
acima do que ele pode fisicamente usar não só desacelera desnecessariamente,
como pode prejudicar ativamente seus resultados. Se você não quer perder os
detalhes de resoluções mais altas, considere recortar suas imagens e ter
múltiplas versões daquela imagem (por exemplo: 1 retrato, 1 três-quartos, 1 corpo
inteiro) e marcar de acordo. De modo geral, seria melhor treinar em um modelo que
suporta a resolução nativa mais alta.\
Quanto aos exemplos de marcação, vou tentar adicionar alguns na próxima
atualização. :)

---

> [**Kazrik**](https://civitai.com/user/Kazrik)
[@Valstrix](https://civitai.com/user/Valstrix) Obrigado pela resposta, muito apreciado!

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@Kazrik](https://civitai.com/user/Kazrik) De nada! Também acabei de incluir alguns exemplos na atualização
que estava trabalhando.

---

> [**GOMIKAZAN**](https://civitai.com/user/GOMIKAZAN)
Literalmente o melhor guia que já li. Segui e enviei mensagem para o criador e ele
teve tempo de me ajudar a configurá-lo e executá-lo, até me ajudou a corrigir algo que
errei porque pulei uma etapa. Valstrix tem que ser a pessoa mais real que existe.

---

> [**danadere742**](https://civitai.com/user/danadere742)
[@Valstrix](https://civitai.com/user/Valstrix) Obrigado pelas novas seções! Estou tentando treiná-lo há um
tempo, usando colab, e consegui me aproximar do estilo. LyCORIS parece ajudar um pouco,
mas ainda há grandes anomalias na anatomia e composição (elas ficam completamente
distorcidas na maioria dos casos). Isso ainda parece um problema de marcação? Ou devo
tentar algo diferente? Estou tentando treiná-lo no fluffyrock-terminal (não vpred). Agora
são cerca de 10-20 tags por imagem, todas são tags e6, a grande maioria delas são
repetidas.

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@danadere742](https://civitai.com/user/danadere742) é difícil dizer só com isso, mas algumas possibilidades
me vêm à mente; A: Você usou muitas tags de composição, eu geralmente limito
essas a 1-2 por imagem, geralmente ângulo e às vezes comprimento do plano.
Verifique se nenhuma delas se contradiz. B: Seu conjunto de dados é grande o
suficiente para você precisar treinar por mais tempo. Não sei quantas imagens
você tem, mas conjuntos de dados maiores precisarão de tempos de treinamento
mais longos. C: Alguma configuração está ativada e não deveria estar. Como você
está usando um modelo SNR sem Vpred, certifique-se de que as configurações de SNR
estejam ativadas, mas as configurações de vpred estejam desativadas ou não
incluídas. Usar configurações de vpred em um modelo não suportado para treinamento
pode causar falhas catastróficas. D: O modelo em que você está testando suas
saídas é incompatível com tags e6. Por exemplo, a maioria dos meus modelos tem
precisão notavelmente reduzida ou simplesmente não funciona na maioria dos modelos
de anime, simplesmente por causa da diferença no modelo de treinamento e nas tags
usadas. Espero que isso ajude!

---

> [**crazyjdevola275**](https://civitai.com/user/crazyjdevola275)
Estou tentando fazer um LoRA de "boss-eyed", ou seja, olhos virados para fora.
É possível aplicar esse estilo a qualquer tipo de personagem que o modelo base tenha sido
treinado. Parece que os rostos são baseados apenas no conteúdo em que o LoRA foi treinado.
Por exemplo, quero criar "boss-eyed Donald Trump" ou "boss-eyed Barack Obama". Se não tiver
exemplos dessas pessoas no meu conjunto de dados, parece que o LoRA não aplicará as mudanças
aos olhos. Alguma dica ou truque para conseguir um modelo geral que possa ser usado em qualquer
personagem?

---

> [**danadere742**](https://civitai.com/user/danadere742)
[@Valstrix](https://civitai.com/user/Valstrix) Meu conjunto de dados tem cerca de 150 imagens, o MinSNR gamma
está configurado para 5. Eu testo no mesmo modelo em que treino. Acabei de perceber que nem
todas as imagens têm tags de composição, vou corrigir isso e tentar novamente.

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@crazyjdevola275](https://civitai.com/user/crazyjdevola275) Contanto que seu conceito seja separado das pessoas,
deve ser amplamente aplicável. Certifique-se de que em suas tags de treinamento,
"boss-eyed" seja uma tag separada por conta própria. Se necessário, marque coisas
como cor dos olhos/pele e gênero, mas não inclua os nomes de pessoas específicas durante
o treinamento. Como você está treinando apenas os olhos, pode tentar cortar suas imagens
para retratos se ainda não estiverem, pois fotos de corpo inteiro não são necessárias para
o conceito. Você também pode estar treinando em excesso, e o treinamento está atribuindo os
olhos a um tipo específico de pessoa. Garantir que seu conjunto de dados seja amplamente variado,
neste caso com muitas etnias, cores de olhos, etc., também deve ajudar a separar os olhos do
resto.

---

> [**Kazrik**](https://civitai.com/user/Kazrik)
O que você usa para seu prompt de classe ao fazer um LoRA de furry? Tipo,
para um personagem cão antropomórfico, eu colocaria "dog", "man", "furry", etc.?

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@Kazrik](https://civitai.com/user/Kazrik) Como você marcaria furries depende do seu modelo. Como eu
treino no fluffyrock que usa tags e6, eu os marcaria como "{activator}, anthro,
{male/female}". Se a espécie não é o assunto do treinamento, eu colocaria
isso depois do gênero. Em um modelo que usa tags booru, você poderia usar algo
como "{activator}, furry {male/female}". Dependendo de quão exageradamente
sobrecarregado o modelo está (muitos modelos de anime não conseguem fazer nada além
de humanos de forma razoável), você pode querer usar tags para cor da pele, orelhas de
animal e caudas, mas geralmente é melhor usar um modelo furry se você está procurando
fazer/usar qualquer coisa relacionada a furries.

---

> [**Kazrik**](https://civitai.com/user/Kazrik)
[@Valstrix](https://civitai.com/user/Valstrix) Desculpe, acho que houve um mal-entendido, quero dizer o campo
Class Prompt em Kohya_ss como visto nesta captura de tela:\
Acho que com um modelo furry você poderia usar anthro, mas ainda estou usando o
SD 1.5 base - realmente preciso experimentar treinar com um modelo furry.

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@Kazrik](https://civitai.com/user/Kazrik) Ah, entendi. Eu não uso a preparação de conjunto de dados
integrada, haha. Se estou lembrando corretamente, instance é o que você
nomeia a pasta e sua primeira tag, e class é um contêiner geral como
"anthro". Dado que faço 100% da marcação manual com um programa externo,
não uso muito os termos.

---

> [**crazyjdevola275**](https://civitai.com/user/crazyjdevola275)
[@Valstrix](https://civitai.com/user/Valstrix) Obrigado por responder. As fotos que usei são apenas de rosto,
mulheres e homens de diferentes idades, então sem fotos de corpo inteiro.
Talvez o conjunto de dados seja pequeno demais? Usei cerca de 50 imagens
de pessoas com olhos tortos, baseadas em "thispersondoesnotexist" e algum
photoshop. Na primeira rodada, incluí apenas uma tag "boss-eyed", mas talvez
precise descrever um pouco mais outros atributos, mas não o nome.\
\
Além disso, baseei o LoRA no EpicPhotoGasm como base. Importa qual modelo
base eu uso? Gostaria que o LoRA fosse compatível com a maioria dos checkpoints
(se possível).

---

> [**Kazrik**](https://civitai.com/user/Kazrik)
Acabei de perceber que o software Booru Dataset Tag Manager parece
usar apenas tags DanBooru, mesmo que haja uma lista E621 na pasta de tags.
Pelo que posso dizer, não há como mudar para tags E6 além de deletar o
Danbooru.csv dessa pasta. Provavelmente isso estava atrapalhando meu treinamento
no Fluffyrock.

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@crazyjdevola275](https://civitai.com/user/crazyjdevola275) treinamento com uma única tag só funciona se todas as
imagens forem do seu sujeito (ou seja, a mesma pessoa), e resultará em uma
saída altamente generalizada. Se você treinou com uma única tag, a IA homogenizou
tudo o que aprendeu das suas imagens naquela tag como uma mistura praticamente
inutilizável. Certifique-se de marcar pelo menos algumas características principais
de cada imagem, para que os olhos possam ser corretamente identificados e separados.
Se você quer que a saída seja geralmente aplicável, manter-se no SD 1.5 base ou algo
próximo funcionaria.

---

> [**Kazrik**](https://civitai.com/user/Kazrik)
Desculpe, tenho outra pergunta... Estive experimentando diferentes
modelos hoje, mas até agora nenhum produziu resultados melhores do que minhas
configurações padrão usando o base SD 1.5, mesmo com marcação E621. Existem tantas
versões do Fluffyrock, quero uma que não seja vpred e possivelmente suporte min SNR.\
\
Este aqui: <https://civitai.com/models/92450?modelVersionId=124680>\
\
Ou há outro que devo usar?

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@Kazrik](https://civitai.com/user/Kazrik) Ah sim, esses são realmente muito desatualizados. Eu pego os
meus do repositório deles no huggingface:\
<https://huggingface.co/lodestones/furryrock-model-safetensors/tree/main/fluffyrock-1088-minsnr-zsnr-vpred-ema>.\
Eu mesmo tenho usado fluffyrock-1088-zsnr-vpred-e257-minsnr-e31-ema.
Esse link leva à pasta com os modelos zsnr/vpred, mas se você voltar ao repositório
principal, eles têm outras categorias.

---

> [**danadere742**](https://civitai.com/user/danadere742)
[@Valstrix](https://civitai.com/user/Valstrix) Parece que melhorei o conjunto de dados, mas ainda está
quebrado. Se eu tentar treinar por mais tempo, ele simplesmente fica
exagerado. Até tentei treinar sem um codificador de texto, como alguns
guias recomendam para estilo, mas o problema é o mesmo. Devo tentar
AdamW? Já tentei treinar usando ele várias vezes, mas talvez você possa
dar conselhos sobre suas configurações?

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@danadere742](https://civitai.com/user/danadere742) Bem, uma melhora é algo pelo menos! Algo que esqueci de
mencionar antes é que com sua anatomia distorcida (se ainda estiver acontecendo),
na minha experiência, isso pode ser potencialmente causado por imagens dentro
do próprio conjunto de dados. Por exemplo, usando meu LoRA de serpente nas
nuvens, eu tinha uma visão específica em uma imagem singular, de uma vista de
três quartos com as costas arqueadas de um jeito muito diferente do resto. O
treinamento se apegou a isso, e até eu remover a imagem, estava fazendo 90% das
saídas gordas, lmao. Talvez dê uma olhada no seu conjunto de dados para ver se
algo se assemelha aos mesmos tipos de distorções que você está obtendo. Caso contrário,
se quiser tentar um Adam opt, tente AdamW8bit. Certifique-se de remover as
configurações para prodigy, então minha configuração anterior era um TE LR de
0,0005 e um Unet LR de 0,0000833. Os Adams geralmente precisam de alguns ajustes
para encontrar um valor bom, mas esses devem ser um bom ponto de partida. Veja
como isso funciona para você :)

---

> [**danadere742**](https://civitai.com/user/danadere742)
[@Valstrix](https://civitai.com/user/Valstrix) Eu diria que isso não é apenas anatomia distorcida, mas
horror Lovecraftiano :) Sim, a razão para isso é o conjunto de dados. Removi
todos os duos e outras imagens "difíceis", e parece estar muito melhor nesse aspecto,
mas agora o conjunto de dados em si está muito pequeno.

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@danadere742](https://civitai.com/user/danadere742) Se você não tem nada que seria afetado negativamente
pela ampliação de flip (como um logotipo/símbolo/etc) que você se
importa, pode tentar usar isso para expandir artificialmente seus dados,
o que pode ajudar bastante. Além disso, não há solução real para um
conjunto de dados ser pequeno demais além de encontrar mais dados apropriados.

---

> [**Wengle**](https://civitai.com/user/Wengle)
Beleza! Valeu demais! Estava querendo experimentar isso. Bom ter alguns
conselhos de um dos meus favoritos!

---

> [**ClemtheGem**](https://civitai.com/user/ClemtheGem)
Bom guia. Cobriu várias coisas que foram perdidas em outros guias. Ainda
estou tendo problemas para treinar um personagem conhecido com múltiplas roupas,
no entanto. Eu obtenho a mesma imagem exata, seja usando minha tag charoutfit1 ou
charoutfit2 (exceto talvez uma folha em uma árvore muda ligeiramente ou algo assim).
Você pode expandir um pouco sobre este tópico - que tipo de imagens você deve ter
em cada pasta e como elas devem ser marcadas?\
\
Talvez você possa me dizer onde estou errando aqui na minha última tentativa
(usando "charname" como um espaço reservado):\
\
Tamanho do lote configurado para 1.\
\
Pastas:\
\
"10_charname" - 30 imagens. Principalmente closes e retratos sem roupas
mostradas. Marcando apenas elementos de fundo, pose, etc. Também tentei incluir fotos
de corpo inteiro vestindo roupas não padrão. As primeiras tags são "charname, 1girl"\
\
"14_charname, charoutfit1" - 22 imagens. Fotos de corpo inteiro vestindo a
roupa 1. Marcando apenas elementos de fundo, pose, etc. As primeiras tags são "charname,
charoutfit1, 1girl"\
\
"13_charname, charoutfit2" - 24 imagens. Fotos de corpo inteiro vestindo a
roupa 2. Marcando apenas elementos de fundo, pose, etc. As primeiras tags são "charname,
charoutfit2, 1girl"\
\
Tentei combinações de marcação da aparência do personagem (cabelo, olhos, etc.),
na pasta charname e nas pastas de roupas, e também marcando as roupas nas pastas de roupas,
mas nada parece funcionar. Tentei com mais e menos imagens, todos os tipos de repetições e
épocas.

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@ClemtheGem](https://civitai.com/user/ClemtheGem) Se você está obtendo a mesma imagem todas as vezes, isso me
diz que algo deu muito errado. Se é a mesma imagem, independentemente da seed,
é um problema massivo de sobrecarga. Se a imagem é diferente por seed, mas as
tags de roupa não fazem nada, isso me diz que as tags de roupa de alguma forma
não reteram nenhum dado. Parece que você está usando um modelo booru, então meu
método de marcação (baseado em tags e6) pode precisar de alguns ajustes. Então,
talvez tente o seguinte:\
\
pasta charname: Mantenha como está. Ter muitas fotos com roupas diferentes
pode ajudar, mas marque-as de acordo.\
\
pastas charoutfit1 & 2: Remova charname, mantenha a pasta como 14_charoutfit1.
Faça charoutfit1, 1girl suas duas primeiras tags e marque as características dos
próprios personagens separadamente. Se você conhece imagens onde outros personagens
estão usando a roupa desejada, essas podem ser boas para incluir. Alternativamente,
você pode manter a tag charname em vez de descrevê-los, mas faça dela sua 3ª ou
tag posterior. Faça isso para ambas.\
\
Marcar roupas geralmente deve ser feito apenas se você quiser separá-las da tag
principal ou se não estiverem sendo absorvidas na tag corretamente. Por exemplo,
em uma roupa com uma tiara, tiara pode ser marcada se você quiser essa parte opcional.
Marcar características do personagem não deve ser feito ao treinar esse personagem
para uma tag, pois marcar coisas como cor dos olhos/cabelos/pele vai separar isso do
conceito principal (a menos que você queira isso), então geralmente só marque as
características se elas se desviarem (como coloração alternativa, olhos diferentes, etc.).\
\
Espero que isso ajude. :)

---

> [**ClemtheGem**](https://civitai.com/user/ClemtheGem)
[@Valstrix](https://civitai.com/user/Valstrix) Obrigado pelo conselho, mas ainda não funcionou. As tags das
roupas simplesmente não parecem estar sendo captadas. Estou pensando que deve
ser algo relacionado às imagens, em vez das tags.

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@ClemtheGem](https://civitai.com/user/ClemtheGem) Hmm, é estranho que não estejam sendo remotamente
captadas de qualquer forma. Você pode tentar ajustar seu tempo de treinamento e/ou
taxas de aprendizado, é possível que esteja simplesmente não absorvendo informações
suficientes para afetar significativamente nada, talvez?

---

> [**Kazrik**](https://civitai.com/user/Kazrik)
Ao treinar um personagem, você pode ter closes das roupas sem o rosto?
Como apenas os calçados ou chapéu, por exemplo (também talvez closes de
partes do corpo como patas?) Meu objetivo é tornar as roupas que não estão
em muitas das imagens mais detalhadas e precisas. Não tenho certeza se isso
atrapalharia as coisas.\
\
Se sim, você marcaria sem o nome do personagem? Tipo "lower body, boots".

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@Kazrik](https://civitai.com/user/Kazrik) Sim, você pode definitivamente fazer isso, mas dependendo de como
você está marcando a roupa, isso pode causar problemas. Se você está treinando a
roupa para uma única tag, deve tentar usar tags de composição para dizer onde está
olhando, em vez de marcar as partes individuais. Se a roupa já tem cada item marcado,
isso é mais fácil para esse caso, e você só marca cada peça conforme vê individualmente,
o lado negativo é que você tem que usar todas as tags juntas em um prompt para obter
a roupa completa. Fazer uma combinação de ambos pode resultar em peças sendo separadas
do conceito principal, o que pode causar inconsistências na geração. Por exemplo de
tag, "headshot portrait, [outfittag]" ou "headshot portrait, glasses, hat" ambos podem
funcionar.

---

> [**Kazrik**](https://civitai.com/user/Kazrik)
[@Valstrix](https://civitai.com/user/Valstrix) Interessante, obrigado pela sua resposta! :)

---

> [**suede2031691**](https://civitai.com/user/suede2031691)
Sobre LyCORIS, você sabe como mesclar vários modelos que foram treinados
em 1 modelo?\
Em vez de treinar um loha multiconceito em uma única execução, você mescla
vários modelos loha em 1.\
Métodos que tentei:\
1\. --network weights Carregue o modelo loha anterior.\
Na versão atual do script kohyaa-ss, não deve estar funcionando
corretamente porque treinei TE no loha carregado, e desta vez, se eu
ativar --network_train_unet_only, a camada TE estará faltando no modelo salvo,
indicando que sua função não é adicionar novos resultados de treinamento ao
modelo loha original.\
2, mesclar antes de extrair. Tentei usar o script LyCORIS para mesclar
o loha no modelo base um por um antes da extração.\
As forças dos modelos obtidos pelos 5 métodos de extração serão alteradas.
Ou seja, preciso alterar a INTENSIDADE para obter uma imagem semelhante à anterior.

---

> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
[@suede2031691](https://civitai.com/user/suede2031691) Já vi alguns desses circulando por aí às vezes, mas não
posso dizer que tentei isso pessoalmente ou realmente olhei para isso. Treinar
um multi-conceito ou vários modelos separados com escala de peso geralmente é a
maneira mais fácil de obter conceitos mutuamente compatíveis, se esse é o seu
plano. A mesclagem parece conveniente se funcionar com ajustes mínimos, mas
caso contrário, para mim, parece muito esforço extra para algo que você pode
fazer apenas carregando um modelo extra.

---

> [**suede2031691**](https://civitai.com/user/suede2031691)
[@Valstrix](https://civitai.com/user/Valstrix) Obrigado, entendi.

---

> [**Varen6**](https://civitai.com/user/Varen6)
Tenho batido de frente com prodigy há um mês tentando treinar um estilo
lycoris, mas sempre encontro o mesmo problema: o codificador de texto simplesmente
não consegue parar de sobrecarregar. Treinando apenas o Unet: tudo +- bom, exceto
que às vezes ele interpreta mal o prompt um pouco, mas quando ativo o treinamento
do codificador de texto - ele bagunça completamente a composição.\
\
Algum conselho sobre isso que possa ajudar?\
Configurações atuais são\
Net/Conv = 32-32/16-16\
Prodigy: Tentei d_coef=2 e d_coef=0.8, weight decay=0.1 e tentei 0.01\
Scale weight norms: com e sem\
Multires iterations = 10, discount = 0.3\
\
268 imagens. Tentei mais de 15, 23, 30, 45 épocas (4k, 6k, 8k, 12k total de passos)\

>> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
Por curiosidade, você já olhou os resultados das contagens de passos abaixo de 4k? Prodigy é muito resistente ao sobrecarregamento do Unet, então é possível que você esteja potencialmente treinando demais e ele só exploda o TE, enquanto o Unet está bem graças à resistência ao sobretreinamento. Quando treinei o meu, estava vendo bons resultados em contagens de passos mais baixas do que meu treinamento normal, em torno de 2k passos comparado aos normais 4k-ish. Eu sugeriria tentar isso, pelo menos, mas não encontrei nenhum problema importante com o TE na minha experiência pessoal.

>>> [**Varen6**](https://civitai.com/user/Varen6)
Eu não tinha tentado, mas vou tentar, muito obrigado. Desconsiderei menos de 4k porque esperava que 4k funcionasse, considerando que você fez um estilo (embora conceitual, não um de artista) com 4,5k (30x150). Talvez precise olhar para resultados mais baixos.

---

> [**mog**](https://civitai.com/user/mog)
A marcação é a parte mais difícil porque nem todo mundo consegue fazer isso com suas próprias palavras (especialmente em inglês)

---

> [**Varen6**](https://civitai.com/user/Varen6)
Para adicionar ao meu post anterior de um mês atrás:\
\
Ao longo deste mês, experimentei uma grande variedade de configurações
tanto para Prodigy quanto para AdamW8Bit (AdamW também), tentando encontrar uma
linha de base para treinar um estilo de artista, sem nunca conseguir os resultados
que eu queria: Mudança de estilo de artista com o mínimo de mudanças na composição,
mas sem sucesso. Algo sempre dá errado, ou não é suficiente (com prodigy menos de 4k
passos está no nível de se assemelhar ao estilo, mas ainda não sendo próximo o
suficiente).\
\
E essa experimentação muitas vezes me levou a um beco sem saída. E o
codificador de texto parece ser o motivo. Desde que eu o treine por tempo suficiente
e use tags reais de uma das imagens do conjunto de dados na amostragem - surpreendentemente,
eu obtenho uma réplica próxima dessa imagem do conjunto de dados, não uma mudança de
estilo da amostra original. E considerando o codificador de texto sendo meio que uma
camada de tradução de linguagem para embeddings - pode estar aprendendo do conjunto de
dados o que eu não quero que ele aprenda. Então, se considerarmos que estou tentando ensinar
um conceito que não tem dependência de tags - estilo de artista sem palavra de ativação,
há alguma vantagem real em treinar o codificador de texto? Porque, pelo que meus testes
me mostraram - treiná-lo para esse propósito sempre me dá resultados piores.

>> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
Hmm, isso é uma ideia, com certeza. Não fiz muito com estilos recentemente,
mas se você está indo sem um token de instância/ativador, treinar apenas o Unet poderia teoricamente funcionar? Você também poderia tentar um LR muito baixo no TE, mas não tenho certeza de como isso afetaria estilos especificamente. O tipo de resultado que você está procurando fica um pouco mais próximo do reino de ajustes finos do que o treinamento normal de LoRA, pois você está efetivamente procurando alterar o modelo base enquanto o LoRA estiver carregado. Eu poderia realmente apontá-lo para olhar como as pessoas fazem os LoRAs "deslizantes", que trabalham em um conceito semelhante - não olhei para eles pessoalmente, mas é possível que isso possa lhe dar uma pista?

>>> [**Varen6**](https://civitai.com/user/Varen6)
Sim, o conceito de LoRA deslizante parece meio semelhante ao que estou
fazendo. Obrigado pela dica! E vou tentar testar mais extensivamente sem treinamento
do codificador de texto. Uma pergunta, no entanto: você sabe, as tags nos arquivos de
legenda afetam apenas o TENC ou também o UNET? Estou interessado se haverá diferença
entre treinar sem codificador de texto (ou com um LR extremamente baixo) e treinar com
legendas vazias.

>>>> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
Não tenho 100% de certeza, mas acredito que as legendas afetem tanto o Unet quanto
o TE, mas são usadas de maneiras ligeiramente diferentes. Não tenho certeza do que
legendas vazias fariam - está tudo bem em alguns casos (como regularização), mas geralmente
é evitado para treinamento de conceito. Nunca vi nenhuma menção a favor ou contra em termos
de treinamento de estilo, então isso também pode ser outra coisa a experimentar.

>>>>> [**Varen6**](https://civitai.com/user/Varen6)
E já que estou perguntando. Enquanto trabalha em Lycoris, você já olhou
para configurações específicas de Lycoris, como "usar decomposição de Tucker" (use Tucker decomposition), "usar escalar" (use scalar) e "treinar norma" (train norm)? Você tem algum comentário sobre elas?

>>>>>> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
Não muito no momento, não. Pelo que entendi, são amplamente opcionais. Acho que
a Decomp de Tucker faz algo semelhante ao LoHa/LoKR (que ouvi dizer que não são os
melhores), mas aplica isso a outros métodos Lycoris. Não tenho certeza do que as
opções escalar ou norma fazem especificamente, no entanto. Vou ter que olhar mais para
elas por conta própria.

---

> [**kosmo**](https://civitai.com/user/kosmo)
Obrigado pelo guia.
>- Com AdamW você menciona seu LR para TE e Unet, mas qual é o seu LR principal?
0.0001 igual ao Unet?\
>- Sobre a marcação, você não mencionou o comprimento máximo do token. É bem
fácil ultrapassar o padrão de 75 tokens, especialmente se você usar as longas
tags de score para modelos Pony. Você teve problemas ao aumentar o comprimento
máximo do token para 225 no Kohya?\
>- Falando de Pony, você tentou treinar LoRA para isso? Estou tentando treinar
estilos nisso, não tenho certeza de quão bem suas configurações se traduzem para
modelos SDXL como Pony, além de obviamente treinar 1024x.
Obrigado

>> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
Ah, o LR principal é um valor geral para o caso de você não configurar
manualmente o TE ou o Unet. Se ambos estiverem configurados, é totalmente substituído.
Geralmente, só faço com que corresponda a um dos dois valores, mas realmente não importa.\
Não tive problemas com o comprimento do token, eu geralmente mantenho em 225 como um
padrão. Pelo que sei, isso não impacta muito.\
Quanto a Pony, só fiz um treinamento mínimo nele, então não tenho uma base muito sólida
para isso ainda. Não me deu muito problema reutilizar os mesmos conjuntos de dados dos
meus LoRAs 1.5, mas não mexi em usar nenhuma das tags específicas de pony durante o treinamento
ainda. Mas sim, fora aumentar a resolução e garantir que você não tenha vpred ativado, foi
simples treinar, não precisei mudar muito nos meus (limitados) testes.

---

> [**shoopdawoop121**](https://civitai.com/user/shoopdawoop121)
Qual taxa de aprendizado você recomenda para o otimizador adamw? Você diz
para usar 1 para prodigy, mas nada para adamw

>> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
Ah, parece que esqueci de colocar esses, quemops lol. Para TE tenho
usado 0,00005, com um Unet de 0,0001.

---

> [**mrseanpaul81**](https://civitai.com/user/mrseanpaul81)
Ótimo guia! Estou apenas começando nesse mundo. Estou realmente interessado
em criar modelos de mim mesmo/amigos/família. Acho que devo me concentrar nas
dicas "gerais". Tenho seguido este guia em vídeo (<https://www.youtube.com/watch?v=y2J7EZUk_a0>)
já que sou novo nisso. Vou querer treinar novamente meus modelos atuais usando seu método
e ver a diferença mais tarde :). obrigado :)

---

> [**spunchebop**](https://civitai.com/user/spunchebop)
Existe um marcador e6 da mesma maneira que o WD14 é um marcador booru? Você
mencionou o BooruDatasetTagManager, e se a resposta for essa, como eu o adapto para
tags e6?

>> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
Se você está procurando um autotagger como o wd14, Thessalo tem trabalhado
em um autotagger e6 há bastante tempo. Não é perfeito, mas alguém no huggingface
configurou um espaço para um dos modelos recentes:\
<https://huggingface.co/spaces/cdnuts/eva02-vit-large-448-8046>. Quanto ao BDTM,
ele tem um autotagger semi-integrado nas versões mais recentes, mas eu não tentei
e parece usar modelos wd por padrão, embora você possa potencialmente adicionar novos
modelos a ele manualmente. Devo adicionar isso ao guia, mas as versões mais recentes
do BDTM carregam listas de tags e6 e booru ao mesmo tempo, mas o autocomplete
favorecerá tags booru e mudará tags e6 que você digitar para equivalentes booru
automaticamente. Você pode mudar isso indo na pasta "Tags" e removendo o danbooru.csv,
depois reiniciando o programa, que carregará apenas as tags e6.

---

> [**mog**](https://civitai.com/user/mog)
criar tutorial MUITO simples de como criar lora quando não tem bom pc e
não sabe programar

---

> [**yofoton174609**](https://civitai.com/user/yofoton174609)
tutorial legal!! Bem feito! duas perguntas:\
1: Qual GPU você usou, quanto tempo leva para treinar? De que tipo de
planejamento de capacidade estamos falando aqui? Horas? Dias? semanas?\
2: Existe alguma GUI que possa fazer a marcação e o treinamento juntos?
(para simplificar a configuração de tudo)

>> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
Fico feliz que tenha gostado! Minha GPU não é exatamente uma escolha
"padrão", mas tenho usado uma A5000 Quadro - é meio que uma mistura de um 3090 com a vram
de um 4090, para comparar com hardware de consumidor. Em média, um LoRA SD 1.5 pode levar
de 15 a 40 minutos, dependendo do tamanho do conjunto de dados e de quanto
regularização eu uso junto com ele. SDXL geralmente leva ~1 hora, apenas pelas resoluções
maiores. Um 4090 veria tempos de treinamento semelhantes, e GPUs mais antigas
provavelmente veriam tempos ligeiramente aumentados, principalmente pela falta de vram,
mas nunca devem ser mais de 2 horas em um conjunto de dados maior. Quanto à GUI, a GUI kohya
que uso tem uma configuração de marcação na aba "utilities", mas pessoalmente ainda prefiro
o BDTM. Não usei realmente outras GUIs de treinamento em profundidade, então não sei se
alguma delas tem um marcador embutido, embora eu tenha certeza de que uma ou duas podem ter.

---

> [**Jellai**](https://civitai.com/user/Jellai)
Parece que algumas pessoas estão usando Dora agora. Você já ouviu
falar de seus pontos fortes/fracos no mundo real?

>> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
Estou esperando minha GUI suportar isso, então, embora eu ainda não tenha
experimentado, ouvi dizer que é bastante sólido. Treina muito mais como um ajuste fino
completo do que um LoRA normal, o que tem muitos pontos positivos teóricos (como
treinamento mais rápido), mas não vou dar nenhuma opinião pessoal ainda sem meus próprios
testes. Vou ter certeza de adicionar um pouco falando sobre isso no guia quando eu tiver
tempo.

---

> [**sheldonng**](https://civitai.com/user/sheldonng)
Recentemente, tentei usar IA3 para treinar LyCORIS (personagens
animados com 6-9 roupas)\
Comparado com dim8/1 LoHA, seu tamanho de arquivo é surpreendentemente menor,
nem mesmo 1/10 é necessário, e é muito rápido.

---

> [**thebrownsauce184**](https://civitai.com/user/thebrownsauce184)
Você pode fazer um com imagens da interface do civitai? Eu aprendo visualmente. Tipo, sou meio burro.

>> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
Hmm, eu posso ver sobre adicionar visuais, sim. Vou dar uma olhada nisso em breve(tm)

>>> [**thebrownsauce184**](https://civitai.com/user/thebrownsauce184)
Show!

---

> [**pihlawrkr738**](https://civitai.com/user/pihlawrkr738)
Guia fantástico, muito útil, obrigado. Algumas coisas eu descobri por conta própria através de falhas, então queria ter olhado isso antes haha.

---

> [**Loraman**](https://civitai.com/user/Loraman)
Ótimo guia, obrigado! Tentei fazer um GLoRA para um estilo de arte com as configurações recomendadas, 2500 passos e 200 imagens (por exemplo, prodigy, 32/32), mas o treinamento não parece captar o estilo de arte tão bem quanto o treinador de LORA no site. Não tenho certeza de onde errei, você tem alguma dica?

>> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
Fico feliz que tenha ajudado! Mas não tenho ideia de quais configurações o treinador no site usa, então não tenho certeza de onde eles diferem. Eu não sou o mais experiente em treinamento de estilo também, daí minha pequena seção sobre isso, haha. Algumas coisas que me vêm à mente para você verificar/tentar são:
>> 1. Dê uma olhada rápida nos seus dados e certifique-se de que nenhuma imagem desvie muito da norma estilística, mesmo que sejam tecnicamente do mesmo estilo. Isso pode (às vezes) atrapalhar o treinamento.
>> 2. Tente um tipo diferente de LyCORIS, como o LoCON - Eles são mais de uso geral, e é possível que minhas configurações não funcionem tão bem com GLoRAs, não tive a oportunidade de testá-los de forma significativa.
>> 3. Compare minhas configurações com as configurações padrão do site: Tenho quase certeza de que você pode abrir uma aba "avançada" em algum lugar no treinador do site para ver melhor. Qualquer desvio significativo provavelmente estará nas configurações de LR, Dim/Alpha e otimizador. Potencialmente, você poderia trocar os valores deles pelos meus, o que pode funcionar melhor para estilos neste caso - Ficarei feliz em saber se funcionar!
>> 4. Deixe treinar por mais tempo: O tempo necessário para treinar sempre varia com o seu conjunto de dados, então é possível que com minhas configurações ele só precise de mais tempo para absorver as informações. Se você salvar épocas intermediárias, geralmente é melhor, na minha opinião, prolongar o treinamento e depois retroceder época por época para encontrar a melhor variação.

>>> [**Loraman**](https://civitai.com/user/Loraman)
Obrigado pelas informações! Vou tentar novamente com as configurações de LORA do Civitai. Vou postar outra resposta aqui se conseguir resolver.

---

> [**QwiziRAM**](https://civitai.com/user/QwiziRAM)
Estou correto ao aplicar todas as mesmas configurações para PonyXL como fazemos para SDXL?
E que o PDXL usa E6 e não Booru Tag também é verdade? (Então usamos "side view" em vez de "from side" por exemplo).

>> [**thebrownsauce184**](https://civitai.com/user/thebrownsauce184)
Pony precisa que Clip skip esteja definido para 2, defina o VAE para sdxl_vae (baixe se você não tiver), eu acho que o Euler A é o melhor sampler, use um número menor de passos com o pony (mesmo um passo a mais ou a menos pode fazer uma grande diferença), CFG definido para 7 ou abaixo funciona melhor para mim. No entanto, você deve realmente ler as configurações recomendadas para cada modelo, pois cada um é treinado para funcionar de maneira diferente. Booru tags como "1girl" etc ainda são necessárias, mas sim "side view" e algumas prompts mais ao estilo SDXL podem e devem ser usadas, especialmente em relação ao fotorrealismo.
Espero que ajude.

---

> [**justafish**](https://civitai.com/user/justafish)
como você etiquetaria imagens de duo ao treinar um único personagem? devo manter todas as tags para os outros personagens?
e há muitas tags de duo como human on anthro, human on female, etc, quais devo manter?

>> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
Geralmente, uso algumas tags para o personagem não treinado, apenas o suficiente para diferenciá-los, o que fica algo como "duo, human, male, X hair, human on anthro" ou similar. Como uma nota geral, "human on anthro" é uma das tags mais fortes para usar com esse tipo de pareamento, enquanto outras como "human on female" devem ser redundantes a partir das suas outras tags. Além disso, se o sujeito não treinado do duo for um personagem específico, eu não o etiqueto especificamente, pois esse não é o objetivo do treinamento e essas tags podem ser inconsistentes entre checkpoints.

---

> [**QwiziRAM**](https://civitai.com/user/QwiziRAM)
Havia uma pergunta sobre o item:
"Taxa de aprendizado do codificador de texto e do Unet":
SD1.5 AdamW: Eu defini esses valores para 0.00005 e 0.0001, respectivamente.
SDXL: 0.0001 & 0.0003
Eu entendo corretamente que com o otimizador Prodigy você só precisa definir 1 e 1? Porque quando tento definir 0.0001 & 0.0003, recebo o seguinte erro.
Definir valores diferentes de lr em diferentes grupos de parâmetros é suportado apenas para valores de 0
(Planejo treinar no PonyXL).

>> [**Valstrix - OP**](https://civitai.com/user/Valstrix)
Sim: Eu digo especificamente na seção "taxa de aprendizado" acima que o valor deve ser 1 para Prodigy e outros otimizadores adaptativos, as taxas de aprendizado especificadas são codificadas por cores para configurações alternativas.

---
