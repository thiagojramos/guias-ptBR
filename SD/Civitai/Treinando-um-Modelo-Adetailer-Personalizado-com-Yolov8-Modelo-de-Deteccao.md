# [Treinando um Modelo Adetailer Personalizado com Yolov8 (Modelo de Detecção)](https://civitai.com/articles/4080/training-a-custom-adetailer-model-with-yolov8-detection-model)

---

Criado em: 05 de julho de 2024 às 20:22 (UTC: -03:00)
Tags: []
Autor: 

---

Eu criei e compartilhei algumas ferramentas para treinar seus próprios modelos Yolov8.

[https://github.com/MNeMoNiCuZ/yolov8-scripts/](https://github.com/MNeMoNiCuZ/yolov8-scripts/)

Este artigo irá espelhar as instruções no GitHub.

Eu também o carreguei como um recurso no Civit: [https://civitai.com/models/302266](https://civitai.com/models/302266) para que você possa baixá-lo de lá se quiser.

---

## 1\. Instruções de Instalação - Windows

1. Baixe ou clone este repositório para qualquer pasta `git clone https://github.com/MNeMoNiCuZ/yolov8-scripts`
    
2. Entre na pasta `cd yolov8-scripts`
    
3. Clone o Ultralytics dentro desta pasta `git clone https://github.com/ultralytics/ultralytics`
    
4. Execute `setup.bat`. Ele pedirá para você digitar um nome de ambiente. Pressione Enter para usar os padrões. Só mude se souber o que está fazendo. O venv deve ser criado dentro da pasta Ultralytics. Isso também criará algumas pastas vazias para você e um script de ativação do ambiente (`activate_venv.bat`). Ele também deve ativar o ambiente para você no próximo passo.
    
5. Instale o torch para sua versão do CUDA ([Pytorch.org](https://pytorch.org/)):

    `pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118`.
    
6. Dentro do (venv), instale os requisitos usando `pip install -r requirements.txt`.


No futuro, você pode entrar no ambiente virtual executando `activate_venv.bat`.

---

## 2\. Baixando um conjunto de dados

- Para nosso exemplo, vamos baixar [este conjunto de dados de marca d'água](https://universe.roboflow.com/mfw-feoki/w6_janf) de [Roboflow.com](http://roboflow.com/) pelo usuário [MFW](https://universe.roboflow.com/mfw-feoki).
    
- Baixe o conjunto de dados no formato Yolov8.


[![image](https://github.com/MNeMoNiCuZ/yolov8-scripts/assets/60541708/c79a6e7c-3f21-421c-8876-03676918afb8)](https://github.com/MNeMoNiCuZ/yolov8-scripts/assets/60541708/c79a6e7c-3f21-421c-8876-03676918afb8)

- Descompacte o arquivo e mova os diretórios train/test/valid para a pasta /dataset/ do seu projeto.


![](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/5610b00f-0a98-4e8c-af5a-e13e3b1fc529/width=525/5610b00f-0a98-4e8c-af5a-e13e3b1fc529.jpeg)

---

## 3\. Preparação do conjunto de dados

Se você baixou um conjunto de dados Yolov8, tudo já deve estar correto. As imagens são colocadas em `/train/images`, e as anotações são colocadas em `/train/labels`.

Você também pode ter as imagens e anotações diretamente na raiz da pasta `/train` sem as subpastas /images e /labels. O mesmo vale para as pastas valid e test.

Se você baixou um conjunto de dados COCO, pode usar os scripts [dataset/CocoGetClasses.py](https://github.com/MNeMoNiCuZ/yolov8-scripts/blob/main/dataset/CocoGetClasses.py) e [cocoToYoloAnnotations.py](http://cocotoyoloannotations.py/) para converter o conjunto de dados para o yolov8. Também há um comando embutido que eu não conhecia quando escrevi os scripts.

Se você criou seu próprio conjunto de dados, precisa garantir que o formato corresponda ao esperado para o treinamento do yolo.

Exemplo de um arquivo de rótulo:

    0 0.76953125 0.665625 0.4609375 0.1015625
    0 0.04375 0.9703125 0.0859375 0.025

    O primeiro valor (0) é o índice da classe.
    O segundo valor (0.769) significa que o centro da caixa delimitadora está a 76,9% na imagem, horizontalmente.
    O terceiro valor (0.665) significa que o centro da caixa delimitadora está a 66,5% na imagem, verticalmente.
    O quarto valor (0.460), significa que a largura da caixa delimitadora é 46% da largura da imagem.
    O quinto valor (0.101), significa que a altura da caixa delimitadora é 10,1% da altura da imagem.

Isso vale para cada caixa delimitadora. Elas podem se sobrepor.

---

## 4\. Configuração do Data.yaml

O [dataset/data.yaml](https://github.com/MNeMoNiCuZ/yolov8-scripts/blob/main/dataset/data.yaml) deve ser configurado para o seu conjunto de dados.

Edite o arquivo e certifique-se de que o número de classes corresponda ao número de classes do seu conjunto de dados, bem como a lista de nomes das classes.

Para o nosso conjunto de dados de marcas d'água, deve ser:

    nc: 1 # Número de classes
    names: ['watermark'] # Nomes das classes

Às vezes, as classes são listadas no site do conjunto de dados, como na captura de tela abaixo.

[![image](https://github.com/MNeMoNiCuZ/yolov8-scripts/assets/60541708/cb85228e-6e17-4be2-aa05-8a89cf352dc5)](https://github.com/MNeMoNiCuZ/yolov8-scripts/assets/60541708/cb85228e-6e17-4be2-aa05-8a89cf352dc5)

O data.yaml baixado também deve conter as configurações de treinamento adequadas, exceto que os caminhos dos arquivos precisam ser alterados para o meu script de treinamento.

    train: ../train/images
    val: ../valid/images
    test: ../test/images
    
    nc: 1
    names: ['watermark']
    
    roboflow:
      workspace: mfw-feoki
      project: w6_janf
      version: 1
      license: CC BY 4.0
      url: https://universe.roboflow.com/mfw-feoki/w6_janf/dataset/1

Se você não conseguir encontrar as classes, pode tentar baixar a versão COCO.JSON do conjunto de dados e executar o script [CocoGetClasses.py](http://cocogetclasses.py/) deste repositório para extrair os valores necessários para o dataset.yaml. Mas isso não é necessário para este guia.

---

## 5\. Executar [train.py](http://train.py/)

Enquanto estiver no ambiente, execute `python train.py` para iniciar o treinamento.

Esperamos que você tenha algo assim agora: [![image](https://private-user-images.githubusercontent.com/60541708/303943205-5cbc9997-5274-42ca-a193-57cd553e5a91.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDc2OTI4NzAsIm5iZiI6MTcwNzY5MjU3MCwicGF0aCI6Ii82MDU0MTcwOC8zMDM5NDMyMDUtNWNiYzk5OTctNTI3NC00MmNhLWExOTMtNTdjZDU1M2U1YTkxLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAyMTElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMjExVDIzMDI1MFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWJiYTk4MzI3NmQzODhhMjQwMmExYzg5Mjk0ZTE2OWIxYjQzNDM2ZjVlZTZjNDJhNDVjNWQ1YzkxZWJlNTVlNTAmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.3Xt5evQcIgVqgzZQHF1ow7P0Fz5WjPBpU_Hlcr5nd0w)](https://private-user-images.githubusercontent.com/60541708/303943205-5cbc9997-5274-42ca-a193-57cd553e5a91.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDc2OTI4NzAsIm5iZiI6MTcwNzY5MjU3MCwicGF0aCI6Ii82MDU0MTcwOC8zMDM5NDMyMDUtNWNiYzk5OTctNTI3NC00MmNhLWExOTMtNTdjZDU1M2U1YTkxLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAyMTElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMjExVDIzMDI1MFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWJiYTk4MzI3NmQzODhhMjQwMmExYzg5Mjk0ZTE2OWIxYjQzNDM2ZjVlZTZjNDJhNDVjNWQ1YzkxZWJlNTVlNTAmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.3Xt5evQcIgVqgzZQHF1ow7P0Fz5WjPBpU_Hlcr5nd0w)

Se você precisar cancelar o treinamento, basta fechar a janela ou pressionar `CTRL + C` para interromper.

Você pode encontrar os resultados dos testes e seus modelos no diretório `training_output`.

O script sempre salvará seu modelo mais recente (last.pt) e o modelo com melhor desempenho até o momento (best.pt), no diretório /training_output/project_name/weights/.

---

## 6\. Gerar / Detectar / Testar seu modelo

Copie seu modelo de saída para o diretório `models`, você também pode renomeá-lo para algo adequado, como `watermarks_s_yolov8_v1.pt`.

Você pode querer tentar tanto o `last.pt` quanto o `best.pt` separadamente para ver qual modelo funciona melhor para você.

Abra `generate.py` para editar alguns parâmetros.

    "model_path" é o caminho para seu modelo.
    "selected_classes" é uma lista das classes que você deseja identificar e detectar ao executar o script.
    "class_overrides" é uma lista de substituições. Use isso se desejar substituir uma classe por outra. Isso pode ser útil se o modelo estiver treinado nas classes na ordem errada, ou se você apenas desejar mudar o nome do rótulo nas imagens sobrepostas.
    "confidence_threshold" é a confiança de detecção necessária para considerar uma detecção positiva.


Agora coloque todas as imagens que você deseja testar seu modelo na pasta `/generate_input`.

Enquanto estiver no ambiente, execute `python generate.py` para iniciar a geração.

As imagens de saída com sobreposições de anotações, bem como os arquivos de texto de detecções, serão encontrados na pasta `/generate_output`.

[![image](https://private-user-images.githubusercontent.com/60541708/303944318-6d3a2c3c-5c84-4970-b016-59c6394e219a.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDc2OTI4NzAsIm5iZiI6MTcwNzY5MjU3MCwicGF0aCI6Ii82MDU0MTcwOC8zMDM5NDQzMTgtNmQzYTJjM2MtNWM4NC00OTcwLWIwMTYtNTljNjM5NGUyMTlhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAyMTElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMjExVDIzMDI1MFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQ1NTc1YzdiNGU0NTQwMTVhZTY4MDVlNGFkMjAxZmEzYjdlOGIyOTE1ZjhiMjkwYzc0ZGI5MmNiMDkxNWY0MDQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.tpuyfqN05gArpyQuakzCZ5LWbyJtq1mqkCQFI9WWzMk)](https://private-user-images.githubusercontent.com/60541708/303944318-6d3a2c3c-5c84-4970-b016-59c6394e219a.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDc2OTI4NzAsIm5iZiI6MTcwNzY5MjU3MCwicGF0aCI6Ii82MDU0MTcwOC8zMDM5NDQzMTgtNmQzYTJjM2MtNWM4NC00OTcwLWIwMTYtNTljNjM5NGUyMTlhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAyMTElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMjExVDIzMDI1MFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQ1NTc1YzdiNGU0NTQwMTVhZTY4MDVlNGFkMjAxZmEzYjdlOGIyOTE1ZjhiMjkwYzc0ZGI5MmNiMDkxNWY0MDQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.tpuyfqN05gArpyQuakzCZ5LWbyJtq1mqkCQFI9WWzMk)

Isso deve ser tudo. Agora você deve ter um modelo de detecção de imagens treinado que pode ser usado com a extensão ADetailer para A1111, ou nós similares no ComfyUI.

Há mais para aprender, você pode fazer modelos de segmentação, e o Yolov8 pode ser usado para coisas como contagem de objetos, detecção de mapas de calor, estimativas de velocidade, cálculos de distância e mais!

[Notas de Lançamento do Yolov8.1](https://www.ultralytics.com/blog/ultralytics-yolov8-turns-one-a-year-of-breakthroughs-and-innovations)

---

## Informações Adicionais

## Estrutura de Pastas

-   **dataset**: Contém suas imagens do conjunto de dados e arquivos de anotações (legendas), em subdiretórios.
    
    -   **train**: Contém seus dados de treinamento.
        
    -   **valid**: Contém seus dados de validação (comece movendo cerca de 10% dos seus dados de treinamento para cá).
        
    -   **test**: Contém seus dados de teste (opcional).
        
-   **generate_input**: Coloque aqui as imagens para testar sua detecção. _Nota: Estas também podem ser colocadas dentro de dataset/test, desde que você atualize o_ [_generate.py_](http://generate.py/) _para esse diretório._
    
-   **generate_output**: As detecções geradas e os arquivos de anotações de saída são colocados aqui.
    
-   **models**: Os modelos base baixados são colocados aqui. Coloque seus modelos aqui ao gerar.
    
-   **training_output**: É aqui que os modelos treinados (pesos), curvas, resultados e testes terminarão após o treinamento.


## Scripts

Lembre-se de entrar no ambiente para executar os scripts.

[train.py](http://train.py/): O script de treinamento. Configure o nome da pasta de treinamento, contagem de épocas, tamanho do lote e modelo inicial. Ele requer que o conjunto de dados esteja devidamente configurado.

[generate.py](http://generate.py/): O script de inferência usado para gerar resultados de detecção. Configure o nome do modelo, quais classes detectar, substituições de classes.

[yoloOutputCopyMatchingImages.py](http://yolooutputcopymatchingimages.py/): Este script é uma pequena ferramenta para ajudar a selecionar e copiar imagens de uma pasta, com base na correspondência de nomes de imagens de outra pasta. Exemplo:

Você tem uma pasta com imagens de entrada (original) para detectar algo.

Você executa um modelo de detecção e obtém outra pasta com sobreposições mostrando a detecção.

Você então executa uma ferramenta como o visualizador img-txt para remover pares de imagens-texto de detecções falhas.

Agora você tem uma pasta com apenas detecções bem-sucedidas (curadas). É agora que este script entra em ação.

Você executa a ferramenta e escolhe esses diretórios, e ela copiará quaisquer imagens originais que correspondam aos nomes das imagens que estão na pasta curada.

Você pode então executar o script ([yoloOutputToYoloAnnotations.py](http://yolooutputtoyoloannotations.py/)) para converter as coordenadas de detecção de saída yolo em anotações de treinamento yolo.

[yoloOutputToYoloAnnotations.py](http://yolooutputtoyoloannotations.py/): Este script converte arquivos de texto de detecção de saída yolo em arquivos de anotação de treinamento yolo. É usado junto com o script ([yoloOutputCopyMatchingImages.py](http://yolooutputcopymatchingimages.py/)) para gerar dados de treinamento a partir de detecções bem-sucedidas.

[cocoGetClasses.py](http://cocogetclasses.py/): Extrai os nomes das classes de um conjunto de dados baixado no formato COCO e os exporta no formato de treinamento Yolo. Coloque o script na mesma pasta que \_annotations.coco.json e execute o script. Pode ser executado fora do venv.

[cocoToYoloAnnotations.py](http://cocotoyoloannotations.py/): Converte anotações do formato COCO para o formato de treinamento Yolo. Coloque o script na mesma pasta que \_annotations.coco.json e execute o script. Pode ser executado fora do venv.

---

### Leitura Adicional / Alternativa

[https://civitai.com/articles/1224/training-a-custom-adetailer-model](https://civitai.com/articles/1224/training-a-custom-adetailer-model)
