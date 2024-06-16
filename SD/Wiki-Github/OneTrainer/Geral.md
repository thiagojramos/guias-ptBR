Nesta guia, você define os diretórios usados pelo OneTrainer.

![imagem](https://github.com/Nerogar/OneTrainer/assets/132208482/8e059014-6b15-48cd-98e2-81db535218ec)

* Diretório de Trabalho (padrão - workspace/run): obrigatório, este diretório conterá as amostras geradas durante um treinamento, as configurações de treinamento, os backups e os logs do tensorboard. Você pode usar uma única pasta ou criar uma pasta por projeto. A segunda opção pode ser útil se você trabalhar em vários projetos/tentativas.
* Diretório de Cache (padrão - workspace-cache/run): obrigatório, usado durante o tempo de execução para armazenar suas imagens e textos em cache.
* Continuar do último backup (padrão - desligado): quando ativado, usará o último backup do espaço de trabalho selecionado.
* Modo de Depuração (padrão - desligado): ativa o modo de depuração. Isso fornecerá dados e imagens do que o OneTrainer está fazendo, incluindo as imagens (previstas vs reais) da etapa de treinamento.
* Diretório de Depuração (padrão - debug): opcional, necessário apenas se você ativar o modo de depuração.
* Tensorboard (padrão - ligado): define se o Tensorboard será iniciado quando o treinamento começar. O Tensorboard é usado para ver estatísticas das suas execuções de treinamento. Todas as execuções do Tensorboard em um espaço de trabalho atual estarão disponíveis para visualização.
* Expor Tensorboard (padrão - desligado): isso moverá o Tensorboard do localhost para ser exposto ao restante da rede.
* Dispositivo de Treinamento (padrão - cuda): um campo de texto livre para escolher a GPU para treinar. O treinamento em multi-GPU não é possível, mas é possível escolher qual GPU usar (exemplo: cuda:1).
* Dispositivo Temporário (padrão - cpu): um campo de texto livre para escolher onde os modelos residirão quando não estiverem em uso. O padrão é a memória da CPU (RAM). Para desativar esta opção, pode-se usar o dispositivo de treinamento (ou seja, cuda).