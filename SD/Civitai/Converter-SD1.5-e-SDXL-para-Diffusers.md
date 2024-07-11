# [Converter SD1.5 e SDXL para Diffusers](https://civitai.com/articles/2756/convert-15-and-sdxl-to-diffusers)

---

Criado em: 08 de julho de 2024 às 11:04 (UTC: -03:00)
Tags: `tool guide` `googlecolab` `sdxl convert` `sd 1.5 convert` `sdxl lora`
Autor: [**duskfallcrew**](https://civitai.com/user/duskfallcrew)

---

## **Introdução**

Bem-vindo a este tutorial sobre como converter modelos 1.5 e SDXL para Diffusers! Se você é um usuário de modelos Stable Diffusion, pode ter encontrado problemas ao executá-los no Google Colab devido a restrições de AUP. Neste tutorial, mostraremos como converter seus modelos 1.5 e SDXL para Diffusers, que podem ser executados em serviços pagos como AWS Sagemaker, que tem opções gratuitas limitadas, bem como serviços pagos como Vast, Runpod, Kaggle e Paperpace.

Por favor, note que este tutorial é destinado a usuários que têm um entendimento básico de modelos Stable Diffusion e Diffusers. Se você é novo nesses tópicos, pode querer começar com alguns recursos introdutórios antes de mergulhar neste tutorial. Os notebooks ainda não foram testados em outros serviços baseados em Jupyter além dos serviços do Google Colab até o momento.

Também esteja ciente: ainda não reformulamos a redação em nenhum dos conversores do Colab e estamos tentando reestruturar a forma como tudo está organizado. Paciência é uma virtude, e estamos cientes de que até nós abusamos desse privilégio diariamente <3. Mantenha-nos informados sobre quaisquer erros e nos avise como está indo.

---

## **Atualizações:**

A versão SDXL AGORA tem um erro de importação JAX corrigido tanto no Colab quanto no GitHub. A edição JUPYTER + arquivos extras de Python NÃO FORAM TESTADOS até esta noite. Eu não sou programador por natureza, coloquei esses códigos em aberto para que você possa testá-los. Eles não estão infectados com vírus, são apenas códigos. Se você encontrar algum problema, poste aqui, no GitHub OU no cartão do modelo aqui para a versão SDXL.

Planejo CORRIGIR a edição SD 1.5, bem como implementar PROVAVELMENTE outras versões agora que o HF tem outros branches. Sim, a maioria das pessoas PODE usar arquivos PY via CLI, mas alguns gostam do conforto de um notebook fácil de usar!

---

## **Repositórios Necessários**

### Conversores 1.5:

[https://civitai.com/models/179789/convert-15-to-diffusers](https://civitai.com/models/179789/convert-15-to-diffusers)

[https://github.com/duskfallcrew/sd15-to-diffusers/](https://github.com/duskfallcrew/sd15-to-diffusers/)

Teoricamente, o SD 1.5 DEVERIA funcionar em uma camada gratuita, e se não funcionar - me avise, pode haver um ou dois erros de importação do Jax - e se for apenas recursos, por favor, altere seu tempo de execução para um TPUV2.

### Conversor SDXL:

[https://github.com/duskfallcrew/sdxl-model-converter](https://github.com/duskfallcrew/sdxl-model-converter)

[https://civitai.com/models/512028/sdxl-to-diffusers-forked-from-linaqruf-colab](https://civitai.com/models/512028/sdxl-to-diffusers-forked-from-linaqruf-colab)

O nosso deve funcionar em camadas pagas/Não Pro gratuitas, já que eu acabei de corrigir isso para funcionar com o TPUV2.  
Se encontrar erros, por favor, informe nos comentários ou no GitHub (de preferência GitHub).

---

## **Convertendo Modelos 1.5 para Diffusers**

Para converter um modelo 1.5 para Diffusers, siga estes passos:

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/eba266df-2468-4661-a24b-f78fec835d85/width=525/eba266df-2468-4661-a24b-f78fec835d85.jpeg)

## INSTRUÇÕES:

♻ - **Instalar/Clonar o repositório**: Clone o repositório de [**<u>https://github.com/duskfallcrew/sd15-to-diffusers/</u>**](https://github.com/duskfallcrew/sd15-to-diffusers/).

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/8d3cb204-aae3-454f-aa98-1d5634995f0d/width=525/8d3cb204-aae3-454f-aa98-1d5634995f0d.jpeg)

♻ - **Baixar modelo e obter o Token de Leitura do Hugging Face**: Baixe o modelo 1.5 que deseja converter e obtenha seu Token de Leitura do Hugging Face.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/76d9b33c-149b-4195-ab6e-acdf16c8ffbb/width=525/76d9b33c-149b-4195-ab6e-acdf16c8ffbb.jpeg)

♻ - **Inserir os detalhes do modelo**: Atualize os detalhes do modelo no script.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/4cec3327-ad85-46c9-8d45-3de83a41d360/width=525/4cec3327-ad85-46c9-8d45-3de83a41d360.jpeg)

♻ - **Verificar o navegador de arquivos**: Verifique se o modelo foi convertido para o formato Diffusers.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/fd0b41b8-5c49-4dea-8064-6290e09642a7/width=525/fd0b41b8-5c49-4dea-8064-6290e09642a7.jpeg)(Não se preocupe, estou desconectado, você verá outras pastas lá quando fizer)

♻ - **Escrever o Token de Escrita do Hugging Face e configurar seu repositório**: Configure seu repositório e insira seu Token de Escrita do Hugging Face.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/74d0ce31-da4e-48d8-947c-11da4655af27/width=525/74d0ce31-da4e-48d8-947c-11da4655af27.jpeg)

