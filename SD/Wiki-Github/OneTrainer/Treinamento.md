### Seções
Observe que há uma dica de ferramenta disponível para cada parâmetro.

![treinamento](https://github.com/Nerogar/OneTrainer/assets/129741936/972e8598-38f5-4eb4-b6e4-ffac0cfd03ae)

## Informações do Otimizador
Informações adicionais podem ser encontradas sobre otimizadores [nesta página](./Otimizadores.md)

## Treinar Codificador de Texto (1 e 2)
A taxa de aprendizado do codificador de texto substitui a taxa base se configurada.

SDXL inclui 2 codificadores de texto (TENC1 - CLIP-ViT/L e TENC2 - OpenCLIP-ViT/G). Sugeriu-se que o TENC1 funciona melhor com tags e o TENC2 funciona melhor com linguagem natural, mas isso não é comprovado e é mais baseado em observação e sensação durante os testes. Determinar a melhor maneira de fazer os codificadores de texto atuarem em conjunto para obter o resultado desejado é um dos maiores desafios com o ajuste fino do SDXL. A maioria das histórias de sucesso teve acesso a hardware de nível comercial, e não de consumo. [Para referência, aqui estão as informações de hardware dos modelos CLIP originais.](https://github.com/Nerogar/OneTrainer/assets/132208482/8e5ecdc2-fc63-47ab-9c74-691902fe681b).

## Treinar UNet
A taxa de aprendizado do codificador unet substitui a taxa base se configurada.

[Codificadores de Texto e Unet](https://rentry.org/59xed3#text-encoder-learning-rate)

## Treinamento com Máscara
Com o treinamento com máscara, você pode instruir o modelo a aprender apenas de algumas partes das suas imagens de treinamento. Por exemplo, se você quiser treinar um sujeito, mas não o fundo, essa configuração ajudará. Para habilitar o treinamento com máscara, você precisa adicionar uma máscara para cada imagem de treinamento. Essa máscara é um arquivo de imagem em preto e branco, onde as regiões brancas definem o que deve ser incluído, e as regiões pretas são excluídas. Os arquivos precisam ter o mesmo nome das imagens, com uma extensão "-masklabel.png" adicionada. As máscaras devem estar no formato png. Essas máscaras podem ser criadas na seção de ferramentas, tanto automaticamente quanto usando uma função de pintura. Ao fazer o treinamento com máscara, o OneTrainer não verá nada onde a máscara está.

As opções disponíveis para o treinamento com máscara são:
_Probabilidade sem máscara_: o número de passos que serão executados sem a máscara. Deve ser considerado como uma porcentagem. Valor: 0 a 1. Padrão: 0.1

_Peso sem máscara_: o valor da perda ponderada durante o treinamento sem máscara da área onde a máscara está. Deve ser considerado como uma porcentagem. Valor: 0 a 1. Padrão: 0.1

_Normalizar Perda de Área com Máscara_: um botão de alternância (ligado/desligado), deve ser usado quando a área mascarada é muito grande (exemplo: joias). Para máscaras de tamanho menor, isso aumentará a perda suave, o que provavelmente não é desejado. Por exemplo, execuções com isso ativado fizeram a perda suave passar de 0.06 para 0.12 com máscaras que são menos de 50% da imagem.

## Configurações Principais
As configurações mais importantes são a Taxa de Aprendizado, Scheduler e Otimizador.
Depois vêm as épocas, tamanho do lote e passos de acumulação.

Finalmente, a resolução de treinamento definida por padrão dependendo do modelo base. Você pode treinar múltiplas resoluções ao mesmo tempo especificando uma lista de números separados por vírgulas no campo de resolução. Cada passo treinará em uma resolução selecionada aleatoriamente da lista. Observou-se que o treinamento em múltiplas resoluções pode melhorar muito a qualidade do modelo.

Exemplo de valores de múltiplas resoluções (a ideia é aumentar e diminuir em 64px):
* SD1.5: 384,448,512,576,640
* SDXL: 896,960,1024,1088,1152

Para mais informações, veja também [Treinamento Multi-Resolução](https://github.com/Nerogar/OneTrainer/wiki/Lessons-Learnt-and-Tutorials#multi-resolution-training) e, opcionalmente, use a [substituição de resolução](https://github.com/Nerogar/OneTrainer/wiki/Concepts#image-augmentation-tab) em Conceitos.

### Checkpoint de Gradiente
Reduz o uso de memória, mas aumenta o tempo de treinamento.

### Otimizador
Esta seção está em andamento, você pode fazer sugestões para melhorá-la na [discussão do Discord](https://discord.com/channels/1102003518203756564/1144311654385983538).
Um otimizador adaptará ligeiramente a taxa de aprendizado em tempo de execução. Eles vêm com suas configurações padrão, clicando nos 3 pontos à direita, você pode ajustá-los.
* ADAMW: Um dos mais usados.
* PRODIGY: Um especial que determina ele mesmo a LR apropriada. Ao usá-lo, defina uma Taxa de Aprendizado para 1 em todos os lugares e use o scheduler Cosine ou Constant.

Use um tamanho de lote pequeno com ele (cerca de 4), funciona bem com conjuntos de dados grandes (>30 imagens ou ainda mais).

Note também que as amostras mudarão após 40-80 épocas, não antes, então seja paciente.
* ADAFACTOR: [Artigo: Taxas de Aprendizado Adaptativas com Custo de Memória Sublinear](https://arxiv.org/abs/1804.04235). Este otimizador ajusta internamente a taxa de aprendizado dependendo das opções scale_parameter, relative_step e warmup_init.
Com um scheduler constante, LR 1e-4, 1e-5, 3e-05... testado com lotes de 1 a 5, aumentar os passos de acumulação não influenciou muito.

### Scheduler de Taxa de Aprendizado
O método de scheduler calculará a progressão de aprendizado com base no valor inicial da taxa de aprendizado (definido no campo Taxa de Aprendizado).

Nota: existem vários campos de taxa de aprendizado além da taxa de aprendizado base (simplesmente chamada de Taxa de Aprendizado). Estes são para o(s) codificador(es) de texto, Unet e embeddings (para embeddings adicionais), se estiverem vazios, usarão a LR base, se configurados, substituirão o valor da LR base.

* Constante: taxa de aprendizado fixa.
* Linear: decaimento linear da taxa de aprendizado da taxa de aprendizado inicial para 0.
![linear](https://github.com/Nerogar/OneTrainer/assets/129741936/95237662-3f87-4383-b7f7-850a4da54e76)

* Cosseno

Este scheduler decai muito rápido, melhor usar com conjunto de dados grande e lote pequeno ou aumentar as épocas.

![cosine](https://github.com/Nerogar/OneTrainer/assets/129741936/1dbcd622-5964-4421-b9e8-4270554d8ed6)

* Cosseno com reinícios

* Cosseno com reinícios duros

* REX: decaimento reverso exponencial da taxa de aprendizado começando da taxa de aprendizado inicial e terminando em 0. Não configure nenhuma etapa de aquecimento da taxa de aprendizado ao usá-lo.

![REX](https://github.com/Nerogar/OneTrainer/assets/129741936/6c6d1ed9-4983-4dd4-aaea-44ae64a279ff)

[Exemplo de uso do Rex](https://github.com/Nerogar/OneTrainer/wiki/Lessons-Learnt-and-Tutorials#rex-scheduler-usage-by-hypopo) Este exemplo dá algumas dicas para usar o REX para treinamento rápido, mas você pode usar este scheduler como qualquer outro scheduler decaindo (linear ou cosseno).

Informações sobre Schedule Personalizado podem ser encontradas aqui: https://github.com/Nerogar/OneTrainer/wiki/Custom-Scheduler

### Tipo de Dados de Treinamento
Internamente, isso define o tipo de dados de precisão mista ao fazer a passagem direta pelo modelo. Essa configuração troca precisão por velocidade durante o treinamento. Nem todos os tipos de dados são suportados em todas as GPUs. Na prática, float16 reduz apenas ligeiramente a qualidade, enquanto proporciona um aumento significativo na velocidade. Mas para a melhor qualidade, use float32 (reduz a velocidade).

### Épocas / lote / passos de acumulação

* Épocas: uma época é um ciclo onde todas as suas imagens são treinadas. Dependendo do tamanho do lote e do conjunto de dados, pode usar uma ou mais iterações.
* Tamanho do lote: número de imagens enviadas para a GPU para processamento.
* Passos de acumulação: multiplicador do tamanho do lote. Ex: se você quer um lote de 16 mas está limitado a um lote de 4, defina o passo de acumulação para 4.

### EMA (Média Móvel Exponencial)
Uma média móvel é uma ferramenta estatística para determinar a direção de uma tendência. A Média Móvel Exponencial é um tipo de Média Móvel, que aplica mais peso aos pontos de dados mais recentes do que aos que ocorreram no passado.

Útil apenas para conjuntos de dados maiores com múltiplos conceitos, pois a EMA reduzirá a diversidade, mais EMA é menos diversidade.
Se o seu conjunto de dados não for muito complexo com grande variação, então deixe DESLIGADO. Se você não conseguir bons resultados com EMA DESLIGADO, então tente ativá-lo. Para conjuntos de dados de centenas ou milhares de imagens, configure a Decaimento EMA para 0.9999. Para conjuntos de dados menores, configure para 0.999 ou até mesmo 0.998.

## Rodapé
Exibe a progressão do treinamento, épocas e passos usados por época.