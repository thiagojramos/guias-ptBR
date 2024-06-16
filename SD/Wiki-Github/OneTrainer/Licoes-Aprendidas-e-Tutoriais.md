Esta seção contém várias informações coletadas do servidor Discord. As informações no topo foram fornecidas pelos desenvolvedores, enquanto outras (seguido de "por username") são experiências compartilhadas, então considere-as pelo que são, não como "um jeito de fazer" e podem conter informações enganosas. Se encontrar alguma, por favor, reporte-as na discussão do wiki do Discord.

### Problemas com Amostras
* Se suas imagens de amostra estão pretas, defina o tipo de dados Override VAE para bfloat16 ou float32 (aba Modelo).
* Para embeddings, se sua imagem de amostra é sempre a mesma, você provavelmente esqueceu de colocar `<embedding>` no prompt de amostra ou nas legendas para o seu conceito.

### Randomizando Legendas
* Você pode adicionar várias legendas. Uma por linha no arquivo txt. E ele escolherá uma aleatoriamente para cada época.

### Treinamento Multi-Resolução
* É um novo recurso que pode aumentar a qualidade do seu treinamento. Testado com um LoRA para SDXL e um pequeno conjunto de dados (30 imagens). Você pode treinar múltiplas resoluções ao mesmo tempo especificando uma lista de números separados por vírgulas no campo de resolução. Cada etapa treinará em uma resolução selecionada aleatoriamente da lista. Foi notado que o treinamento multi-resolução pode melhorar muito a qualidade do modelo.
* Exemplo: Defina a resolução de treinamento para 896,960,1024,1088,1152 (para cima e para baixo por 64px) e adicione 5 conceitos idênticos prontos para pelo menos a resolução 1152 com substituição de resolução configurada respectivamente para 896,960,1024,1088,1152, dividindo suas épocas necessárias por 5, pois usará 5 vezes mais etapas.
* Opcionalmente, use a [substituição de resolução](https://github.com/Nerogar/OneTrainer/wiki/Concepts#image-augmentation-tab) em Conceitos.


### Diagnóstico Diferencial de Resultados de Imagem, por Alaiya @ OnePawProductions:
![Tipos de Erro de IA](https://camo.githubusercontent.com/2a69f8de8a0608ceeefd4bfecda70c8ecfbbf2a7f61de071ca407d862cd0ae59/68747470733a2f2f696d616765732d7769786d702d6564333061383662386334636138383737373335393463322e7769786d702e636f6d2f662f33363361323530342d346531302d343331352d396430652d3638636235363334376665612f6467706c7365622d30303033386562612d366339652d346331392d396337662d3162623265353764653036302e706e672f76312f6669742f775f3832382c685f3439362c715f37302c737472702f61695f6572726f72735f696e5f7364786c5f6c6f72615f747261696e696e675f5f7573696e675f6f6e65747261696e65725f62795f6f6e6570617770726f64756374696f6e735f6467706c7365622d343134772d32782e6a70673f746f6b656e3d65794a30655841694f694a4b563151694c434a68624763694f694a49557a49314e694a392e65794a7a645749694f694a31636d3436595842774f6a646c4d4751784f4467354f4449794e6a517a4e7a4e684e5759775a4451784e5756684d4751794e6d55774969776961584e7a496a6f6964584a754f6d467763446f335a54426b4d5467344f5467794d6a59304d7a637a5954566d4d4751304d54566c5954426b4d6a5a6c4d434973496d39696169493657317437496d686c6157646f64434936496a77394e7a5934496977696347463061434936496c77765a6c77764d7a597a595449314d4451744e4755784d4330304d7a45314c546c6b4d4755744e6a686a596a55324d7a51335a6d56685843396b5a334273633256694c5441774d444d345a574a684c545a6a4f5755744e474d784f533035597a646d4c544669596a4a6c4e54646b5a5441324d433577626d63694c434a336157523061434936496a77394d5449344d434a3958563073496d46315a43493657794a31636d343663325679646d6c6a5a5470706257466e5a53357663475679595852706232357a496c31392e34436e485748545f6e4f6e4e56627238416c7265416c336b6f492d667a474f594d6f5f6e4e4e5049755a67)

Aqui vemos três exemplos diferentes de imagens ruidosas geradas durante o processo de amostragem.

Na primeira imagem, a saída é totalmente ruidosa, como se pode perceber pela aleatoriedade e alto grau de contraste colorido. Isso indica que a resolução não está correspondendo. Se o LORA que está sendo treinado é SDXL (uma resolução de 1024x1024) e você define a saída de imagem de amostragem para 512x512, não será capaz de gerar imagens coerentes.

Na segunda imagem, vemos algo coeso, mas ruidoso. Há gradientes, cores tanto suaves quanto de alto contraste, e funciona como uma imagem, embora ruidosa. Não há formas realmente identificáveis. Isso pode acontecer quando o identificador único não é parseável para a IA, como "lkjasdf", ou "myGreatOC", ou, neste caso, "Muse". À medida que a IA analisa seu conjunto de dados e aprende a associar as imagens e tags com o identificador único, deve convergir (resolver-se em padrões identificáveis e replicáveis relacionados ao seu conjunto de dados) entre as etapas 2-50, tornando-se cada vez mais claro.

Na última imagem, vemos uma imagem identificável, embora altamente incorreta e não relacionada ao conjunto de dados. Isso pode acontecer quando o identificador único usado carrega consigo outros conceitos que a IA pensa estarem associados a essas palavras/essa palavra. O identificador único pode ser algo como "Hannah Bearlet", ou "myElegantCharacterBearnois", ou algo semelhante. Esta imagem veio de "Taylor Hebert", e a convergência aconteceu muito cedo, no Época 2, quando a IA percebeu que Taylor Hebert não é, de fato, um urso.

![IA Não Reconhecendo Dados](https://camo.githubusercontent.com/2b1cf328b56d264a55e9de8b8b6ea3e5d892fbecaad9b1e9b311f8ff559b73cf/68747470733a2f2f696d616765732d7769786d702d6564333061383662386334636138383737373335393463322e7769786d702e636f6d2f662f33363361323530342d346531302d343331352d396430652d3638636235363334376665612f6467706c6962332d33323138386331392d653932622d343930312d626134342d3764303631626530306463342e706e672f76312f6669742f775f3832382c685f3337322c715f37302c737472702f6f6e65747261696e65725f74726f75626c6573686f6f74696e675f62795f6f6e6570617770726f64756374696f6e735f6467706c6962332d343134772d32782e6a70673f746f6b656e3d65794a30655841694f694a4b563151694c434a68624763694f694a49557a49314e694a392e65794a7a645749694f694a31636d3436595842774f6a646c4d4751784f4467354f4449794e6a517a4e7a4e684e5759775a4451784e5756684d4751794e6d55774969776961584e7a496a6f6964584a754f6d467763446f335a54426b4d5467344f5467794d6a59304d7a637a5954566d4d4751304d54566c5954426b4d6a5a6c4d434973496d39696169493657317437496d686c6157646f64434936496a77394e546332496977696347463061434936496c77765a6c77764d7a597a595449314d4451744e4755784d4330304d7a45314c546c6b4d4755744e6a686a596a55324d7a51335a6d56685843396b5a3342736157497a4c544d794d546734597a45354c5755354d6d49744e446b774d533169595451304c54646b4d445978596d55774d47526a4e433577626d63694c434a336157523061434936496a77394d5449344d434a3958563073496d46315a43493657794a31636d343663325679646d6c6a5a5470706257466e5a53357663475679595852706232357a496c31392e583870342d735851534f6f6f6d4d5052745351376832773372564a574b6444427061685434516a4b38446f)

Neste treinamento de 100 épocas, vemos que a IA não está reconhecendo e conectando o conjunto de dados ao treinamento e saída de imagem. A primeira imagem é puro ruído, e por ser SDXL, tentou incluir algum texto. Mas mesmo com o progresso das épocas, as imagens permanecem ruidosas, apesar do aumento na clareza da resolução.

Isso pode ser remediado levando em consideração como o OneTrainer lê seus dados. Em OneTrainer > Concepts > General, você encontrará:
* de arquivos de texto por amostra
* de um único arquivo de texto
* do nome do arquivo de imagem 

De Arquivos de Texto por Amostra: isso lerá os dados de um arquivo .txt que compartilha o mesmo nome que sua imagem, como image1.png, image1.txt. Isso permite que você tenha uma descrição mais longa e mais tags associadas à sua imagem do que usando o nome do arquivo de imagem, que (pelo menos no Windows) tem um limite de tamanho para o título da imagem.

De um Único Arquivo de Texto: isso lerá um único arquivo de texto e aplicará a todas as imagens, semelhante (acredito) ao processo de uma incorporação de inversão textural.

Do Nome do Arquivo de Imagem: isso lerá os dados do nome do arquivo de imagem, o que pode ser uma medida útil para economizar trabalho, mas observe que os nomes dos arquivos de imagem geralmente têm um limite de tamanho. Se você exceder, pode corromper seu arquivo (aconteceu comigo).

![IA Funcionando Bem](https://camo.githubusercontent.com/81e31157e066035a81490fe3622c2555c9824b07f0dacb3c79e3b28b2bc1d680/68747470733a2f2f696d616765732d7769786d702d6564333061383662386334636138383737373335393463322e7769786d702e636f6d2f662f33363361323530342d346531302d343331352d396430652d3638636235363334376665612f6467706c6d39642d64643937653435652d363064642d343739392d616236662d3932363865356433646131362e706e672f76312f6669742f775f3832382c685f3530362c715f37302c737472702f6f6e65747261696e65725f74726f75626c6573686f6f74696e675f5f61695f66756e6374696f6e696e675f77656c6c5f62795f6f6e6570617770726f64756374696f6e735f6467706c6d39642d343134772d32782e6a70673f746f6b656e3d65794a30655841694f694a4b563151694c434a68624763694f694a49557a49314e694a392e65794a7a645749694f694a31636d3436595842774f6a646c4d4751784f4467354f4449794e6a517a4e7a4e684e5759775a4451784e5756684d4751794e6d55774969776961584e7a496a6f6964584a754f6d467763446f335a54426b4d5467344f5467794d6a59304d7a637a5954566d4d4751304d54566c5954426b4d6a5a6c4d434973496d39696169493657317437496d686c6157646f64434936496a77394e7a6778496977696347463061434936496c77765a6c77764d7a597a595449314d4451744e4755784d4330304d7a45314c546c6b4d4755744e6a686a596a55324d7a51335a6d56685843396b5a33427362546c6b4c57526b4f54646c4e44566c4c5459775a4751744e4463354f533168596a5a6d4c546b794e6a686c4e57517a5a4745784e693577626d63694c434a336157523061434936496a77394d5449344d434a3958563073496d46315a43493657794a31636d343663325679646d6c6a5a5470706257466e5a53357663475679595852706232357a496c31392e52793243684653616c78745a7752527036324f4135744e4971556b49656d6f44564e7247455f7947644e59)

Aqui vemos o que se pode esperar quando o processo dá certo. Meu texto de amostragem "Muse jumping" inicialmente foi renderizado pela IA nos primeiros estágios (menos de 10) como a banda de rock Muse, pulando. Nos estágios intermediários das épocas (10-50 épocas) vemos as figuras começarem a desenvolver características animais, já que o conjunto de dados é composto por fotos de Muse, a Mascote Pink Calico. Em épocas posteriores, vemos que a convergência aconteceu, e a IA agora reconhece a tag "Muse" (ou, meuMuse) como pertencente às imagens do conjunto de dados. Vemos a sombra, o fundo cinza, o estilo e as características que foram etiquetadas no conjunto de dados.


### Notas sobre Acumulação de Gradiente e Tamanho do Lote, por Alaiya @ OnePawProductions

A Acumulação de Gradiente e o Tamanho do Lote (ou seja, quantas imagens são referenciadas durante o treinamento, e depois durante o uso do LoRA) podem ter um grande impacto na saída do LoRA, tanto de forma positiva quanto negativa. Você pode ver esse padrão através do uso do seu LoRA ao longo do tempo quando, devido à acumulação de gradiente de 1 (imagem que é referenciada é uma única imagem, não muitas imagens), algumas das imagens são muito claramente geradas a partir de uma imagem em particular ou de outra.

Acumulação de Gradiente e Tamanho do Lote de "1" têm aspectos positivos e negativos:

Positivo: mais diferenciação no conjunto de dados entre conceitos relacionados, ou seja, estou fazendo um LoRA para Taylor Hebert...mas qual Taylor? Linda!Taylor? Canon!Taylor? Tinker Taylor, ou a variante de uma fanfic específica? Com uma acumulação de gradiente de 1, cada vez que executo uma sessão de geração (usando a Extensão de Prompt Combinatorial, que recomendo muito para diversificação do conjunto de dados) eu obtenho uma infinidade de imagens diferentes geradas, cada uma a partir de uma única imagem. Estas vão para diferentes pastas, para cada uma das diferentes variantes, para que meu LoRA para Taylor possa focar na flexibilidade. É uma grande parte de como estou projetando todos os conjuntos de dados de entrada para LoRA. Esperançosamente, evitando o colapso do modelo. Então, imagens geradas usando um GA de 1 podem produzir variações na variante Linda, variante Canon/Perfeita, variante Tinker, ou variante Skitter, e usando a Extensão de Prompt Dinâmico Combinatorial, essas variantes podem ser feitas para fazer uma variedade de coisas diferentes, vestindo roupas diferentes, em diferentes cenários, com diferentes expressões faciais e corporais.

Negativos: Se você referenciar, em seus prompts, tags que derivam de várias imagens, mas usando um LoRA treinado com um GA de 1, pode acabar com coisas malucas como gêmeos de duas cabeças (tanto quanto eu posso dizer, um resultado infeliz do uso da tática de diversificação de dados "cópia invertida". Acho que a cópia invertida é útil, no entanto, especialmente em casos onde a simetria bilateral não importa). Também pode dificultar a criação de um "look" coeso, ou seja, fidelidade facial completa é um pouco mais difícil de alcançar. Usando um GA mais alto, você referencia várias imagens para cada saída, o que pode ajudar a resolver a fidelidade facial em um todo mais coeso.

### Sugestões de Treinamento LoRA com Prodigy por Malessar
* Use o mesmo valor para o Rank do Lora e alpha. 8/8 parece bom para uma pessoa, 16/16 pode ser melhor, mas menos flexível a menos que com uma legendagem de imagem muito boa. Note que o alpha precisa ser igual ou menor que o rank.
* Use o otimizador Prodigy com correção de viés ativada e um decaimento de peso de 0,01 (o padrão é 0,1 e também pode funcionar).
* Scheduler Cosine
* Use uma taxa de aprendizado de 1 para TE e Unet, Nota: o TE é opcional e difícil de treinar para SDXL, fica a seu critério usá-lo ou não, você pode tentar TE 1 ou 2, pois funcionam de maneira diferente.
* Tamanho do lote: o que seu Vram pode suportar, múltiplos de 2 parecem mais eficientes.
* Tipo de dado configurado para float 32 em todos os lugares (abas de treinamento e modelo) se seu Vram puder suportar. Mas os valores padrão que vêm com a predefinição funcionam bem.

### Uso do agendador REX por Hypopo
* Este agendador reduzirá a taxa de aprendizado (LR) do valor inicial (configurado no campo de taxa de aprendizado) para 0 seguindo uma curva exponencial inversa. Sua habilidade é treinar muito rápido. Eu testei combinado com otimizadores padrão como ADAMW ou CAME para **principalmente embedding** e alguns LoRA, também funciona bem com um pequeno conjunto de dados (6-20) para **embedding** novamente. LoRa gosta de mais imagens em geral, mesmo 20 podem ser suficientes para uma pessoa.

Treinar um embedding com ele no SD1.5 com 100 épocas e 15-20 imagens leva menos de 5 minutos (Vram de 16 GB), 20 minutos no SDXL para dar uma ideia.

![REX](https://github.com/Nerogar/OneTrainer/assets/129741936/6c6d1ed9-4983-4dd4-aaea-44ae64a279ff)

* Não use nenhum aquecimento com ele.
* Tente usar uma alta combinação de lote x AS (=< `#`imagens) com pequeno conjunto de dados. Reduza quando seu conjunto de dados for versátil. Note que se todas as suas imagens não estiverem na mesma proporção, lote x AS = `#`imagens falhará e algumas imagens serão descartadas. A ideia é otimizar o tempo de treinamento, um valor mais baixo exigirá mais épocas.
* O truque é encontrar o bom LR inicial, para **embeddings** começo com 0,0038 para ter uma primeira ideia e depois reduzo para encontrar o valor mágico, geralmente entre 0,004 e 0,002 pela minha experiência, sinta-se à vontade para tentar outros. Para LoRA, você precisará de um LR mais baixo. O bom LR depende do conjunto de dados, número de imagens e sua consistência. Quando o modelo estiver supertreinado, você pode tentar reduzi-lo ou as épocas.
* Não precisa de muitas épocas, pois treina muito rápido. Para embedding, ter muitas épocas não terá grande influência. Tive bons resultados com 100 épocas, às vezes um pouco mais. Para um LoRA, você precisará de um LR mais baixo e mais épocas.
* Deixe o treinamento ir até o fim, pois ele precisa seguir a curva completa para ser eficiente. Também pode não suportar ser continuado a partir de um último backup (não testei eu mesmo, mas alguns usuários conseguiram).
* Eu geralmente tento encontrar primeiro a boa combinação lote*AS, depois o bom LR e, finalmente, reduzir as épocas apenas ao necessário.

### Treinamento Cascade LORA Estável por Drift Johnson
[Treinamento Cascade LORA Estável (Youtube)](https://www.youtube.com/watch?v=dmSZ5TWEWTQ)

[Treinamento Cascade LORA Estável com Onetrainer (Civitai)](https://civitai.com/articles/4253/stable-cascade-lora-training-with-onetrainer)

### Problema Estranho com ZLUDA por Heasterian
* Se você está obtendo amostras non-ema com ruído a cada x época ao usar ZLUDA, verifique se a perda não é menor nessas épocas. Se for o caso, desligue o Autocast Cache. Parece que está causando esse problema.

### Tutorial em Vídeo para Refinamento com Baixo Vram por SECourses
* [Tutorial de 133 Min em Vídeo - Completo](./Tutorial-Completo-de-Ajuste-Fino-do-Stable-Diffusion-SD-&-XL-com-OneTrainer-no-Windows-e-na-Nuvem---De-Zero-a-Heroi.md)

### Observações sobre o ZLUDA
Zluda está sendo desenvolvido para permitir que placas gráficas AMD funcionem com CUDA. Atualmente, RoCm está disponível apenas no Linux, o que limita o uso de placas AMD no Windows.
O melhor fork para ZLUDA [está aqui](https://github.com/lshqqytiger/ZLUDA). Apesar do desenvolvedor original ter postado o seu trabalho, infelizmente não irá mais trabalhar nele) 
Não é necessário usar este pacote ZLUDA, o OneTrainer permitirá a opção de instalar o ZLUDA como parte do processo install.bat e ele será instalado em uma subpasta no OneTrainer.
As melhores informações sobre como fazer funcionar e notas sobre isso estão atualmente na [Wiki do SD.next](https://github.com/vladmandic/automatic/wiki/ZLUDA)
Algumas outras observações sobre o ZLUDA:
- Verifique seu dispositivo. (https://rocm.docs.amd.com/projects/install-on-windows/en/develop/reference/system-requirements.html) Se sua placa não tiver caixas marcadas em ambas as colunas, você precisará baixar as bibliotecas especiais para Rocmlibs encontradas no wiki do SD.next ou construí-las você mesmo.
- Certifique-se de ter os drivers AMD pro (opcional?) e o framework HIP instalado. Links estão no wiki do SD.next.
- Tenha paciência. O ZLUDA precisa compilar na primeira utilização. Nenhuma informação é exibida de que está fazendo isso, e pode levar um tempo considerável. Esta compilação é provavelmente para cada nova função CUDA que ocorre (a primeira vez).
- XFormers não funciona, então use padrão ou SDP ao treinar.
- ZLUDA é um wrapper CUDA, então use CUDA como dispositivo.
- Esteja ciente das limitações padrão do ZLUDA, especialmente com relação às gráficas AMD integradas.

### Configuração de Baixo Vram - por efhosci
Usando as configurações certas, é quase possível no OneTrainer ficar abaixo de 3 GB de uso de Vram durante o treinamento com SD 1.5, o que pode ser viável para GPUs de laptops mais antigos, GPUs de desktop de baixo custo ou GPUs "de mineração" limitadas em Vram. Provavelmente não será rápido em placas mais antigas, mas para um único conceito LoRA, você pode conseguir completar em menos de uma hora, em comparação a várias horas rodando na CPU. Disclaimer - Eu realmente não entendo o mecanismo subjacente da maioria dessas configurações, estou apenas observando como mudá-las afeta o uso de Vram. O sistema está rodando Linux Mint, a GPU é P100, o uso de Vram foi monitorado usando nvidia-smi. Informações estão atualizadas até maio de 2024.

Aqui estão as principais coisas que parecem ajudar, ou eu verifiquei que não têm efeito. Qualquer coisa não listada provavelmente não tem efeito - eu não ajustei todas as configurações, mas qualquer coisa que seja um multiplicador para a taxa de aprendizado ou força do ruído ou similar seria improvável ter um efeito no uso de memória. Aproximadamente na ordem por abas:
- Não importa se você usa um modelo "poda" ou "completo", pelo menos com as configurações de peso a seguir. Os podados devem ter cerca de 2 GB de tamanho de arquivo, em oposição aos checkpoints completos que têm cerca de 5-6 GB.
- Defina todos os pesos no "modelo" para float8 se possível, float16 caso contrário. O maior efeito parece ser com o UNet, a diferença entre defini-lo para float8/16 é de cerca de 0,8 GB. O codificador de texto é cerca de 100 MB e o VAE não tem diferença perceptível. Definir o tipo de dados de saída para float8 causou erros com amostragem e salvamento, então foi mantido em float16, e esteja ciente de que baixa precisão para todos os estágios pode potencialmente causar alguns problemas de qualidade na saída.
- Ligar o cache latente economiza cerca de 100 MB.
- Usar ADAMW_8BIT em vez de ADAMW economizou cerca de 50 MB, independentemente de os pesos do modelo estarem definidos para float16 ou float8. ADAFACTOR estava por volta do mesmo. Algumas das configurações específicas do otimizador podem afetar o uso de Vram, mas os padrões devem estar bem. Veja [aqui](./Otimizadores.md) para mais detalhes sobre isso.
- O tamanho do lote precisa ser definido em 1 - ir para 2 mais do que dobrou o uso de Vram (de 2,8 GB para quase 7 GB). O tamanho do lote 3 e 4 também estava em torno de 7 GB, indo para 8 era cerca de 8,3 GB, e em 32 parecia atingir o pico em 13,5, o que sugere cerca de 250 MB por imagem adicional em um lote (em resolução 512).
- Etapas de acumulação tiveram um efeito menor de 20 MB ou menos ao aumentar acima de 1. Aumentar isso deve ser funcionalmente semelhante a um tamanho de lote maior, mas significativamente mais lento. Comparando tamanho de lote 8 e etapas de acumulação de gradiente 1 com o inverso, o primeiro terminou mais de duas vezes mais rápido.
- Em comparação com a "atenção" padrão, xformers e SDP economizam cerca de 2,8 GB, cerca de metade do que usou de outra forma, então sempre habilite um dos dois.
- Ligar o checkpointing de gradiente também economiza quase 2 GB, mas reduziu a velocidade em pelo menos 30%.
- Definir o tipo de dado de treinamento como float16 em vez de float32 economiza cerca de 80 MB.
- O cache de autocast menciona uma diferença em velocidade e memória, mas eu não vi diferença entre ligado/desligado.
- Resolução de imagem em 768 (tanto no "treinamento" quanto no conceito ativo) vs em 512 em ambos foi uma diferença de cerca de 0,7 GB, então mantenha resoluções máximas baixas.
- AlignProp usa MUITO Vram, EMA teve pouco ou nenhum impacto. Nenhum deve ser necessário para habilitar para treinamento simples de LoRA ou embedding.
- Habilitar treinamento mascarado não aumentou muito o uso de Vram, talvez alguns MB, mas atualmente não funciona quando o cache latente está habilitado.
- Diferentes funções de peso de perda variaram apenas alguns MB.
- Durante a amostragem, com uma imagem de 512x512, o Vram atingiu o pico em cerca de 2,7 GB. Isso é menos do que se estabiliza durante o treinamento (cerca de 2,8 GB com todas as otimizações acima), então não se preocupe em desabilitar a amostragem para economizar Vram. Imagens de amostra 768x768 pareciam ser estáveis em torno de 2,8 GB, mas podem ter brevemente atingido pouco mais de 3 GB ao mudar para treinamento, então não aumente muito a resolução se estiver perto do limite.
- Sem pico significativo no uso de Vram durante o backup ou salvamento.
- Eu treinei em LoRA rank 16 para todos os testes anteriores - rank 32 era cerca de 60 MB maior, rank 64 era cerca de 200 MB maior, rank 8 era cerca de 40 MB menor, então é aproximadamente linear. Rank 16 deve ser suficiente para a maioria dos propósitos. Alpha e dropout não tiveram efeito no Vram.
- Mudar o tipo de dado do peso do LoRA de float32 para bfloat16 economizou talvez 30 MB de Vram, no entanto, estou usando uma placa mais antiga que não suporta, então o benefício pode ser maior se usar placas >3000 series - mas então você é menos provável de ter restrições pesadas de Vram.
- Treinar um embedding adicional junto com um LoRA adiciona cerca de 80 MB ao uso de Vram, maior contagem de tokens não tem nenhum efeito.

Com todas as otimizações acima, o programa atingiu o máximo em torno de 2,8 GB enquanto em execução. O treinamento foi a cerca de 1.1 it/s para mim, com 1000 etapas totais (mais algum tempo para amostragem), o tempo total de treinamento foi de cerca de 1100 segundos, ou 19 minutos. A placa que estou usando é listada com 19 TFLOPS fp16 e 9,5 FLOPS fp32 no TechPowerup, você pode tentar estimar o tempo de treinamento para outras GPUs com base nisso. Por exemplo, com uma GPU de laptop T1200 (7.3/3.6 fp16/fp32, 4 GB Vram) teoricamente levaria cerca de 2.5x mais tempo, cerca de 50 minutos, embora a adição de núcleos tensor para GPUs mais recentes signifique que podem ter um desempenho mais rápido na prática. Também será difícil estimar diferenças devido à velocidade de memória, cache e alguns outros fatores. Se uma placa tem um fp16 listado menor do que fp32, ou nenhum listado, você pode assumir para fins práticos que é igual ao fp32. GPUs Nvidia muito antigas (mais de 10 anos) podem não funcionar devido a problemas de suporte de software, e a AMD provavelmente é similar.

[Aqui está um exemplo de arquivo de configuração para treinamento com menos de 3 GB.](https://github.com/Nerogar/OneTrainer/files/15445637/lowVram-3GB.json) Configurações relacionadas à taxa de aprendizado e épocas podem precisar ser alteradas dependendo dos seus dados, mas eu consegui obter um LoRA de um único personagem razoavelmente decente usando isso.

Se você tiver 4 GB de Vram disponíveis, as configurações mais benéficas para alterar provavelmente seriam mudar os pesos do modelo para fp16, tipo de dado de treinamento para fp32, usar um otimizador não de 8 bits e talvez aumentar a resolução de treinamento para 640. Isso ficou pouco abaixo de 4 GB para mim. Com 6 GB, você provavelmente pode desligar o checkpointing de gradiente para o aumento de velocidade, ou você poderia tentar resoluções e precisões de float ainda mais altas.