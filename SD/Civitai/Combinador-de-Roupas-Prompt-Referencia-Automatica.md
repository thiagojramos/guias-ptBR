# [Combinador de Roupas (Prompt/Referência Automática)](https://civitai.com/articles/5994/clothing-combiner-automatic-promptreference)

---

Criado em: 02 de July de 2024 às 21:53 (UTC: -03:00)
Tags: []
Autor:

---

![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/9dc28e83-4052-4c51-945d-7d2f40236a56/width=525/9dc28e83-4052-4c51-945d-7d2f40236a56.jpeg)
![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/1a92137a-42cb-4837-ba7b-bae317535520/width=525/1a92137a-42cb-4837-ba7b-bae317535520.jpeg)

Este fluxo de trabalho usa o Stable Diffusion 1.5, mas pode ser adaptado para usar qualquer outra versão. Ele também utiliza LLMs (visão) para gerar prompts automaticamente. A versão mais recente do IPAdapter é usada, assim como o pose controlnet. Sinta-se à vontade para usá-lo ou me perguntar se precisar de ajuda.

Checkpoint que eu recomendo: [https://civitai.com/models/132632/epicphotogasm](https://civitai.com/models/132632/epicphotogasm)

Ollama Describer - [https://github.com/alisson-anjos/ComfyUI-Ollama-Describer](https://github.com/alisson-anjos/ComfyUI-Ollama-Describer)

IPAdapter Plus - [https://github.com/cubiq/ComfyUI_IPAdapter_plus](https://github.com/cubiq/ComfyUI_IPAdapter_plus)

Layer Style - [https://github.com/chflame163/ComfyUI_LayerStyle](https://github.com/chflame163/ComfyUI_LayerStyle)

KJNodes - [https://github.com/kijai/ComfyUI-KJNodes](https://github.com/kijai/ComfyUI-KJNodes) (Get, Set)

\[Obrigatório\] *Baixe todos os arquivos de modelo de [**https://huggingface.co/mattmdjaga/segformer_b2_clothes**](https://huggingface.co/mattmdjaga/segformer_b2_clothes) [**(ou clone via git**](https://huggingface.co/mattmdjaga/segformer_b2_clothes) [**https://huggingface.co/mattmdjaga/segformer_b2_clothes**](https://huggingface.co/mattmdjaga/segformer_b2_clothes) [**)**](https://huggingface.co/mattmdjaga/segformer_b2_clothes) para a pasta ComfyUI/models/segformer_b2_clothes.

LoRa Weight Slider - [https://civitai.com/models/180008/weight-slider](https://civitai.com/models/180008/weight-slider)

**Nota: Se você acha que usar LLM está sobrecarregando sua GPU, pode usar suas referências manuais. Existem alguns controles onde você pode optar por usar referências e prompts manuais. Agora, um detalhe importante: seu prompt precisa forçar a geração de uma imagem de uma única pessoa de corpo inteiro e que a roupa gerada para essa pessoa tenha um formato similar à roupa de referência (Top, Bottom). Como uma máscara de atenção é usada, se não for assim, pode gerar imagens deformadas, já que o formato é o que definirá a máscara de atenção usada no IPAdapter.**

Se precisar de algo, você pode me mandar uma mensagem no Discord: **alissonerdx**

## Anexos

[workflow-clothing-combiner-automatic-prompt.json 77.75 KB (civitai.com)](https://civitai.com/api/download/attachments/73268)