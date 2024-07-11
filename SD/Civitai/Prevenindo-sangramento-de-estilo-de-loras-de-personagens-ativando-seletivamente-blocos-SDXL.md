# [Prevenindo sangramento de estilo de loras de personagens ativando seletivamente blocos (SDXL)](https://civitai.com/articles/5301/preventing-style-bleeding-from-character-loras-by-selectively-enabling-blocks-sdxl)

---

Criado em: 05 de julho de 2024 às 20:25 (UTC: -03:00)
Tags: []
Autor: [**Queria - OP**](https://civitai.com/user/Queria)

---

Resumo: Segundo parágrafo

Você já usou um lora de personagem, sozinho ou com um lora de estilo, e percebeu que ele não funciona como esperado? Como quando ele cria o personagem, mas apenas em um único estilo (o estilo com o qual foi treinado), bem, então este artigo é para você!  
O sangramento de estilo geralmente acontece quando o lora é treinado por muito tempo e/ou em um único estilo, felizmente para nós, a arquitetura SDXL parece ser robusta o suficiente para (principalmente) desvincular essas características em alguns blocos muito específicos à medida que as imagens passam pelo U-Net, o que significa que podemos obter personagens com muito pouco ou nenhum sangramento de estilo. Isso também nos permite obter estilos sem sangramento de pose/composição/gênero, embora isso dependa um pouco do estilo específico, gosto e do que se considera uma estilização eficaz. Ainda não fiz muitos testes empíricos sobre isso, mas vou tocar brevemente no assunto.

Então, como fazer isso? No forge/automatic11111 você pode usar [esta extensão](https://github.com/hako-mikan/sd-webui-lora-block-weight) (ps: se usar múltiplos loros, essa extensão tem um [bug](https://github.com/hako-mikan/sd-webui-lora-block-weight/issues/155), então esteja ciente) para poder ativar/desativar blocos seletivamente. Aqui estão algumas imagens de referência da arquitetura SDXL e onde cada bloco está / como parece [https://imgur.com/a/K9Rhomv](https://imgur.com/a/K9Rhomv)  
Agora você deve ser capaz de usar o lora com <lora:my\_lora:1:1:lbw=0,0,0,0,0,0,1,1,0,0,0,0>, isso desativará todos os blocos, exceto OUT0 e OUT1. Aqui estão algumas imagens de comparação de como alguns loros aleatórios que eu peguei do civit ficam sendo usados normalmente <lora:my\_lora:1> (todos os blocos), com lbw=0,0,0,0,0,0,1,1,0,0,0,0 (apenas OUT0 OUT1) e sem ele (para mostrar que apenas esses dois blocos estão realmente fazendo algo e a imagem resultante não é apenas a saída do modelo regular), todos os prompts tinham "3d" e/ou "realístico" na tentativa de fazer uma versão 3D do personagem  
[https://imgur.com/a/dH3jUVG](https://imgur.com/a/dH3jUVG)  
Em alguns exemplos eu tive que desativar ou reduzir o peso do bloco OUT1 (natsu (cabelo rosa) e asta (cabelo grisalho espetado)) porque ainda se recusava a fazer uma imagem 3D corretamente com ele ativado, você notará que alguns loros ainda conseguem fazer versões 3D mesmo com todos os blocos ativados, mas mesmo esses podem se beneficiar da desativação de blocos desnecessários, pois a qualidade da imagem melhora quando eles são desativados e a estilização é melhor aplicada. Uma desvantagem disso é que nenhum desses loros foi treinado com blocos específicos em mente, o que significa que, quando desativamos ou reduzimos o peso dos blocos, o personagem pode se tornar menos preciso, não há muito o que se possa fazer a respeito, exceto ativar novamente alguns blocos e ver os resultados. Mas se os loros fossem treinados apenas nesses blocos, talvez a informação pudesse ter sido condensada neles (não tenho certeza, pois não testei isso para treinar personagens).

Agora, de onde vem tudo isso? De ler [este](https://arxiv.org/abs/2404.02733) e [este](https://arxiv.org/abs/2403.14572) artigo (clique em **View PDF** à direita da tela para acessar o artigo completo), ambos chegaram independentemente à mesma conclusão: no SDXL, "estilo" parece estar principalmente presente no bloco de atenção OUT1, com "sujeitos" principalmente no bloco OUT0, e outro componente de estilo no bloco IN08 ("layout" conforme o artigo InstantStyle). Isso parece ainda ser amplamente verdadeiro para pônei, mas claro que não é tudo tão claramente separado, embora a forma do personagem esteja de fato principalmente contida no bloco OUT0, muitos recursos (especialmente cores) só podem ser recuperados quando ativamos o bloco OUT1. Você pode, é claro, também testar o resto dos blocos para ver como eles afetam seu lora.  
Agora, e quanto aos estilos? Bem, um princípio semelhante se aplica a eles, você pode ativar apenas OUT1 e IN08 para estilizar imagens de forma menos agressiva. Você pode ter notado como alguns loros de estilo fazem poses completamente diferentes e outras coisas a partir do modelo base, enquanto isso é desejável em alguns casos (onde diferenças anatômicas e outras coisas fazem parte do estilo), em muitos outros não é, porque o lora de estilo acaba interferindo no que o modelo base sabe e pode fazer, degradando a qualidade e a variabilidade das saídas. Treinando ou ativando apenas blocos específicos relacionados ao estilo, pode-se preservar as capacidades do modelo base enquanto ainda aplica a estilização. Um bônus interessante de tudo isso é que os loros treinados nesses poucos blocos são muito menores também.  
E quanto ao 1.5? Eu realmente não sei, seus blocos parecem ser muito mais instáveis do que os do SDXL, se você quiser, pode tentar as coisas mencionadas [neste artigo](https://arxiv.org/abs/2403.07500).  
Se você quiser treinar blocos específicos, recomendo usar o [sistema de configuração lycoris](https://github.com/KohakuBlueleaf/LyCORIS/blob/4cd782c819738985853c076ade197f9cd5fc48c6/docs/Preset.md), um [exemplo](https://github.com/bmaltais/kohya_ss/discussions/2395). Também anexei um .txt neste artigo contendo os nomes dos blocos U-Net para usar na configuração do lycoris.  
Loros que usei neste artigo: [1](https://civitai.com/models/453117/lady-amalthea-the-last-unicorn-pony-xl-bu-uoc?modelVersionId=504470), [2](https://civitai.com/models/149490/della-duck-or-ducktales-2017-pdxl-and-15), [3](https://civitai.com/models/413778/natsukawa-masuzu-oreshura), [4](https://civitai.com/models/398918/asta-black-clover-pony?modelVersionId=444889)  
Outros artigos sobre este tema:  
[https://civitai.com/articles/76/bdsqlsz-lora-training-advanced-tutorial1lora-block-training](https://civitai.com/articles/76/bdsqlsz-lora-training-advanced-tutorial1lora-block-training)

[https://civitai.com/articles/2322/lora-block-weight-extension-in-automatic1111-weights-usage-xyz-plots-for-sdxl-lora](https://civitai.com/articles/2322/lora-block-weight-extension-in-automatic1111-weights-usage-xyz-plots-for-sdxl-lora)

Edit: Após treinar alguns loros (tanto de personagem quanto de estilo), posso definitivamente dizer que você não só pode, mas provavelmente deve treinar apenas esses blocos: in08, out1 ou out0 para estilos (out1, in8 é opcional) e personagens/objetos/conceitos (out0) (especialmente personagens). O treinamento será mais rápido e você evitará muito sangramento e interações indesejadas (embora alguns possam preferir treinar todas as camadas de atenção para estilos para que isso afete a imagem o máximo possível, você pode selecionar o preset "attn-only" no lycoris, também provavelmente conv se você realmente quiser replicar o estilo o máximo possível. Lembre-se que treinar todos os blocos pode reintroduzir o risco de overfitting que o treinamento por bloco parece prevenir).

# Comentários

> [**Akasa**](https://civitai.com/user/Akasa)  
Este artigo salvou minha vida! Eu estava procurando na internet como resolver a supersaturação causada por múltiplos loros e lidar com loros invasivos com efeitos de estilo indesejados. 
Mas tudo o que eu encontrava eram pesos de bloco lora para SD1.X, e nada para SDXL. 
Com seu artigo, agora posso facilmente fazer imagens que não conseguia antes, sem problemas. Obrigado.

---

> [**Queria - OP**](https://civitai.com/user/Queria)  
Legal, os problemas causados ao usar múltiplos loros parecem ser causados por pesos conflitantes nos vários loros, redimensionar loros também pode ajudar um pouco nisso, há também este feito especificamente para isso: https://ziplora.github.io/, https://github.com/mkshing/ziplora-pytorch

---

> [**opt404723**](https://civitai.com/user/opt404723)  
Mesmo sabendo do bug, eu esperava que o Forge Couple pudesse talvez evitá-lo. Sem chance.  
Ainda assim, quando usado conforme o esperado, os resultados são impressionantes. Coletei vários loros de personagens que têm sangramento de estilo excessivo, e isso ajuda muito.

---

> [**pamdevilcs**](https://civitai.com/user/pamdevilcs)  
Ei, há algum artigo que você recomendaria para aprender sobre os blocos específicos do UNET e como eles se comportam? Tenho tentado usar essa técnica no Comfyui usando um nó para separar o condicionamento em blocos específicos, mas não tenho um bom entendimento de exatamente a que cada bloco corresponde.

---

> [**Queria - OP**](https://civitai.com/user/Queria)  
Não é uma ciência exata e você tem que testar para realmente ver o que está acontecendo, já que o modelo descobrirá sozinho e, mesmo assim, as camadas não são exatamente separadas em diferentes categorias. Você pode procurar no Google por mapa de recursos Unet e como eles funcionam, mas basicamente, camadas mais profundas = recursos maiores/mais abstratos e camadas rasas = recursos menores/granulares. Quando dizemos profundas, queremos dizer aquelas em direção ao meio: https://i.imgur.com/9XX9v3a.png, alguns artigos que investigaram o Unet do SD:  https://arxiv.org/abs/2403.14572, https://arxiv.org/abs/2403.07500, https://arxiv.org/abs/2404.02733, https://arxiv.org/abs/2403.07500, https://arxiv.org/abs/2303.09522, https://arxiv.org/abs/2311.11919, https://arxiv.org/abs/2405.07913

---

> [**Zeid**](https://civitai.com/user/Zeid)  
Quando você diz "Após treinar alguns loros (tanto de personagem quanto de estilo), posso definitivamente dizer que você não só pode, mas provavelmente deve treinar apenas esses blocos: in08, out1 ou out0 para estilos (out1, in8 é opcional) e personagens/objetos/conceitos (out0) (especialmente personagens)"  
É possível fazer este tipo de configuração com o kohya_ss, ou preciso usar um script básico? Você tem algum exemplo de arquivos de configuração com esse tipo de treinamento?  
De qualquer forma, artigo muito interessante e informativo, obrigado por compartilhar essas informações :)

---

> [**Queria - OP**](https://civitai.com/user/Queria)  
Sim, tenho adiado fazer um guia para isso porque queria criar exemplos de comparação entre loros completos vs attn-only vs poucos blocos para o guia (para que eu pudesse pelo menos respaldar minhas alegações com evidências físicas lol), mas realmente me falta potência de GPU para isso. De qualquer forma, é bastante simples: na GUI, em "LoRA type", você seleciona Lycoris/Locon (mas isso também deve funcionar com lokr, loha, etc.) e, em "LyCORIS Preset", você cola o caminho para um arquivo .toml contendo isto https://pastebin.com/yH2w4JPp. input_blocks.8.1. = IN8, output_blocks.0.1. = OUT0, output_blocks.1.1. = OUT1, então você pode editar e ajustar o arquivo conforme necessário removendo/adicionando-os. O bloco IN8 captura algum tipo de característica das imagens, não é estritamente necessário para conteúdo (personagens, objetos) ou estilo, mas adicioná-lo deve ajudar se alguém sentir que apenas um bloco não está fazendo o trabalho, mesmo após muito treinamento, https://b-lora.github.io/B-LoRA/static/source/B-LoRA.pdf no artigo W2 é IN8, W4 OUT0 e W5 OUT1  
Para quem está fazendo isso no CLI, é network_args = ["preset=C:/whatever/myfile.toml", "conv_dim=0", "conv_alpha=0", "algo=locon"] e network_module = "lycoris.kohya"

---

> [**Zeid**](https://civitai.com/user/Zeid)  
Muito obrigado pelos detalhes e explicações. Vou ver como me saio, se eu conseguir, posso enviar meus futuros testes entre meus diferentes loros ;)

---

> [**Zeid**](https://civitai.com/user/Zeid)  
Desculpe pela resposta dupla, mas queria distinguir entre as duas solicitações. Eu tenho o preset LyCORIS aparecendo, mas além de uma lista fixa no Kohya_SS, não tenho como adicionar um caminho para um arquivo toml. Dei uma olhada rápida e não consegui encontrar uma maneira de adicionar um preset a essa lista. Alguma ideia?

---

> [**Queria - OP**](https://civitai.com/user/Queria)  
Você precisa atualizar a GUI se ela não permitir colar um caminho de arquivo, não mostrará nenhuma opção para "adicionar caminho de arquivo", você só precisa selecionar o campo de texto e colar o caminho, se aceitar significa que sua GUI está atualizada, se redefinir para um dos presets padrão, você precisa atualizar

---

> [**Felldude**](https://civitai.com/user/Felldude)  
Na aba avançada, você pode especificar para treinar blocos, o ponto médio, o alfa por bloco e o peso mínimo necessário para ativar um bloco.  
O Civitai não gosta de alfas misturados para geração no site em alguns casos.

---

> [**Queria - OP**](https://civitai.com/user/Queria)  
Verdade, mas tenho quase certeza de que isso treina mais do que apenas a atenção para o UNET e, para o clip, certamente treinará também o MLP em vez de apenas a atenção, mas talvez seja próximo o suficiente, eu não testei

---

> [**Felldude**](https://civitai.com/user/Felldude)  
Pelo que eu entendi, os pesos (In Weights) afetariam o U-NET, mas definir uma matriz de blocos para zero deve impedir completamente o treinamento desse bloco.  
Referência Kohya "Block Zero"

---

> [**Queria - OP**](https://civitai.com/user/Queria)  
Sim, mas há mais do que apenas a atenção naquele único bloco, não sei se o kohya por padrão treinará apenas a atenção quando você selecionar um único bloco, veja https://github.com/kohya-ss/sd-scripts/issues/1215

---

> [**Felldude**](https://civitai.com/user/Felldude)  
Eles estavam se referindo aos pesos projetando para a atenção do bloco pelo que entendi, esse problema também foi encerrado. Trecho do link:  
"Blocos: Dimensões dos blocos, Alfas dos blocos
Aqui você pode definir diferentes valores de rank (dim) e alfa para cada um dos 25 blocos IN011, MID, OUT011.  
Veja Network Rank, Network alpha para valores de rank e alfa.  
Blocos com rank mais alto devem conter mais informações.  
Você deve sempre especificar 25 números para este valor de parâmetro, mas como os loros têm como alvo os blocos de atenção, IN0, IN3, IN6, IN9, IN10, IN11, OUT0 e IN1 não têm blocos de atenção. , configurações de IN2 (1º, 4º, 7º, 11º, 12º, 14º, 15º, 16º dígitos) são ignoradas durante o aprendizado."  
Ainda assumo que definir a dimensão do bloco ou o alfa do bloco para zero faria 0, definir o peso da rede ou o alfa para zero, e ainda pode projetar.

---
