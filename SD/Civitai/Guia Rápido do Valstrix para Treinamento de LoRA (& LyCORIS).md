# [**Guia Rápido do Valstrix para Treinamento de LoRA (& LyCORIS)**](https://civitai.com/articles/3522/valstrixs-crash-course-guide-to-lora-and-lycoris-training)

![Guia Rápido do Valstrix para Treinamento de LoRA (& LyCORIS)](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/dea27b4a-6037-4a06-aa01-2db153f5e444/width=300/dea27b4a-6037-4a06-aa01-2db153f5e444.jpeg)

-Tags: `#training guide`, `#lycoris/locon`, `#lora training`, `#sdxl`, `#sd1.5`, `#lora guide`
-Autor: [**Valstrix**](https://civitai.com/user/Valstrix)

---
<p></p>

Originalmente um guia no Discord que escrevi, estas são minhas dicas para a galera do Civit. Com tantos guias desatualizados e/ou com informações incorretas, espero que este seja útil para treinadores aspirantes e atuais.

Lembre-se de que isso é baseado no meu próprio fluxo de trabalho e experiências _pessoais_! Provavelmente não será perfeito, e não planejo fazer disso uma enciclopédia absoluta. Apenas um bom e velho guia rápido para você começar - embora tenha evoluído para algo parecido com um manual agora.

Vou assumir que você tem um sistema capaz de treinar localmente. (Embora isso _teoricamente_ deva ser aplicável em outros ambientes de treinamento.)

Dito isso, este guia assumirá que você ainda não reuniu um conjunto de dados, então vamos mergulhar nisso!

**Aviso:** Jogue fora tudo o que você aprendeu naquele vídeo do YouTube do Senhor Genérico AI postado há 3 meses - o YouTube está _sempre_ atrasado/desatualizado, não se confunda.

## Parte 1 | Conjuntos de Dados: Coleta e Noções Básicas

Seu conjunto de dados é **O ASPECTO MAIS IMPORTANTE** do seu LoRA, sem dúvidas. **<u>Um conjunto de dados ruim produzirá um LoRA ruim todas as vezes, independentemente das suas configurações.</u>** Dados ruins produzem resultados ruins!

Idealmente, treinar um bom LoRA usará um número decente de imagens: Para SD1.5, recomendo cerca de 30 imagens, com um mínimo de 15: Mas já usei apenas 8 e obtive um resultado 'decente'.

Para SDXL, tive mais sorte com cerca de 20 imagens, com um mínimo de 12. Algumas pessoas encontraram maneiras razoáveis de fazer treinamentos com uma única imagem no XL, mas ainda não tentei isso.

Dito isso, você pode usar mais ou menos imagens do que meus números recomendados, mas, de modo geral, esses são bons valores para se almejar - mas não sature demais seu conjunto de dados! Ter **muitas** imagens vai desacelerar desnecessariamente seu treinamento para retornos rapidamente decrescentes, e dependendo do conteúdo pode te dar mais problemas do que vale. Se você estiver tendo dificuldade em conseguir um conjunto de dados grande o suficiente, não se preocupe muito, pois há um método para expandir artificialmente seu conjunto de dados, mas falaremos sobre isso mais tarde.

Ao reunir suas imagens, garanta que você priorize a qualidade sobre a quantidade. Um conjunto bem curado de 30 imagens pode facilmente superar um conjunto de 100 imagens pobres e medianas. Especialmente em conjuntos de dados menores, uma única imagem "ruim" pode desestabilizar todo o modelo de uma maneira horrível. Dito isso, imagens ruins _PODEM_ ser usadas para completar um conjunto de dados, mas devem ser devidamente marcadas (como com "esboço colorido", que será discutido mais tarde).

Você também deve garantir que suas imagens sejam variadas. Muitas imagens de um estilo similar/igual tentarão se incorporar ao seu conceito, dificultando a mudança de estilo e tornando qualquer alteração de estilo tendenciosa. Especialmente quando se lida com muitas capturas de tela e renderizações, você deve ter cuidado. Se você tiver uma quantidade significativa, marcá-las com o artista que as fez, como uma renderização, etc., pode ajudar a associar o estilo a outra etiqueta e reduzir o impacto. Por outro lado, estilos incrivelmente distintos/poderosos podem influenciar o estilo geral aprendido, mesmo em pequenas quantidades, então recomendo marcá-los também.

Eu também recomendaria evitar imagens com temas fetichistas ao trabalhar com personagens (a menos que você queira isso em seu LoRA), pois mesmo quando marcadas, sua anatomia frequentemente extrema pode distorcer seu modelo de uma maneira pior do que se elas não estivessem presentes. Você _pode_, claro, usá-las para expandir seu conjunto de dados se realmente precisar, mas certifique-se de que estão **bem** marcadas.

Pessoalmente, eu obtenho meus dados de uma variedade de sites: e621, furaffinity, deviantart, pixiv e o wiki do monster hunter (e wikis de jogos em geral) são minhas fontes comuns. Novamente, certifique-se de evitar puxar muito do mesmo artista e estilos similares. A pesquisa de imagens do Google também vale a pena ser observada se você precisar de mais dados, pois muitas vezes pode encontrar instâncias isoladas do reddit, feeds da comunidade steam e outros sites que você pode não ter pensado em olhar.

Pixiv é uma bênção para monster hunter e outras franquias orientais como um site de arte japonês, mas encontrar especificidades pode ser difícil às vezes, pois você precisa usar texto em japonês para garantir sua busca. Felizmente, várias wikis também incluem nomes japoneses, se aplicável. _Nota: A partir de 25/04/24, o Pixiv bloqueou 'conteúdo sensível' nos EUA e no Reino Unido - Isso pode ser contornado configurando a região da sua conta para qualquer lugar fora desses locais, caso você se importe em usar esse conteúdo para treinamento._

À medida que você reúne suas imagens, deve considerar o que deseja treinar: Um personagem? Um estilo? Um objeto? Algumas roupas?

Para a maioria desses, você deve procurar principalmente por imagens solo do assunto em questão. Duos/trios também funcionam, mas você deve pegá-los apenas se seu assunto principal estiver em grande parte desobstruído. Alternativamente, indivíduos extras podem ser facilmente removidos ou recortados. Se você incluir imagens de vários personagens, certifique-se de que estejam devidamente e completamente marcadas. Incluir duo/trios/etc _pode_ ser benéfico para usar seu LoRA em gerações de vários personagens, mas não é necessário de forma alguma.

Se você está planejando treinar um estilo, saiba que eles são mais avançados e são mais adequados para LyCORIS do que LoRA. Este guia ainda será amplamente aplicável, mas confira as partes posteriores para detalhes específicos deles.

Depois de ter suas imagens, coloque-as em uma pasta para preparação na próxima parte. (Por exemplo, "Pastas de Treinamento/Pasta de Conceito/Bruto")

## Parte 2 | Conjuntos de Dados: Preparação

Depois de ter suas imagens brutas da parte 1, você pode começar a pré-processá-las para deixá-las prontas para o treinamento. Você precisará de um programa de edição de fotos: Recomendo o Photopea como uma alternativa gratuita ao Photoshop. [Paint.net](http://paint.net/) e Krita também são opções válidas.

Pessoalmente, eu separo minhas imagens em dois grupos: Imagens que estão ok por conta própria e imagens que requerem algum tipo de edição antes de serem usadas. As que atendem aos critérios abaixo são movidas para outra pasta e editadas conforme necessário.

Dê uma olhada na extensão das suas imagens. Imagens **.webp** (geralmente retiradas de wikis) são incompatíveis com os treinadores atuais e **devem ser convertidas para PNG ou JPEG**. Enquanto você faz isso, note que **imagens com fundos transparentes** também causam problemas. Elas devem ser levadas ao seu editor de imagem de escolha e devem receber um fundo. Eu recomendaria usar várias cores sólidas, se você tiver mais de uma - a variação de fundo pode ser incrivelmente útil. Alternar entre branco/preto/azul/verde/vermelho/etc. e marcá-los como tal pode ajudar seu treinamento se os fundos estiverem causando problemas, e em geral.

Em seguida, considere a resolução do seu treinamento. Resoluções mais altas permitem obter mais detalhes de uma imagem, mas irão desacelerar seu tempo de treinamento. Usando o SD1.5, a maioria das pessoas treina em **512 ou 768**, mas resoluções intermediárias também são aplicáveis, como treinar em 704 se você não conseguir treinar em 768.

Se você estiver usando o SDXL, sua resolução _mínima_ deve ser pelo menos 1024, o que quase todos treinam.

**Qualquer imagem maior que sua resolução será redimensionada automaticamente.** A resolução será detalhada mais adiante.

Depois de ter uma ideia da sua resolução, dê uma olhada no seu conjunto de dados. Lembre-se de que imagens não quadradas serão redimensionadas para manter suas proporções (com base na resolução do bucket), então ter **muito fundo vazio** pode ser prejudicial para obter detalhes. Imagens largas e altas com muito fundo vazio podem ser **recortadas para focar no assunto**.

Além disso, se você tiver **imagens de alta resolução com várias representações** do seu assunto (como uma folha de referência), você pode **recortar a imagem em várias partes** para treiná-la em várias imagens em vez de uma única imagem excessivamente comprimida. Tais imagens também podem ser treinadas sem cortar e recortar, apenas certifique-se de marcá-las com "várias vistas" e "folha de referência" mais tarde.

Imagens com pessoas além do assunto devem ser focadas no assunto se você planeja usá-las como estão, ou se planeja transformá-las em imagens solo, edite os outros assuntos se possível, seja por recorte ou remoção.

Embora não seja necessário, elementos sobrepostos, como texto, balões de fala e linhas de movimento, podem ser removidos. Você deve lembrar que a IA aprende com a repetição, e o mesmo elemento no mesmo local em várias imagens será algo que ela tentará manter. Está tudo bem se um punhado tiver isso

, mas idealmente você quer o mínimo de repetição entre elas, e aqueles que não podem ser removidos, mas se repetem com frequência, devem ser marcados. Como a repetição é a chave, os outliers são (geralmente) menos propensos a se manter.

Se você tiver imagens menores que sua resolução de treinamento, considere aumentá-las. Ferramentas de aumento como 4x\_Ultrasharp são ótimas para isso. Pessoalmente, tenho usado 4x\_foolhardy\_Remacri.

Se você tiver imagens **maiores que 3000 pixels, redimensione para 3000 ou menos.** Aparentemente, o treinador Kohya tem alguns problemas menores ao lidar com imagens muito grandes - não tenho certeza do porquê, mas redimensionar imagens muito grandes no meu conjunto de dados mostrou alguma melhoria.

Por fim, se você tiver um assunto com detalhes assimétricos (como uma marca, logo, braço robótico único, etc.), certifique-se de que esteja voltado para o mesmo lado em cada imagem. Imagens orientadas incorretamente devem ser invertidas para consistência. Nesse caso, certifique-se de _não_ ativar "flip augmentation", detalhado mais adiante.

Depois de fazer o seguinte, coloque _todas_ as suas imagens editadas e as imagens que você não editou em uma nova pasta assim: "Pastas de Treinamento/Pasta de Conceito/X\_NomeDoConceito". O 'NomeDoConceito' será seu token de instância (o que você vai colocar no prompt), e o 'X' será o número de repetições na pasta por época, que será detalhado mais tarde. Deve ficar algo como "1\_Hamburger".

## Parte 2.5 | Conjuntos de Dados: Curando o Veneno

Como o Nightshade especialmente está ganhando muita tração agora, acho que devo colocar uma seção aqui cobrindo imagens "envenenadas". Você não vai encontrá-las com muita frequência, mas é bem possível à medida que ganham popularidade.

O propósito de ferramentas de envenenamento de imagens como Glaze e Nightshade é adicionar "ruído adversarial" a uma imagem, o que interrompe o processo de aprendizado ao adicionar essencialmente outliers insanos e obscurecer os dados originais nos quais ela treinaria. Como tal, incluir uma imagem envenenada em seu conjunto de dados pode resultar em anomalias estranhas, sejam variações de cor, anatomia distorcida, etc. Quanto mais veneno você tiver, piores serão os efeitos. Idealmente, você não terá nenhum - mas AINDA PODE usá-los.

Esses "venenos" têm uma fraqueza hilária - o próprio ruído que estão introduzindo. Simplesmente pegando a imagem desejada e colocando-a em um upscaler de IA bom em remover ruído (como com artefatos jpeg ou algo do tipo), ou mesmo apenas um upscaler geral, você pode simplesmente... remover o veneno. É tão fácil, geralmente. As pessoas ainda estão experimentando os "melhores" métodos de remoção, mas, francamente, especialmente com o Nightshade, praticamente qualquer método pode limpar a imagem para um estado utilizável.

Upscalers de "Smoothing" ou "anti-artifact" funcionam melhor para o trabalho, usados com um dos dois métodos a seguir:  
A: Apenas aumente a escala. 2x geralmente é suficiente.  
B: Reduza a escala para metade ou 3/4 do tamanho, depois aumente com IA. Funciona melhor com imagens já grandes, imagens de resolução pequena perderiam muitos detalhes.

Alternativamente, "adverse cleaner" pode fazer um trabalho decente, e existe como uma [extensão para A1111](https://github.com/gogodr/AdverseCleanerExtension) ou como um [HF Space](https://huggingface.co/spaces/p1atdev/AdverseCleaner). Combinado com os métodos de upscaling acima, você pode neutralizar efetivamente o "veneno" por completo.

"Mas como eu reconheço uma imagem envenenada?"

Depende de quão agressivamente o trabalho foi envenenado - se parece uma porcaria porque parece que uma criança de 3 anos colocou um filtro de câmera engraçado nela, ou tem alguns artefatos bastante óbvios, ou parece que toda a imagem está coberta de um artefato de compressão Jpeg - É 9/10 vezes envenenada. Venenos menos agressivos são mais difíceis de detectar, mas têm menos impacto no seu treinamento. Se você não tiver certeza, dê uma olhada de perto em um editor de fotos, e/ou apenas passe pelos métodos de limpeza antes para estar seguro.

Como uma nota geral, os indivíduos que colocam venenos hiper-agressivos em seu trabalho geralmente são tão delirantes que sua arte nem vale a pena usar em primeiro lugar - Artistas que se respeitam geralmente mantêm o veneno mínimo para não afetar seu trabalho visualmente de maneira significativa, ou simplesmente não usam. Se você não quiser lidar com veneno, puxe seus dados de imagens mais antigas se quiser estar totalmente seguro, ou apenas aprenda a identificar venenos e pule por eles.

## Parte 3 | Conjuntos de Dados: Marcação

Quase terminamos com o conjunto de dados! Estamos na etapa final agora, a marcação. Isso definirá seu token de instância e determinará como seu LoRA será usado.

Existem várias maneiras de fazer sua marcação e uma infinidade de programas para ajudar na marcação ou fazer a marcação automática. No entanto, na minha _opinião_ pessoal, você não deve usar marcação automática (_especialmente_ em designs e assuntos específicos) pois isso dá mais trabalho do que ajuda. (No entanto, os marcadores automáticos estão melhorando rapidamente.)

Pessoalmente, uso o [Booru Dataset Tag Manager](https://github.com/starik222/BooruDatasetTagManager) e marco todas as minhas imagens manualmente. Você PODERIA marcar sem um programa, mas simplesmente... **não faça isso.** Criar, nomear e preencher manualmente um .txt para cada imagem **não** é algo que você quer fazer com seu tempo.

Felizmente, o BDTM tem uma opção legal para adicionar uma marca a todas as imagens do seu conjunto de dados de uma vez, o que torna o início do processo **muito** **mais fácil**.

Antes de marcar, você precisa escolher um modelo para treinar! Para fins de compatibilidade, sugiro que você treine em um **Modelo Base**, que é qualquer ajuste fino que **NÃO seja uma mistura de outros modelos**. Treinar em uma mistura ainda é viável, mas na minha experiência faz com que as saídas sejam menos compatíveis com qualquer coisa que não seja aquele modelo. Se você só quer usar seu LoRA naquele mix específico, está perfeitamente bem treinar nele, no entanto.

Agora, para a própria marcação. Antes de fazer _qualquer coisa_, descubra que tipo de tags você usará:

-   Atualmente, existem três tipos de estilos de prompting, como segue: Natural Language Prompting, (dan)Booru Tag Prompting e e6 Tag Prompting, que você deve usar com base na "ancestralidade" dos seus modelos.
    
-   O SD1.5 Base, SDXL Base e (atualmente) a maioria dos modelos SDXL usam o Prompting Natural, ex: "A brown dog sleeping at night, they are very fluffy." Às vezes funciona em outros modelos, mas _não é recomendado_.
    
-   A grande maioria dos modelos de anime que você vê usam o Booru Prompting, especificamente usando a lista de tags do Danbooru, um image board de anime. Ouvi dizer que Anything v4.5 é uma boa escolha para 1.5.
    
-   Modelos com ancestralidade baseada em modelos furry usam o e6 prompting, usando a lista de tags do e621. Fluffyrock ou BB95 é uma boa escolha aqui para 1.5. PonyXL é a melhor para suas escolhas de XL.
    

Depois de saber qual modelo e tags você está usando, pode começar a marcar.

**Sua PRIMEIRA tag em CADA imagem deve ser seu token de instância**, também conhecido como o que você nomeou X\_NomeDoConceito, neste caso "nomedoconceito". Se seu modelo já tiver seu assunto remotamente treinado para essa tag, considere mudar seu token de instância para uma string que ele não teria. Por exemplo, "hamburger" poderia ser "hmbrgrlora". Isso nem sempre é necessário, mas se você vir resultados estranhos que derivam da interpretação original do modelo, talvez queira fazer isso.

**Sua SEGUNDA tag em CADA imagem deve ser seu token de classe**, um "contêiner" geral para seu token de instância. Isso diz à IA o que é seu assunto, geralmente, para ajudar no treinamento. Por exemplo, uma espada é uma "arma", Lola Bunny é um "antropomorfo" e assim por diante. Nem toda imagem precisa ter o mesmo token de classe, no entanto! Frequentemente executo conjuntos de dados mistos, e ter várias classes para um token de instância é perfeitamente ok - desde que seu modelo possa fazer sentido disso.

Meu processo funciona assim:

-   Adicione a todas as imagens de uma vez: token de instância (geralmente espécie), token de classe (antropomorfo/feral/humano), gênero (se aplicável, ferais não especificados), elementos controláveis (ou seja, uma roupa específica do personagem), nu, outros controláveis comuns (como a cor dos olhos mais comum).
    
-   Mova para a primeira imagem; Remova se necessário: elementos controláveis. Mude se necessário: nu (para marca(s) geral(is) de roupa), cor dos olhos, etc.
    
-   Adicione tags que você consideraria "elementos chave" para a imagem: Meios específicos (como aquarela), composicionais (ou seja, retrato de três quartos), etc.
    
-   Adicione tags para descrever aspectos desviados: seios enormes/hiper, chifres/escamas/pele de cor variada, etc.
    
-   Repita para cada imagem.
    

Dito isso, não exagere nas suas tags. Se você usar muitas, vai "sobrecarregar" o treinador e obter resultados menos precisos, pois ele está tentando treinar muitas tags. É geralmente uma boa prática marcar apenas os itens que você consideraria um "elemento chave" da imagem. **Marcar pouco é melhor do que marcar demais, então se estiver em dúvida, mantenha mínimo**. Eu geralmente tenho cerca de 5 a 20 tags por imagem, dependendo de sua complexidade.

Fundos e poses podem frequentemente ser ignorados, mas se você tiver tipos específicos de locais/poses/fundos em um número significativo de suas imagens, _deve_ marcá-los para evitar biasing.

Por exemplo, se você tiver muitos fundos brancos, deve marcar "fundo branco". Se após um treinamento você vir uma pose específica sendo padrão, deve encontrar todas as instâncias no seu conjunto de dados usando essa pose e marcá-las.

Você também deve ter cuidado com tags "implícitas". Estas são tags que implicam outras tags apenas por sua presença. Ao ter uma tag implícita, você não deve usar a(s) tag(s) que ela implica junto com ela. Por exemplo, "pernas abertas" implica "pernas", "pastor alemão" implica "cão", e assim por diante. Ter as tags que são implicadas por outra espalha o treinamento entre elas, enfraquecendo o efeito do seu treinamento. Em grandes quantidades, isso pode ser bastante prejudicial aos seus resultados finais.

**Marcando imagens de baixa qualidade:** Às vezes, você simplesmente não tem escolha a não ser usar dados ruins. Esboços rudes, capturas de tela de baixa resolução, anatomia ruim e outros caem nessa categoria.

-   Esboços podem geralmente ser marcados como "esboço colorido" ou "esboço", o que geralmente é tudo o que você precisa fazer. Se não coloridos, "monocromático" e "escala de cinza" geralmente são bons para adicionar também.
    
-   Imagens de baixa resolução devem ser aumentadas com um upscaler apropriado, se possível, como um dos muitos feitos para aumentar capturas de tela de desenhos animados antigos, por exemplo. Se você não conseguir um bom aumento, use a tag apropriada para seu modelo para denotar a qualidade da resolução, mas às vezes é melhor deixá-las de fora.
    
-   Anatomia ruim deve ser marcada conforme você vê, ou recortada do quadro, se possível. Imagens com desvios significativos que não podem ser recortadas ou editadas, como o pescoço/cabeça/ombros fora do centro, geralmente são melhores deixadas de fora do conjunto de dados inteiramente.
    

Depois de marcar todas as suas imagens, certifique-se de salvar tudo e você estará pronto para a próxima etapa.

## Parte 3.5 | Conjuntos de Dados: Preservação de Prioridade (Regularização)

Embora completamente opcional, outro método de combater o viés de estilo e a atribuição inadequada de tags é através do uso de um conjunto de dados de preservação de prioridade. Este atuará como um conjunto de dados separado, mas generalizado, para uso junto com seu conjunto de dados de treinamento, e geralmente pode ser usado de forma geral entre várias sessões de treinamento. Eu recomendaria criar uma nova pasta para eles assim: "Pastas de Treinamento/Regularização/RegConceitoA/1\_RegNomeDoConceito".

"Mas como exatamente eu faço e uso esses?"

Você pode começar nomeando sua pasta com um token - seu token de classe é frequentemente uma boa escolha.

Criar um conjunto de dados para isso é _incrivelmente_ fácil - nenhuma marcação é necessária. Dentro da pasta que você criou para a tag, você simplesmente precisa colocar um número de imagens aleatórias, únicas e variadas que caem dentro do domínio dessa tag. **Não inclua imagens de nada do que você estará treinando.** Com base em meus próprios testes, pessoalmente recomendo um número aproximadamente igual ao número de imagens em seu conjunto de dados principal para treinamento, mas mantenha uma pasta maior com cerca de 50-100 imagens para tirar se você treinar com mais dados no futuro, em vez de expandir seu conjunto de regularização toda vez que tiver mais dados do que antes.

Dito isso, seu conjunto de regularização não precisa ser explicitamente variado. Embora a variância seja boa para uso geral, digamos que você tenha muitas capturas de tela no seu conjunto de dados principal, ou muitas imagens do(s) mesmo(s) artista(s) ou artista(s) semelhante(s). Em qualquer caso, o viés estilístico pode ser difícil de remover. Embora marcar os estilos das imagens possa ajudar, nem sempre é _suficiente_ para separar completamente esse estilo. Nesse caso, você pode criar um conjunto de regularização do estilo especificamente: Basta colocar um monte de obras do artista em uma pasta, tirar um monte de capturas de tela, etc., e depois nomear a pasta de regularização com a tag apropriada.

Durante o treinamento, o treinador alternará entre treinar em seu conjunto de dados principal e o conjunto de dados de regularização - isso exigirá que você tenha um treinamento mais longo para alcançar a mesma quantidade de aprendizado, mas reduzirá muito o viés.

Você também pode usar vários conjuntos de dados de regularização diferentes no mesmo treinamento, basta colocar ambas as pastas no diretório de regularização que você definiu durante o treinamento. Lembre-se de que as repetições das pastas importarão aqui - você sempre pode executar o treinamento novamente com mais repetições da regularização se não for poderoso o suficiente, mas tenha cuidado ao aumentar muito sua contagem de passos.

Outra coisa a se notar é que você pode influenciar diretamente a força do seu aprendizado de regularização sem apenas adicionar repetições ou mais imagens ao conjunto de dados. Na aba "avançado" das configurações do GUI, a configuração "Peso da perda de prioridade" controla isso. Por padrão, está em 1: Quanto mais próximo esse número estiver de 0, mais forte será o efeito - apenas lembre-se de que você precisará de mais passos de aprendizado para compensar. Se configurado muito alto, você pode arruinar completamente sua execução de treinamento ao aprender muito pouco para ter qualquer efeito.

## Parte 3.6 | Marcação: Exemplos

Como exemplos geralmente são bastante úteis, colocarei um punhado de exemplos dos meus próprios conjuntos de dados aqui para sua própria referência. **Lembre-se: Eu geralmente treino no fluffyrock e no Pony Diffusion, modelos que usam marcação e6. Outros modelos devem trocar tags para suas próprias variantes, quando necessário. (ex: side view (e6) > from side (booru))**

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/fefc051a-fd52-4644-ba70-f8cd3308dfc6/width=525/fefc051a-fd52-4644-ba70-f8cd3308dfc6.jpeg)

mizutsune, feral, blue eyes, no sclera, bubbles, soap, side view, action pose, open mouth, realistic, twisted torso, looking back, hand on ground, white background

-   White backgrounds eram mais prevalentes neste conjunto de dados, então o fundo foi marcado.
    

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/4ce8f39e-375d-4e63-8a6a-a22954d3c244/width=525/4ce8f39e-375d-4e63-8a6a-a22954d3c244.jpeg)

arzarmorm, human, male, black hair, brown eyes, dark skin, three-quarter view, full-length portrait, asymmetrical armwear, skirt, pouches, armband, pants

-   Nesse caso, o modelo não estava cooperando apenas com o token de instância, então as tags "asymmetrical armwear, skirt, pouches, armband, pants" foram usadas como reforço, o que também as desvinculou do conceito principal, permitindo que fossem controladas individualmente.
    
-   Este LoRA também tinha muito poucas instâncias de fundos brancos, então deixar sem marcar não foi um problema.
    

## Parte 4 | Treinamento: Noções Básicas

Agora que você tem seu conjunto de dados, você precisa realmente treiná-lo, o que requer um script de treinamento. O script mais comumente usado, que também uso, são os Scripts Kohya. Eu pessoalmente uso o [Kohya-SS GUI](https://github.com/bmaltais/kohya_ss#windows), um fork do [SD-Scripts](https://github.com/kohya-ss/sd-scripts) treinador de linha de comando. Geralmente está um pouco atras

ado nas atualizações, mas é perfeitamente utilizável. Ambos são opções válidas, e outras opções existem, mas para fins de compatibilidade, vou me manter no Kohya GUI como referência. A maioria das configurações deve funcionar decentemente em outros treinadores também.

Depois de instalá-lo e abri-lo (a instalação é na verdade bastante fácil.), certifique-se de navegar até a guia LoRA no topo (ela padrão para dreambooth, um método mais antigo.)

Há muitas coisas que podem ser ajustadas e alteradas no Kohya, então vamos com calma. Suponha que qualquer coisa que eu não mencione aqui possa ser deixada em branco.

Texto amarelo como este denota configurações alternativas, semi-experimentais que estou testando. Sinta-se à vontade para dar feedback se usá-las, mas se você está procurando algo estável, ignore essas. Essas configurações mudarão frequentemente à medida que eu testá-las e treiná-las. Quando eu estiver satisfeito com uma configuração estável incorporando-as, elas serão adotadas nas configurações principais.

Texto verde denotará configurações para minha configuração SDXL. Se você está usando SD1.5, ignore essas.

Vamos descer verticalmente, aba por aba.

```
Accelerate Launch
```

Esta aba é onde estão suas configurações de multi-gpu, se você as tiver. Caso contrário, pule esta aba inteiramente, pois os padrões estão perfeitamente bem.

```
Model
```

Esta aba, como você provavelmente adivinhou, é onde você define seu modelo para treinamento, seleciona seu conjunto de dados, etc.

-   Nome ou caminho do modelo pré-treinado:
    
    -   Insira o caminho completo do arquivo para o modelo que você usará para treinar.
        
-   Nome do modelo treinado de saída:
    
    -   Será o nome do seu arquivo de saída. Nomeie como quiser.
        
-   Pasta de imagens (contendo subpastas de imagens de treinamento):
    
    -   Deve ser o caminho completo da pasta de treinamento, mas não a que tem o X\_. Você deve definir o caminho para a pasta onde essa pasta está dentro. Ex: "C:/Pastas de Treinamento/Pasta de Conceito/".
        
-   Abaixo disso, há 3 caixas de seleção:
    
    -   v2: Marque se você estiver usando um modelo SD 2.X.
        
    -   v\_parameterization: Marque se seu modelo suporta V-Prediction (VPred).
        
    -   Modelo SDXL: Marque se você estiver usando algum tipo de SDXL, obviamente.
        
-   Salvar modelo treinado como:
    
    -   Pode permanecer como "safetensors". "ckpt" é um formato mais antigo, menos seguro. A menos que você esteja usando propositalmente uma versão antiga de algo pré-safetensor, ckpt nunca deve ser usado.
        
-   Precisão de salvamento:
    
    -   "fp16" tem dados de maior precisão, mas internamente tem valores máximos menores. "bf16" mantém dados menos precisos, mas pode usar valores maiores, e parece ser mais rápido para treinar em placas não de consumo (se você tiver uma). Escolha com base nas suas necessidades, mas eu fico com fp16, pois a maior precisão é geralmente melhor para designs mais complexos. "float" salva seu LoRA em formato fp32, o que dá um tamanho de arquivo exagerado. Uso específico.
        

```
Metadata
```

Uma seção para informações meta. Isso é totalmente opcional, mas pode ajudar as pessoas a descobrir como usar o LoRA (ou quem o fez) se o encontrarem fora do site. Recomendo colocar seu nome de usuário no slot de autor, pelo menos.

```
Folders
```

Tão simples quanto possível: Defina suas pastas de saída/regularização aqui, e o diretório de log se desejar.

-   Pasta de saída:
    
    -   Onde seus modelos acabarão quando forem salvos durante/depois do treinamento. Defina isso onde quiser.
        
-   Diretório de regularização:
    
    -   Deve ser deixado vazio, a menos que você planeje usar um conjunto de dados de preservação de prioridade da seção 3.5, seguindo um caminho semelhante ao da pasta de imagens. Ex: "C:/Pastas de Treinamento/Regularização/RegConceitoA/".
        

```
Parameters
```

O essencial do treinamento. Quase tudo o que vamos definir está nesta seção: Não se preocupe com os presets, na maioria das vezes.

-   Tipo de Lora: Padrão (Lora Type: Standard)
    
    -   Alt: LyCORIS/LoCon
        
    -   LoCons, após uma quantidade decente de treinamentos e testes, parecem sobreajustar mais facilmente do que um LoRA padrão, o que pode causar alguns problemas. Voltei aos LoRAs padrão por enquanto, mas as próximas implementações DoRA merecem uma olhada eventualmente.
        
-   Preset LyCORIS: Completo (LyCORIS Preset: Full)
    
-   Tamanho do lote de treino (Train Batch Size):
    
    -   Quantas imagens serão treinadas simultaneamente. Isso pode acelerar seu treinamento, mas pode causar resultados menos precisos/mais generalizados, e nem sempre é benéfico. Eu geralmente mantenho isso em 1, mas nunca ultrapasse 4. Isso também aumentará substancialmente seu uso de VRAM.
        
    -   Há algumas discussões sobre valores exponenciais serem os melhores para usar (1/2/4/8/16/etc), mas na maioria dos casos você terá dificuldades para ajustar valores maiores na maioria das GPUs de consumo.
        
    -   Atualmente, tenho usado um lote de 2.
        
-   Época (Epoch):
    
    -   Uma única época em passos é o número de imagens que você tem, multiplicado pelo número "X\_".
        
    -   O que você define esse valor dependerá do seu conjunto de dados, mas como regra geral, começo com um número que tem cada imagem treinada 100 vezes.
        
        -   Ex: "1\_Hamburger" tem 30 imagens. A repetição da pasta (1\_) é um valor de 1: Para atingir 900 passos, executaríamos por 30 épocas.
            
        -   Ex: "10\_Hamburger" tem 30 imagens. A repetição da pasta (10\_) é um valor de 10: Para o treinador, ele vê isso como _300_ imagens. Para atingir 900 passos, executaríamos por 3 épocas.
            
        -   Ambos os métodos treinam a mesma quantidade: Como você faz isso é puramente uma preferência pessoal.
            
    -   Para SD 1.5, geralmente treino por 100 épocas (10 com método alternativo.)
        
    -   Para SDXL, geralmente treino por 50 épocas (5 com método alternativo.)
        
    -   Embora **não perfeito**, esses são bons valores iniciais para testar seu conjunto de dados.
        
-   Época máxima de treino (Max train epoch):
    
    -   Opcional. Força seu treinamento a parar em X épocas, útil em alguns cenários. Substituído pelo próximo valor.
        
-   Passos máximos de treino (Max train steps):
    
    -   Opcional. Força o treinamento a parar na contagem exata de passos fornecida, substituindo épocas. Útil se você quiser parar em 2000 passos exatos ou algo semelhante.
        
-   Salvar a cada n épocas (Save every n epochs):
    
    -   Opcional. Salva um LoRA antes de terminar a cada X número de épocas que você definir. Isso pode ser útil para voltar e ver onde pode estar o ponto ideal.
        
    -   Eu geralmente mantenho isso em 1, salvando a cada época. Se a época final estiver sobreajustada, volto para encontrar a melhor versão. Recomendo valores maiores para contagens altas de épocas.
        
    -   Sua época final **sempre** será salva, então definir isso para um número ímpar pode ser útil, como salvar a cada 3 épocas com um treinamento de 10 épocas dará a você épocas 3, 6, 9 e 10, dando uma alternativa bem no final se começar a sobreajustar.
        
-   Cache de latentes _e_ Cache de latentes para disco (Cache latents and Cache latents to disk):
    
    -   Esses afetam onde seus dados são carregados durante o treinamento. Se você tem uma placa gráfica recente, "cache de latentes" é a escolha melhor e mais rápida que mantém seus dados carregados na placa enquanto treina. Se você estiver com pouca VRAM, a versão "para disco" é mais lenta, mas não consome sua VRAM para isso.
        
    -   Cache para disco, no entanto, evita a necessidade de recarregar os dados se você executá-lo várias vezes, desde que não haja mudanças nele. Útil para ajustar configurações do treinador.
        
-   Agendador de LR (LR Scheduler):
    
    -   Ao usar Prodigy/DAdapt, use apenas Cosine. Ao usar um Adam opt, Cosine With Restarts geralmente é o melhor. Outros agendadores podem funcionar, mas afetam como a IA aprende de maneiras bastante drásticas, então não mexa nisso até entender melhor.
        
-   Otimizador (Optimizer):
    
    -   Existem várias opções para escolher, mas as quatro que valem a pena usar, na minha opinião, são **Prodigy, DAdaptAdam, AdamW e AdamW8bit**.
        
    -   Prodigy é o mais novo, mais fácil de usar e produz resultados excepcionais.
        
    -   Os otimizadores AdamW são bastante antigos, mas com ajustes finos podem produzir resultados melhores que Prodigy em um tempo

 mais rápido.
        
    -   Para os propósitos deste guia, usaremos Prodigy e AdamW.
        
    -   DAdaptAdam é muito semelhante a Prodigy, e essas configurações devem ser amplamente aplicáveis a ele também. Tem um método de aprendizado menos agressivo, então se você estiver tendo problemas com Prodigy, experimente isso.
        
-   Argumentos extras do otimizador (Optimizer extra arguments):
    
    -   Prodigy/DAdapt: Defina para "decouple=True weight\_decay=0.1 betas=0.9,0.99".
        
    -   AdamW: weight\_decay=0.12 betas=0.9,0.99
        
-   "Taxa de Aprendizado" ("Learning Rate"):
    
    -   Ao usar Prodigy/DAdapt, defina isso para 1. Prodigy e DAdapt são adaptativos e definem isso automaticamente enquanto treinam.
        
        -   Como uma nota geral, as caixas específicas de "text encoder" e "unet" mais abaixo substituirão a caixa principal, se valores forem definidos nelas.
            
-   Aquecimento de LR [(]% dos passos totais] (LR warmup []% of total steps]):
    
    -   Opcional. 10% é um bom valor na maioria dos cenários.
        
    -   SDXL: 20%
        
-   "Número de ciclos de LR" (LR # cycles):
    
    -   Se estiver usando um Adam opt, defina isso para 3. Apenas afeta agendadores específicos.
        
-   "Resolução máxima" (Max resolution):
    
    -   Para a maioria dos modelos, você vai querer isso definido para 768,768.
        
    -   Modelos que permitem geração nativa maior (como SDXL, por exemplo) podem usar valores maiores como 1024,1024.
        
    -   Você não deve definir isso maior do que seu modelo pode gerar nativamente. Placas menos potentes podem treinar em 512,512, mas terão qualidade reduzida. Alternativamente, muitos modelos baseados no antigo vazamento do NAI (a maioria dos modelos de anime SD1.5), podem ser treinados em 640,640.
        
    -   SDXL: Não defina abaixo de 1024.
        
-   Habilitar buckets: Verdadeiro (Enable buckets: True).
    
    -   Isso agrupa imagens de tamanhos semelhantes durante o treinamento. Isso é destinado ao treinamento em lote, mas não faz mal manter ligado.
        
-   "Resolução máxima do bucket" (Max bucket resolution):
    
    -   Deve ser maior que sua resolução de treinamento, pessoalmente uso 960 ao treinar em 768. Qualquer imagem maior que este tamanho será redimensionada e/ou recortada para caber na sua maior largura, tentando preservar sua proporção.
        
    -   SDXL: 2048
        
-   "Taxa de aprendizado do Text Encoder e Unet" (Text Encoder & Unet learning rate):
    
    -   SD1.5 AdamW: Tenho isso definido para 0.00005 e 0.0001, respectivamente.
        
    -   SDXL: 0.00045 & 0.0004
        
-   Rank da Rede e Alpha da Rede (Network Rank & Network Alpha):
    
    -   Esses afetam o tamanho do seu arquivo e a quantidade de dados que ele pode armazenar. O que você define isso dependerá do seu assunto.
        
    -   Se você estiver treinando algo semelhante ao que seu modelo já conhece (como uma garota de anime em um modelo de anime) um Rank/Alpha de 8/8 provavelmente funcionará. Na maioria dos casos, 32/32 é um bom ponto de partida. Embora você possa ir até 128/128, isso é um exagero absoluto que apenas incha seu arquivo e, em alguns casos, pode piorar seus resultados de treinamento. Geralmente, você não deve precisar ir além de 64. Seu Alpha deve ser mantido no mesmo número que seu Rank, na maioria dos casos. **Otimizadores adaptativos como Prodigy e DAdapt devem definir seu alpha para 1.**
        
    -   Quando não se usa otimizadores adaptativos, há algumas discussões sobre usar um alpha que na verdade é muito maior do que seu rank, seguindo a equação "(net alpha \* sqrt(net dim))", que deve preservar melhor as taxas de aprendizado. Valores comuns usando isso seriam 64/512, 32/181.02, 16/64 e 8/22.63, como rank/alpha, respectivamente.
        
        -   Testar isso mostrou alguns resultados interessantes, mas optei por não usar.
            
    -   SDXL: 16/16
        
-   Rank e Alpha de Convolução (Convolution Rank & Alpha):
    
    -   Rank de 16 com um alpha de 1. Ir além de 16 parece dar retornos decrescentes e pode realmente prejudicar as saídas.
        
-   Normas de peso de escala (Scale weight norms):
    
    -   Isso ajuda seu LoRA a funcionar bem com outros LoRAs em conjunto, mas _pode_ ser semi-destrutivo para sua saída.
        
    -   Pessoalmente, recomendo usar um conjunto de dados de regularização em vez de usar isso.
        
        -   Se você planeja usar seu LoRA com outros LoRAs, defina este valor para 1.
            
        -   Se seu LoRA provavelmente será usado apenas sozinho, deixe em 0.
            
        -   Dependendo do seu conceito, seus pesos que ficam muito "pesados" são reduzidos, reduzindo seu impacto. Isso permite que vários LoRAs funcionem em conjunto sem lutar por valores, mas em alguns casos PODE afetar negativamente suas saídas finais.
            
        -   Definir valores mais altos que 1 reduzirá o impacto, mas também reduzirá a compatibilidade cruzada.
            
        -   A escala parece ter um impacto significativamente menor no treinamento de LyCORIS, dado que o aprendizado é espalhado por mais pesos. Pode geralmente ser mantido em 1 sem preocupações.
            
        -   Atualmente, optei por manter isso em 0 de agora em diante.
            
-   Desligamento da Rede (Network Dropout):
    
    -   Recomendado, mas opcional. Um valor de 0.1 é um bom valor universal. Ajuda com o sobreajuste na maioria dos cenários.
        

```
Advanced (Subtab)
```

Não tocaremos muito aqui, pois a maioria dos valores tem propósitos específicos.

-   Peso da perda de prioridade (Prior loss weight):
    
    -   Especificamente para conjuntos de dados de regularização, 1 é um bom valor.
        
-   Parâmetros adicionais (Additional parameters):
    
    -   Se seu modelo suporta zSNR, use "--zero\_terminal\_snr".
        
-   Manter n tokens (Keep n tokens):
    
    -   Para uso com embaralhamento de legendas, para evitar que os primeiros X tokens sejam embaralhados, **incluindo as vírgulas usadas para separá-los**. Geralmente mantenho isso definido para 4 ou 6, para manter os primeiros 2-3 tokens de serem embaralhados.
        
-   Pular clipe (Clip skip):
    
    -   Deve ser definido para o valor de pulo de clipe do seu modelo. A maioria dos modelos de anime e SDXL usa 2, a maioria dos outros usa 1. Se você não tiver certeza, a maioria dos modelos civit observa o valor usado em sua página.
        
-   Verificação de ponto de gradiente (Gradient Checkpointing):
    
    -   Marque para economizar VRAM a um pequeno custo de velocidade. Não tem efeito na qualidade de saída.
        
-   Embaralhar legenda (Shuffle Caption):
    
    -   Opcional. Se verdadeiro, isso embaralhará as tags (fora dos primeiros X mantidos no lugar por "manter n tokens") toda vez que a imagem for treinada, o que ajuda com flexibilidade geral.
        
    -   Alguns consideram isso inútil ou uma "desculpa", mas sua utilidade varia com seu conjunto de dados. Também adiciona randomização ao seu treinamento, e com isso ligado, executar o mesmo treinamento duas vezes pode dar a você dois LoRAs ligeiramente diferentes no final.
        
-   Carregador de dados persistente (Persistent Data Loader):
    
    -   Opcional. Esta opção mantém suas imagens carregadas entre épocas. **Isso consome MUITA VRAM**, mas acelerará o treinamento. Se você puder _se dar ao luxo_ de usar, use.
        
-   CrossAttention:
    
    -   Sempre xformers.
        
-   Aumento de cor (Color augmentation):
    
    -   **_Não faça._**
        
-   Aumento de espelhamento (Flip Augmentation):
    
    -   Opcional. Isso permite que você basicamente dobre seu conjunto de dados espelhando aleatoriamente suas imagens horizontalmente durante o treinamento. Isso pode ser especialmente útil se você tiver poucas imagens, mas **NÃO** use isso se tiver detalhes assimétricos que deseja preservar.
        
-   Min SNR Gamma:
    
    -   5 é um valor conhecido como bom. Acelera o treinamento ligeiramente.
        
-   Perda de estimativa não enviesada (Debiased Estimation Loss):
    
    -   Verdadeiro. Ajuda com a desvio de cor e supostamente faz com que o treinamento precise de menos passos.
        
-   Tipo de deslocamento de ruído (Noise offset type):
    
    -   Original
        
-   Deslocamento de ruído (Noise offset):
    
    -   0
        

E isso é tudo! Role para o topo, abra o menu suspenso "configuração" e salve suas configurações com o nome que preferir. Uma vez feito isso, clique em "iniciar treinamento" na parte inferior e aguarde! Dependendo da sua placa, configurações e quantidade de imagens, isso pode levar um bom tempo.

## Parte 5 | Perguntas e Respostas

Esta seção é reservada para dicas, truques e outras coisas úteis que não se encaixam em outras partes. Tentarei atualizá-la periodicamente.

**P:** Vejo em muitos guias que recomendam treinar por 2000 etapas ou algo assim, mas você usa épocas. Por quê?

**R:** Devido à inconsistência no tamanho e qualidade dos conjuntos de dados, as etapas acabam sendo uma medida completamente arbitrária. As épocas, pelo menos, contam repetições completas das pastas e servem como uma melhor forma de medir a quantidade de treinamento feita em seus dados. Muitos desses guias também estão treinando conceitos muito fáceis, que o treinamento capta mais rapidamente. Não se preocupe se ultrapassar massivamente essa contagem de etapas, mas 2000 ainda é um bom número para se almejar.

**P:** Vejo outros guias dizendo para definir o Network Alpha para metade do Rank, por que você não faz isso?

**R:** Isso é um mal-entendido bastante antigo que ainda é muito disseminado. O Alpha funciona como uma forma de alterar suas taxas de aprendizado: sendo metade do Rank, é metade da taxa de aprendizado. Não faz mal ter em metade ou até mais baixo, mas você provavelmente precisará de um treinamento mais longo.

**P:** Meu script de treinamento está mostrando um valor de perda que continua mudando conforme o treinamento avança, o que é isso?

**R:** Na maioria dos casos, você não precisa se preocupar com a perda, nem deve se preocupar com valores ou intervalos específicos. A única vez que deve prestar atenção é se você vê-lo em torno de um certo intervalo na maior parte do treinamento, apenas para fazer uma mudança massiva mais tarde. Isso é um sinal de que algo pode ter dado errado ou que começou a treinar demais.

**P:** Como posso saber se meu LoRA está sub/treinado demais?

**R:** Ambos devem ser bastante óbvios, mesmo para o olho destreinado. Se estiver subtreinado, você provavelmente verá detalhes "borrados" ou incompletos, ou uma adesão muito baixa aos detalhes. Se estiver sobrecarregado, você pode ter cores estranhas e supersaturadas, viés de estilo, viés de pose, etc. Isso variará dependendo do seu conjunto de dados, então fique atento.

**P:** Você mencionou brevemente fp16 e bf16, mas quais são as versões "completas" que estou vendo?

**R:** O fp/bf16 "padrão" usa precisão mista, enquanto as versões "completas" não. É enganoso, mas as versões completas mantêm dados menos precisos, mas podem ser incrivelmente rápidas para treinar. Tenho certeza de que têm seus usos, mas na maioria dos casos você está perfeitamente bem ficando com precisão mista.

**P:** Continuo vendo menções de "Vpred", o que exatamente é isso?

**R:** Vpred, ou V-Prediction, ou V-Parameterization, são todas a mesma coisa. Embora eu não entenda completamente em nível técnico, pelo que sei, é uma otimização para os agendadores de ruído que "predizem" resultados durante a geração de imagens, permitindo que um resultado final seja gerado em menos etapas.

**P:** O que é Min SNR? zSNR? Zero Terminal SNR? Eles são iguais ou diferentes?

**R:** Não, embora semelhantes, fazem coisas bastante diferentes. Para simplificar, o zSNR (Zero Terminal SNR) é uma técnica que permite que a IA gere usando um espaço de cores mais amplo, incluindo pretos perfeitos. Pense nisso como a diferença entre um monitor normal e um monitor HDR OLED. Min SNR é um método de convergência acelerada de treinamento, que permite que modelos sejam treinados em menos etapas.

**P:** Posso treinar em uma resolução maior do que o meu modelo de treinamento pode fazer?

**R:** Pode? Sim. Deve? Não. Normalmente, resoluções mais altas são uma troca de qualidade por velocidade, neste caso você estaria trocando velocidade por piores resultados. Sem entrar em detalhes técnicos, treinar em uma resolução maior do que o seu modelo pode lidar não é bom para seus resultados.

**P:** Você mencionou não "sobrecarregar" suas imagens com tags (overtag), mas quantas são muitas?

**R:** Isso realmente dependerá do seu conjunto de dados e configurações de treinamento. Treinamentos mais longos podem ajudar com o excesso de tags, mas correm um risco maior de sobrecarregar. Geralmente, tente manter seu total por imagem em 20 ou menos em média, mas ter exceções com mais não é o pior. Tente evitar tags que não sejam importantes para a imagem (a menos que você ache que os resultados estão se apegando demais a algo, nesse caso marque), e tags que seu modelo tenha pouco ou nenhum conhecimento. Tags vazias são vistas como alvos de treinamento e tentarão ser preenchidas. Se preenchidas com dados errados, você pode acabar com tags aparentemente aleatórias sendo necessárias para obter o resultado pretendido.

**P:** Qual é a diferença entre um LoRA e um LyCORIS? Eles são diferentes?

**R:** Todo LyCORIS é um LoRA, mas nem todo LoRA é um LyCORIS. LyCORIS se refere especificamente a um subconjunto de métodos mais novos de treinamento de LoRA (LoCon, LoHa, LoKR, DyLoRA, etc.). Todos esses ainda são LoRAs, mas suas novas metodologias os tornam estruturalmente diferentes o suficiente para terem sua própria designação. Agora que a maioria das interfaces gráficas tem suporte embutido para eles, para um usuário final eles funcionam de maneira funcionalmente igual. LoRA por si só se refere ao método original.

**P:** Meu LoRA meio que funciona, mas às vezes apresenta uma anatomia muito estranha e distorcida. O que aconteceu?

**R:** Na maioria das vezes, a anatomia distorcida se origina do seu conjunto de dados. Revise-o em busca de imagens que sejam semelhantes às distorções que você está vendo. Poses incomuns, ângulos de câmera estranhos, imagens de duplas/grupos incorretamente marcadas e outras exceções podem ser causas prováveis. Tente marcar o que for aplicável, mas geralmente é melhor remover a imagem completamente ou recortar as partes que estão causando problemas, se possível.

**P:** Ouvi um pouco sobre treinamento com uma única tag (single-tag training), o que é isso?

**R:** Treinar com uma única tag é um método muito antigo comumente usado por iniciantes que não querem perder tempo marcando. Ao treinar com uma única tag, a IA "homogeneizará" **_tudo_** que aprende de uma imagem na tag, resultando em resultados altamente generalizados. Isso só começará a funcionar se cada imagem for de um assunto específico (como um personagem), e tem uma alta probabilidade de se fixar em fundos, poses e outras variáveis indesejadas. Se usado com qualquer outra coisa que não seja repetitiva, você acabará com algo que é efetivamente uma bagunça digital. Não recomendaria isso para nenhuma aplicação.

**P:** Já vi outras pessoas dizendo para marcar suas imagens de uma forma diferente da que você faz, sem ter tags para descrever o assunto fora do seu token principal. Isso é melhor? Pior?

**R:** Nenhum! É apenas um método diferente de marcação: no entanto, é muito menos flexível. Se você pegar um personagem, marcar o personagem apenas como o personagem pode dificultar a mudança da cor dos olhos, roupa ou outros detalhes. Se você não se importa com isso, é perfeitamente válido! Alternativamente, meu método é muito mais flexível, mas obter a semelhança exata do personagem exigirá várias tags.

## Parte 6 | Treinamento Avançado: Multi-Conceito LoRA

Então, você já se familiarizou e quer mais um desafio? Ou talvez você tenha um personagem com muitos trajes? Armaduras específicas de gênero? É aí que entra o treinamento de multi-conceito.

As configurações reais de treinamento para estes são quase exatamente as mesmas em comparação com os LoRAs normais, com algumas ressalvas:

-   Não use um tamanho de lote maior que 1. Se imagens de vários conceitos forem carregadas, elas se generalizarão em uma bagunça, ou você terá uma sobrepondo a outra.
    
    -   Possivelmente não é mais o caso, mas não tenho certeza no momento.
        
-   Tenha cuidado ao usar aumento de flip (flip augmentation), pois ele será aplicado a todas as imagens, não apenas a um conceito.
    
-   Dependendo de quantos conceitos você está treinando e quão complexos eles são, você pode querer aumentar seus valores de Rank e Alpha. Recomendo tentar 32 primeiro e ver como ele se comporta.
    

Agora, reúna suas imagens da mesma maneira que detalhei antes, mas separe-as com base em seus conceitos (trajes, armaduras, etc). Qualquer edição também deve ser feita como antes.

Depois de preparar completamente seus dados, descubra qual conceito tem mais dados e, em sua pasta de conceito, crie uma pasta 1\_nomeDoConceito para ele.

Agora, faça o mesmo com seus outros conceitos, obviamente substituindo "nomeDoConceito"

 pelo token de instância deles.

Depois de nomear e preencher suas pastas, faça o seguinte:

-   Pegue o número de imagens na sua maior pasta, depois multiplique pelo "X\_" para obter sua contagem total de etapas. (imagens\*repetições da pasta) = etapas
    
    -   Por exemplo, a Pasta A tem 51 imagens, a Pasta B tem 43 imagens. A Pasta A seria usada. Supondo 10 repetições de pasta, isso nos dá 510 etapas para a Pasta A. (51\*10)
        
-   Agora, divida a contagem de etapas pelo número de imagens na sua segunda maior pasta. O número resultante, arredondado para o mais próximo inteiro, é o número que o "X\_" dessa pasta deve ser alterado.
    
    -   Então, a Pasta B tem 43 imagens. (510/43) = 11.86, que arredondamos para 12. Agora temos 10\_PastaA e 12\_PastaB.
        
-   Repita isso para cada pasta aplicável.
    
    -   A Pasta C tem 32 imagens, então comparamos com a Pasta A como antes. (510/32) = 15.93, que arredondamos para 16.
        
-   Em nosso exemplo, agora temos três pastas equilibradas juntas. Estas poderiam ser deixadas como estão ou, como todas são divisíveis por dois, você pode reduzir cada X\_ pela metade para obter 6\_, 8\_ e 5\_ respectivamente. Lembre-se de que você estará multiplicando esses valores pelas suas épocas!
    

Por que fazer isso, você pergunta?

Fazemos isso para equilibrar o conjunto de dados. Se você mantiver tudo igual, a pasta com mais imagens dominará o treinamento, deixando os outros conceitos com uma fração. Equilibramos o conjunto de dados para garantir que cada conceito receba tempo de treinamento igual, o que evita que um domine e os outros conceitos subtreinem.

Você deve ter em mente, no entanto, que se tiver muito poucas imagens em uma pasta de conceito, esse conceito individual pode supertreinar, mesmo que o resto do LoRA esteja bem. Isso é um problema maior quanto maior for a discrepância entre ele e a maior pasta.

Agora que suas pastas estão equilibradas, devemos observar como você as nomeia e qual será a sua tag de ativação para cada uma.

Se você estiver treinando um personagem com vários trajes, nomeie suas pastas como "1\_charactertag, outfittag". Suas duas primeiras tags devem ser essas, nessa ordem.

Se você estiver treinando algo não relacionado a um personagem, como uma armadura de gênero, geralmente crio uma tag para cada versão. Por exemplo, "armortagm" e "armortagf" para homens e mulheres respectivamente. Como antes, essas devem ser a primeira tag em suas respectivas imagens.

Agora que seus nomes e tags de ativação estão definidos, você pode começar a marcar! Isso pode ser feito como um lora normal, você só tem muito mais imagens para percorrer.

E é isso! depois de marcar, você pode treiná-lo como antes. Provavelmente terá tempos de treinamento muito mais longos, devido ao aumento de imagens, mas no final terá vários conceitos em um único LoRA para usar como desejar.

## Parte 7 | Treinamento Avançado: LyCORIS e Seus Muitos Métodos

LyCORIS fica mais avançado a cada dia e, à medida que aumenta em popularidade, sinto que é melhor ter uma seção falando sobre isso. Esta será um pouco mais técnica do que o resto, mas tentarei manter apenas o essencial.

**Tipos de LyCORIS:**

-   **LoCON:** Um LoRA que também afeta as camadas de convolução do modelo base, permitindo saídas mais dinâmicas.
    
-   **LoHa & LoKR:** Um LoRA que essencialmente é duas versões diferentes de si mesmo, que são combinadas/média pelo Produto de Hadamard e Produto de Kronecker respectivamente. Demoram mais para treinar e são mais orientados para o treinamento generalizado.
    
-   **DyLoRA:** Abreviação de LoRA Dinâmico, esta é uma implementação de LoRA que permite que o Rank mude dinamicamente, mas de outra forma é um LoRA normal.
    
-   **GLoRA:** Abreviação de LoRA Generalizado, esta é uma implementação feita para generalizar conjuntos de dados diversos de maneira flexível e capaz.
    
-   **iA3:** Em vez de afetar o rank como a maioria dos LoRA, o iA3 afeta os vetores aprendidos, resultando em um método de treinamento muito eficiente. Semelhante (aparentemente um pouco melhor?) a um LoRA normal, em um pacote muito menor.
    
-   **Diag-OFT:** Esta implementação "preserva a energia hiperesférica ao treinar transformações ortogonais que se aplicam às saídas de cada camada". Em resumo, esse tipo é melhor para preservar a compreensão original do modelo base sobre itens coincidentes ao treinamento (como fundos e poses). Isso também aparentemente converge (treina) mais rápido que um LoRA padrão.
    
-   **Fine-Tuning Nativo:** Também conhecido como dreambooth, que não estamos focando e ignoraremos neste guia. A implementação de LyCORIS permite que seja usado como um LoRA, mas produz arquivos **muito** grandes.
    

"Então, o que devo usar?"

Eu diria pessoalmente que cada um tem seus próprios usos, então os categorizei semi-geralmente. Ainda não tenho muito conhecimento sobre suas minúcias, mas **baseei-me amplamente nas notas e documentações de implementação _oficiais_**. O que você escolher depende de você e é totalmente baseado em suas necessidades.

-   **Propósito Geral:**
    
    -   LoCON, DyLoRA, iA3, Diag-OFT
        
-   **Multi-Conceito:**
    
    -   LoCON, LoHa, LoKR
        
-   **Conceitos:**
    
    -   LoCON, LoHa, LoKR, GLoRA
        
-   **Estilos:**
    
    -   LoCON, GLoRA, iA3
        

**Benefícios, Desvantagens e Notas de Uso:**

-   **LoCON:**
    
    -   Amplamente Aplicável
        
    -   Afeta Mais Camadas do Modelo
        
    -   Arquivos Levemente Maiores
        
    -   Basicamente Apenas Um LoRA, Mas Melhor
        
        -   Dim <= 64 Máximo, 32 Recomendado
            
        -   Alpha >= 0.01, Metade Recomendado (Quando não estiver usando um otimizador adaptativo)
            
-   **LoHa & LoKR:**
    
    -   Bom Com Treinamento Multi-Conceito
        
    -   Bom Com Generalização
        
    -   Tempos de Treinamento Mais Longos
        
    -   Ruim Com Conceitos Altamente Detalhados
        
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
        
    -   De Outra Forma Apenas Um LoRA
        
        -   Use com Dim grande (~128), Alpha Metade (Quando não estiver usando um otimizador adaptativo)
            
        -   Use Acumulação de Gradiente
            
        -   Tamanho do Lote Máximo de 1
            
-   **GLoRA:**
    
    -   Muito Bom Em Generalização (Estilos e Conceitos)
        
    -   Tempos de Treinamento Mais Curtos (?, A Testar)
        
    -   Não Muito Bom Em Treinar Assuntos Não Generalizados
        
-   **iA3:**
    
    -   Tamanhos de Arquivo Muito Pequenos
        
    -   Geralmente Aplicável
        
    -   Geralmente Desempenha Melhor Que LoRA
        
    -   Bom Com Estilos
        
    -   Pode Ser Difícil de Transferir
        
        -   Use com Alta LR (High LR) []Quando não estiver usando um adaptive optimizer], a implementação oficial recomenda 5e-3 (0.005) ~ 1e-2 (0.01)
            
-   **Diag-OFT:**
    
    -   Tempo de Treinamento Mais Rápido
        
    -   Melhor Preserva Coincidências
        
    -   Geralmente Aplicável
        

## Parte 8 | Treinamento Avançado: Estilos e Temas

Então, você quer treinar um estilo de algum tipo. Independentemente do que seja, para conceitos mais amplos, um LyCORIS é a ferramenta ideal, mas ao contrário de um LoRA, há vários tipos de LyCORIS para escolher. Se você pulou a Parte 7, recomendo um LoCON, GLoRA ou iA3.

Depois de escolher seu tipo, certifique-se de que seu rank está definido para 32 ou menos. LyCORIS parece ter alguns problemas acima de certos pontos (embora você possa ir até 64, acredito), mas 32 é o máximo geralmente aceito antes de começar a ter problemas.

Agora que isso está fora do caminho, você deve começar a construir um conjunto de dados, assim como antes. No entanto, treinamentos de estilo se beneficiam muito mais de conjuntos de dados maiores, então, em vez da faixa de 15-50 de antes, procure obter cerca de 50-200, na minha experiência, 125-150 é um bom lugar para estar.

Depois de conseguir suas imagens, comece a marcá-las. Você pode geralmente marcar da mesma maneira que antes, mas lembre-se de que você quer o estilo, não um personagem ou artigo de roupa. Você deve especialmente garantir que marque fundos, roupas e qualquer outro elemento chave.

Depois de marcar, você está pronto para começar a treinar. Na minha experiência, esses geralmente levam menos épocas para treinar em comparação a um LoRA: Embora eu recomende ~100 repetições para um LoRA, esses geralmente estão ok com ~30-40 repetições, mas sua experiência pode variar, dado o tamanho e composição do seu conjunto de dados.

## Registro de Alterações

28/05/24

-   Introdução:
    
    -   Pequenos ajustes.
        
-   Parte 1:
    
    -   Feitas distinções entre recomendações 1.5 e novas XL.
        
    -   Pequenos ajustes de linguagem.
        
    -   Adicionada nota de isenção ao meu recomendação do Pixiv.
        
-   Parte 2:
    
    -   Pequenos ajustes para corrigir erros gramaticais que notei.
        
    -   Adicionada pequena quantidade de explicação adicional a algumas seções.
        
    -   Adicionado um upscale recomendado.
        
-   Parte 3:
    
    -   Adicionadas algumas distinções entre SD1.5 e XL.
        
    -   Ajustado texto para adicionar ênfase em certas partes.
        
    -   Ajustadas algumas palavras na parte 3.5.
        
    -   Levemente atualizados exemplos de tags na parte 3.6.
        
-   Parte 4:
    
    -   bmaltis mudou o layout da GUI, então agora tenho que reescrever as instruções >:(
        
        -   Reformatei a maior parte da seção e adotei algumas configurações experimentais na configuração padrão.
            
        -   Também adicionei especificidades para minhas configurações de SDXL.
            

24/03/24

-   Adicionados taxas de aprendizado ausentes para minha configuração AdamW. Oops.
    
-   Levemente expandida Parte 3.5.
    

04/03/24

-   Adicionada seção cobrindo tokens de classe à Parte 3.
    
-   Mudada Parte 3.5 (Exemplos de marcação) para 3.6.
    
-   Adicionada nova Parte 3.5, cobrindo Preservação de Prioridade.
    
    -   Adicionada configuração de Pasta de Regularização à Parte 4.
        
-   Atualizadas configurações experimentais.
    
-   Ajustadas algumas descrições de configurações.
    
-   Adicionados alguns esclarecimentos em áreas diversas.
    

29/02/24

-   Extensa mas geral reestruturação, implementando críticas de outros treinadores, principalmente ArgentVASIMR.
    
    -   Parte 1:
        
        -   Expandida sobre Imagens Duo/Trio/Grupo.
            
    -   Parte 2:
        
        -   Expandido/editado vários parágrafos para fornecer mais alternativas e informações mais claras.
            
    -   Parte 3:
        
        -   Pequenas edições, mudado terminologia para ser mais apropriada.
            
    -   Parte 4:
        
        -   Ajustadas algumas configurações, principalmente em relação ao uso de otimização Adam.
            
        -   Adicionado um pouco mais de descrição a algumas configurações.
            
        -   Adicionados parâmetros para embaralhamento de legendas.
            
        -   Adicionada seção experimental sobre escalonamento alpha quando não se utiliza otimizadores adaptativos.
            
    -   Parte 5:
        
        -   Ajustadas algumas palavras para melhor aderir aos termos corretos.
            
        -   Ajustada Q&A em relação ao treinamento completo fp16.
            
    -   Parte 6:
        
        -   Ajustada alguma terminologia, novamente.
            
        -   Levemente alterada e fornecido um exemplo para a fórmula de balanceamento de conjunto de dados.
            

26/02/24

-   Configurações experimentais atualizadas.
    
-   Ajustadas algumas explicações pequenas em relação ao LyCORIS.
    

09/02/24

-   Configurações experimentais atualizadas.
    
-   Adicionados mais detalhes à parte 4.
    
-   Adicionada breve seção sobre algumas novas descobertas na parte 2.
    

01/02/24

-   Adicionada parte 2.5, uma subseção sobre Nightshade e outros "venenos" de IA.
    

07/01/24

-   Movida parte 7 para parte 8 & removida explicação de LyCORIS.
    
-   Adicionada (nova) parte 7, aprofundando mais sobre LyCORIS.
    
-   Ajustadas configurações experimentais.
    

05/01/24

-   Ajustadas configurações experimentais & adicionadas algumas explicações para alguns valores.
    
-   Adicionadas perguntas de Q&A.
    
-   Expandido sobre o valor "normas de peso de escala" na parte 4.
    
-   Corrigidas seções sobre minsnr e zsnr para diferenciá-las corretamente.
    
-   Ajustado "parâmetros adicionais": Valor não mais necessário.
    

30/12/23

-   Adicionadas configurações experimentais à parte 4.
    
-   Alterado título para incluir LyCORIS.
    
-   Adicionada pergunta de Q&A.
    

28/12/23

-   Correção de mais erros gramaticais.
    
-   Levemente expandida Parte 1 e 2.
    
-   Adicionada seção cobrindo tags implícitas à Parte 3.
    
-   Adicionadas pequenas elaborações em algumas áreas.
    

27/12/23

-   Correção de pequenos erros gramaticais nas partes 3 e 4.
    
-   Adicionadas novas perguntas de Q&A.
    
-   Adicionadas partes 6 e 7, cobrindo treinamento Multi-conceito e Estilo respectivamente.
    
-   Adicionada parte 3.5 para exemplos de marcação, adicionados dois para começar.
    

26/12/23

-   Guia criado
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
