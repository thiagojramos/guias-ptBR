# [Sintaxe básica de gravação e formatação no GitHub - GitHub Docs](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#links)

---

Criado em: 02 de July de 2024 às 12:14 (UTC: -03:00)
Tags: []
Autor: 

---

Crie formatação sofisticada para narração e código no GitHub com sintaxe simples.

## [Títulos](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#headings)

Para criar um título, adicione de um a seis símbolos # antes do texto do título. O número de # que você usa determina o nível hierárquico e o tamanho da face de tipos do título.

    # A first-level heading
    ## A second-level heading
    ### A third-level heading
    

![Captura de tela do GitHub Markdown renderizado mostrando exemplos de cabeçalhos h1, h2 e h3, que são descendentes em tamanho de tipo e peso visual para indicar um nível de hierarquia descendente.](https://docs.github.com/assets/cb-11407/images/help/writing/headings-rendered.png)

Quando você usa dois ou mais cabeçalhos, o GitHub gera automaticamente um sumário que pode ser acessado clicando em no cabeçalho do arquivo. Cada título do cabeçalho está listado na tabela de conteúdo e você pode clicar em um título para acessar a seção selecionada.

![Captura de tela do arquivo LEIAME no repositório de código aberto do GitHub Docs com o menu suspenso do sumário exposto. O ícone do sumário tem o contorno em laranja escuro.](https://docs.github.com/assets/cb-82863/images/help/repository/headings-toc.png)

## [Estilo do texto](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#styling-text)

Você pode indicar ênfase com texto em negrito, itálico, tachado, subscrito ou sobrescrito em campos de comentários e arquivos `.md`.

| Estilo | Sintaxe | Atalho do teclado | Exemplo | Saída |
| --- | --- | --- | --- | --- |
| Negrito | `** **` ou `__ __` | Comando+B (Mac) ou CTRL+B (Windows/Linux) | `**This is bold text**` | **Este texto está em negrito** |
| Itálico | `* *` ou `_ _`      | Comando+I (Mac) ou CTRL+I (Windows/Linux) | `_This text is italicized_` | _Este texto está em itálico_ |
| Tachado | `~~ ~~` | Nenhum | `~~This was mistaken text~~` | ~Este texto contém um erro~ |
| Negrito e itálico aninhado | `** **` e `_ _` | Nenhum | `**This text is _extremely_ important**` | **Este texto é _extremamente_ importante** |
| Todo em negrito e itálico | `*** ***` | Nenhum | `***All this text is important***` | _**Todo este texto é importante**_ |
| Subscrito | `<sub> </sub>` | Nenhum | `This is a <sub>subscript</sub> text` | Este é um texto <sub>subscrito</sub> |
| Sobrescrito | `<sup> </sup>` | Nenhum | `This is a <sup>superscript</sup> text` | Este é um texto <sup>sobrescrito</sup> |

## [Texto de referência](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#quoting-text)

Você pode citar um texto com \>.

    Text that is not a quote
    
    > Text that is a quote
    

O texto citado é recuado, com uma cor de tipo diferente.

![Captura de tela do GitHub Markdown renderizado mostrando um exemplo de texto citado. A citação é recuada com uma linha vertical à esquerda e o texto é cinza escuro em vez de preto.](https://docs.github.com/assets/cb-13462/images/help/writing/quoted-text-rendered.png)

**Observação:** ao ver uma conversa, você pode citar automaticamente o texto em um comentário realçando o texto e digitando R. Cite um comentário inteiro clicando em e em **Resposta de citação**. Para obter mais informações sobre atalhos de teclado, confira "[Atalhos do teclado](https://docs.github.com/pt/get-started/accessibility/keyboard-shortcuts)".

## [Citar código](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#quoting-code)

Você pode chamar código ou um comando em uma frase com aspas simples. O texto entre as aspas não será formatado. Você também pode pressionar o atalho de teclado Comando+E (Mac) ou Ctrl+E (Windows/Linux) para inserir os acentos graves para um bloco de código dentro de uma linha de Markdown.

    Use `git status` to list all new or modified files that haven't yet been committed.
    

![Captura de tela do GitHub Markdown renderizado mostrando a aparência dos caracteres cercados por aspas invertidas. As palavras "git status" aparecem em uma face de tipos de largura fixa realçadas em cinza claro.](https://docs.github.com/assets/cb-24556/images/help/writing/inline-code-rendered.png)

Para formatar código ou texto no próprio bloco distinto, use aspas triplas.

    Some basic Git commands are:
    ```
    git status
    git add
    git commit
    ```
    

![Captura de tela do GitHub Markdown renderizado mostrando um bloco de código. As palavras "git status", "git add" e "git commit" aparecem em uma face de tipos de largura fixa, realçadas em cinza claro.](https://docs.github.com/assets/cb-34231/images/help/writing/code-block-rendered.png)

Para obter mais informações, confira "[Criar e realçar blocos de código](https://docs.github.com/pt/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks)".

Se você estiver editando tabelas e snippets de código com frequência, poderá se beneficiar da habilitação de uma fonte de largura fixa em todos os campos de comentário no GitHub. Para obter mais informações, confira "[Sobre gravação e formatação no GitHub](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/about-writing-and-formatting-on-github#enabling-fixed-width-fonts-in-the-editor)".

## [Modelos de cores com suporte](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#supported-color-models)

Em problemas, solicitações de pull e discussões, você pode chamar cores dentro de uma frase usando aspas invertidas. Um modelo de cor com suporte em aspas invertidas exibirá uma visualização da cor.

    The background color is `#ffffff` for light mode and `#000000` for dark mode.
    

![Captura de tela do GitHub Markdown renderizado mostrando como valores hexadecimais dentro de aspas invertidas criam pequenos círculos de cor. #ffffff mostra um círculo branco e #000000 mostra um círculo preto.](https://docs.github.com/assets/cb-11643/images/help/writing/supported-color-models-rendered.png)

Veja abaixo os modelos de cores com suporte no momento.

| Cor | Sintaxe | Exemplo | Saída |
| --- | --- | --- | --- |
| HEX | `` `#RRGGBB` `` | `` `#0969DA` `` | ![Captura de tela do GitHub Markdown renderizado mostrando como o valor hexadecimal #0969DA aparece com um círculo azul.](https://docs.github.com/assets/cb-1558/images/help/writing/supported-color-models-hex-rendered.png) |
| RGB | `` `rgb(R,G,B)` `` | `` `rgb(9, 105, 218)` `` | ![Captura de tela do GitHub Markdown renderizado mostrando como os valores RGB 9, 105 e 218 aparecem com um círculo azul.](https://docs.github.com/assets/cb-1962/images/help/writing/supported-color-models-rgb-rendered.png) |
| HSL | `` `hsl(H,S,L)` `` | `` `hsl(212, 92%, 45%)` `` | ![Captura de tela do GitHub Markdown renderizado mostrando como os valores HSL 212, 92% e 45% aparecem com um círculo azul.](https://docs.github.com/assets/cb-2066/images/help/writing/supported-color-models-hsl-rendered.png) |

**Observações:**

-   Um modelo de cor com suporte não pode ter espaços à esquerda ou à direita dentro das aspas invertidas.
-   A visualização da cor só tem suporte em problemas, solicitações de pull e discussões.

## [Links](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#links)

Você pode criar um link embutido colocando o texto do link entre colchetes `[ ]` e colocando a URL entre parênteses `( )`. Também é possível usar o atalho de teclado Command+K para criar um link. Quando você tiver o texto selecionado, poderá colar uma URL da área de transferência para criar automaticamente um link por meio da seleção.

Você também pode criar um hiperlink Markdown realçando o texto e usando o atalho de teclado Command+V. Para substituir o texto pelo link, use o atalho de teclado Command+Shift+V.

`This site was built using [GitHub Pages](https://pages.github.com/).`

![Captura de tela do GitHub Markdown renderizado mostrando como o texto entre aspas invertidas, "GitHub Pages", aparece como um hiperlink azul.](https://docs.github.com/assets/cb-8312/images/help/writing/link-rendered.png)

**Observação:** o GitHub cria links automaticamente quando URLs válidas são escritas em um comentário. Para obter mais informações, confira "[URLs e referências autovinculadas](https://docs.github.com/pt/get-started/writing-on-github/working-with-advanced-formatting/autolinked-references-and-urls)".

## [Links de seção](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#section-links)

Você pode vincular diretamente a uma seção de um arquivo interpretado, passando o mouse sobre o título da seção para expor o .

![Captura de tela de um LEIAME de um repositório. À esquerda de um título de seção, um ícone de link é descrito em laranja escuro.](https://docs.github.com/assets/cb-55933/images/help/repository/readme-links.png)

## [Links relativos](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#relative-links)

É possível definir links relativos e caminhos de imagens em seus arquivos representados para ajudar os leitores a acessar outros arquivos no repositório.

Um link relativo é um link que é relativo ao arquivo atual. Por exemplo, se você tiver um arquivo LEIAME na raiz do repositório e tiver outro arquivo em _docs/CONTRIBUTING.md_, o link relativo para _CONTRIBUTING.md_ no LEIAME poderá ter esta aparência:

    [Contribution guidelines for this project](docs/CONTRIBUTING.md)
    

GitHub transformará automaticamente o seu link relativo ou caminho da imagem baseado em qualquer branch em que você estiver no momento para que o link ou caminho sempre funcione. O caminho do link será relativo ao arquivo atual. Links que começam com `/` serão relativos à raiz do repositório. Você pode usar todos os operandos de link relativos, como `./` e `../`.

O texto do link deve estar em uma única linha. O exemplo abaixo não funcionará.

    [Contribution 
    guidelines for this project](docs/CONTRIBUTING.md)
    

Os links relativos são mais fáceis para usuários que clonam o seu repositório. Os links absolutos podem não funcionar em clones do seu repositório - recomendamos usar links relativos para referir-se a outros arquivos no seu repositório.

## [Imagens](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#images)

Você pode exibir uma imagem adicionando ! e colocando o texto Alt entre `[ ]`. O texto alternativo é um texto curto equivalente às informações da imagem. Em seguida, coloque o link da imagem entre parênteses `()`.

`![Screenshot of a comment on a GitHub issue showing an image, added in the Markdown, of an Octocat smiling and raising a tentacle.](https://myoctocat.com/assets/images/base-octocat.svg)`

![Captura de tela de um comentário em um problema do GitHub mostrando uma imagem, adicionada no Markdown, de um octogato sorrindo e levantando um tentáculo.](https://docs.github.com/assets/cb-39744/images/help/writing/image-rendered.png)

O GitHub dá suporte à inserção de imagens em problemas, solicitações de pull, discussões, comentários e arquivos `.md`. Você pode exibir uma imagem do seu repositório, adicionar um link para uma imagem on-line ou fazer o upload de uma imagem. Para obter mais informações, confira "[Como carregar ativos](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#uploading-assets)".

**Observação:** para exibir uma imagem que está no repositório, use links relativos em vez de links absolutos.

Aqui estão alguns exemplos para usar links relativos para exibir uma imagem.

| Contexto | Link relativo |
| --- | --- |
| Em um arquivo `.md` no mesmo branch | `/assets/images/electrocat.png` |
| Em um arquivo `.md` em outro branch | `/../main/assets/images/electrocat.png` |
| Em problemas, pull requests e comentários do repositório | `../blob/main/assets/images/electrocat.png?raw=true` |
| Em um arquivo `.md` em outro repositório | `/../../../../github/docs/blob/main/assets/images/electrocat.png` |
| Em problemas, pull requests e comentários de outro repositório | `../../../github/docs/blob/main/assets/images/electrocat.png?raw=true` |

**Observação**: os dois últimos links relativos na tabela acima só funcionarão para imagens em um repositório privado se o visualizador tiver pelo menos acesso de leitura a esse repositório privado que contém as imagens.

Para obter mais informações, confira [Links relativos](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#relative-links)".

### [Especificando o tema para o qual uma imagem será exibida](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#specifying-the-theme-an-image-is-shown-to)

Você pode especificar o tema para o qual uma imagem é exibida no Markdown usando o elemento HTML `<picture>` em combinação com o recurso de mídia `prefers-color-scheme`. Nós distinguimos entre os modos de cores claro e escuro. Portanto, há duas opções disponíveis. Você pode usar essas opções para exibir imagens otimizadas para fundos escuros ou claros. Isso é particularmente útil para imagens PNG transparentes.

Por exemplo, o seguinte código exibe uma imagem de sol para temas claros e uma lua para temas escuros:

    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://user-images.githubusercontent.com/25423296/163456776-7f95b81a-f1ed-45f7-b7ab-8fa810d529fa.png">
      <source media="(prefers-color-scheme: light)" srcset="https://user-images.githubusercontent.com/25423296/163456779-a8556205-d0a5-45e2-ac17-42d089e3c3f8.png">
      <img alt="Shows an illustrated sun in light mode and a moon with stars in dark mode." src="https://user-images.githubusercontent.com/25423296/163456779-a8556205-d0a5-45e2-ac17-42d089e3c3f8.png">
    </picture>
    

O método antigo de especificar imagens com base no tema, usando um fragmento acrescentado à URL (`#gh-dark-mode-only` ou `#gh-light-mode-only`), foi preterido e será removido em favor do novo método descrito acima.

## [Listas](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#lists)

Você pode criar uma lista não ordenada precedendo uma ou mais linhas de texto com \-, \* ou +.

    - George Washington
    * John Adams
    + Thomas Jefferson
    

![Captura de tela do GitHub Markdown renderizado mostrando uma lista com marcadores dos nomes dos três primeiros presidentes americanos.](https://docs.github.com/assets/cb-7925/images/help/writing/unordered-list-rendered.png)

Para ordenar a lista, coloque um número na frente de cada linha.

    1. James Madison
    2. James Monroe
    3. John Quincy Adams
    

![Captura de tela do GitHub Markdown renderizado mostrando uma lista numerada dos nomes do quarto, quinto e sexto presidentes americanos.](https://docs.github.com/assets/cb-8162/images/help/writing/ordered-list-rendered.png)

### [Listas aninhadas](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#nested-lists)

Você pode criar uma lista aninhada recuando um ou mais itens da lista abaixo de outro item.

Para criar uma lista aninhada usando o editor Web do GitHub ou um editor de texto que use uma fonte monoespaçada, como o [Visual Studio Code](https://code.visualstudio.com/), você pode alinhar a lista visualmente. Digite os caracteres de espaço na frente do item de lista aninhado até que o caractere do marcador da lista (\- ou \*) fique diretamente abaixo do primeiro caractere do texto no item acima dele.

    1. First list item
       - First nested list item
         - Second nested list item
    

**Observação**: no editor baseado na Web, você pode recuar uma ou mais linhas de texto ou desfazer o recuo realçando primeiro as linhas desejadas e, depois, usando Tab ou SHIFT+Tab, respectivamente.

![Captura de tela do Markdown no Visual Studio Code mostrando como os marcadores recuados se alinham verticalmente à primeira letra das linhas de texto superiores.](https://docs.github.com/assets/cb-7202/images/help/writing/nested-list-alignment.png)

![Captura de tela do GitHub Markdown renderizado mostrando um item numerado seguido por um item com marcadores aninhado um nível à direita e outro item com marcadores aninhado ainda mais à direita.](https://docs.github.com/assets/cb-7206/images/help/writing/nested-list-example-1.png)

Para criar uma lista aninhada no editor de comentários do GitHub, que não usa uma fonte monoespaçada, você pode observar o item da lista logo acima da lista aninhada e contar o número de caracteres que aparecem antes do conteúdo do item. Em seguida, digite esse número de caracteres de espaço na fonte do item da linha aninhada.

Neste exemplo, você pode adicionar um item de lista aninhada no item de lista `100. First list item` recuando o item de lista aninhada no mínimo cinco espaços, pois há cinco caracteres (`100 .`) antes de `First list item`.

    100. First list item
         - First nested list item
    

![Captura de tela do GitHub Markdown renderizado mostrando um item de lista precedido pelo número 100 seguido por um item com marcadores aninhado um nível à direita.](https://docs.github.com/assets/cb-4906/images/help/writing/nested-list-example-3.png)

Você pode criar vários níveis de listas aninhadas usando o mesmo método. Por exemplo, como o primeiro item de lista aninhada tem sete caracteres (`␣␣␣␣␣-␣`) antes do conteúdo da lista aninhada `First nested list item`, você precisa recuar o segundo item de lista aninhada com pelo menos mais dois caracteres (nove espaços no mínimo).

    100. First list item
           - First nested list item
             - Second nested list item
    

![Captura de tela do GitHub Markdown renderizado mostrando um item de lista precedido pelo número 100 seguido por um item com marcadores aninhado um nível à direita e outro item com marcadores aninhado ainda mais à direita.](https://docs.github.com/assets/cb-7734/images/help/writing/nested-list-example-2.png)

Para obter mais exemplos, confira a [Especificação do GitHub Flavored Markdown](https://github.github.com/gfm/#example-265).

## [Listas de tarefas](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#task-lists)

Para criar uma lista de tarefas, coloque um hífen e um espaço seguidos de `[ ]` antes dos itens de lista. Para marcar uma tarefa como concluída, use `[x]`.

    - [x] #739
    - [ ] https://github.com/octo-org/octo-repo/issues/740
    - [ ] Add delight to the experience when all tasks are complete :tada:
    

![Captura de tela mostrando a versão renderizada do markdown. As referências a problemas são renderizadas como títulos de problema.](https://docs.github.com/assets/cb-64626/images/help/writing/task-list-rendered-simple.png)

Se a descrição de um item da lista de tarefas começar com parênteses, você precisará fazer escape dele com \\:

`- [ ] \(Optional) Open a followup issue`

Para obter mais informações, confira "[Sobre listas de tarefas](https://docs.github.com/pt/get-started/writing-on-github/working-with-advanced-formatting/about-task-lists)".

## [Mencionar pessoas e equipes](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#mentioning-people-and-teams)

Você pode mencionar uma pessoa ou uma [equipe](https://docs.github.com/pt/organizations/organizing-members-into-teams) no GitHub digitando @ mais seu nome de usuário ou o nome da equipe. Isto desencadeará uma notificação e chamará a sua atenção para a conversa. As pessoas também receberão uma notificação se você editar um comentário para mencionar o respectivo nome de usuário ou da equipe. Para obter mais informações sobre notificações, confira "[Sobre notificações](https://docs.github.com/pt/account-and-profile/managing-subscriptions-and-notifications-on-github/setting-up-notifications/about-notifications)".

**Observação:** Uma pessoa será notificada sobre uma menção somente se ela tiver acesso de leitura ao repositório e, caso o repositório pertença a uma organização, se ela for membro da organização.

`@github/support What do you think about these updates?`

![Captura de tela do GitHub Markdown renderizado mostrando como a menção da equipe "@github/support" é renderizada como um texto clicável e em negrito.](https://docs.github.com/assets/cb-6949/images/help/writing/mention-rendered.png)

Quando você menciona uma equipe principal, os integrantes de suas equipes secundárias também recebem notificações, simplificando a comunicação com vários grupos de pessoas. Para obter mais informações, confira "[Sobre equipes](https://docs.github.com/pt/organizations/organizing-members-into-teams/about-teams)".

Se você digitar um símbolo @, uma lista de pessoas ou de equipes em um projeto será exibida. A lista é filtrada à medida que você digita. Portanto, assim que você achar o nome da pessoa ou da equipe que está procurando, use as teclas de seta para selecioná-lo e pressione tab ou enter para completar o nome. Para equipes, insira o @organization/team-name e todos os membros dessa equipe serão inscritos na conversa.

Os resultados do preenchimento automático são restritos aos colaboradores do repositório e qualquer outro participante no thread.

## [Fazer referências a problemas e pull requests](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#referencing-issues-and-pull-requests)

Você pode mostrar uma lista de solicitações de pull e problemas sugeridos no repositório digitando #. Digite o número ou o título do problema ou da pull request para filtrar a lista e, em seguida, pressione tab ou enter para completar o resultado destacado.

Para obter mais informações, confira "[URLs e referências autovinculadas](https://docs.github.com/pt/get-started/writing-on-github/working-with-advanced-formatting/autolinked-references-and-urls)".

## [Fazer referência a recursos externos](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#referencing-external-resources)

Se as referências de link automático personalizado estão configuradas para um repositório, referências a recursos externos, como um problema do JIRA ou um ticket do Zendesk, serão convertidas em links encurtados. Para saber quais links automáticos estão disponíveis no repositório, entre em contato com alguém com permissões de administrador no repositório. Para obter mais informações, confira "[Configurar links automáticos para fazer referência a recursos externos](https://docs.github.com/pt/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/configuring-autolinks-to-reference-external-resources)".

## [Fazer upload de ativos](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#uploading-assets)

Você pode fazer upload de ativos como imagens, arrastando e soltando, fazendo a seleção a partir de um navegador de arquivos ou colando. Carregue ativos em problemas, solicitações de pull, comentários e arquivos `.md` no seu repositório.

## [Usar emojis](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#using-emojis)

É possível adicionar um emoji à escrita digitando `:EMOJICODE:`, dois pontos e, em seguida, o nome do emoji.

`@octocat :+1: This PR looks great - it's ready to merge! :shipit:`

![Captura de tela do GitHub Markdown renderizado mostrando como os códigos de emoji para +1 e shipit são renderizados visualmente como emojis.](https://docs.github.com/assets/cb-17228/images/help/writing/emoji-rendered.png)

Se você digitar :, uma lista de emojis sugeridos será exibida. A lista será filtrada à medida que você digitar algo. Portanto, assim que encontrar o emoji que estava procurando, pressione **Tab** ou **ENTER** para completar o resultado realçado.

Para obter uma lista completa de emojis e códigos disponíveis, confira [Emoji-Cheat-Sheet](https://github.com/ikatyang/emoji-cheat-sheet/blob/master/README.md).

## [Parágrafos](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#paragraphs)

Você pode criar um parágrafo deixando uma linha em branco entre as linhas de texto.

Você pode adicionar notas de rodapé ao seu conteúdo usando esta sintaxe entre colchetes:

    Here is a simple footnote[^1].
    
    A footnote can also have multiple lines[^2].
    
    [^1]: My reference.
    [^2]: To add line breaks within a footnote, prefix new lines with 2 spaces.
      This is a second line.
    

A nota de rodapé será interpretada da seguinte forma:

![Captura de tela do Markdown renderizado mostrando números sobrescritos que são usados para indicar notas de rodapé, com quebras de linha opcionais em uma nota.](https://docs.github.com/assets/cb-27017/images/help/writing/footnote-rendered.png)

**Observação**: a posição de uma nota de rodapé no Markdown não influenciará o lugar em que a nota de rodapé será renderizada. Você pode escrever uma nota de rodapé logo após sua referência à nota de rodapé, e ela continuará sendo interpretada na parte inferior do Markdown.

Não há suporte para notas de rodapé em wikis.

## [Alertas](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#alerts)

Alertas são uma extensão Markdown baseada na sintaxe blockquote que você pode usar para enfatizar informações críticas. Em GitHub, eles são exibidos com cores e ícones distintos para indicar a importância do conteúdo.

Use alertas apenas quando eles forem cruciais para o sucesso do usuário e limite-os a um ou dois por artigo para evitar sobrecarregar o leitor. Além disso, você deve evitar colocar alertas consecutivamente. Os alertas não podem ser aninhados em outros elementos.

Para adicionar um alerta, use uma linha de citação especial especificando o tipo de alerta, seguida pelas informações do alerta em uma citação padrão. Cinco tipos de alertas estão disponíveis:

    > [!NOTE]
    > Useful information that users should know, even when skimming content.
    
    > [!TIP]
    > Helpful advice for doing things better or more easily.
    
    > [!IMPORTANT]
    > Key information users need to know to achieve their goal.
    
    > [!WARNING]
    > Urgent info that needs immediate user attention to avoid problems.
    
    > [!CAUTION]
    > Advises about risks or negative outcomes of certain actions.
    

Aqui estão os alertas renderizados:

![Captura de tela de alertas de Markdown renderizados mostrando como Observação, Dica, Importante, Aviso e Cuidado são renderizados com texto e ícones de cores diferentes.](https://docs.github.com/assets/cb-24696/images/help/writing/alerts-rendered.png)

## [Ocultando o conteúdo com comentários](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#hiding-content-with-comments)

Você pode dizer a GitHub para ocultar o conteúdo do markdown interpretado, colocando o conteúdo em um comentário HTML.

    <!-- This content will not appear in the rendered Markdown -->
    

## [Ignorar formatação markdown](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#ignoring-markdown-formatting)

Instrua o GitHub a ignorar a formatação Markdown (ou fazer escape dela) usando \\ antes do caractere Markdown.

`Let's rename \*our-new-project\* to \*our-old-project\*.`

![Captura de tela do GitHub Markdown renderizado mostrando como as barras invertidas impedem a conversão de asteriscos em itálicos. O texto diz: "Vamos renomear nosso-novo-projeto para nosso-antigo-projeto".](https://docs.github.com/assets/cb-5440/images/help/writing/escaped-character-rendered.png)

Para saber mais sobre barras invertidas, confira "[Sintaxe de Markdown](https://daringfireball.net/projects/markdown/syntax#backslash)" da Daring Fireball.

**Observação**: a formatação markdown não será ignorada no título de um problema ou de uma solicitação de pull.

## [Desabilitar a interpretação do Markdown](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#disabling-markdown-rendering)

Ao visualizar um arquivo Markdown, você pode clicar em **Código** na parte superior do arquivo para desabilitar a renderização do Markdown e, em vez disso, visualizar o código-fonte do arquivo.

![Captura de tela de um arquivo Markdown em um repositório do GitHub mostrando as opções para interagir com o arquivo. Um botão, rotulado "Código", está destacado em laranja-escuro.](https://docs.github.com/assets/cb-11496/images/help/writing/display-markdown-as-source-global-nav-update.png)

Se você desabilitar a renderização do Markdown, poderá usar recursos de exibição de origem, como vinculação de linha, o que não é possível ao exibir arquivos Markdown renderizados.

## [Leitura adicional](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#further-reading)

-   [Especificações do GitHub Flavored Markdown](https://github.github.com/gfm/)
-   "[Sobre gravação e formatação no GitHub](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/about-writing-and-formatting-on-github)"
-   "[Trabalhar com formatação avançada](https://docs.github.com/pt/get-started/writing-on-github/working-with-advanced-formatting)"
-   "[Guia de início rápido de gravação no GitHub](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/quickstart-for-writing-on-github)"
