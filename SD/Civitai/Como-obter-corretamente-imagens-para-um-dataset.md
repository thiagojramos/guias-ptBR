# [Como "obter corretamente" imagens para um dataset. | Civitai](https://civitai.com/articles/91/how-to-correctly-obtain-images-for-a-dataset)


![Como "obter corretamente" imagens para um dataset](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/b3de4695-26f6-4fab-8695-bbc466842b79/width=730/b3de4695-26f6-4fab-8695-bbc466842b79.jpeg)


tags: `# DATA PREP` `# DATASET` `# IMAGES` `# HOWTO` `# GETTING STARTED`
autor: [**guy90**](https://civitai.com/user/guy90)

---
Ao montar um dataset para o treinamento de modelos, diversos fatores contribuem para o sucesso na criação de um modelo eficaz. Este guia tem como objetivo fornecer insights valiosos e abordar perguntas comuns, ajudando você a navegar no processo de forma mais eficiente. _Embora este artigo se concentre em princípios gerais e evite detalhes técnicos excessivos, ele busca direcioná-lo na direção certa, enfatizando conceitos fundamentais que geralmente se alinham com o senso comum. Este artigo não explicará como treinar ou testar um modelo. Se você está procurando informações sobre esses tópicos, leia os dois artigos abaixo:_

1.  [**[Guia] Faça seus próprios Loras, fácil e gratuito**](https://civitai.com/models/22530?modelVersionId=26903)
    
2.  [**Personagens, Roupas, Poses, Entre Outras Coisas: Um Guia**](https://civitai.com/articles/680)
    

Ambos os artigos fazem um ótimo trabalho ao explicar como treinar e testar um modelo. Eles também têm informações adicionais sobre como reunir um dataset que podem ser úteis. Eles definitivamente valem a leitura, na minha opinião, se você quer aprender mais sobre SD, especificamente Loras.

Este artigo está dividido em 3 Seções e destacado nesta cor para facilitar a acessibilidade:

1.  **Ferramentas e Programas Que Podem Ajudar a Acelerar o Processo**
    
2.  **Método Manual de Coleta de Dados**
    
3.  **Preparação de Imagens**
    

**_Créditos:_** [**_justTNP_**](https://civitai.com/user/justTNP/models) **_e seu artigo:_** [**_Personagens, Roupas, Poses, Entre Outras Coisas: Um Guia_**](https://civitai.com/articles/680)

### [**_Grabber_**](https://github.com/Bionus/imgbrd-grabber)

Grabber é uma ferramenta inestimável projetada para simplificar e acelerar o processo de encontrar imagens que você pode usar em seus datasets. Em vez de vasculhar inúmeros sites em busca de imagens para melhorar seu modelo, Grabber automatiza a tarefa navegando por vários image boards. Isso permite que você acesse facilmente uma ampla gama de imagens potencialmente adequadas para seu modelo. Particularmente vantajoso ao lidar com projetos com disponibilidade limitada de imagens, o Grabber simplifica significativamente a fase de aquisição de dataset. Para uma compreensão mais profunda das capacidades do Grabber, recomendo explorar a [página do GitHub](https://github.com/Bionus/imgbrd-grabber) e mergulhar no [artigo do justTNP](https://civitai.com/articles/680).  

### [**dupeGuru**](https://dupeguru.voltaicideas.net/)

dupeGuru é uma solução formidável para examinar meticulosamente seu dataset, identificando quaisquer instâncias de imagens duplicadas. Esta ferramenta realiza uma busca minuciosa para identificar imagens duplicadas e oferece a funcionalidade adicional de refinar sua busca para descobrir imagens semelhantes, caso seu objetivo seja reduzir o número de imagens. A habilidade do dupeGuru realmente se destaca em compilar uma lista abrangente de imagens duplicadas e semelhantes. Sua interface amigável facilita a comparação e exclusão eficientes, tornando a tarefa de gerenciar seu dataset uma experiência totalmente simplificada. Para mais informações, visite o [site do dupeGuru](https://dupeguru.voltaicideas.net/) e também o [artigo do justTNP](https://civitai.com/articles/680).

### [**XnView MP**](https://www.xnview.com/en/xnviewmp/#downloads)

XnView MP é um visualizador de imagens gratuito que possui uma variedade de ferramentas extras embutidas para ajudar a editar e também a classificar imagens. Esta ferramenta é muito útil se você tiver muitas imagens, especialmente mais de 50. Para alguns exemplos, por favor, acesse [meu artigo sobre o XnView MP](https://civitai.com/articles/2499/the-best-image-viewer-in-my-opinion). Também há alguns comentários sobre outros visualizadores de imagens que eu não testei, você pode achá-los úteis.

## **<ins>Coleta de Dados:</ins>**

_Você não quer usar o método rápido e fácil, ou simplesmente quer aprender todas as formas de coletar imagens para um dataset. Este método mostra como coletar imagens manualmente._

Reunir imagens para seu dataset pode de fato ser uma tarefa exigente e árdua. Para simplificar este processo, recomendo utilizar uma extensão de navegador útil chamada "[Salvar no Google Drive.](https://chrome.google.com/webstore/detail/save-to-google-drive/gmbmikajjgmnabiglmofipeabaddhgne)" Esta extensão permite que você armazene convenientemente imagens, vídeos e arquivos diretamente no seu Google Drive. Após fazer login, basta clicar com o botão direito na imagem desejada e aparecerá uma opção para salvá-la no seu Google Drive. Pessoalmente, acho essa extensão benéfica, pois centraliza todos os dados coletados em um único local. Posteriormente, acessar seu Google Drive permite baixar facilmente a pasta inteira para seu computador com apenas alguns cliques.

Onde Encontrar Imagens: Quando você está procurando por imagens, pode ser desafiador localizar as mais adequadas dependendo da natureza do seu modelo. Aqui estão algumas fontes valiosas que podem ajudar você a encontrar imagens:

### **Motores de Busca**

-   [<ins>Google</ins>](https://images.google.com/): O Google é um motor de busca popular que muitas vezes fornece resultados de imagens relevantes.
    
-   [<ins>Bing</ins>](https://www.bing.com/images): O Bing é outro motor de busca que pode render resultados frutíferos de busca de imagens.
    
-   [<ins>Yandex</ins>](https://yandex.com/images/): O Yandex é um motor de busca conhecido por exibir imagens que podem não ser mostradas pelo Google ou Bing, tornando-o uma alternativa útil.
    

### **Sites**

-   [<ins>Danbooru</ins>](https://danbooru.donmai.us/): Danbooru é uma plataforma de hospedagem de imagens focada em conteúdo de Anime e Hentai.
    
-   [<ins>Safebooru</ins>](https://safebooru.donmai.us/): Safebooru é a versão SFW (Safe for Work) do Danbooru, um site de hospedagem de imagens de propósito geral.
    
-   [<ins>Gelbooru</ins>](https://gelbooru.com/): Gelbooru é uma plataforma de hospedagem de imagens versátil, adequada para diversos fins.
    
-   [Deviant Art](https://www.deviantart.com/): O Deviant Art hospeda uma ampla gama de criações artísticas de criadores talentosos.
    
-   [<ins>Pinterest</ins>](https://www.pinterest.com/): O Pinterest é uma plataforma dirigida por usuários onde eles fazem upload e compartilham imagens, tornando-se um recurso valioso para encontrar visuais diversos.
    
-   [<ins>Fancaps</ins>](https://fancaps.net/): O Fancaps é útil para encontrar imagens/capturas de tela de Animes, Filmes e Shows.
    
-   Listas de Sites Booru que encontrei: [Booru Project](https://booru.org/top), [lista do lxfly2000](https://gist.github.com/lxfly2000/c183fcd23cfb447b2b9cb353e103c8fe), [Awesome-Booru por celsriseup](https://github.com/celsriseup/awesome-booru#awesome-booru-)
    
-   [New Grounds](https://www.newgrounds.com/art): Lembra dos jogos em flash? Este lugar tem aqueles e algumas imagens de artistas
    
-   [Buhitter](https://buhitter.com/): Agregador de posts de imagem do AI Twitter (X) (Acho, nada está em inglês, mas é útil para algumas coisas). Digite o que você quer e ele encontrará.
    

### **Sites NSFW**

-   [<ins>Rule34</ins>](https://rule34.xxx/): Rule34 é um site conhecido por sua extensa coleção de conteúdo explícito cobrindo uma ampla gama de assuntos.
    
-   [<ins>E-hentai</ins>](https://e-hentai.org/): E-hentai é uma coleção abrangente de imagens focadas principalmente em obras hentai. Por favor, esteja ciente de que o conteúdo desses sites é explícito e deve ser abordado com cautela.
    
-   [<ins>F95zone</ins>](https://f95zone.to/): Este é um lugar que hospeda tanto jogos NSFW quanto coleções de imagens/vídeos.
    

Embora essas fontes possam fornecer uma riqueza de imagens, é essencial usá-las de forma responsável e considerar quaisquer implicações legais e éticas.

## **<ins>Preparação de Imagens:</ins>**

### **Primeiros Passos na Preparação de Imagens**

Determinar quais imagens coletar para seu modelo é uma etapa crucial no processo. Para garantir a eficácia, considere o seguinte conjunto de regras que pode orientar sua seleção:

1.  **Imagens de Alta Qualidade:** Procure por imagens que tenham alta resolução e detalhes visuais claros. Evite imagens de baixa resolução ou borradas, pois podem prejudicar a capacidade do modelo de aprender e generalizar.
    
2.  **Imagens Relevantes e Representativas:** Busque imagens que representem com precisão o assunto que você está treinando seu modelo. Essas imagens devem mostrar as características, atributos ou elementos-chave que você quer que o modelo aprenda e reconheça.
    
3.  **Saída Desejada do Modelo:** Considere o que você quer que seu modelo produza ou se pareça. Procure por imagens que estejam alinhadas com sua saída ou estilo visual desejado. Isso pode ajudar a orientar o processo de aprendizado do modelo e influenciar seus resultados gerados.
    

Seguindo essas diretrizes, você pode curar um dataset que melhora o desempenho do seu modelo, permitindo que ele aprenda com imagens de alta qualidade e representativas e produza resultados desejados de maneira mais eficaz.

### **Qualidade da Imagem**

Para um modelo focado em Kazuma Kiryu, é crucial priorizar imagens de alta qualidade. Optar por imagens com resolução de 512 ou superior (1024 para modelos XL) é uma abordagem sensata, pois permite que o modelo capture detalhes finos de forma eficaz. Embora seja possível incluir imagens de qualidade inferior, é importante estar ciente de que o modelo pode ter dificuldades para treinar bem, especialmente quando se trata de detalhes intrincados como características faciais, padrões ou tatuagens que são significativos na aparência de Kazuma Kiryu. Esforçar-se por imagens de alta qualidade melhorará a capacidade do modelo de aprender e gerar representações precisas do personagem.

**_<ins>Exemplo de boa qualidade:</ins>_**

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f0bd988a-4bd8-4344-b72c-5bd771f1f5a4/width=525/f0bd988a-4bd8-4344-b72c-5bd771f1f5a4.jpeg)

  
**_<ins>Exemplo de má qualidade:</ins>_**  

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/b2f54935-2814-4baa-9664-12fd7ebf57a0/width=525/b2f54935-2814-4baa-9664-12fd7ebf57a0.jpeg)

### **Trajes**

Outro aspecto importante a considerar ao selecionar imagens para seu modelo, particularmente para modelos baseados em personagens como Kazuma Kiryu, é garantir que as imagens se assemelhem de perto ao personagem em sua representação autêntica e oficial. Ter uma base forte de imagens apresentando a roupa original é crucial para que o modelo aprenda e reproduza com precisão a aparência distintiva do personagem, garantindo coerência e consistência visual.

No entanto, incorporar imagens com trajes alternativos pode melhorar a flexibilidade do modelo. Ao incluir uma variedade de vestimentas, o modelo pode aprender a reconhecer o personagem em diferentes contextos e aparências. Esta abordagem amplia a capacidade do modelo de lidar com variações, mantendo a identidade central do personagem. Assim, enquanto a roupa oficial deve formar a base do seu dataset, integrar trajes alternativos pode fornecer versatilidade adicional, permitindo que o modelo se adapte a diversos cenários sem comprometer o reconhecimento e a representação do personagem.

**_<ins>Exemplo de traje oficial:</ins>_**

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/0b81277d-e77c-404f-bf51-31349ac45f6d/width=525/0b81277d-e77c-404f-bf51-31349ac45f6d.jpeg)

  
**_<ins>Exemplo de traje alternativo:</ins>_**  

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/ed2d28d7-db78-446b-b394-4f62667ef78f/width=525/ed2d28d7-db78-446b-b394-4f62667ef78f.jpeg)

### **Posições das Imagens**

Ao reunir imagens para seu dataset, é importante considerar a variedade de poses e posições de câmera para otimizar o desempenho do seu modelo. Levar em conta diferentes posições de câmera permite que o modelo aprenda a gerar imagens a partir de várias perspectivas e pontos de vista. Aqui estão algumas posições de câmera a considerar ao selecionar imagens:

1.  **Close-up:** Incorpore fotos close-up que destaquem as características faciais do personagem, permitindo que o modelo aprenda detalhes mais finos, como expressões, texturas e elementos intrincados.  
    
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/a1e149d5-fffe-4c2e-bae9-752b5be98f64/width=525/a1e149d5-fffe-4c2e-bae9-752b5be98f64.jpeg)
    
2.  **Portrait (Retrato):** Inclua imagens que retratem o personagem em uma composição de estilo retrato, enfatizando sua aparência geral, incluindo cabeça, rosto e ombros.  
    
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6869e981-8a9b-4691-93b1-7182afae13ff/width=525/6869e981-8a9b-4691-93b1-7182afae13ff.jpeg)
    
3.  **Upper Body (Parte Superior do Corpo):** Inclua imagens que focam na parte superior do corpo do personagem, capturando detalhes como expressões faciais, roupas e parte superior do tronco.  
    
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/b2adf82d-1a3f-4f0d-855e-1859eca62723/width=525/b2adf82d-1a3f-4f0d-855e-1859eca62723.jpeg)
    
4.  **Cowboy Shot:** Esta posição de câmera enquadra o personagem das coxas ou joelhos para cima, proporcionando uma foto média que mostra a linguagem corporal, vestimenta e postura geral do personagem.  
    
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/cef6bfa7-0825-442c-9c00-f834f0d305f2/width=525/cef6bfa7-0825-442c-9c00-f834f0d305f2.jpeg)
    
5.  **Full Body (Corpo Inteiro):** Incorpore imagens que capturam o corpo inteiro do personagem, permitindo que o modelo aprenda as proporções, postura e físico geral do personagem.  
    
    ![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/d4758234-6d92-43ec-910a-39446dba54a0/width=525/d4758234-6d92-43ec-910a-39446dba54a0.jpeg)
    

Ao incluir uma variedade de poses e posições de câmera no seu dataset, você proporciona ao modelo uma compreensão abrangente da aparência do personagem a partir de diferentes ângulos e perspectivas. Essa variedade permitirá que o modelo gere saídas mais precisas e versáteis em diversas visualizações e poses.

### **Foco da Imagem**

Ao buscar imagens especificamente para modelos de personagens, é crucial garantir que o foco permaneça exclusivamente no personagem que está sendo modelado. Siga estas diretrizes ao selecionar imagens:

1.  **Excluir Segunda Pessoa:** As imagens devem idealmente apresentar apenas o personagem que está sendo modelado, sem qualquer indivíduo proeminente adicional. No entanto, se houver transeuntes ou figuras desfocadas no fundo, é aceitável desde que não desviem a atenção do personagem principal.
    
2.  **Evitar Foco Principal em Outros:** O foco principal de uma imagem deve sempre ser o personagem que está sendo modelado. Outros indivíduos não devem dominar a imagem ou distrair a presença do personagem.
    
3.  **Recortar Imagens como Último Recurso:** Se você encontrar imagens onde uma segunda pessoa está presente, mas não é o foco principal, recortar a imagem para remover ou minimizar a segunda pessoa pode ser considerado como último recurso. No entanto, priorize o uso de imagens onde o personagem é o sujeito principal sem distrações.
    

Seguindo estas diretrizes, você garante que o modelo se concentre exclusivamente no personagem que está sendo modelado, melhorando sua capacidade de aprender as características distintas, expressões e atributos do personagem com precisão.

**_<ins>Exemplo de uma segunda pessoa em foco:</ins>_**

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/b8a7b184-c2e2-44eb-ae47-b43936709105/width=525/b8a7b184-c2e2-44eb-ae47-b43936709105.jpeg)

### **Preparação de Estilo**

Ao selecionar as imagens para um **modelo focado em estilo**, é crucial manter a consistência e escolher imagens que sigam um único estilo específico. É comum que artistas experimentem diferentes estilos ao longo do tempo, e o mesmo se aplica a equipes criativas que trabalham em shows, filmes, mangás ou animes. Para garantir os melhores resultados, siga estas diretrizes:

1.  Escolha um Estilo Específico: Determine o estilo específico que você deseja que seu modelo aprenda e gere. Pode ser o estilo de um artista específico ou um estilo consistente visto em um show, filme ou obra de arte.
    
2.  Evite Misturar Estilos: Evite incluir imagens que mostrem vários estilos diferentes dentro do seu dataset. Misturar estilos pode confundir o modelo e resultar em saídas inconsistentes ou imprevisíveis. O objetivo é que o modelo aprenda e reproduza um único estilo com precisão.
    
3.  Foque na Coesão: Selecione imagens que estejam intimamente alinhadas com o estilo escolhido ao longo do seu dataset. Isso proporcionará uma experiência de aprendizado coesa para o modelo, permitindo que ele capture as características únicas e nuances específicas daquele estilo particular.
    

Sendo atento a essas considerações e garantindo uma seleção clara e consistente de estilo, seu modelo focado em estilo estará melhor equipado para aprender e gerar saídas que se alinhem com sua visão artística desejada.

**_<ins>Exemplo de diferentes estilos de arte:</ins>_**

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/eea6b79d-e50e-471e-978a-59bcc4b00e84/width=525/eea6b79d-e50e-471e-978a-59bcc4b00e84.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/fa541d08-1977-4096-a893-6bdb57b6381c/width=525/fa541d08-1977-4096-a893-6bdb57b6381c.jpeg)

**Ampliação de Imagem por IA**

Se você estiver usando imagens de baixa qualidade, recomendo ampliá-las. Para mais informações sobre como eu amplio, leia este artigo -> [**_<ins>Esta coisa fará seu modelo melhor!</ins>_**](https://civitai.com/articles/2727) Lembre-se de que nem todas as imagens podem ser ampliadas com qualidade perfeita e este método pode não funcionar para você.  

### **Regra Geral**

Determinar o número ideal de imagens para um dataset pode variar dependendo do projeto específico e dos resultados desejados. No entanto, como regra geral, considere as seguintes recomendações:

1.  **Personagem:** Mire em aproximadamente 50 ou mais imagens para modelos focados em personagens. Esse número fornece uma base razoável para o modelo aprender e reconhecer as características e atributos distintivos do personagem.
    
2.  **Estilo:** Para modelos baseados em estilo, recomenda-se ter cerca de 100 ou mais imagens. Esse dataset maior ajuda o modelo a entender as nuances do estilo, permitindo que ele gere saídas consistentes com o estilo artístico ou de design desejado.
    
3.  **Outros Projetos:** Enquanto a quantidade pode variar dependendo do projeto específico, um mínimo de 50 ou mais imagens é um ponto de partida razoável para modelos não baseados em personagens ou projetos. No entanto, observe que esses números podem ser ajustados com base nas suas necessidades específicas e na complexidade da tarefa em questão.
    

É importante enfatizar que a qualidade nunca deve ser comprometida pela quantidade. Se você tem um número menor de imagens de alta qualidade que capturam efetivamente os traços desejados, isso ainda pode levar a um treinamento e geração de modelo bem-sucedidos. Priorize a seleção de imagens que sejam representativas, diversas e de alta qualidade para garantir resultados ótimos.

## **Conclusão**

Espero que este guia forneça uma visão clara e compreensível sobre como reunir imagens para um dataset. Se você tiver alguma dúvida ou conselhos adicionais que possam aprimorar ainda mais este guia, sinta-se à vontade para compartilhá-los nos comentários. Seu feedback não apenas ajudará na atualização e melhoria deste guia, mas também beneficiará outros que o encontrarem. Obrigado por ler e espero que você tenha compreendido tudo neste artigo.

-Guy ❤️

## **Changelog**

```
v2.62 - Editei algumas coisas porque algumas delas estão desatualizadas
v2.61 - Adicionei um novo site
v2.6 - Adicionei um novo site, sublinhei os links e também mudei informações sobre ampliação
v2.5 - Mudei algumas seções, adicionei novo visualizador de fotos e novos sites que podem ajudar.
v2.0 - Adicionei (Melhor + Método Simples) Ferramentas de Coleta de Dados Rápidas e Fáceis
     - Adicionei links para guias que ajudam tanto no treinamento quanto no teste de modelos
     - Reescrevi algumas seções para torná-las mais claras
V1.1 - Adicionei nova seção para Ampliação de Imagens por IA
V1.0 - Lançamento Original
```