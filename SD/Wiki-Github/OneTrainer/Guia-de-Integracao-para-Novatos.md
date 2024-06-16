OneTrainer é uma solução completa para treinar quase tudo no Stable Diffusion. À primeira vista, a interface parece simples, mas sempre surgem as mesmas dúvidas ao começar a usá-la. Este guia foi criado como um passo a passo muito simples para entender a interface e onde definir suas configurações de treinamento. Não é exaustivo e não é um guia de treinamento geral. Além disso, pode estar desatualizado com as frequentes melhorias do OneTrainer, então leve isso em consideração. Para informações mais detalhadas, confira as outras [seções da Wiki](./Inicio.md).

Como estou mais acostumado com embedding e LoRA, vou me ater a isso neste guia de introdução.

### Começando

No canto superior esquerdo, você tem uma lista suspensa onde pode escolher uma configuração pré-definida para um tipo de treinamento. Você pode se manter nisso por enquanto. Você encontrará mais informações nas outras páginas do Wiki.

Em seguida, defina algumas pastas locais para o OneTrainer. Vá na aba General e defina pelo menos as pastas para o Workspace e o Cache. A pasta de debug é necessária apenas para depuração.

### Modelo & Dados

Na aba Model, defina o modelo base que você quer usar com seu caminho completo.
Ex: `C:\stable-diffusion-webui\models\Stable-diffusion\v1-5-pruned.safetensors`
Defina o modelo de saída, que será seu embedding ou LoRA. Você deve criar manualmente o arquivo safetensors primeiro, ex: 
`C:\stable-diffusion-webui\models\Lora\TomCruise.safetensors` 
ou 
`C:\stable-diffusion-webui\embeddings\TomCruise.safetensors`.

Pessoalmente, uso meu repositório do SD para isso, mas você pode escolher qualquer pasta local.

TomCruise será sua palavra-chave para chamar o embedding ou LoRA.

Na aba Data, você pode manter as configurações padrão. Aspect Ratio Bucketing é um recurso legal que permite não redimensionar suas imagens, sem necessidade de cortá-las ou usar imagens quadradas. Latent caching economizará algum tempo de treinamento, mas você precisará desativá-lo em alguns casos (explicado no capítulo de conjunto de dados).

### Conjunto de Dados

Prepare seu conjunto de dados como de costume, com imagens e textos de legenda como arquivos de texto separados ou no nome da imagem. Legendas são opcionais, mas ainda recomendadas.

Você também pode primeiro ir na aba Tools, abrir seu conjunto de dados e gerar legendas a partir daí. Ela oferece mais opções que não descreverei aqui.

Importante: para embedding, você precisa incluir o placeholder do embedding na legenda, ele atua como um placeholder. LoRA não precisa de palavra-chave na legenda.

Agora, navegue para a aba Concepts, adicione um conceito, clique nele e forneça o caminho do seu conjunto de dados. Na fonte do prompt, indique como você legendou suas imagens. Se estiver usando "from single text file" (apenas relevante para embedding), significa que você não manteve nenhuma legenda e precisa fornecer um modelo, um exemplo é dado na pasta embedding_templates.

Para mais informações sobre a opção de conceito, confira a página dedicada [Conceitos](./Conceitos.md).

### Treinamento

Confira esta [página](./Treinamento.md) para mais informações ou mantenha os valores padrão no início.

### Amostras, Backup e Salvamento

Opcional, mas útil.

Confira as páginas [Amostragem](./Amostragem.md) e [Backup e Salvamento](./Backup-e-Salvamento.md) para mais informações.

### Aba LoRA ou Embedding

O lugar para adicionar algumas configurações para seu LoRA ou embedding.

Rank do LoRA: o valor padrão é 16 para SD1.5, é bom para começar, quanto mais, mais forte será seu LoRA.

Contagem de tokens para embedding: é o número de tokens que o embedding usará em um prompt. Você pode ter bons resultados com apenas 2 tokens, mas não pense que quanto mais, melhor será o embedding. Eu nunca uso mais de 8 pessoalmente.

Texto inicial do embedding: isso ajudará seu embedding a treinar. `*` pode ser suficiente, mas algumas palavras são melhores. Não use uma frase que use mais tokens do que a contagem de tokens. Normalmente "Man", "Blonde woman" são perfeitamente adequados.

### Comece!

Clique no botão de início de treinamento, você pode ver o progresso do treinamento no canto inferior esquerdo e monitorá-lo com o tensorboard.

