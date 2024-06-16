[**Tutorial Completo de Ajuste Fino do Stable Diffusion SD & XL com OneTrainer no Windows e na Nuvem - De Zero a Herói**](https://youtu.be/0t5l6CP9eBg)

[![imagem](https://cdn-uploads.huggingface.co/production/uploads/6345bd89fe134dfd7a0dba40/fzw4B_eMUEQulB1v_3xtB.png)](https://youtu.be/0t5l6CP9eBg)

A descrição do vídeo é a seguinte:

Neste tutorial, vou mostrar como instalar o OneTrainer do zero no seu computador e fazer o treinamento de modelos Stable Diffusion SDXL (Ajuste fino completo de 10,3 GB de Vram) e SD 1.5 (Ajuste fino completo de 7GB de Vram) no seu computador, e também fazer o mesmo treinamento em uma máquina em nuvem muito barata da MassedCompute, caso você não tenha um computador assim.

Arquivo README do Tutorial ⤵️
https://github.com/FurkanGozukara/Stable-Diffusion/blob/main/Tutorials/OneTrainer-Master-SD-1_5-SDXL-Windows-Cloud-Tutorial.md

Registre-se na Massed Compute pelo link abaixo (pode ser necessário usar nosso cupom especial para GPU A6000 por 31 centavos de dólar por hora) ⤵️
* https://bit.ly/SECoursesMassedCompute
* https://vm.massedcompute.com/signup?linkId=lp_034338&sourceId=secourses&tenantId=massed-compute

Código do cupom para GPU A6000 é: SECourses

* 0:00 Introdução ao tutorial De Zero-a-Herói de Ajuste Fino do Stable Diffusion (SD) com OneTrainer (OT)
* 3:54 Introdução às instruções do README no GitHub
* 4:32 Como se registrar na Massed Compute (MC) e iniciar uma máquina virtual (VM)
* 5:48 Qual modelo escolher na MC
* 6:36 Como aplicar o cupom da MC
* 8:41 Como instalar o OT no seu computador para treinar
* 9:15 Como verificar a instalação do Python, Git, FFmpeg e Git
* 12:00 Como instalar o ThinLinc e começar a usar sua VM na MC
* 12:26 Como configurar a sincronização de pastas e compartilhamento de arquivos entre seu computador e a VM na MC
* 13:56 Finalizar a sessão existente no ThinClient
* 14:06 Como desligar a VM da MC
* 14:24 Como conectar e começar a usar a VM
* 14:41 Quando usar a sessão existente
* 16:38 Como baixar a melhor configuração predefinida de treinamento do OT para os modelos SD 1.5 e SDXL
* 18:00 Como carregar a configuração predefinida
* 18:38 Explicação completa da configuração do OT e os melhores hiperparâmetros para SDXL
* 24:10 Como configurar conceitos de treinamento com precisão no OT
* 24:52 Como legendar imagens para treinamento de SD
* 30:17 Por que meu conjunto de dados de imagens de treinamento não é ótimo e qual é um conjunto de dados melhor
* 31:41 Como fazer o efeito DreamBooth no OT com o conceito de imagens de regularização
* 32:44 Efeito de usar o conjunto de dados de imagens de regularização de verdade
* 34:41 Como configurar a repetição das imagens de regularização
* 35:55 Explicação da configuração e parâmetros da aba de treinamento
* 41:58 O que é treinamento mascarado, como fazê-lo e gerar máscaras
* 44:53 Gerar amostras durante a configuração do treinamento
* 46:05 Como salvar checkpoints durante o treinamento para comparar e encontrar o melhor depois
* 47:11 Como salvar sua configuração no OT
* 47:22 Como instalar e utilizar o nvitop para ver o uso da Vram
* 48:06 Por que o treinamento super lento acontece devido ao Vram compartilhado e como consertar isso
* 48:40 Como reduzir o uso de Vram antes de iniciar o treinamento
* 49:01 Iniciar treinamento no Windows
* 49:11 Começar a configurar tudo na MC igual ao Windows
* 49:37 Carregar dados para a MC
* 51:11 Atualizar o OT na MC
* 52:33 Como baixar imagens de regularização
* 53:42 Como minimizar todas as janelas na MC
* 54:00 Iniciar OT na MC
* 54:20 Configurando tudo na MC igual ao Windows
* 55:22 Como configurar pastas na VM da MC
* 56:31 Como recortar e redimensionar corretamente suas imagens de treinamento
* 57:47 Pasta de modelos Auto1111 precisa na MC
* 58:05 Copiar caminho de arquivo e pasta na MC
* 58:54 Todo o resto da configuração na MC
* 1:03:29 Como utilizar uma segunda GPU se você tiver
* 1:05:45 Verificando novamente nosso treinamento no Windows
* 1:06:06 Como usar a interface web do SD Automatic1111 (A1111) na MC e no Windows
* 1:11:35 Como usar o Python padrão na MC
* 1:11:55 Verificando a velocidade do treinamento e explicando o que isso significa
* 1:12:13 Quantos passos vamos treinar, explicação
* 1:13:40 Primeiro checkpoint e como os checkpoints são nomeados
* 1:14:15 Como corrigir erros do A1111
* 1:15:44 Como iniciar a interface web do A1111 e usá-la com o Gradio Live share e localmente
* 1:17:45 O que fazer se o carregamento do modelo demorar muito no Gradio e como consertar isso
* 1:19:01 Onde ver o status do treinamento do OT
* 1:19:43 Como carregar checkpoints / qualquer coisa no Hugging Face para salvar permanentemente
* 1:26:21 Como atualizar automaticamente o A1111 e instalar as extensões ADetailer e ControlNet
* 1:29:10 Como usar os checkpoints do modelo treinado na Massed Compute
* 1:30:08 Como testar checkpoints para encontrar o melhor
* 1:32:15 Por que você deve usar o After Detailer (adetailer) e como usá-lo corretamente
* 1:34:48 Como fazer o upscale de alta resolução corretamente
* 1:36:19 Por que ocorre imprecisão na anatomia
* 1:37:07 Como gerar imagens para sempre no A1111
* 1:38:02 Onde as imagens geradas são salvas e como baixá-las
* 1:40:30 Super Importante
* 1:45:16 Analisando resultados da comparação de checkpoints x/y/z para encontrar o melhor checkpoint
* 1:48:20 Como entender se o modelo está sobre-treinado
* 1:52:27 Como gerar diferentes expressões tendo fotos
* 1:54:53 Como fazer inpainting no Stable Diffusion A1111
* 1:56:34 Como gerar LoRA a partir do seu checkpoint treinado
* 1:58:03 Treinamento do OneTrainer no Windows completado, então como usá-los no seu computador
* 2:00:24 Melhores configurações de treinamento de modelos SD 1.5 / DreamBooth / hiperparâmetros
* 2:03:50 Como saber se você tem Vram suficiente?
* 2:05:36 O que fazer antes de encerrar a VM da MC
* 2:06:55 Como encerrar sua VM para não gastar mais dinheiro
* 2:08:35 Como fazer treinamento de estilo, objeto, etc.
* 2:09:47 O que fazer se seu cliente Thin não sincronizar