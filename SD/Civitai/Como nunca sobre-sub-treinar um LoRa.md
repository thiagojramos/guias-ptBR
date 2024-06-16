# [Como nunca sobre/sub-treinar um LoRa](https://civitai.com/articles/5603/how-to-never-overundertrain-a-lora)

![Como nunca sobre/sub-treinar um LoRa](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/e6bffa9c-e541-4ecf-ab82-e8f9794c0436/width=1320/123.jpeg)

-Autor: [**Queria**](https://civitai.com/user/Queria)

---
<p>
</p>

Usando o mesmo princípio dos [conjuntos de snapshots](https://paperswithcode.com/method/snapshot-ensembles), podemos salvar uma quantidade indefinida de loras enquanto os treinamos, para escolher o que parece melhor sem se preocupar com quantos passos definimos para o treinamento.

Agora você pode estar pensando, "Mas não podemos apenas usar a opção de salvar a cada N passos/épocas para isso?" Bem, sim e não. Se você simplesmente definir para salvar cegamente de vez em quando, vai salvá-los enquanto a taxa de aprendizado está alta, resultando em piores resultados (geralmente resultando em uma anatomia ruim, pelo que eu vi). Para evitar isso, só precisamos usar um agendador cíclico e salvar no ponto mais baixo do ciclo. Para isso, podemos usar o cosine_with_restarts e definir nossos passos máximos de treinamento para 10000. Para salvar a cada 500 passos, fazemos 10000/500=20, significando que definimos o número de ciclos para 20 no agendador. Se quisermos salvar a cada 1000 passos, seriam 10 ciclos (10000/1000=10) e é isso, a taxa de aprendizado decairá corretamente e salvará um lora cada vez que isso acontecer.

Há um porém nisso, se estivermos usando otimizadores adaptativos como Dadapt e Prodigy, isso pode acabar fritando o lora no primeiro reinício, porque se o otimizador sentir que a taxa de aprendizado está muito baixa, ele tentará compensar isso aumentando sua estimativa da taxa de aprendizado, e quando o agendador reiniciar de volta para 1, a taxa de aprendizado estará muito alta e arruinará tudo. Não sei exatamente o que causa isso, mas você tem que ficar de olho nisso no tensorboard  
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/be96e4b9-d87f-43fc-a3f6-e61c1129e70b/width=525/be96e4b9-d87f-43fc-a3f6-e61c1129e70b.jpeg)  
![](https://i.imgur.com/x6cUlnV.png)  
Nesta imagem de exemplo, apenas o primeiro gráfico roxo é bom (você tem que observar o gráfico lr/d\*lr), os picos antes da taxa de aprendizado decair nos outros significam que ela explodirá quando reiniciar.

Dito isso, eu prefiro usar REX em vez de cosine porque sinto que cosine decai a LR muito cedo. Como não consegui encontrar uma versão do REX com reinícios, fiz uma eu mesmo (código anexado a este artigo). Eu uso salvando em venv\\Lib\\site-packages\\custom\\custom.py, depois definindo lr_scheduler_type = "custom.custom.RexWithRestarts" e lr_scheduler_args = "first_cycle=1000", então ele cicla a cada 1000 passos (não sei se há algo "errado" com o código do agendador, mas está funcionando para mim).