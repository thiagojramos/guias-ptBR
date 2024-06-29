# [Como treinar LoRA para iniciantes sem GPU (então, por que pagar por LoRA?)](https://civitai.com/articles/5853/how-to-train-lora-for-pure-beginner-wo-a-gpu-so-why-pay-money-for-lora)


![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/d240456a-5942-48af-a8d0-7f01e6070ef3/width=450/1.jpeg)


criado: 23 de Jun de 2024
tags: `training guide` `die to paid lora`
autor: [**Flujoru**](https://civitai.com/user/Flujoru)

---

## **INTRO**

Que intro, você viu o título, vamos nessa.

## **PREPARAÇÃO DOS DADOS**

Existem muitas maneiras diferentes de fazer isso, mas confie em mim, se você seguir meu método, vai se sentir bobo até por existirem LoRA pagas.

## **PRIMEIRO**

Decida qual estilo você quer.

## **Mantendo o Estilo Original**

## **Animações**

Consiga o vídeo de qualquer forma (compre a série, YouTube, pirateie, etc.) e tire prints.

Preferencialmente ~60.

O artigo que me ensinou diz que até 20 é suficiente, mas por que não, quanto mais dados, mais fácil brincar com o LoRA.

Se não conseguir chegar a 60, não se preocupe, você ainda pode fazer e eu vou te contar quando chegar na parte do setup do Civitai (mas pelo menos tenha 15, pense nisso como o mínimo).

## **Jogos**

Da mesma forma se for jogo 3D.

Você pode até gravar um vídeo da sua jogatina e tirar capturas de tela dele.

Se for jogo 2D e não for visual novel.

Eu diria apenas desista de conseguir o estilo original com isso e, em vez disso, faça um LoRA sem nenhum estilo específico e depois faça um LoRA de estilo para usar junto.

Se for visual novel, bem, pegue todas as CGs e sprites para fazer 60 imagens.

## **Não se Importa com o Estilo Original**

Apenas colete 60 fan arts de qualquer lugar.

Você pode até usar a busca do Google para isso.

Uma coisa a se atentar é não coletar imagens do mesmo estilo mais de 5 vezes.

Seu LoRA vai se configurar para aquele estilo.

## **CONTINUAÇÃO DA PREPARAÇÃO DOS DADOS**

Jogue todas as imagens em uma pasta

Apenas coloque a palavra-chave (geralmente o nome do personagem) como nome da pasta.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/89f3fed6-2ca9-4984-9a72-2784c5069216/width=525/89f3fed6-2ca9-4984-9a72-2784c5069216.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/a9bb3c1d-93c9-4025-9151-96f4d4b78633/width=525/a9bb3c1d-93c9-4025-9151-96f4d4b78633.jpeg)

Corte as partes que não deseja.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/779f86d7-710d-408a-9276-28fa28057792/width=525/779f86d7-710d-408a-9276-28fa28057792.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/486dd731-2a57-4b96-a883-88596298fe02/width=525/486dd731-2a57-4b96-a883-88596298fe02.jpeg)

Depois use qualquer ferramenta para converter todas as imagens em .png

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/f4132ae7-289f-463f-bde5-426e51294314/width=525/f4132ae7-289f-463f-bde5-426e51294314.jpeg)

Razão? Não sei, o ChatGPT disse que png é o melhor.

Eu pessoalmente não acho que há qualquer diferença significativa entre as extensões de arquivo, mas pelo menos coloque todas na mesma extensão para evitar confusão.

Se você tiver Python, anexei um código do ChatGPT que converte todas as imagens em arquivos png e coloca em uma pasta chamada 123, então você pode simplesmente usar isso.

Para isso, vou explicar como usar mais tarde (confie em mim, isso também é fácil).

Então, selecione todas as suas imagens, pressione F2 (mudar nome) e mude o nome para a palavra-chave que você quer usar ("mylene" no meu caso).

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/b00b0cbb-bf57-4e7c-a385-e140fe773aa3/width=525/b00b0cbb-bf57-4e7c-a385-e140fe773aa3.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c41d8315-c993-461e-ac1b-5d671b3188ba/width=525/c41d8315-c993-461e-ac1b-5d671b3188ba.jpeg)

Depois, na mesma pasta, crie um arquivo txt.

No arquivo txt, insira apenas sua palavra-chave (novamente, "mylene" no meu caso).

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/abf28bed-2746-499c-b9ba-905eeaf1b9a7/width=525/abf28bed-2746-499c-b9ba-905eeaf1b9a7.jpeg)

