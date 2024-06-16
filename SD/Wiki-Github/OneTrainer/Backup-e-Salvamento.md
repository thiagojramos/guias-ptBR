# A Aba de Backup

À medida que os modelos se tornam maiores e mais complexos e as GPUs de consumo maiores se tornam disponíveis, é importante esclarecer a diferença entre um backup e um salvamento.

* **Backup** - Este é um backup de todos os componentes internos que o OneTrainer utiliza em um determinado período de tempo. Ele é destinado a ser algo que você pode usar para reiniciar o OneTrainer nesse ponto. Alterar certas funções pode exigir a entrada de Nero ou de outras pessoas para obter as alterações exatas sobre como fazer isso. O principal caso de uso para backups são FineTunes com horas e horas de trabalho, onde você não quer começar do início novamente se as coisas não saírem como o esperado.
* **Salvar** - Se você deseja testar seu trabalho durante etapas intermediárias do treinamento, então essas são as configurações nas quais você deve se concentrar.

## Configurações Focadas em Backup
* **Backup após XY**. Isso criará um backup após X Épocas/Passos/Segundo/Minuto/Hora/Nunca/Sempre. Note que Nunca e Sempre são especiais e ignorarão seu número inteiro.
* **Fazer Backup agora**. Pressionar este botão instruirá o OneTrainer a fazer um backup na próxima oportunidade.
* **Backup Contínuo** - Uma configuração para economizar espaço de armazenamento. Habilitar isso irá automaticamente excluir backups à medida que novos forem criados.
* **Contagem de Backup Contínuo** - O número de backups que o OneTrainer manterá. Os mais antigos serão automaticamente excluídos se o Backup Contínuo estiver habilitado.
* **Backup Antes de Salvar** - Esta configuração vincula a criação de backup à criação de salvamento. Pode ser usada em conjunto com suas outras configurações de backup, ou você pode usar suas configurações de salvamento para controlar a criação do backup.

## Configurações Focadas em Salvamento
* **Salvar após XY**. Isso criará um salvamento após X Épocas/Passos/Segundo/Minuto/Hora/Nunca/Sempre. Note que Nunca e Sempre são especiais e ignorarão seu número inteiro.
* **Prefixo do Nome do Arquivo de Salvamento**. Um recurso solicitado para ajudar a organizar melhor os salvamentos. Isso adicionará um prefixo ao formato padrão de salvamento do OneTrainer.

## Notas e Comentários
* As mudanças nesta aba serão refletidas em um treinamento em andamento. Por exemplo, se você estiver na época 9 e quiser salvar na 10, se você fizer as alterações para salvar a cada 10 épocas, o OneTrainer salvará quando a época 9 for concluída.
* Os backups podem ser muito grandes e ocupar muito espaço rapidamente.
* Salvamentos para FineTunes (especialmente Stable Cascade ou Pixart Alpha) exigem muita RAM (24 GB ou mais). Isso pode fazer com que o arquivo de troca seja usado, o que causará uma diminuição no desempenho e pode fazer com que os salvamentos sejam muito lentos.