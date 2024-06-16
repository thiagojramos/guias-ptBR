# Primeiras Coisas Primeiro

![imagem](https://github.com/Nerogar/OneTrainer/assets/132208482/0b40c78e-7ad3-4c9c-988a-0efee36f30da)

Existem três observações importantes sobre os otimizadores.
* Você pode pressionar os três pontos (...) para abrir as configurações desse otimizador.
* As configurações padrão são as configurações do próprio otimizador e provavelmente não são as melhores para treinar modelos de difusão.
* O OneTrainer armazena as últimas configurações que você definiu para cada otimizador; você pode recuperá-las pressionando "Carregar Padrões".

***

### Otimizadores padrão:
* Adagrad - Algoritmo de Gradiente Adaptativo
* ADAM - Estimação de Momentos Adaptativos
* ADAMW - Estimação de Momentos Adaptativos com decaimento de peso
* LAMB - Otimizador de Momentos Adaptativos por Camada para Treinamento em Lote
* LARS - Escalonamento da Taxa Adaptativa por Camada
* LION - Integração Linear de Neurônios
* RMSPROP - Propagação da Média Quadrática
* SGD - Descida do Gradiente Estocástica

Todos os otimizadores padrão também estão disponíveis em 8 bits.

Se você não se incomodar com fórmulas matemáticas complexas, um bom vídeo técnico introdutório que discute alguns desses otimizadores está [aqui](https://www.youtube.com/watch?v=NE88eqLngkg).

_**Obs.: As versões de 8 bits economizam Vram usando quantização de 8 bits. Há uma perda de qualidade ao fazer isso.**_

***

### Família de Otimizadores DAdapt
(Adaptação de Decaimento Adaptativo Distribuído com Passo de Tempo Parametrizado).
Esses otimizadores são versões adaptativas dos otimizadores padrão
* DADAPT_ADA_GRAD
* DADAPT_ADAM
* DADAPT_ADAN
* DADAPT_LION
* DADAPT_SGD

***

### O Único Otimizador Adaptativo
* Prodigy - Um otimizador especial e um favorito, pois é fácil de usar, mas também pode ser difícil obter os resultados exatos que você procura. Embora ADAM puro ou ADAMW possam fornecer melhores resultados, Prodigy é um bom ponto de partida se você não souber qual taxa de aprendizado usar ou quantos passos são necessários. Prodigy consome muita Vram (especialmente para modelos 1024x) e provavelmente só pode ser usado para treinamento de LoRA ou Embeddings, a menos que você tenha uma quantidade massiva de Vram. Prodigy é ADAMW por trás dos panos. Tensorboard pode ser usado para ver a taxa de aprendizado em que Prodigy se estabiliza para uso em AdamW ou AdamW 8bit.

***

### O Especial
* Adafactor - Adafactor pode ser usado como um ADAMW de baixa memória com um agendador constante (ou outro) ou como um otimizador adaptativo com o agendador Adafactor.


***

### Detalhes Específicos das Configurações do Prodigy
Primeiramente, observe que o Prodigy vem de https://github.com/konstmish/prodigy
Existem alguns detalhes excelentes de uso no README de nível superior lá.

Valores Padrão do OneTrainer Listados. Valores Recomendados Listados Após o Padrão, se aplicável
| |  |
|-----------------------------------------------------------|---------------------------------------------------------|
| Beta1: 0.9                 | Beta2: 0.999, ****Recomendado 0.99 a 0.999**** |
| Beta3: 0                                          | EPS: 1e-08                                      |
| Decaimento de Peso: 0.0 _**Recomendado 0.001 a 0.01**_ | Desacoplar: Verdadeiro                                  |
| Correção de Viés: Falso **_Recomendado Verdadeiro_**  (a menos que seja agendador constante/sdxl?)| Aquecimento de Salvaguarda: Falso  **_Recomendado Verdadeiro?_**    |
| D Inicial: 1e-06     | Coeficiente D: 1.0 _**Recomendado Ver Observação**_                       |
| Taxa de Crescimento: inf   | FSDP em uso: Falso                      |
| Taxa de aprendizado: 1   |                       |

Notas do Prodigy: **A taxa de aprendizado deve ser 1** e deve ser a mesma para ambos os codificadores de texto e unet. Caso contrário, o lora não fará nada.

Para o aquecimento de salvaguarda, pode ser melhor definir como verdadeiro para treinamento curto, mas pode ser verdadeiro/falso para treinamento mais longo (época=100).
Para a Correção de Viés: infelizmente, essa é outra configuração em que o melhor é realmente "depende, tente ambos" ☹️

Para ajustar o Prodigy, você deve usar o coeficiente D, que atua como um multiplicador da taxa de aprendizado. Usar um coeficiente D de 0.5 vai desacelerar o Prodigy, enquanto um coeficiente D de 2 vai acelerar. Usar uma função COSINE pode ajudar a forçar o Prodigy a aprender detalhes no final de um treino, já que o Prodigy nunca diminuirá a taxa de aprendizado por conta própria. Usar um Beta 2 de 0.99 tem mostrado ajudar o Prodigy a convergir melhor. Usar decaimento de peso pode ajudar o Prodigy a não sobre-treinar. Outras técnicas incluem dropout para LoRAs (encontrado na aba LoRA). Habilitar a correção de viés ajuda o Prodigy a agir mais como ADAM.

![Capture d'écran 2024-05-02 133822](https://github.com/Nerogar/OneTrainer/assets/129741936/95b3691c-ed07-4bcf-98fc-55d121df3e8b)

Efeito do coeficiente D na taxa de aprendizado com Cosine
* Azul: 1.2
* Preto: 1
* Amarelo: 0.5


***

### Detalhes Específicos das Configurações do Adafactor (Adaptativo)
Valores Padrão Listados. Valores Recomendados Listados Após o Padrão, se aplicável
|  | |
|-----------------------------------------------------------|---------------------------------------------------------|
| EPS: 1e-30                    | EPS2: .001                |
| Limiar de Clip: 1.0           | Taxa de Decaimento: -0.8          |
| Beta 1: Em branco (Deixe em branco a menos que você realmente saiba o que isso faz)                | Decaimento de Peso: 0.0        |
| Parâmetro de Escala: Verdadeiro         | Passo Relativo: Verdadeiro       |
| Inicialização de Aquecimento: Falso  | Arredondamento Estocástico: Verdadeiro |
| Passagem Posterior Fundida: Falso  |  |

Notas do Adafactor em modo adaptativo:
* Ignorará qualquer valor de taxa de aprendizado.
* Adafactor com passo relativo e parâmetro de escala define uma taxa de aprendizado muito alta para um ajuste fino. Se você quiser usar o adafactor para ajustes finos, veja a próxima configuração. Use o agendador Adafactor junto com essas configurações para aprendizado adaptativo. Adafactor em passo relativo é mais lento, pois precisa fazer mais cálculos para determinar seu próximo movimento. Inserir um valor para Beta 1 no adafactor aumentará significativamente o uso de Vram devido a uma função do tipo EMA sendo habilitada. Habilitar "Passagem Posterior Fundida" pode reduzir significativamente o uso de Vram sem perda de qualidade, mas esta opção não é compatível com acumulação de gradientes.

***

### Detalhes Específicos das Configurações do Adafactor (ADAMW de Baixa Memória)
Valores Padrão Listados. Valores Recomendados Listados Após o Padrão, se aplicável
|  | |
|-----------------------------------------------------------|---------------------------------------------------------|
| EPS: 1e-30                    | EPS2: .001                |
| Limiar de Clip: 1.0           | Taxa de Decaimento: -0.8          |
| Beta 1: Em branco (Deixe em branco a menos que você realmente saiba o que isso faz)                    | Decaimento de Peso: 0.0 _**Recomendado 0.0 a 0.01**_      |
| Parâmetro de Escala: Verdadeiro _**Necessário ser FALSE**_        | Passo Relativo: Verdadeiro  _**Necessário ser FALSE**_     |
| Inicialização de Aquecimento: Falso  | Arredondamento Estocástico: Verdadeiro |
| Passagem Posterior Fundida: Falso  |  |

Notas do Adafactor em modo constante: Um agendador padrão (constante, cosseno, etc.) deve ser usado com adafactor nesse modo. Use uma taxa de aprendizado semelhante ao AdamW ou a padrão para sua tarefa de treinamento. Adafactor nesse modo usa menos Vram que ADAMW, mas mais que ADAMW 8bit sem a perda de qualidade de quantização de 8 bits. Inserir um valor para Beta 1 no adafactor aumentará significativamente o uso de Vram devido a uma função do tipo EMA sendo habilitada. Habilitar "Passagem Posterior Fundida" pode reduzir significativamente o uso de Vram sem perda de qualidade, mas esta opção não é compatível com acumulação de gradientes.

***

### Detalhes Específicos das Configurações do ADAM(W)

***

### Todas as Opções
* adam_w_mode: Se deve usar a correção de decaimento de peso para o otimizador Adam. tipo: booleano
* alpha: Parâmetro de suavização para RMSprop e outros. tipo: float
* amsgrad: Se deve usar a variante AMSGrad para Adam. tipo: bool
* Beta1: termo de otimizador_momentum. tipo: float
* Beta2: Coeficientes para calcular médias móveis de gradiente. tipo: float
* Beta3: Coeficiente para calcular o passo do Prodigy. tipo: float
* correção_de_viés: Se deve usar a correção de viés em algoritmos de otimização como Adam. tipo: bool
* block_wise: Se deve realizar a atualização do modelo por bloco. tipo: bool
* capturable: Se alguma propriedade do otimizador pode ser capturada. tipo: bool
* centrado: Se deve centralizar o gradiente antes de escalonar. Ótimo para estabilizar o processo de treinamento. tipo: bool
* limiar_de_clip: Valor de recorte para gradientes. tipo: float
* D Inicial: Estimativa inicial de D para adaptação de D. tipo: float
* Coeficiente D: Coeficiente na expressão para a estimativa de d. tipo: float
* Amortecimento: Amortecimento para otimizador_momentum. tipo: float
* Taxa de Decaimento: Taxa de decaimento para estimativa de momento. tipo: float
* Desacoplar: Usar otimizador_desacoplado estilo AdamW. tipo: bool
* Diferenciável: Se a função de otimização é otimizador_diferenciável. tipo: bool
* EPS: Um pequeno valor para evitar divisão por zero. tipo: float
* EPS 2: Um pequeno valor para evitar divisão por zero. tipo: float
* ParaCada: 'Se deve usar uma implementação para cada um se disponível. Esta implementação é geralmente mais rápida. tipo: bool
* FSDP em Uso: Bandeira para usar parâmetros fragmentados. tipo: bool
* Fundido: Se deve usar uma implementação fundida se disponível. Esta implementação é geralmente mais rápida e requer menos memória. tipo: bool
* Taxa de Crescimento: Limite para a taxa de crescimento da estimativa de D. tipo: float
* Valor Inicial do Acumulador: Valor inicial para o otimizador Adagrad. tipo: float
* É Paginado: Se o estado interno do otimizador deve ser paginado para a CPU. tipo: bool
* Logar Cada: Intervalos nos quais o log deve ocorrer. tipo: int
* Decaimento da LR: Taxa na qual a taxa de aprendizado diminui. tipo: float}
* Máx Unorm: Valor máximo para recorte de gradientes por normas. tipo: float
* Maximizar: Se deve maximizar a função de otimização. tipo: bool
* Tam Mínimo de 8 bits: Tamanho mínimo do tensor para quantização de 8 bits. tipo: int
* Otimizador Momentum: Fator para acelerar SGD na direção relevante. tipo: float
* Nesterov: Se deve habilitar otimizador_momentum de Nesterov. tipo: bool
* Sem Prox: Se deve usar atualizações de proximidade ou não. tipo: bool
* Bits do Otimizador: Número de bits usados para otimização. tipo: int
* Recorte Percentil: Recorte de gradiente com base em valores percentuais. tipo: float
* Passo Relativo: Se deve usar um tamanho de passo relativo. tipo: bool
* Aquecimento de Salvaguarda: Evitar problemas durante a fase de aquecimento. tipo: bool
* Parâmetro de Escala: Se deve escalonar o parâmetro ou não. tipo: bool
* Arredondamento Estocástico: Arredondamento estocástico para atualizações de peso. Melhora a qualidade ao usar pesos bfloat16. tipo: bool
* Correção de Viés: Ativar a correção de viés do Adam. tipo: bool
* Usar Triton: Se a otimização Triton deve ser usada. tipo: bool
* Inicialização de Aquecimento: Se deve aquecer a inicialização do otimizador. tipo: bool
* Decaimento de Peso: Regularização para evitar overfitting. tipo: float