Selecione o arquivo txt e crie 60 cópias (para corresponder à quantidade do seu conjunto de imagens).

Você pode copiar e colar (ctrl + c, ctrl + v), depois pegar 2 para copiar e colar, depois pegar 4 para... para acelerar as coisas.

Mude o nome como fez com as imagens.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/de3f1985-bf52-4d62-b112-d59a3fddbc1e/width=525/de3f1985-bf52-4d62-b112-d59a3fddbc1e.jpeg)

Faça um arquivo .zip da pasta.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c7af4844-7cf8-4581-8ee9-57ecb5ff5f05/width=525/c7af4844-7cf8-4581-8ee9-57ecb5ff5f05.jpeg)

## **Parabéns, sua preparação está concluída.**

Você provavelmente leu outros artigos e me perguntou.

Por que os prompts são tão simples?

Não deveriam ter detalhes das imagens?

Confie em mim, funciona.

Eu tentei com prompts pesados, prompts leves e um meio termo.

Só acabei percebendo que era tudo inútil.

Se você quiser adicionar outras roupas no seu LoRA, terá que brincar um pouquinho com o prompt, mas vou falar sobre isso em outro artigo se você quiser (deixe um comentário).

## **Configuração do CIVITAI**

Clique em Create - Train a LoRA.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6e394981-8a2e-49f7-902a-8bbdab1797e9/width=525/6e394981-8a2e-49f7-902a-8bbdab1797e9.jpeg)
Clique em character, insira o nome (novamente "mylene" para mim).

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/3b23b026-3d18-4094-8cf5-635f7794e058/width=525/3b23b026-3d18-4094-8cf5-635f7794e058.jpeg)
Clique em Next.

Arraste e solte o arquivo zip.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/62af73a3-a6a0-42c7-b3bd-2cb5d610dcb8/width=525/62af73a3-a6a0-42c7-b3bd-2cb5d610dcb8.jpeg)
Certifique-se de que diz que tem legenda.

Se não disser, provavelmente os nomes dos seus arquivos txt e de imagem não coincidem (devem ser algo como mylene (1).png, mylene (1).txt).

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/6cbedec4-c4c1-400a-a378-46668450f598/width=525/6cbedec4-c4c1-400a-a378-46668450f598.jpeg)
Clique em Next.

Qual é o ponto disso, de qualquer forma, existe algum LoRA feito pelo proprietário real?

Espere um pouco, e ele vai mostrar isso.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/3f472107-748c-4a0a-848b-28199ba36177/width=525/3f472107-748c-4a0a-848b-28199ba36177.jpeg)
Certifique-se de clicar no que você quer (pony para mim).

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c671380a-e497-4497-b869-630b4fe7c2a9/width=525/c671380a-e497-4497-b869-630b4fe7c2a9.jpeg)
Abra os parâmetros de treinamento.

Apenas copie a captura de tela.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/ec4262dd-c847-4bd8-a0fc-c13e068d6525/width=525/ec4262dd-c847-4bd8-a0fc-c13e068d6525.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/5c8d965a-059e-4f5d-ae57-5b41d7aabe1b/width=525/5c8d965a-059e-4f5d-ae57-5b41d7aabe1b.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/0445d46b-17e2-427b-9de8-5f5a9947df41/width=525/0445d46b-17e2-427b-9de8-5f5a9947df41.jpeg)
Se você não fez 60 imagens, mude "Num Repeat" para fazer 60, como se você tiver 20 imagens, repita 3, 15 imagens - repita 4, etc.

Ponto decimal não funciona.

Alta prioridade não é necessária, mas custa alguns buzz (cerca de 20), então por que não.

Clique em submit e vá fazer outras coisas e volte depois.

Horas depois, clique no canto superior direito - Seu perfil - Aba Models

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/ba717e78-92d2-4b61-b06d-bbe9d7307761/width=525/ba717e78-92d2-4b61-b06d-bbe9d7307761.jpeg)
Clique em Training logo abaixo da aba Models.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/814f6103-c8d1-4669-ba61-7cf8e0d6861b/width=525/814f6103-c8d1-4669-ba61-7cf8e0d6861b.jpeg)
Deve dizer READY no Status se estiver concluído.

Clique nisso.

Ele mostra algumas pré-visualizações a cada 3 épocas.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/8964b3ce-6168-4ef0-9dc8-9d2e946356c3/width=525/8964b3ce-6168-4ef0-9dc8-9d2e946356c3.jpeg)
Verifique as pré-visualizações e você pode reduzir as épocas da próxima vez se achar que não melhorou após certo número.