♻ - **Upload para Diffusers**: Faça upload do modelo convertido para seu repositório.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/24468e7d-b772-4444-9643-b54b84b2d0bd/width=525/24468e7d-b772-4444-9643-b54b84b2d0bd.jpeg)

**Notas Importantes:**

- Certifique-se de ter RAM suficiente para executar o processo de conversão.
    
- Teoricamente, esse processo deve funcionar no Vast/Runpod, mas não foi testado nessas plataformas.
    
- Se encontrar algum problema, verifique o repositório GitHub para dicas de solução de problemas.
    

Ao incluir os Tokens de Leitura e Escrita do Hugging Face, os usuários poderão usar o Hub do Hugging Face para servir seu modelo Diffusers.

---

## **Convertendo Modelos SDXL para Diffusers**

♻ - **Instalar/Clonar o repositório**: Clone o repositório de [https://github.com/duskfallcrew/sdxl-model-converter](https://github.com/duskfallcrew/sdxl-model-converter)

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/b81affc8-0656-424c-bb6c-c98b9202bc81/width=525/b81affc8-0656-424c-bb6c-c98b9202bc81.jpeg)

♻ - **Baixar modelo e obter o Token de Leitura do Hugging Face**: Baixe o modelo 1.5 que deseja converter e obtenha seu Token de Leitura do Hugging Face.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/dbaa6f38-dcee-4553-b6f1-10c426ce5077/width=525/dbaa6f38-dcee-4553-b6f1-10c426ce5077.jpeg)

♻ - **Inserir os detalhes do modelo**: Atualize os detalhes do modelo no script.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/497aefce-693b-4b40-9315-431d253cc58c/width=525/497aefce-693b-4b40-9315-431d253cc58c.jpeg)

♻ - Nossa versão removeu a inferência para cumprir a falta de recursos para usuários gratuitos.

♻ - **Escrever o Token de Escrita do Hugging Face e configurar seu repositório**: Configure seu repositório e insira seu Token de Escrita do Hugging Face.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c9936d3f-fc9d-4bc5-92f4-ccc2945bba30/width=525/c9936d3f-fc9d-4bc5-92f4-ccc2945bba30.jpeg)

