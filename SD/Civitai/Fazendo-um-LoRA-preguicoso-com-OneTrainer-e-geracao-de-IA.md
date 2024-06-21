# [Fazendo um LoRA preguiçoso com OneTrainer e geração de IA](https://civitai.com/articles/4789/lazy-lora-making-with-onetrainer-and-ai-generation)

criado: 09-06-2024
tags: `training guide`
autor: [phil866](https://civitai.com/user/phil866)

---
## Introdução

Sou novo na criação de LoRA e tive dificuldade em encontrar um bom guia. Ou havia poucos detalhes, ou havia detalhes DEMAIS. Então, este é o tipo de guia que eu procurava, mas não encontrei. Sem teoria, apenas "aqui está o que você deve fazer".

Para constar, eu **achei** que poderia simplesmente criar um Embedding. No entanto, por algum motivo, os resultados não saíram como eu queria. Então, decidi treinar um LoRA em vez disso. ... e isso também não deu muito certo.... Até que encontrei este truque simples!

(sem brincadeira, sério. Vou falar sobre isso na seção "Ajustando valores"!)

Você pode conferir meu LoRA em ... ---> [https://civitai.com/models/381785/faeryqueen-sd](https://civitai.com/models/381785/faeryqueen-sd)

Não é um LoRA particularmente incrível. Fiz principalmente como um experimento para aprender o processo. Mas não está tão ruim, acho :-)  

### - Seu modelo favorito + Ferramenta de geração

Usei [StableSwarmUI](https://github.com/Stability-AI/StableSwarmUI) e [GhostXL](https://civitai.com/models/312431/ghostxl)

### - OneTrainer

Pegue o OneTrainer em [https://github.com/Nerogar/OneTrainer](https://github.com/Nerogar/OneTrainer)

Escolhi o OneTrainer depois de ver alguns posts dizendo que ele é mais rápido/melhor/mais fácil que o Koyha.

## O Processo Real

### Visão Geral

1.  Gere imagens de entrada e salve em um diretório
    
2.  Elimine as que ficaram ruins
    
3.  Diga ao OneTrainer: "Faça um LoRA para mim"!
    

## 1. Gerar imagens de entrada

A maioria das pessoas tenta "coletar" imagens para treinamento. Mas eu sou preguiçoso, então decidi apenas gerá-las com um bom modelo SDXL!

A versão original deste guia mencionava como criar um pipeline customizado no ComfyUI para redimensionar imagens SDXL, porque, por algum motivo, o redimensionador embutido no OneTrainer não estava funcionando para mim. Mas eu tentei de novo, e funcionou, então essa seção ficou bem mais fácil.

Etapas que fiz nessa fase:

-   Brinquei no StableSwarmUI até encontrar um prompt que gostei, usando o modelo GhostXL.  
    
-   Gere um conjunto de 100 imagens salvas em um diretório.
    
-   Criei um prompt que imaginei que os usuários usariam para obter o LoRA. (Semelhante, mas NÃO o mesmo que usei para criar as imagens)  
    
-   Repeti o processo acima com outras 100 imagens, desta vez de um ângulo diferente, e incluí isso no arquivo de texto local
    

## 2. Filtrar imagens de treinamento ruins

Depois de terminar a etapa acima, eu tinha minhas imagens alvo. Idealmente, você deve revisá-las e descartar manualmente as que ficaram ruins.

Se você estiver no Linux, um programa leve para fazer isso é chamado "feh". Execute-o em um diretório, use as setas para avançar/voltar, e a tecla DEL para excluir uma imagem.

Para Windows, algumas pessoas sugerem "IrFanView". No entanto, para "Passar por um diretório de imagens uma de cada vez, em tela cheia", parece que [nomacs](https://nomacs.org/docs/getting-started/installation/) é uma escolha melhor para mim.  

PS: [este guia](https://civitai.com/articles/3522/valstrixs-crash-course-guide-to-lora-and-lycoris-training) tem algumas dicas úteis sobre como selecionar imagens de treinamento de forma inteligente.

## 3. Faça o LoRA!

Agora, inicie o OneTrainer e selecione a configuração pronta "# sd 1.5 lora".  
(Então, por segurança, "salve a configuração" com um nome personalizado!)

Mudanças mínimas obrigatórias

Para fazer um LoRA no OneTrainer, você precisa, no mínimo, fazer o seguinte:

1.  Na área "modelo", defina "Modelo Base" e "Destino de Saída do Modelo".  
    Note que, para Modelo Base, tive dificuldade em dizer para usar um arquivo local. Tive que usar o formato huggingface. por exemplo: "stablediffusionapi/ghostmix"
    
2.  Na área "conceitos", crie um conceito. Defina "nome", "caminho" (para os dados de entrada) e "fonte do prompt". Para a fonte do prompt, selecionei arquivo único e criei um arquivo basicamente com meu prompt original que gerou as imagens.  
    Você _poderia_ ser mais sofisticado e escolher uma fonte de prompt por imagem. Mas este é um guia **PREGUIÇOSO**, então vou pular isso!
    
3.  Se, como eu, você estiver fazendo uma conversão de SDXL -> SD, **você deve habilitar a substituição de resolução**, em Conceitos-> (seu conceito) -> aumento de imagem
    
4.  Na área "Lora", defina o "rank" desejado. Veja abaixo para mais detalhes sobre isso  
    

## Ajustando valores

Para ajustar valores, consultei as tabelas de ajustes em [https://rentry.org/59xed3](https://rentry.org/59xed3)

Para esta execução, alterei principalmente **UM** valor de ajuste:

Na tabela Lora, mude o rank de 16 para 32. Então funcionou!

Se você quiser ainda mais detalhes, aumente para 64. Isso dobrará o tamanho do arquivo do lora.  
Isso é para SD1.5, mas para SDXL, provavelmente você precisará aumentar para 96, ou em casos extremos, 128

### Valores de ajuste não-preguiçosos

O acima foi para a abordagem preguiçosa, de mudanças mínimas absolutas para obter algum tipo de LoRA funcional.

No entanto, se você está disposto a mexer mais do que em um único ajuste... Para um resultado **muito** melhor e mais fiel ao conjunto de dados, faça as seguintes alterações:

-   Em "treinamento", altere o otimizador para "Prodigy". Ele é muito, muito melhor que ADAMW.
    
-   Em "treinamento", altere a taxa de aprendizado para "1". **Você deve fazer isso ao escolher o Prodigy**
    
-   Em "lora", aumente o valor de rank para "64"... ou às vezes mais.  
    

Você pode querer desativar "Treinar Codificador de Texto" para máxima portabilidade entre modelos.

### Segredos ocultos dos prompts

Na área Conceitos, se você especificar

```
Prompt source: From text file per sample
```

e não se preocupar em criar nenhum arquivo de texto... Ainda funcionará. Parece que cria um lora "sem necessidade de palavra de ativação" nesse caso.

Por outro lado, se você escolher associar prompts ao seu LORA, lembre-se de que as próprias palavras podem ter efeitos embutidos. Experimente seus prompts associados com seus modelos de renderização alvo, **sem** seu LoRA ativado, para ver se eles trazem efeitos indesejados.

## Começar o Treinamento

Aperte o botão, e espere o tempo necessário, e então você pode aproveitar seu novo Lora!

(ou, obcecar infinitamente sobre ajustes finos conforme o artigo do rentry.org mencionado acima.)

### Escolha o melhor resultado, nem sempre o resultado final

Infelizmente, muitas vezes, o "resultado final" pode não ser o seu melhor. É como na geração de imagens, às vezes, 20 etapas são melhores que 40 etapas.

Então, mesmo que você tenha planejado 100 épocas de treinamento, deve fazer amostras de tempos em tempos. Os melhores resultados podem estar em 96. ou 90. ou 84.

### Conjunto de dados real que usei para treinamento

O CONJUNTO COMPLETO de imagens que usei para a última versão do LoRA acima, **junto com os arquivos de configuração do OneTrainer,** pode ser encontrado em

[https://huggingface.co/datasets/ppbrown/faeryqueen](https://huggingface.co/datasets/ppbrown/faeryqueen)

## Conclusão

Espero que este artigo tenha sido útil para você. Se tiver comentários ou mais dicas, por favor, me avise!

___

## Algumas imagens de saída de exemplo

Ao usar o LoRA, ele pode gerar imagens assim:  
prompt= woman,,masterpiece,8k, modelo=aniverse 1.5 passos: 20, CFG: 4
aspectratio: 2:3

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/cfe5f8d0-6bd2-41d9-8c30-95c3c33d2768/width=525/cfe5f8d0-6bd2-41d9-8c30-95c3c33d2768.jpeg)

faery,facing viewer,warm smile (negativo: olhos vermelhos)

passos: 30, CFG: 7
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/61f80db4-25b3-422f-b56d-a6a2abee867b/width=525/61f80db4-25b3-422f-b56d-a6a2abee867b.jpeg)

# Comentários
<p>
</p>

> [**004brad**](https://civitai.com/user/004brad)
Obrigado por criar isso. É uma grande ajuda para aqueles que podem não saber exatamente o que fazer, mas gostariam de saber.

---

> [**ApexThunder**](https://civitai.com/user/ApexThunder)
Oi, você poderia fazer um tutorial em vídeo? Honestamente, sem um suporte visual, não consigo seguir bem o guia escrito.

>> [**phil866 - OP**](https://civitai.com/user/phil866)
Oh, pedido interessante!
Pessoalmente, odeio aprender através de vídeos :D mas reconheço que algumas pessoas preferem essa abordagem.
A vantagem do formato escrito é que é trivial voltar e corrigir erros, ou adicionar coisas que você esqueceu. Não dá para fazer isso facilmente com vídeo.
>>
>> Se este guia chegar a um ponto em que esteja estável, e eu sentir que está "completo", então considerarei fazer um vídeo complementar. Obrigado pela sugestão.

>> [**[deleted]**](https://civitai.com/user/[deleted])
Sim

---

> [**vokar28**](https://civitai.com/user/vokar28)
Quantas épocas você costuma usar? Tenho visto muito debate sobre isso.

>> [**phil866 - OP**](https://civitai.com/user/phil866)
Ainda não sou um especialista, mas eu diria "vá com o que te dá cerca de 2000 passos".
Então, se você tem uma configuração simples, e 200 imagens, então 10 épocas.
Não precisa ser exato... só mais ou menos...
Mas, resultados são o que importam. Então, idealmente, faça amostras de teste antes do seu alvo, para verificar se você precisa reduzir.
Se você for super exigente, então faça salvamentos em 1500, 2000, 2500 (e quem sabe, 3000? 4000?) e use o salvamento que funcionar melhor.
>>
>> NOTA: Acabei de aprender que "backups" não são "salvamentos". Para economizar tempo, talvez seja melhor ultrapassar deliberadamente, e usar "salvamentos" nos pontos que você acha que serão interessantes.

>> [**phil866 - OP**](https://civitai.com/user/phil866)
Ah, só vi a notificação disso agora? :(
Algumas pessoas dizem "2000 passos", mas acho que é só porque estão usando 20 imagens de treinamento.
Até agora, parece que o alvo padrão mais razoável é "100 épocas, batch size=1". Ou seja: coloque 100 passos por imagem, para treinamento SDXL.
Exceto quando você quer mais!
Acho que pode ser 40 passos para SD, 100 para SDXL. Mas isso é um palpite totalmente sem fundamento.
>> 
>> Atualmente estou no meio de experimentos de ajuste detalhado, com 90 imagens, e otimizador prodigy + sdxl. A coisa mais importante parece ser encontrar as configurações "certas" para seu conjunto de treinamento, o que muda devido ao número de imagens e conteúdo específico da imagem, eu acho. Então veremos.
Prévia do que está por vir:
SDXL, prodigy, weight decay=0.01, bias correction FALSE, safeguard warmup TRUE, EMA=ON (gpu se você tiver vram, cpu se não tiver)

>>> [**phil866 - OP**](https://civitai.com/user/phil866)
Não, retiro o que disse. os números para contagem de passos ainda estão em fluxo. Acho que depende muito das configurações do seu otimizador. Talvez seja "cerca de 2000 passos se você tiver um treinador agressivo, mas 100 passos por imagem se você tiver um treinador incremental lento"?
Com minhas últimas configurações de prodigy, 100 passos parece que pode ser demais. Mas, caso contrário, 100 parece muito bom.
A diferença neste caso pode ser a escolha de usar safeguard warmup. Desligado: mais passos = melhor. Ligado: mantenha em menos passos, talvez.
>>> 
>>> Isso, é claro, está relacionado ao fato de que os "passos de aquecimento da taxa de aprendizado" são o valor padrão de 200 em vez de 0, caso contrário, isso não faria nada.
>>> 
>>> edit:
é meio chato; as prévias de treinamento parecem ótimas, mas ao renderizar... bleahhh.
>>> 
>>> estou voltando a pensar que 100+ passos por imagem é o melhor

---

> [**QwiziRAM**](https://civitai.com/user/QwiziRAM)
E quão relevante é este guia para modelos PDXL?

>> [**phil866 - OP**](https://civitai.com/user/phil866)
Nunca ouvi falar de PDXL.
Se você quer dizer SDXL... parece que é um pouco relevante.
SDXL parece mais difícil de treinar, então você tem que ser mais exigente com seu conjunto de dados e tags, ao que parece.

>>> [**QwiziRAM**](https://civitai.com/user/QwiziRAM)
PDXL - PonyDiffusionXL (PonyXL)

---

> [**mog**](https://civitai.com/user/mog)
você pode adicionar a opção de criar tag para preguiçosos e o modelo?

>> [**phil866 - OP**](https://civitai.com/user/phil866)
você quer um guia de "fazer tags para pessoas preguiçosas"?
>> 
>> Apenas encontre um autotagger de IA.

>>> [**mog**](https://civitai.com/user/mog)
também, mas quero dizer criar um modelo e um autotagger que não faça nsfw ou que faça de maneira errada

>>>> [**Otreas**](https://civitai.com/user/Otreas)
Mano, o que foi?

>>>>> [**mog**](https://civitai.com/user/mog)
é o que eu escrevi

---
