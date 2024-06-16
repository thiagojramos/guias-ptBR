# Aba de Amostragem
A aba de amostragem é onde você pode controlar quais amostras deseja criar e quando.
![imagem](https://github.com/Nerogar/OneTrainer/assets/132208482/3056963c-3a00-4b95-97b7-feb496083033)

Na aba principal, você pode fazer o seguinte:

## Itens Principais
* Amostrar após X Y. Neste caso, x é um número inteiro e y pode ser qualquer um dos seguintes: Época/Passo/Segundo/Minuto/Hora/Nunca/Sempre. Note que Nunca e Sempre são especiais e ignoram a entrada do número inteiro.
* Escolher o formato da imagem. O padrão é JPG. PNG também está disponível.
* Amostrar agora. Quando você pressionar este botão, o OneTrainer irá amostrar na próxima oportunidade disponível.
* Amostragem manual. Pressionar este botão abrirá a janela de amostragem manual, veja mais informações abaixo.
* Amostragem Non-EMA. Ativar esta opção permitirá a geração de amostragens EMA e Non-EMA quando EMA estiver em uso.
* Amostras para Tensorboard. Ativar esta opção permitirá que as amostras sejam carregadas como parte dos dados da sua execução no Tensorboard.

## Gerenciamento de Amostras
* O menu suspenso mostrará suas configurações de amostras que foram criadas.
* O botão adicionar configuração permitirá que você crie um novo arquivo de configuração de amostra.
* O botão adicionar amostra criará uma nova amostra no espaço abaixo.
* Nota: Uma maneira fácil de duplicar amostras de uma configuração para outra é ir até a pasta training_samples e encontrar o arquivo .json que você deseja copiar. Faça uma cópia dele e renomeie-o para o nome da configuração que você deseja. Você pode então carregar essa nova configuração no OneTrainer e modificá-la. Ou pode ser modificada no editor de texto de sua escolha.

## Prompt(s) de Amostra
* Nesta área, os prompts de amostra aparecerão e você poderá fazer algumas modificações de alto nível neles aqui.
* Para ver todas as opções, você deve pressionar o botão ... à direita da amostra.
* Você pode usar os botões na frente da amostra para excluir ou duplicar facilmente a amostra.
* A alternância na frente de uma amostra controla se a amostra está ativada ou não. Nota: Não é possível editar usando o botão ... amostras que não estão ativadas.

## Janela de Configuração de Prompt de Amostra
![imagem](https://github.com/Nerogar/OneTrainer/assets/132208482/44558955-2003-46c1-86f2-bb6c9630b146)

Nesta janela que você vê após pressionar o botão ..., você pode fazer alterações em todos os itens relacionados a um prompt de amostra. Isso inclui:
* Prompt - Você pode editar o prompt principal.
* Prompt Negativo - Você pode editar o prompt negativo para a amostra.
* Largura e altura - Você pode editar a largura e a altura para o prompt. Nota: Esses valores devem ser válidos para o modelo com o qual você está trabalhando. O padrão é 512x512.
* Seed - Você pode inserir a seed que deseja usar. O valor padrão é 42, afinal de contas, é a resposta.
* Seed aleatória - Ativar esta opção substituirá sua seed e usará uma seed aleatória.
* Escala cfg - A escala de configuração a ser usada para seu prompt, o padrão é 7.0.
* Sampler - O sampler que você deseja usar para seu prompt de amostra. O padrão é DDIM. As opções incluem Euler/Euler A/UniPC/Euler Karras/DPM++ Karras/DPM++ SDE Karras.
* Passos - O número de passos que você deseja usar para uma amostra. O padrão é 20.

## Janela de Amostra Manual
![imagem](https://github.com/Nerogar/OneTrainer/assets/132208482/f951a76a-7c07-45c6-b862-9a92bce6932e)

Esta janela que abre quando você pressiona amostra manual permitirá que você gere uma amostra manual personalizada quando quiser. Isso é ótimo para FineTunes e outras execuções longas de treinamento com múltiplos conceitos.
As configurações para isso são as mesmas da Janela de Configuração de Prompt de Amostra, mas são para uso único. Pressionar amostra fará com que o OneTrainer amostre quando possível.

## Notas e Comentários
* Esta aba pode ser atualizada durante o treinamento e atualizará suas amostras e frequência quando você fizer alterações. Isso pode levar a consequências inesperadas se você começar a configurar as amostras da próxima execução e seu treinamento atual iniciar uma execução de geração de amostra.
* Amostras para embeddings devem usar o placeholder `<embedding>` para funcionar.
* As amostras são colocadas na pasta de amostras do diretório do seu espaço de trabalho quando criadas.
* As amostras são colocadas em subpastas, baseadas na ordem da amostra (0,1,2) seguida pela primeira parte do seu prompt. Nota: Alterar o início do seu prompt mudará a pasta para onde as amostras irão após esse passo.
* Amostras manuais vão para suas próprias subpastas.
* 