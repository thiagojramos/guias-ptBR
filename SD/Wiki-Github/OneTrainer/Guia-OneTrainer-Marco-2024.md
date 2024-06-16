Esta página está em desenvolvimento

# Prefácio
Este guia tentará ajudar você a entender o básico do programa OneTrainer. Como configurar seu treinamento usando a GUI, de uma maneira semilógica (também conhecida como 'o que eu faço').

Este guia não cobre:
* Configuração de um runpod ou outra instalação remota. Como esses serviços não são gratuitos, a expectativa de suporte ao tentar fazer as coisas funcionarem neles não é algo que qualquer voluntário gostaria de lidar.
* Como utilizar a CLI (interface de linha de comando). É melhor (quase obrigatório) usar a GUI para fazer as configurações para executar o OneTrainer com a CLI.

Esta área (modelos de difusão) está continuamente se atualizando e mudando, então este guia pode estar desatualizado antes mesmo de ser publicado na web.
Este guia também considera o uso de um hardware Nvidia e uma instalação do Windows, WSL ou Linux, que são os sistemas com os quais estou familiarizado.
Os Hardwares AMD/Intel/Apple podem ser um desafio de usar e a experiência ainda é mínima para aprender, fazendo com que trabalhar com isso sem um conhecimento profundo e o desejo de mexer e ajustar configurações pode não ser possível.

Este guia é mais uma lista de verificação, com uma tentativa de dar orientações da melhor forma possível. Se você tiver perguntas específicas, por favor, entre no servidor do Discord. O servidor nunca será capaz de responder a todas as perguntas de todos, mas há pessoas conhecedoras nele que podem ser capazes de ajudar se estiverem disponíveis.

# Os Modelos
O OneTrainer atualmente funciona com os seguintes modelos
* Stable Diffusion 1.5
* Stable Diffusion 2 e 2.1
* Stable Diffusion XL
* Würstchen 2
* Stable Cascade (Würstchen 3)
* Pixart Alpha

Na minha experiência, a maioria das pessoas foca seus esforços no SD 1.5 e SDXL. As pessoas tentam treinar o SD2.x ocasionalmente. O Stable Cascade é observado por pessoas com interesses de pesquisa, pois possui certas vantagens para ajustes finos nesse aspecto.

Modelos derivados devem ser capazes de serem treinados com configurações do modelo base. Por exemplo, SDXL Lightning e Pony devem ser capazes de serem treinados usando a configuração do SDXL. Isso não quer dizer que um LoRA destinado ao Pony funcionará quando treinado a partir do base, mas que as configurações do SDXL com o modelo base sendo Pony devem ser uma solução viável.

## Observações
* O Stable Cascade atualmente tem desafios inerentes como um modelo apenas para pesquisa. Isso significa que a distribuição e geração é difícil. Isso pode relegá-lo a fluxos de trabalho personalizados do ComfyUI. Note que o pacote padrão StableSwarm para SC difere da distribuição original.
* Pixart Alpha teve grandes expectativas no lançamento, mas o uso do T5 faz com que seja um modelo treinável apenas com unet para GPUs de consumidores.
* SD2.x usa treinamento de predição v, que tinha expectativas mas não conseguiu entregar. As pessoas ainda tentam tirar algo mais deste modelo, já que os requisitos de recursos não são grandes.

# Tipos de Treinamento
Atualmente, há 4, em breve 5 tipos de treinamento feitos no OneTrainer:

* Inversão Textual (abreviado para apenas Embedding). O modelo inteiro é carregado (requisito de Vram), mas os cálculos são rápidos enquanto o programa tenta encontrar os tokens que farão seu conceito de treinamento. Embeddings são arquivos pequenos, pois são apenas tokens.
* LoRA (Adaptação de Baixa Classificação de Grandes Modelos de Linguagem). Partes ou o modelo inteiro podem ser carregados (Requisito de Vram variável), para criar camadas que se sobrepõem ao seu modelo base de difusão. LoRAs podem treinar unet, codificadores de texto ou ambos. Como os LoRAs se sobrepõem ao modelo de difusão, é quase impossível que o LoRA não se espalhe para outras partes do modelo base que não eram pretendidas.
* Ajuste Fino. Partes ou o modelo inteiro podem ser carregados (Requisito de Vram variável) e diretamente afetados pelo treinamento. Para evitar destruir o modelo base, é importante minimizar ao máximo as atualizações de gradientes. Isso requer grandes tamanhos de lote ou tamanhos de lote virtual e, portanto, Ajustes Finos requerem o melhor hardware disponível para fazer um ajuste fino significativo em um tempo que não se estenda para semanas ou meses.
* Treinamento InPaint. O mesmo que Ajuste Fino, mas destinado a um fluxo de trabalho de inpainting. Eu não tenho experiência com isso, então não posso compartilhar muitos detalhes.
* Trabalho em Progresso - EM DESENVOLVIMENTO - Embedding Universal. Uma técnica para incorporar tokens especiais. Em desenvolvimento e mais detalhes a seguir.

