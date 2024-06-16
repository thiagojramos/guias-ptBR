## Aqui você define o modelo base que usará para treinamento e o modelo de saída que deseja obter.

![modelo](https://github.com/Nerogar/OneTrainer/assets/132208482/ea3f29ea-e576-4d09-9c9a-ffe1ac320fb5)

### Modelos

- **Modelo Base (padrão: link do hugging face para base)**: forneça o caminho de um modelo base salvo ou um link no formato do huggingface (exemplo: stabilityai/stable-diffusion-xl-base-1.0).
- **VAE (padrão: em branco)**: Se quiser usar um VAE customizado, deve ser um link do huggingface (exemplo: madebyollin/sdxl-vae-fp16-fix).
- **Destino do Modelo de Saída**: defina qualquer pasta local com seu nome e tipo (ex: C:/OneTrainer/Output/xxx.safetensors). Nota - modelos que produzem saídas do diffusor esperam um diretório, não um arquivo safetensor. Atualmente, isso inclui Pixart Alpha.
- **Formato de Saída (padrão: safetensors)**: Aqui você pode escolher entre o formato padrão safetensors e o opcional checkpoint.
- **Tipo de Dados**: Existem 4 opções disponíveis. Da menos para a mais precisa, são: float8, bfloat16, float16 e float32. Quanto mais precisão usar, mais Vram será necessária para carregar os pesos.

### Valores Recomendados para Uso de DType (A serem verificados, em progresso)

| DType para | Fine Tune | LoRA | Embedding | Fine Tune VAE | Comentário |
|------------|-----------|------|-----------|---------------|------------|
| Saída      | Todos     | Todos| Todos     | Todos         | Comentário |
| Peso       | Todos     | Todos| Todos     | Todos         | Comentário |
| Substituir Codificador de Texto | float32 | Todos | Não relevante | Não relevante | Comentário |
| Substituir Unet | Todos | Todos | Não relevante | Não relevante | Comentário |
| Substituir VAE | float32, bfloat16 | float32, bfloat16 | float32, bfloat16 | float32, bfloat16 | Comentário |

Se todas as amostras estiverem pretas, tente definir o tipo de dados do VAE para float32 ou bfloat16, ou use a substituição do VAE para carregar o VAE corrigido.

### Observação
Os presets (menu suspenso superior esquerdo: #SD1.5 LoRA, #SD1.5 Embedding, ...) irão configurar os valores padrão, você pode manter esses valores, eles funcionam.