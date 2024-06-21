# [Dicas e Truques para LoRA com Grandes Conjuntos de Dados (Google Colab + SD 1.5 otimizado)](https://civitai.com/articles/699/large-dataset-lora-tips-and-tricks-google-colab-sd-15-optimized)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/e056a4d4-952c-4b40-9d8b-f49092cc3f3e/width=150/e056a4d4-952c-4b40-9d8b-f49092cc3f3e.jpeg)



criado: 2024-06-19T23:49:50 (UTC -03:00)
tags: `training guide` `generative ai` `epoch` `lora training` `howto` `text encoder` `getting started`
autor: [duskfallcrew](https://civitai.com/user/duskfallcrew)

---

## **Dicas e Truques para LoRA com Grandes Conjuntos de Dados**

**Provavelmente publicado originalmente no início de 2023.**

Este guia é otimizado para o Stable Diffusion 1.5, com considerações para treinamento no site Civitai e Google Colab. Embora seja focado principalmente no SD 1.5, atualizações para SDXL, Pony XL e outras versões estão planejadas.

___

## **Introdução**

Se você é como nós e carrega pelo menos 300 itens em uma pasta e depois se pergunta por que o treinamento demora tanto, você está no lugar certo!

Este guia não é sobre taxas de aprendizado ou agendadores; esses são tópicos complexos deixados para usuários avançados. Em vez disso, vamos focar em como gerenciar os tempos de treinamento e manter a qualidade ao usar uma taxa de aprendizado de 5e-4 para o UNet no treinamento do LoRA.

## **Configurações Padrão**

Para a maioria dos treinamentos de LoRA baseados em anime, uma taxa de aprendizado de 5e-4 para o UNet e uma taxa de aprendizado de 1e-4 para o codificador de texto são recomendadas

. Esta configuração geralmente proporciona um aprendizado forte e LoRAs de alta qualidade. No entanto, as taxas de aprendizado podem variar, e os detalhes precisos são frequentemente uma questão de preferência.

## **Tamanho do Conjunto de Dados e Estratégias de Treinamento**

## **Usuários do Colab**

### **50-100 Imagens**

-   **Tamanho do lote**: 1-3
    
-   **Épocas**: 7-10
    

Esse tamanho não vai estourar seu tempo no Colab ou aluguel, mas ajustar o tamanho do lote e as épocas pode reduzir o tempo de treinamento.

### **100-300 Imagens**

-   **Tamanho do lote**: 2-3
    
-   **Épocas**: 5-8
    

À medida que você se aproxima de 300 imagens, o treinamento pode desacelerar. Reduzir as épocas e aumentar ligeiramente o tamanho do lote pode ajudar a gerenciar o tempo.

### **300-500 Imagens**

-   **Tamanho do lote**: 4
    
-   **Épocas**: 5
    

Treinar com 350-400 imagens requer equilibrar o tamanho do lote e as épocas. Esta configuração é para conservar créditos do Colab e gerenciar o tempo de forma eficaz.

### **Usuários do Colab**

Usuários gratuitos do Colab devem prestar atenção especial a essas diretrizes, já que o Colab Pro também pode desconectar antes de 5 horas. Esteja ciente de que o Colab pode gerar um erro de script em torno da marca de 1,5-2 horas, o que geralmente não interrompe o treinamento.

### **500+ Imagens**

-   **Tamanho do lote**: 5-6
    
-   **Épocas**: 5
    

Para conjuntos de dados maiores, pesquise cuidadosamente seus agendadores e taxas de aprendizado para otimizar o tempo de treinamento. Evite tamanhos grandes no Colab, a menos que você seja experiente.

### **500-1000 Imagens**

-   **Taxa de aprendizado**: 5e-4
    
-   **Repetições**: 5-8 para 800+ imagens
    
-   **Tamanho do lote**: Máximo de 4
    

### **1000+ Imagens**

Treinou com sucesso mais de 1000 imagens com uma ligeira perda de qualidade devido a menos repetições. Aumente os passos o máximo possível para manter o estilo.

## **Dicas Gerais para Treinamento de LoRA no Stable Diffusion**

1.  **Gerenciamento da Taxa de Aprendizado**: Ajuste com base no tamanho do conjunto de dados para evitar overfitting.
    
2.  **Otimização do Agendador**: Experimente diferentes agendadores de taxa de aprendizado.
    
3.  **Aumento de Dados**: Use aumento de dados para aumentar o tamanho do conjunto de dados e melhorar o desempenho do modelo.
    
4.  **Validação**: Mantenha uma parte do conjunto de dados para validação para monitorar o overfitting.
    
5.  **Técnicas de Regularização**: Implemente dropout, decaimento de peso ou outros métodos de regularização.
    
6.  **Treinamento com Precisão Mista**: Use treinamento com precisão mista para acelerar o processo e reduzir o uso de memória.
    
7.  **Utilização da GPU**: Garanta a plena utilização da GPU verificando gargalos no carregamento e pré-processamento de dados.
    
8.  **Experimentação**: Acompanhe experimentos para entender o impacto de diferentes configurações.
    

## **Especificidades do Treinador do Civitai**

O treinador do Civitai suporta configurações variadas para diferentes modelos, incluindo:

-   **SD 1.5** com modelos Anime, Realísticos, Semi-Realísticos e Base SD 1.5
    
-   **SDXL**
    
-   **Pony XL**
    
-   **Modelos de Treinamento Personalizados**
    

## **Limitações de Imagens**

-   **Auto-Tags no Site**: Máximo de 1.000 imagens no arquivo zip sem legendas.
    
-   **Legendas Fora do Site**: Máximo de 1.000 arquivos no total.
    

## **Otimizações**

-   **SDXL e Pony XL**: Use AdaFactor e Prodigy
    
-   **SD 1.5**: Use AdamW8Bit
    

Independentemente do modelo ou otimizador, uma taxa de aprendizado de 5e-4 é geralmente eficaz em todas as configurações.

## **Aviso**

Não sou um treinador de nível top de guias de LoRA. Estas são apenas dicas que aprendi para gerenciar minhas próprias sessões de treinamento. Podemos ajudar a guiar pessoas através dos notebooks de treinamento do Colab se quiserem criar seus próprios LoRAs.

## **Como nos Apoiar**

-   **Junte-se ao nosso Reddit**: [EarthNDusk](https://www.reddit.com/r/earthndusk/)
    
-   **Discord**: [Junte-se à Discussão](https://discord.gg/5t2kYxt7An)
    
-   **Ouça Nossa Música**: [Playlist no Spotify](https://open.spotify.com/playlist/00R8x00YktB4u541imdSSf?si=b60d209385a74b38)
    
-   **Assista Nossas Transmissões**: [Canal do Twitch](https://www.twitch.tv/duskfallcrew)
    
-   **Pré-lançamentos Exclusivos e Brindes**: [Nos Apoie no Ko-fi](https://ko-fi.com/DUSKFALLcrew)
    

Sinta-se à vontade para adicionar quaisquer dicas ou truques que você tenha descoberto!