## Resumindo os Treinamentos
* Embedding - Encontrando os tokens que resolvem a solução para seu(s) conceito(s). Arquivos pequenos, cálculos rápidos, mas pode ser difícil encontrar a solução certa dependendo do seu conceito.
* LoRA - Construindo um novo modelo em cima de um modelo existente. Geralmente a melhor opção para distribuição. Tamanho variável, dependendo da classificação da rede. LoRAs podem ser mesclados em checkpoints.
* Ajuste Fino - Ajustando ou expandindo um modelo existente diretamente. Geralmente um processo muito lento para não destruir o modelo existente. Construir o conjunto de dados para um ajuste fino versátil e poderoso pode levar muito tempo, mas uma vez construído, este conjunto de dados pode então ser usado para treinar futuros modelos à medida que a tecnologia muda. O que os grandes players podem fazer com quantidade e hardware comercial pode ser feito com um trabalho melhor e focado em alta qualidade.

# Fluxo de Trabalho Geral do OneTrainer
Pode ser bom pensar em configurar sua execução na seguinte ordem.

## Preparando o ambiente
1. Determine o modelo e o treinamento que você quer fazer e carregue o template
2. Determine o modelo a ser usado (base do huggingface ou checkpoint diferente?)
3. Configure seu ambiente de trabalho (pastas)

## Preparando o treinamento
1. Configure seu(s) conceito(s)
2. Configure sua(s) amostra(s)
3. Configure sua frequência de salvamento e backup, se desejado
4. Configure o local de salvamento do modelo final

## Decidindo os detalhes do modelo e do treinamento
1. Determine os pesos do modelo, com base no seu orçamento de Vram e suporte de hardware.
2. Determine o tipo de dados de treinamento, com base no seu orçamento de Vram e suporte de hardware.
3. Determine a resolução do seu treinamento, idealmente com base no modelo com o qual você está trabalhando. (por exemplo, 1024 para SDXL, 512 para SD1.5)
4. Determine o tamanho do lote, com base no número de conceitos e orçamento de Vram e suporte de hardware
* Seu lote deve ser divisível uniformemente pelas imagens usadas em uma época. Isso não é um requisito rígido, mas ao mesmo tempo você não sabe quais imagens serão descartadas se não for.
* Você deve ter imagens suficientes em um bucket de proporção de aspecto para preencher um lote. Se não tiver, esse bucket nunca será treinado. O OneTrainer atualmente não exibe essa informação, e o único feedback que você receberá sobre isso é se os passos x lote não forem iguais às suas imagens de conceito.

### Observações
* Placas GTX 1000 e mais antigas não suportam pesos ou cálculos BF16.
* Placas RTX 3000 e mais novas são necessárias para fazer cálculos TF32. Nota: Verifique o desempenho TF32 vs FP32.
* Placas RTX 4000 e (presumivelmente) mais novas suportam FP8. Placas mais antigas podem se beneficiar da economia de memória, mas não do aumento de velocidade.

### Pesos do Modelo FP32 - Aproximadamente.
* Stable Diffusion v1.5 (512px) - Unet: 3.44GB, TENC: .48GB, VAE: .32GB, Total: 4.27GB
* Stable Diffusion v2 (512px) - Unet: 3.46GB, TENC: 1.36GB, VAE: .32GB, Total: 5.21GB
* Stable Diffusion v2 (768px) - Unet: 3.46GB, TENC: 1.36GB, VAE: .32GB, Total: 5.24GB
* Stable Diffusion XL (1024px) - Unet: 10.3GB, TENC1: .48GB, TENC2: 2.78GB, VAE: .32GB, Total: 13.88GB. Nota: SDXL é quase sempre distribuído em pesos de modelo fp16 de 6.94GB. Até mesmo o Huggingface só tem safetensors combinados fp16.
* Stable Cascade (1024px, Stage C - 3.6 bilhões de parâmetros) - Stage C: 14.4GB, TENC: 1.39GB, Total: 15.69GB
* Stable Cascade (1024px, Stage C - 1 bilhão de parâmetros, denotado como 'lite') - Stage C: 4.2GB, TENC: 1.39GB, Total: 5.59GB
* Nota, o TENC do Stable Cascade parece já estar em peso BF16.
* fp16 ou bf16 cortará os pesos necessários na Vram pela metade. fp8 cortará o tamanho do arquivo para um quarto do original.

## Decida seu otimizador e especificidades de treinamento
1. Escolha seu otimizador, com base no seu tipo de treinamento e orçamento de Vram
2. Escolha seu agendador de aprendizado, com base no seu otimizador e tipo de treinamento
3. LoRA - Vá para a aba LoRA e determine sua classificação de rede, alpha e dropout
4. Embedding - Vá para a aba Embedding e determine seu(s) token(s)
5. Ajuste Fino - Não é necessário nenhuma aba ou configuração especial.

## Verifique as configurações do seu otimizador
1. Clique nos ... ao lado do seu otimizador e verifique as configurações habilitadas ou inseridas, lembrando que as últimas configurações usadas para cada otimizador virão por padrão.
2. Verifique [aqui](./Otimizadores.md) para mais informações sobre essas configurações.

## Decida sua taxa de aprendizado, Épocas, Aquecimento
1. Insira sua taxa de aprendizado principal para o treinamento, com base no seu tipo de treinamento e otimizador
2. Escolha o número de épocas, com base no seu otimizador, tipo de treinamento e agendador
3. Decida o número de passos de aquecimento, com base no seu otimizador e tipo de treinamento
4. Decida se deseja treinar o(s) Codificador(es) de Texto, por quanto tempo e as taxas de aprendizado
5. Decida se deseja treinar o Unet (ou anterior para SC), por quanto tempo e a taxa de aprendizado para ele.

