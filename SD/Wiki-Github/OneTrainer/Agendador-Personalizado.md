Esta página é sobre o recurso de agendador personalizado encontrado na aba de treinamento. Esta opção permite que você use um agendador que não está atualmente no OneTrainer, mas pode existir em outros lugares, por exemplo, no PyTorch. Este recurso foi adicionado principalmente para permitir o uso do agendador OneCycle.

# Seleção

Para usar o agendador personalizado, você precisa selecioná-lo na aba de treinamento.

![imagem](https://github.com/Nerogar/OneTrainer/assets/132208482/1e5d02c2-e629-4dec-b883-aa344c464f7f)

# Configuração

Para realmente definir o agendador personalizado, você precisa clicar nos três pontos (...) que abrirão a janela do agendador personalizado.

![imagem](https://github.com/Nerogar/OneTrainer/assets/132208482/3d465c19-a42c-4c66-b683-464cb169c737)

## Configurações

Nesta janela, normalmente você será apresentado a um modelo em branco que você precisará preencher com informações para usar o agendador personalizado de sua escolha. Existem dicas de ferramenta para a maioria dos campos se você passar o mouse sobre eles.

* Nome da Classe (Padrão: em branco) - Aqui você define o módulo e o nome da classe do agendador que deseja usar. Neste exemplo, é torch.optim.lr_scheduler.MultiStepLR
* adicionar parâmetro - Este botão adicionará dois campos em branco. O nome do parâmetro e o valor que seu agendador personalizado está procurando. Existem algumas variáveis do OneTrainer que você pode passar, conforme mostrado na dica de ferramenta.

No exemplo acima, dois parâmetros são passados para o agendador personalizado MultiStepLR. O primeiro são os marcos com valores de 50, 100, 150 e o gamma de 0,5. Neste exemplo, o agendador personalizado reduzirá a taxa de aprendizado em 0,5 em etapas de 50, 100 e 150 (após o aquecimento ser concluído, se você usá-lo).
Isso criaria as seguintes taxas de aprendizado como exemplo para uma sintonia pontual.

| Etapa | LoRA Unet LR | Embedding LR  |
|-------|--------------|---------------|
| 0     | 1e-4         | 1e-3          |
| 50    | 5e-5         | 5e-4          |
| 100   | 2.5e-5       | 2.5e-4        |
| 150+  | 1.25e-5      | 1.25e-4       |

# Agendadores Personalizados

O PyTorch inclui agendadores personalizados que não estão incluídos no OneTrainer. Eles podem ser encontrados aqui: (https://pytorch.org/docs/stable/optim.html#how-to-adjust-learning-rate)

Cada agendador listado aqui mostrará o nome e os parâmetros que ele procura.

Alguns Exemplos:
* MultiStep Linear: https://pytorch.org/docs/stable/generated/torch.optim.lr_scheduler.MultiStepLR.html#torch.optim.lr_scheduler.MultiStepLR
* OneCycle: https://pytorch.org/docs/stable/generated/torch.optim.lr_scheduler.OneCycleLR.html#torch.optim.lr_scheduler.OneCycleLR

# Leitura Adicional
O Medium tem vários artigos sobre os agendadores, incluindo o OneCycle, mas alguns estão atrás de um paywall.

Esta página aprofunda-se no OneCycle: https://www.deepspeed.ai/tutorials/one-cycle/

Esta imagem mostra como cada agendador no PyTorch se parece graficamente:

![imagem](https://github.com/Nerogar/OneTrainer/assets/132208482/62788047-0fff-4d05-a95e-432e615d6947)