Clique em download para usar no seu PC ou publique para que você possa compartilhar ou usar com o gerador do Civitai.

FEITO.

Aqui está a comparação de dois LoRA (um do acima e outro que gerei com minha GPU).

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/8023d6c7-aba6-4fb3-a416-64479c10c128/width=525/8023d6c7-aba6-4fb3-a416-64479c10c128.jpeg)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/3a55e484-c21b-4bd2-8c46-c188b6742176/width=525/3a55e484-c21b-4bd2-8c46-c188b6742176.jpeg)
Aqui está o prompt que usei se você estiver curioso.

score 9, score 8 up, score 7 up, detailed background, shiny skin,

<lora:loraloralora:1>,

mylene

## **OUTRO**

## **(Isso só sou eu reclamando, se você não quiser ver, pule para o final)**

Não vou dizer qual é qual dessa comparação, porque não vejo nenhuma diferença na parte do personagem.

Bem, a única coisa é que eu não tenho que usar buzz, mas 620 buzz? é tipo 0,6 USD.

Enquanto aqueles f@d!d#s pedem mais de 5 USD (eu até vi 15, WTF).

Estou sinceramente cansado dessas práticas nojentas.

Usamos dados de graça, usamos essas tecnologias incríveis de graça.

Então, por que eles cobram por isso?

Algum esforço nisso? Você viu como é fácil.

Porque eles possuem GPU? Eles a projetaram?

E agora eu mostrei, você literalmente não precisa de GPU para isso, e adivinhe.

Foi minha primeira tentativa com o treinamento do Civitai, apenas copiando o parâmetro que eu tinha.

Se você não gosta do sistema de Buzz do Civitai, por favor, verifique outros sites também.

Eu sei que algumas pessoas usam algo do Google para treinar.

Eu entendo (um pouco) aquelas pessoas que pedem algumas moedas pela sua motivação, e as pessoas decidem compartilhar um pouco do seu dinheiro se a coisa delas for boa.

Mas vender LoRA?

WTF.

Ainda sobre a parte da motivação.

Mesmo que eles se cansem e parem de fazer LoRA.

Veja como é fácil.

Desculpe dizer isso, mas haverá um substituto para eles.

Claro, eu também, mas eu me importo?

Nem tanto, porque eu não me importo com dinheiro.

Estou apenas curtindo esse brinquedo novo e incrível que as pessoas são boas o suficiente para compartilhar de graça.

Eu adoraria ver outras pessoas gerarem LoRA melhor do que eu.

Caramba, foi muito legal quando vi alguém gerando LoRA 3D de Uma Musume.

Por fim, se você é alguém muito ocupado com outras coisas e não quer perder tempo, apenas gaste algum dinheiro (só me levou 10 minutos para a preparação).

Use a recompensa neste site.

Veja, se você pagar 10~15 USD para um usuário por um LoRA.

Você obtém 1 LoRA, e eu vi que a maioria desses LoRA "sob encomenda" ou "por pedido" era da pior qualidade.

Mas se você gastar essa quantia em buzz (10000~15000) e colocá-la em uma recompensa?

Não posso garantir 100%, mas não seria apenas 1 LoRA, e você pode escolher um.

Ou apenas entre em contato com alguém como eu, faço isso de graça, se o seu personagem me agradar.

Eu não aceito pessoas rudes, porém.

+Como usar topng.py

Faça uma pasta "python" bem em C:\\

Coloque ambas as pastas dentro da pasta python.

Mude a extensão do arquivo topng.txt para .bat (apenas mude o nome).

Copie esse arquivo bat e cole onde estão seus arquivos de imagem.

Use-o (clique duas vezes, pressione enter, tanto faz)

Seus arquivos originais serão deletados e os convertidos serão salvos na pasta "123" que ele criou (mesma pasta).

FEITO

++Esse topng terá alguns problemas se sua imagem for gif, mas bem, o que você esperava.

gif geralmente não é uma única imagem.

+++Graças a este post, salvei meses atrás.

[https://civitai.com/models/351583/sdxl-pony-fast-training-guide](https://civitai.com/models/351583/sdxl-pony-fast-training-guide)

---

# Anexos

- [topng.py](https://civitai.com/api/download/attachments/68293)

- [topng.txt](https://civitai.com/api/download/attachments/68294)

---

# Comentários

<p>
</p>
