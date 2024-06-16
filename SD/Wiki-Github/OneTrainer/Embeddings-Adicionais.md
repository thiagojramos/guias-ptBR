WIP - Como esta é uma nova funcionalidade, informações serão adicionadas conforme forem encontradas.

Esta aba permite adicionar embeddings adicionais ao seu treinamento.

![imagem](https://github.com/Nerogar/OneTrainer/assets/132208482/77a68b43-6418-42ec-916f-9d31bc2c0a36)

# Descrição
Uma técnica para adicionar embeddings de texto em QUALQUER treinamento. Isto não é limitado ao treinamento LoRA e pode ser usado com FineTunes ou até mesmo para treinar múltiplos embeddings sem treinar nenhum Unet ou Text Encoders.

Um caso de uso para este tipo de treinamento é combinar o treinamento de um LoRA unet com um embedding de texto. Isso permite que você incorpore um conceito, estilo ou pessoa em um checkpoint enquanto mal toca nos encoders de texto, usando o unet treinado com o LoRA para fornecer informações gráficas para o embedding.

A aba de embeddings adicionais estará sempre disponível, não importa o tipo de treinamento que você esteja fazendo. É assim que você pode combinar embeddings adicionais com treinamento LoRA ou FineTune.

# Uso
Atualmente, quando um embedding adicional é criado, uma subpasta é criada com o embedding dentro dela. Esta subpasta estará no local onde o arquivo safetensor finalizado é armazenado. Isso também se aplica a salvamentos incrementais durante o treinamento; subpastas serão criadas para cada salvamento incremental com o embedding dentro dela.

Para um LoRA, você tem uma nova opção (ativada por padrão) para agrupar os embeddings no LoRA, assim a subpasta não será criada.

Para usar na geração, você precisa incluir o LoRA e o embedding, ou o FineTune e o embedding, no seu programa de geração de escolha para obter os resultados combinados.

# Configuração da GUI de Embeddings Adicionais

## Adicionar embedding
* Pressionar este botão irá adicionar um embedding padrão à aba.

## Configurações de embedding adicional
* X vermelho - Pressionar este botão irá excluir este embedding adicional
* Mais verde - Pressionar este botão irá duplicar seu embedding adicional
* embedding base (padrão: vazio) - Use este campo para especificar se deseja carregar um embedding existente para treinamento adicional
* placeholder (padrão: `<embedding>`) - o placeholder para seu embedding. Será usado como o nome do arquivo (gerado em uma pasta separada próxima ao modelo definido na aba de modelo) e deve corresponder à palavra gatilho que você usa nas suas legendas. Este também será o placeholder que você usa no seu prompt, por exemplo, no Automatic1111. Observe que não precisa ser uma única palavra, várias palavras separadas por espaço também funcionarão.
* contagem de tokens (padrão: 1) - o número de tokens que você quer usar para seu embedding
* treinar (padrão: ligado) - um toggle que especifica se sua próxima execução de treinamento irá treinar este embedding adicional
* parar treinamento após (padrão: vazio - NUNCA) - Dois campos que você pode usar para dizer ao OneTrainer quanto tempo você quer treinar este embedding. Especificar NUNCA permitirá que o treinamento dure o tempo todo.
* texto inicial do embedding (padrão: * ) - A palavra ou palavras ou letras que você quer usar para seu embedding. Especificar menos tokens do que a contagem de tokens fará com que o OneTrainer use * como tokens adicionais. Usar um texto mais longo do que sua contagem de tokens fará com que o OneTrainer corte seu texto para a contagem de tokens. Tente fazer este texto útil (ou único) e corresponder à contagem de tokens.

# Observações
* Use uma ferramenta como o automatic1111 ou stable swarm para determinar quantos tokens seu texto inicial tem que você quer usar. Isso pode ajudar a definir o número de tokens que você quer usar no embedding. Você também pode usar este [link](https://novelai.net/tokenizer) para determinar informações sobre tokens. Certifique-se de usar o CLIP Tokenizer na lista suspensa.
* Usar um embedding adicional com treinamento LoRA com ambos unet e text encoders resultará em aprendizado muito rápido no caso de treinamento de assunto
* Como os embeddings adicionais são atualmente arquivos separados, tentar treinar um número muito grande de embeddings não é fácil de gerenciar ou criar.
* Provavelmente há muitas mais ideias do que pode ser feito com esta técnica. Experimente e compartilhe no Discord!
* Usar sua palavra gatilho da legenda como o placeholder para o embedding tornará as coisas muito mais fáceis. Você não precisa mais usar `<embedding>` e ter legendas separadas para execuções de embedding e LoRA/FineTune/Embedding Adicional.
* O Prodigy luta se você treinar apenas o unet e um embedding adicional (e não treinar o(s) Text Encoder(s)). Limitar o crescimento para 2 e usar pesos e cálculos BF16 tem se mostrado funcionar, pelo menos no SDXL. Se você tentar o Prodigy com FP16 e conseguir fazer funcionar, por favor compartilhe suas configurações.
* Na aba de treinamento, sob a taxa de aprendizado de embeddings, há uma opção "Preservar Norm do Embedding", que serve para redimensionar os embeddings treinados para a norm média de todos os outros embeddings (entre 0,35 e 0,45).
* A LR pode precisar ser adaptada a cada encoder, mas não haverá nenhuma recomendação aqui, pois depende muito do que você está treinando.

# Mais Informações
Pivotal tuning é um conceito semelhante, e aqui estão algumas informações adicionais para entender mais.
* https://github.com/danielroich/PTI
* https://huggingface.co/blog/sdxl_lora_advanced_script