♻ - **Upload para Diffusers**: Faça upload do modelo convertido para seu repositório.

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/c7f24c06-9e13-4ce6-a62a-3e242b654754/width=525/c7f24c06-9e13-4ce6-a62a-3e242b654754.jpeg)

**Notas Importantes:**

- Certifique-se de ter RAM suficiente para executar o processo de conversão.
    
- Teoricamente, esse processo deve funcionar no Vast/Runpod, mas não foi testado nessas plataformas.
    
- Se encontrar algum problema, verifique o repositório GitHub para dicas de solução de problemas.
    

Ao incluir os Tokens de Leitura e Escrita do Hugging Face, os usuários poderão usar o Hub do Hugging Face para servir seu modelo Diffusers.

---

## **Sobre & Links**

---

Sobre Nós

Somos a Duskfall Portal Crew, um sistema DID com mais de 300 alters, navegando pela vida com DID, TDAH, Autismo e CPTSD. Acreditamos no potencial da IA para derrubar barreiras e melhorar a saúde mental, apesar dos seus desafios. Junte-se a nós na nossa jornada criativa explorando identidade e expressão.

---

Junte-se à Nossa Comunidade

-   **Website:** [End Media](https://end-media.org/)
    
-   **Discord:** [Junte-se ao nosso Discord](https://discord.gg/5t2kYxt7An)
    
-   **Backups:** [Hugging Face](https://huggingface.co/EarthnDusk)
    
-   **Apoie-nos:** [Mande uma Pizza](https://ko-fi.com/duskfallcrew/)
    
-   Café: [https://www.buymeacoffee.com/duskfallxcrew](https://www.buymeacoffee.com/duskfallxcrew)
    
-   Patreon: [https://www.patreon.com/earthndusk](https://www.patreon.com/earthndusk)
    

**Grupos da Comunidade:**

-   **Subreddit:** [Reddit](https://www.reddit.com/r/earthndusk/)
    

---

Embeddings para Melhorar a Qualidade

**Negative Embeddings:** Use embeddings específicos para cenários para refinar os resultados.

-   [Pony Negative Embeds](https://civitai.com/models/389486/negative-embeds-for-pony-xl?modelVersionId=564545)
    

**Positive Embeddings:** Melhore a qualidade da imagem com esses embeddings.

-   [PDXL Score Embed](https://civitai.com/models/384756/pdxl-score-embed?modelVersionId=563234)
    

---

Extensões

-   **ADetailer:** [ADetailer GitHub](https://github.com/Bing-su/adetailer.git)
    
    -   **Uso:** Use esta extensão para aprimorar e refinar imagens, mas use com moderação para evitar excesso de processamento com SDXL.
        
-   **Batchlinks:** [Batchlinks para A1111](https://github.com/etherealxx/batchlinks-webui)
    
    -   **Descrição:** Gerencie vários links ao executar A1111 localmente ou em um servidor.
        
    -   **Addon:** Addon @nocrypt (O link está quebrado por enquanto, vou encontrar depois OOPS)
        

**Extensões Adicionais:**

-   [Danbooru Prompt](https://github.com/EnsignMK/danbooru-prompt.git)
    
-   [SD Civitai Browser Plus](https://github.com/BlafKing/sd-civitai-browser-plus)
    
-   [Embedding Merge](https://github.com/klimaleksus/stable-diffusion-webui-embedding-merge)
    
-   [SD WebUI AR](https://github.com/alemelis/sd-webui-ar.git)
    
-   [Supermerger](https://github.com/hako-mikan/sd-webui-supermerger.git)
    
-   [Lobe Theme](https://github.com/canisminor1990/sd-webui-lobe-theme)
    
-   [Model Toolkit](https://github.com/arenasys/stable-diffusion-webui-model-toolkit.git)
    
-   [Model Converter](https://github.com/Akegarasu/sd-webui-model-converter.git)
    
-   [Lora Metadata](https://xypher7.github.io/lora-metadata-viewer/)
    

---

Backups para Loras no SDXL & Pony XL:

-   2024: [https://huggingface.co/EarthnDusk/SDXL\_Lora\_Dump\_2024/tree/main](https://huggingface.co/EarthnDusk/SDXL_Lora_Dump_2024/tree/main)
    
-   2023: [https://huggingface.co/EarthnDusk/Loras-SDXL/tree/main](https://huggingface.co/EarthnDusk/Loras-SDXL/tree/main)

# Comentários

> [**Edwardsmith3500**](https://civitai.com/user/Edwardsmith3500)
Olá senhor, obrigado por dedicar seu tempo para escrever este guia, eu realmente tentei o conversor SDXL no Colab, recebi um erro e não consegui converter

---

>> [**duskfallcrew - OP**](https://civitai.com/user/duskfallcrew)
Pode me dizer qual foi o erro?

---

>>> [**Edwardsmith3500**](https://civitai.com/user/Edwardsmith3500)
Bem, não me lembro exatamente, você tentou converter um modelo sdxl no colab gratuito?

---

>>>> [**duskfallcrew - OP**](https://civitai.com/user/duskfallcrew)
\>_> não me diga que estão ferrando os diffusers para conversões na linha t4 agora - e não, ainda tenho créditos pro até que expirem, vou ver se consigo converter algo nos próximos dias e ver o que está acontecendo - em TEORIA DEVERIA haver uma maneira de fazer isso fora disso, mas ainda não descobri como fazer funcionar no huggingface

---

>>>>> [**Edwardsmith3500**](https://civitai.com/user/Edwardsmith3500)
Ótimo, me avise quando tentar!

---

> [**duskfallcrew - OP**](https://civitai.com/user/duskfallcrew)
Além disso, os dele estão arquivados (LINAQRUF) e eu fiz um fork aqui: https://github.com/duskfallcrew/sdxl-model-converter - isso não significa que o meu vai funcionar, no entanto, se algo estiver errado, nesta etapa, quando eu tiver tempo livre em uma semana ou mais, vou tentar ver o que está errado, o script específico que ele usa pode ser porque os repositórios dele estão arquivados e o meu é apenas um fork do dele...
Me avise <3

---

> [**duskfallcrew - OP**](https://civitai.com/user/duskfallcrew)
Ah, Colab, sua peça aleatória e irritante de PALAVRÕES ALEATÓRIOS - sim, ok, infelizmente mesmo na camada paga (porque deixei meu pro expirar) não consigo usar isso e recebo este erro: convert_sdxl_to_diffusers.py não encontrado, baixando... Traceback (most recent call last): Arquivo "/content/convert_sdxl_to_diffusers.py", linha 4, em <module> do diffusers import StableDiffusionXLPipeline Arquivo "/usr/local/lib/python3.10/dist-packages/diffusers/__init__.py", linha 38, em <module> do .models import ( Arquivo "/usr/local/lib/python3.10/dist-packages/diffusers/models/__init__.py", linha 33, em <module> do .controlnet_flax import FlaxControlNetModel Arquivo "/usr/local/lib/python3.10/dist-packages/diffusers/models/controlnet_flax.py", linha 25, em <module> do .modeling_flax_utils import FlaxModelMixin Arquivo "/usr/local/lib/python3.10/dist-packages/diffusers/models/modeling_flax_utils.py", linha 45, em <module> classe FlaxModelMixin: Arquivo "/usr/local/lib/python3.10/dist-packages/diffusers/models/modeling_flax_utils.py", linha 194, em FlaxModelMixin def init_weights(self, rng: jax.random.KeyArray) -> Dict: Arquivo "/usr/local/lib/python3.10/dist-packages/jax/_src/deprecations.py", linha 54, em getattr raise AttributeError(f"module {module!r} não tem atributo {name!r}") AttributeError: module 'jax.random' não tem atributo 'KeyArray'

---

>> [**Edwardsmith3500**](https://civitai.com/user/Edwardsmith3500)
Então esse guia está obsoleto?

---

>>> [**Edwardsmith3500**](https://civitai.com/user/Edwardsmith3500)
Deixa pra lá, está

---

>>>> [**duskfallcrew - OP**](https://civitai.com/user/duskfallcrew)
DE CERTA FORMA, aguarde um pouco - estou apenas exausto, tive um funeral hoje - não consegui corrigir isso de jeito nenhum, tentei 5 coisas diferentes, incluindo atualizar jax:
Colab desconectou minha tentativa de conversão matando o processo e nunca me mostrando nada no log de recursos, então -- infelizmente, estão fazendo coisas estranhas novamente.
NO ENTANTO, TENTAREI reorganizar ambos para AWS/Runpod/Vast etc - Amazon Sagemaker te dá cerca de 6 horas grátis e o notebook permite que você faça upload para o Huggingface, então se eu conseguir fazer funcionar (o que provavelmente não é difícil, eu só preciso literalmente descobrir a estrutura no código) -- estou apenas exausto agora e não tenho energia.

---

>>>>> [**Edwardsmith3500**](https://civitai.com/user/Edwardsmith3500)
Desculpe ouvir isso, se cuida, ninguém está apressado para isso,

---

>>>>> [**Edwardsmith3500**](https://civitai.com/user/Edwardsmith3500)
Então você está dizendo que existem outras plataformas onde você pode rodar notebooks e eles fornecem GPU de graça?

---

>>>>>> [**duskfallcrew - OP**](https://civitai.com/user/duskfallcrew)
Amazon Sagemaker é uma, mas para rodar SD nela - você provavelmente terá que fazer um fork de um notebook para isso - você pode realmente rodar WebUI's no sagemaker, você só tem espaço limitado - você PODE rodar o notebook diffusers nele, até mesmo os forks que eu tenho - mas você terá que descobrir como modificar o código para entender onde estão suas pastas, o que é algo que continuo esquecendo como fazer - eu não sou programador por natureza, mas estarei corrigindo esses para tentar resolver as coisas - provavelmente mover tudo para widgets Ipython em vez de depender de markdown

---

>>> [**duskfallcrew - OP**](https://civitai.com/user/duskfallcrew)
Corrigido, o colab que está linkado no meu github que eu fiz fork agora está corrigido.

---

>>>> [**Edwardsmith3500**](https://civitai.com/user/Edwardsmith3500)
Obrigado por corrigir, foi realmente difícil de ler e ainda não entendi se vai funcionar na camada gratuita do colab

---

>>>>> [**duskfallcrew - OP**](https://civitai.com/user/duskfallcrew)
Com a correção do TPU vai funcionar, ainda não tive chance de corrigir o artigo - desculpe.

---

>>>>>> [**Edwardsmith3500**](https://civitai.com/user/Edwardsmith3500)
Então eu devo escolher o TPU V2 em vez do T4 GPU?

---

>>>>>>> [**duskfallcrew - OP**](https://civitai.com/user/duskfallcrew)
Sim, se você puder. para SDXL use TPU, pois tem cerca de 30x a RAM - Use meu fork, pois corrigi o erro do jax que o do Linaqruf pode não ter corrigido.

---

>>>>>>>> [**Edwardsmith3500**](https://civitai.com/user/Edwardsmith3500)
Pode fornecer o link para seu fork?

---

>>>>>>>>> [**duskfallcrew - OP**](https://civitai.com/user/duskfallcrew)
Está no artigo acima, removi a versão arquivada do Linaqruf e substituí pela minha.

---

> [**duskfallcrew - OP**](https://civitai.com/user/duskfallcrew)
Reformulei com Llama3b, apenas meio que trapaceei, pois o tutorial para SDXL terá que mudar já que meu notebook não tem o teste de inferência.

---

> [**6DammK9**](https://civitai.com/user/6DammK9)
FYI: O script OG convert_sdxl_to_diffusers.py ainda está funcionando. Acabei de fazer meu modelo em ambiente local recentemente.
Para SD1.5 e 2.1, use convert_original_stable_diffusion_to_diffusers.py em vez disso. Minha página de modelo SD1.5. Também a página do modelo raro SD2.1.

---
