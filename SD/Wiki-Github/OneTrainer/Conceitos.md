Atualização WIP

A aba Conceitos é onde você informa ao OneTrainer onde estão suas entradas. Conceitos podem ser seus dados de treinamento, dados de regularização ou quaisquer outros dados que você deseja treinar.

# Visão Geral da Interface da Aba Conceitos
![imagem](https://github.com/Nerogar/OneTrainer/assets/132208482/2d02d53d-3937-444e-9b6a-9c8930924f57)

A aba Conceitos é composta pelos seguintes elementos:
* Menu suspenso (padrão: conceitos)
Você pode configurar várias configurações para seus conceitos, e o menu suspenso é como você os seleciona. O OneTrainer só treina a partir da configuração selecionada atualmente.
* Adicionar configuração
Pressionar este botão abrirá um elemento de interface que solicitará um nome para criar uma nova configuração de conceitos.
* Adicionar conceito
Pressionar este botão criará um novo conceito em branco na configuração atual.
* Excluir conceito (X vermelho)
Pressionar este botão em um conceito o excluirá da configuração.
* Duplicar conceito (mais verde)
Pressionar este botão duplicará este conceito, incluindo todas as configurações.
* Ativar conceito (alternância, padrão: ligado)
Este botão deslizante informará ao OneTrainer se deve ou não treinar usando este conceito. O botão fica azul quando ativado.
* Editar conceito
Clicar em qualquer parte da imagem do conceito, fora dos botões e alternâncias, abrirá a janela de configurações do conceito. Esta é uma janela separada onde todas as outras configurações do conceito podem ser modificadas e é a próxima parte desta seção do wiki.

# Configurações de Conceitos - Geral
![imagem](https://github.com/Nerogar/OneTrainer/assets/132208482/b2853b89-2c82-4de5-8642-330302bee080)

A aba geral do Conceito foca nas informações básicas, ajustes e configurações de cache.
* Nome (Padrão: Em branco)
Este campo permite que você insira um nome para seu conceito. Se você não escolher um nome, ele usará o nome da pasta que você inserir ao fechar a janela.
* Ativado (Padrão: Verdadeiro)
Uma alternância que espelha a alternância na própria aba. Esta alternância controla se o OneTrainer treinará ou não com esses dados.
* Caminho (Padrão: Em branco)
Este campo permitirá que você digite ou cole o caminho para a localização das suas imagens de conceito. Você também pode usar o botão ao lado do campo (...) para procurar a pasta.
* Fonte do Prompt (Padrão: de arquivo de texto por amostra)
Este menu suspenso possui três opções.
1. De arquivo de texto por amostra - `0001.jpg` usará o arquivo `0001.txt` como o prompt
2. De arquivo de texto único - o arquivo de texto no campo à direita do menu suspenso será usado para todas as imagens
3. Do nome do arquivo da imagem - `tag1 tag2 tag3.jpg` usará tag1 tag2 tag3 como o prompt

* Incluir Subdiretórios (Padrão: Falso)
Esta alternância permitirá que você use subdiretórios para facilitar o uso, mas o OneTrainer os tratará como um conceito único para gerenciamento interno.
* Variações de Imagem (Padrão: 1)
Este campo controla quantas imagens serão armazenadas em cache usando as variáveis da aba de aumento de imagem.
* Variações de Texto (Padrão: 1)
Este campo controla quantos prompts serão armazenados em cache para cada imagem. Nota: Se você estiver treinando o codificador de texto, os prompts não são armazenados em cache e esta configuração não precisa ser alterada.
* Balanceamento (Padrão: 1 Repetições)
Esses dois campos controlam o balanceamento para o conceito. O balanceamento permite que você, como o nome sugere, equilibre um conceito entre outros. Um caso de uso para isso é imagens de regularização. Se você tiver 100 imagens de origem, mas 10.000 imagens de regularização, você pode usar o balanceamento para treinar apenas uma fração das imagens a cada época. Existem duas maneiras de usar essa configuração.
  * Repetições - Suas imagens de origem vezes o valor serão usadas a cada época. Por exemplo, se você tiver 10.000 imagens e usar 0,01, 100 imagens serão usadas a cada época.
  * Amostras - Informa explicitamente ao OneTrainer quantas imagens usar em cada época. Por exemplo. Usar 100 amostras sempre usará 100 imagens por época. Isso é verdade se você tiver 10.000 imagens ou 20.
* Peso da Perda (Padrão: 1)
Outra técnica para equilibrar suas entradas. Um caso de uso para isso é usar um valor menor que 1 para imagens de regularização se você achar que elas estão afetando a execução do treinamento mais do que você gostaria.

# Aba de Aumento de Imagem
![imagem](https://github.com/Nerogar/OneTrainer/assets/132208482/66d59e79-39d1-4740-b1e4-862162636d4d)

Esta aba foca no aumento de imagem para ajudar a diversificar seu conjunto de imagens. Isso tende a se tornar mais importante à medida que o tamanho da sua entrada se torna menor. A maioria das opções de aumento de imagem tem uma opção para ser aleatória ou fixa. Aleatório escolherá um valor até o número inserido, fixo usará esse valor. É importante notar que, ao usar aumentos de imagem, é necessário armazenar em cache a cada época (desativando o cache latente) ou usar variações de imagem. Para conjuntos de dados pequenos, isso não é custoso, mas quanto mais imagens você tiver, mais tempo levará para usar os aumentos de imagem.
* Atualizar Pré-visualização - Pressionar este botão dará uma ideia do que seus aumentos estão fazendo. Ao usar aumentos aleatórios, pressionar o botão várias vezes ajudará a ter uma ideia melhor do que acontecerá.
* Deslocamento de Recorte (Padrão: Ligado) - O OneTrainer tentará escolher o balde de resolução padrão mais próximo para sua imagem, mas se precisar cortar sua imagem e esta opção estiver selecionada, ele realizará um recorte não central aleatoriamente para permitir que a imagem seja diferente.
* Espelhamento Aleatório (Padrão: Ligado) - A imagem será espelhada, refletida sobre o ponto médio vertical. Pode ser fixo (sempre) ou aleatório.
* Rotação Aleatória (Padrão: Desligado - 0) - A imagem será rotacionada. Aleatoriamente, rotacionará em qualquer direção até o número especificado em graus. Fixo, sempre rotacionará para o número especificado em graus.
* Brilho Aleatório (Padrão: Desligado - 0) - O brilho da imagem será alterado. Quando usado aleatoriamente, mudará para cima e para baixo com o valor especificado sendo um limite. Quando usado fixo, mudará pelo número especificado.
* Contraste Aleatório (Padrão: Desligado - 0) - O contraste da imagem será alterado. Quando usado aleatoriamente, mudará para cima e para baixo com o valor especificado sendo um limite. Quando usado fixo, mudará pelo número especificado.
* Saturação Aleatória (Padrão: Desligado - 0) - A saturação da imagem será alterada. Quando usado aleatoriamente, mudará para cima e para baixo com o valor especificado sendo um limite. Quando usado fixo, mudará pelo número especificado.
* Matiz Aleatória (Padrão: Desligado - 0) - O matiz (cor) da imagem será alterado. Quando usado aleatoriamente, mudará para cima e para baixo com o valor especificado sendo um limite. Quando usado fixo, mudará pelo número especificado.
* Substituição de Resolução (Padrão: Desligado - 512) - Este recurso pode ser usado para substituir a resolução de treinamento do conceito. Quando desativado, o OneTrainer redimensiona as imagens para suas resoluções de treinamento.
  * Quando ativado, pode ser usado para dois propósitos:
    * Com o [treinamento de múltiplas resoluções](https://github.com/Nerogar/OneTrainer/wiki/Lessons-Learnt-and-Tutorials#multi-resolution-training) (várias resoluções de treinamento separadas por vírgulas), usará imagens do conceito da mesma resolução.
    * Para evitar a ampliação da imagem, você pode treinar em 1024 (resolução alvo) com imagens de 512 ou 256 que não serão ampliadas. O treinamento será feito em 512 ou 256. Isso pode ajudar com imagens de baixa qualidade.

# Dados do Wiki de Conceitos Original
**Este é o lugar para colocar seu conjunto de dados.**

Para uma única execução, você pode simplesmente adicionar conceitos e editá-los com um link para seu conjunto de dados (com ou sem legendas) e então selecionar a fonte do prompt (arquivo de texto único se você não usar nenhuma legenda, de arquivo de texto por amostra se você definir arquivos de texto para a legenda ou do nome da imagem se você colocar a legenda no nome da imagem).
Ao trabalhar em várias execuções e assuntos para treinar, você pode criar uma configuração (config) e adicionar conceito(s) a ela. Você poderá chamá-la novamente mais tarde.

Obs. para embeddings e embeddings adicionais, você precisa incluir o placeholder do embedding (por padrão `<embedding>`) na legenda, pode ser `<embedding>` ou qualquer outra palavra que você quiser. Seja na legenda da imagem ou no arquivo de texto único. Sim, pode ser várias palavras separadas por espaços, então com uma legenda como "Fotografia embaçada de um homem vestindo um uniforme militar", você poderia usar 'Fotografia embaçada", "homem" e "uniforme militar" como placeholders para 3 embeddings.

**Opções de conceito.**

Você pode usar as configurações padrão ou habilitar variações de imagem para melhorar a qualidade do treinamento.

Cada conceito tem quatro opções:

* Variações de imagem: Especifica o número de versões de imagem armazenadas em cache de um conceito.
* Variações de texto: O mesmo para o texto. Isso só se aplica se o codificador de texto não estiver sendo treinado.
* Balanceamento: Com essa opção, você pode ajustar o número de imagens de um conceito que são incluídas em cada época. Use Repetições como um multiplicador do conceito (pode ser menos que 1) ou Amostras para especificar o número exato de imagens usadas por época (pode ser maior que o número de imagens no conceito).
* Peso da Perda: Este é um multiplicador para a perda deste conceito.

Exemplos:

* Conceito A: 10 imagens, 10 variações de imagem, 5.0 repetições
* Conceito B: 100 imagens, 2 variações de imagem, 1.0 repetições
* Conceito C: 5000 imagens, 1 variação de imagem, 0.1 repetições

O Conceito A será armazenado em cache 10 vezes, o Conceito B duas vezes e o Conceito C apenas uma vez. Isso é semelhante às épocas de cache latente, mas ajustável para cada conceito. As épocas de cache latente foram removidas.
Cada época é treinada com 50 imagens do Conceito A, 100 imagens do Conceito B e 500 imagens do Conceito C.

Algumas observações adicionais:
* Substituição de resolução para cada conceito. Você pode definir a resolução individualmente para cada conceito.
* Aumentos de imagem fixos: em vez de usar números aleatórios, você pode especificar valores fixos para aumentos de imagem.
* Ativação ou desativação de conceitos. Cada conceito pode ser desativado individualmente.
* Agora você pode adicionar conceitos individuais à execução de treinamento sem armazenar novamente todo o conjunto de dados. Apenas o novo conceito será armazenado em cache.
* Peso de perda para cada conceito. Isso pode ser útil para coisas como imagens de regularização que não devem ser treinadas como imagens de treinamento regulares.
* Variações de texto podem ser embaralhamento de tags (veja a opção) ou arquivos de legenda com várias linhas. Como observado, isso é necessário quando o codificador de texto não está sendo treinado.