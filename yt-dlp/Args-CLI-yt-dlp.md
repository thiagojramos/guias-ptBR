
# yt-dlp - Argumentos CLI

`yt-dlp [OPTIONS] [--] URL [URL...]`

# Opções Gerais

| Argumento | Descrição |
|---|---|
| `-h, --help` | Exibe este texto de ajuda |
| `--version` | Exibe a versão do programa |
| `-U, --update` | Atualiza este programa para a versão mais recente |
| `--no-update` | Não verifica por atualizações (padrão) |
| `--update-to [CHANNEL]@[TAG]` | Atualiza/regride para uma versão específica. CHANNEL pode ser um repositório também. CHANNEL e TAG por padrão são "stable" e "latest" respectivamente se omitidos; Veja "UPDATE" para detalhes. Canais suportados: stable, nightly, master |
| `-i, --ignore-errors` | Ignora erros de download e pós-processamento. O download será considerado bem-sucedido mesmo que o pós-processamento falhe |
| `--no-abort-on-error` | Continua com o próximo vídeo em caso de erro no download; por exemplo, para pular vídeos indisponíveis em uma playlist (padrão) |
| `--abort-on-error` | Interrompe o download de outros vídeos se ocorrer um erro (Alias: `--no-ignore-errors`) |
| `--dump-user-agent` | Exibe o user-agent atual |
| `--list-extractors` | Lista todos os extractors suportados |
| `--extractor-descriptions` | Exibe descrições de todos os extractors suportados |
| `--use-extractors NAMES` | Nomes dos extractors a serem usados separados por vírgulas. Você também pode usar regexes, "all", "default" e "end" (fim da correspondência da URL); por exemplo, --ies "holodex.*,end,youtube". Prefixe o nome com um "-" para excluí-lo, por exemplo, `--ies default`,`-generic`. Use `--list-extractors` para uma lista de nomes de extractors. (Alias: --ies) |
| `--default-search PREFIX` | Usa este prefixo para URLs não qualificadas. Ex: "gvsearch2:python" baixa dois vídeos do Google Videos para o termo de busca "python". Use o valor "auto" para deixar o yt-dlp adivinhar ("auto_warning" para emitir um aviso ao adivinhar). "error" apenas lança um erro. O valor padrão "fixup_error" repara URLs quebradas, mas emite um erro se isso não for possível em vez de pesquisar |
| `--ignore-config` | Não carrega mais nenhum arquivo de configuração exceto aqueles dados a `--config-locations`. Para compatibilidade retroativa, se esta opção for encontrada dentro do arquivo de configuração do sistema, a configuração do usuário não é carregada. (Alias: `--no-config`) |
| `--no-config-locations` | Não carrega nenhum arquivo de configuração personalizado (padrão). Quando dado dentro de um arquivo de configuração, ignora todas as `--config-locations` definidas anteriormente no arquivo atual |
| `--config-locations PATH` | Localização do arquivo de configuração principal; pode ser o caminho para o arquivo de config ou seu diretório contido ("-" para stdin). Pode ser usado várias vezes e dentro de outros arquivos de configuração |
| `--flat-playlist` | Não extrai os vídeos de uma playlist, apenas os lista |
| `--no-flat-playlist` | Extrai completamente os vídeos de uma playlist (padrão) |
| `--live-from-start` | Baixa livestreams desde o início. Atualmente suportado apenas para YouTube (Experimental) |
| `--no-live-from-start` | Baixa livestreams a partir do horário atual (padrão) |
| `--wait-for-video MIN[-MAX]` | Aguarda até que as transmissões agendadas fiquem disponíveis. Passa o número mínimo de segundos (ou intervalo) para esperar entre as tentativas |
| `--no-wait-for-video` | Não espera por transmissões agendadas (padrão) |
| `--mark-watched` | Marca vídeos como assistidos (mesmo com `--simulate`) |
| `--no-mark-watched` | Não marca vídeos como assistidos (padrão) |
| `--color [STREAM:]POLICY` | Define se deve emitir códigos de cores na saída, opcionalmente prefixado pelo STREAM (stdout ou stderr) para aplicar a configuração. Pode ser um dos valores: "always", "auto" (padrão), "never" ou "no_color" (usa sequências de terminal sem cor). Pode ser usado várias vezes |
| `--compat-options OPTS` | Opções que podem ajudar a manter a compatibilidade com configurações do youtube-dl ou youtube-dlc revertendo algumas das mudanças feitas no yt-dlp. Veja "Differences in default behavior" para detalhes |
| `--alias ALIASES OPTIONS` | Cria aliases para uma string de opção. A menos que um alias comece com um hífen "-", ele é prefixado com "--". Argumentos são analisados de acordo com a mini-linguagem de formatação de strings do Python. Ex: --alias get-audio,-X "-S=aext:{0},abr -x --audio-format {0}" cria opções "--get-audio" e "-X" que aceitam um argumento (ARG0) e expandem para "-S=aext:ARG0,abr -x --audio-format ARG0". Todos os aliases definidos são listados na saída de --help. Opções de alias podem acionar mais aliases; portanto, tenha cuidado para evitar a definição de opções recursivas. Como medida de segurança, cada alias pode ser acionado no máximo 100 vezes. Esta opção pode ser usada várias vezes |
| `--proxy URL` | Usa o proxy HTTP/HTTPS/SOCKS especificado. Para habilitar proxy SOCKS, especifique um esquema apropriado, por exemplo, socks5://user:pass@127.0.0.1:1080/. Passe uma string vazia (`--proxy ""`) para conexão direta |
| `--socket-timeout SECONDS` | Tempo de espera antes de desistir, em segundos |
| `--source-address IP` | Endereço IP do lado do cliente para vincular |
| `--impersonate CLIENT[:OS]` | Cliente a ser personificado para solicitações. Ex: chrome, chrome-110, chrome:windows-10. Passe --impersonate="" para personificar qualquer cliente. Observe que forçar a personificação para todas as solicitações pode ter um impacto negativo na velocidade e estabilidade do download |
| `--list-impersonate-targets` | Lista os clientes disponíveis para personificação. |
| `-4, --force-ipv4` | Faz todas as conexões via IPv4 |
| `-6, --force-ipv6` | Faz todas as conexões via IPv6 |
| `--enable-file-urls` | Habilita URLs file://. Isso é desabilitado por padrão por razões de segurança. |

# Opções de Restrição Geográfica

| Argumento | Descrição |
|---|---|
| `--geo-verification-proxy URL` | Usa este proxy para verificar o endereço IP para alguns sites com restrição geográfica. O proxy padrão especificado por --proxy (ou nenhum, se a opção não estiver presente) é usado para o download real |
| `--xff VALUE` | Como falsificar o cabeçalho HTTP X-Forwarded-For para tentar contornar a restrição geográfica. Um dos valores: "default" (apenas quando se sabe ser útil), "never", um bloco de IP em notação CIDR ou um código de país ISO 3166-2 de duas letras |
| `-I, --playlist-items ITEM_SPEC` | Índice da playlist dos itens a serem baixados separados por vírgulas. Você pode especificar um intervalo usando "[START]:[STOP][:STEP]". Para compatibilidade retroativa, START-STOP também é suportado. Use índices negativos para contar da direita e STEP negativo para baixar na ordem inversa. Ex: "-I 1:3,7,-5::2" usado em uma playlist de tamanho 15 baixará os itens nos índices 1,2,3,7,11,13,15 |
| `--min-filesize SIZE` | Aborta o download se o tamanho do arquivo for menor que SIZE, por exemplo, 50k ou 44.6M |
| `--max-filesize SIZE` | Aborta o download se o tamanho do arquivo for maior que SIZE, por exemplo, 50k ou 44.6M |
| `--date DATE` | Baixa apenas vídeos enviados nesta data. A data pode ser "YYYYMMDD" ou no formato [now, today, yesterday][-N[day, week, month, year]]. Ex: "--date today-2weeks" baixa apenas vídeos enviados no mesmo dia duas semanas atrás |
| `--datebefore DATE` | Baixa apenas vídeos enviados até esta data. Os formatos de data aceitos são os mesmos de --date |
| `--dateafter DATE` | Baixa apenas vídeos enviados a partir desta data. Os formatos de data aceitos são os mesmos de --date |
| `--match-filters FILTER` | Filtro genérico de vídeos. Qualquer campo de "OUTPUT TEMPLATE" pode ser comparado com um número ou uma string usando os operadores definidos em "Filtering Formats". Você também pode simplesmente especificar um campo para verificar se o campo está presente, usar "!field" para verificar se o campo não está presente, e "&" para verificar múltiplas condições. Use uma "\" para escapar "&" ou aspas se necessário. Se usado várias vezes, o filtro corresponderá se pelo menos uma das condições for atendida. Ex: --match-filter !is_live --match-filter "like_count>?100 & description~='(?i)\bcats \& dogs\b'" corresponde apenas a vídeos que não estão ao vivo OU aqueles que têm uma contagem de curtidas superior a 100 (ou o campo like não está disponível) e também têm uma descrição que contém a frase "cats & dogs" (sem diferenciar maiúsculas de minúsculas). Use "--match-filter -" para perguntar interativamente se deseja baixar cada vídeo |
| `--no-match-filters` | Não usa nenhum --match-filter (padrão) |
| `--break-match-filters FILTER` | Igual a "--match-filters" mas interrompe o processo de download quando um vídeo é rejeitado |
| `--no-break-match-filters` | Não usa nenhum `--break-match-filters` (padrão) |
| `--no-playlist` | Baixa apenas o vídeo, se a URL se referir a um vídeo e uma playlist |
| `--yes-playlist` | Baixa a playlist, se a URL se referir a um vídeo e uma playlist |
| `--age-limit YEARS` | Baixa apenas vídeos adequados para a idade especificada |
| `--download-archive FILE` | Baixa apenas vídeos não listados no arquivo de arquivo. Registra os IDs de todos os vídeos baixados nele |
| `--no-download-archive` | Não usa arquivo de arquivo (padrão) |
| `--max-downloads NUMBER` | Aborta após baixar NUMBER arquivos |
| `--break-on-existing` | Interrompe o processo de download ao encontrar um arquivo que está no arquivo de arquivo |
| `--no-break-on-existing` | Não interrompe o processo de download ao encontrar um arquivo que está no arquivo de arquivo (padrão) |
| `--break-per-input` | Altera `--max-downloads`, `--break-on-existing`, `--break-match-filter`, e autonumber para redefinir por URL de entrada |
| `--no-break-per-input` | `--break-on-existing` e opções similares terminam toda a fila de download |
| `--skip-playlist-after-errors N` | Número de falhas permitidas até que o resto da playlist seja ignorado |

# Opções de Download

| Argumento | Descrição |
|---|---|
| `-N, --concurrent-fragments N` | Número de fragmentos de um vídeo dash/hlsnative que devem ser baixados simultaneamente (padrão é 1) |
| `-r, --limit-rate RATE` | Taxa máxima de download em bytes por segundo, por exemplo, 50K ou 4.2M |
| `--throttled-rate RATE` | Taxa mínima de download em bytes por segundo abaixo da qual o throttling é assumido e os dados do vídeo são re-extraídos, por exemplo, 100K |
| `-R, --retries RETRIES` | Número de tentativas (padrão é 10), ou "infinite" |
| `--file-access-retries RETRIES` | Número de vezes para tentar em caso de erro de acesso ao arquivo (padrão é 3), ou "infinite" |
| `--fragment-retries RETRIES` | Número de tentativas para um fragmento (padrão é 10), ou "infinite" (DASH, hlsnative e ISM) |
| `--retry-sleep [TYPE:]EXPR` | Tempo de espera entre as tentativas em segundos (opcionalmente) prefixado pelo tipo de tentativa (http (padrão), fragment, file_access, extractor) para aplicar a espera. EXPR pode ser um número, linear=START[:END[:STEP=1]] ou exp=START[:END[:BASE=2]]. Esta opção pode ser usada várias vezes para definir o tempo de espera para os diferentes tipos de tentativa, por exemplo, --retry-sleep linear=1::2 --retry-sleep fragment:exp=1:20 |
| `--skip-unavailable-fragments` | Pula fragmentos indisponíveis para downloads DASH, hlsnative e ISM (padrão) (Alias: --no-abort-on-unavailable-fragments) |
| `--abort-on-unavailable-fragments` | Aborta o download se um fragmento estiver indisponível (Alias: `--no-skip-unavailable-fragments`) |
| `--keep-fragments` | Mantém os fragmentos baixados no disco após o download ser concluído |
| `--no-keep-fragments` | Exclui os fragmentos baixados após o download ser concluído (padrão) |
| `--buffer-size SIZE` | Tamanho do buffer de download, por exemplo, 1024 ou 16K (padrão é 1024) |
| `--resize-buffer` | O tamanho do buffer é redimensionado automaticamente a partir de um valor inicial de `--buffer-size` (padrão) |
| `--no-resize-buffer` | Não ajusta automaticamente o tamanho do buffer |
| `--http-chunk-size SIZE` | Tamanho de um chunk para download HTTP baseado em chunks, por exemplo, 10485760 ou 10M (padrão é desativado). Pode ser útil para contornar a limitação de largura de banda imposta por um servidor web (experimental) |
| `--playlist-random` | Baixa vídeos da playlist em ordem aleatória |
| `--lazy-playlist` | Processa as entradas na playlist conforme elas são recebidas. Isso desativa n_entries, --playlist-random e --playlist-reverse |
| `--no-lazy-playlist` | Processa vídeos na playlist apenas após toda a playlist ser analisada (padrão) |
| `--xattr-set-filesize` | Define o xattribute ytdl.filesize do arquivo com o tamanho esperado do arquivo |
| `--hls-use-mpegts` | Usa o contêiner mpegts para vídeos HLS; permitindo que alguns players reproduzam o vídeo durante o download, e reduzindo a chance de corrupção do arquivo se o download for interrompido. Isso é habilitado por padrão para transmissões ao vivo |
| `--no-hls-use-mpegts` | Não usa o contêiner mpegts para vídeos HLS. Isso é padrão quando não está baixando transmissões ao vivo |
| `--download-sections REGEX` | Baixa apenas capítulos que correspondam à expressão regular. Um prefixo "*" denota intervalo de tempo em vez de capítulo. Timestamps negativos são calculados a partir do final. "*from-url" pode ser usado para baixar entre o "start_time" e "end_time" extraídos da URL. Requer ffmpeg. Esta opção pode ser usada várias vezes para baixar várias seções, por exemplo, `--download-sections "*10:15-inf"` // `--download-sections "intro"` |
| `--downloader [PROTO:]NAME` | Nome ou caminho do downloader externo a ser usado (opcionalmente) prefixado pelos protocolos (http, ftp, m3u8, dash, rstp, rtmp, mms) para usá-lo. Atualmente suporta native, aria2c, avconv, axel, curl, ffmpeg, httpie, wget. Você pode usar esta opção várias vezes para definir diferentes downloaders para diferentes protocolos. Ex: --downloader aria2c --downloader "dash,m3u8:native" usará aria2c para downloads http/ftp, e o downloader nativo para downloads dash/m3u8 (Alias: `--external-downloader`) |
| `--downloader-args NAME:ARGS` | Passa esses argumentos para o downloader externo. Especifica o nome do downloader e os argumentos separados por dois pontos ":". Para ffmpeg, argumentos podem ser passados para diferentes posições usando a mesma sintaxe de `--postprocessor-args`. Você pode usar esta opção várias vezes para dar diferentes argumentos para diferentes downloaders (Alias: `--external-downloader-args`) |


# Opções de Sistema de Arquivos

| Argumento | Descrição |
|---|---|
| `-a, --batch-file FILE` | Arquivo contendo URLs para baixar ("-" para stdin), uma URL por linha. Linhas começando com "#", ";" ou "]" são consideradas como comentários e ignoradas |
| `--no-batch-file` | Não lê URLs do arquivo batch (padrão) |
| `-P, --paths [TYPES:]PATH` | Os caminhos onde os arquivos devem ser baixados. Especifique o tipo de arquivo e o caminho separados por dois pontos ":". Todos os mesmos TYPES de --output são suportados. Adicionalmente, você também pode fornecer "home" (padrão) e caminhos "temp". Todos os arquivos intermediários são baixados primeiro para o caminho temp e então os arquivos finais são movidos para o caminho home após o download ser concluído. Esta opção é ignorada se --output for um caminho absoluto |
| `-o, --output [TYPES:]TEMPLATE` | Template do nome do arquivo de saída; veja "OUTPUT TEMPLATE" para detalhes |
| `--output-na-placeholder TEXT` | Placeholder para campos indisponíveis em --output (padrão: "NA") |
| `--restrict-filenames` | Restringe nomes de arquivos a apenas caracteres ASCII, e evita "&" e espaços nos nomes de arquivos |
| `--no-restrict-filenames` | Permite caracteres Unicode, "&" e espaços nos nomes de arquivos (padrão) |
| `--windows-filenames` | Força nomes de arquivos compatíveis com Windows |
| `--no-windows-filenames` | Torna nomes de arquivos compatíveis com Windows apenas se estiver usando Windows (padrão) |
| `--trim-filenames LENGTH` | Limita o comprimento do nome do arquivo (excluindo a extensão) ao número especificado de caracteres |
| `-w, --no-overwrites` | Não sobrescreve nenhum arquivo |
| `--force-overwrites` | Sobrescreve todos os arquivos de vídeo e metadados. Esta opção inclui `--no-continue` |
| `--no-force-overwrites` | Não sobrescreve o vídeo, mas sobrescreve arquivos relacionados (padrão) |
| `-c, --continue` | Continua downloads de arquivos/fragmentos parcialmente baixados (padrão) |
| `--no-continue` | Não continua fragmentos parcialmente baixados. Se o arquivo não estiver fragmentado, reinicia o download do arquivo inteiro |
| `--part` | Usa arquivos .part em vez de escrever diretamente no arquivo de saída (padrão) |
| `--no-part` | Não usa arquivos .part - escreve diretamente no arquivo de saída |
| `--mtime` | Usa o cabeçalho Last-modified para definir o tempo de modificação do arquivo (padrão) |
| `--no-mtime` | Não usa o cabeçalho Last-modified para definir o tempo de modificação do arquivo |
| `--write-description` | Escreve a descrição do vídeo em um arquivo .description |
| `--no-write-description` | Não escreve a descrição do vídeo (padrão) |
| `--write-info-json` | Escreve metadados do vídeo em um arquivo .info.json (isso pode conter informações pessoais) |
| `--no-write-info-json` | Não escreve metadados do vídeo (padrão) |
| `--write-playlist-metafiles` | Escreve metadados da playlist além dos metadados do vídeo ao usar `--write-info-json`, `--write-description` etc. (padrão) |
| `--no-write-playlist-metafiles` | Não escreve metadados da playlist ao usar `--write-info-json`, `--write-description` etc. |
| `--clean-info-json` | Remove alguns metadados internos, como nomes de arquivos, do infojson (padrão) |
| `--no-clean-info-json` | Escreve todos os campos no infojson |
| `--write-comments` | Recupera comentários do vídeo para serem colocados no infojson. Os comentários são buscados mesmo sem esta opção se a extração for conhecida por ser rápida (Alias: `--get-comments`) |
| `--no-write-comments` | Não recupera comentários do vídeo, a menos que a extração seja conhecida por ser rápida (Alias: --no-get-comments) |
| `--load-info-json FILE` | Arquivo JSON contendo as informações do vídeo (criado com a opção "`--write-info-json`") |
| `--cookies FILE` | Arquivo formatado Netscape para ler cookies e salvar cookie jar |
| `--no-cookies` | Não lê/salva cookies de/para arquivo (padrão) |
| `--cookies-from-browser BROWSER[+KEYRING][:PROFILE][::CONTAINER]` | Nome do navegador para carregar cookies Atualmente, navegadores suportados são: brave, chrome, chromium, edge, firefox, opera, safari, vivaldi, whale. Opcionalmente, o KEYRING usado para descriptografar cookies do Chromium no Linux, o nome/caminho do PROFILE para carregar cookies e o nome do CONTAINER (se Firefox) ("none" para nenhum container) podem ser fornecidos com seus respectivos separadores. Por padrão, todos os containers do perfil mais recentemente acessado são usados. Keyrings suportados atualmente são: basictext, gnomekeyring, kwallet, kwallet5, kwallet6 |
| `--no-cookies-from-browser` | Não carrega cookies do navegador (padrão) |
| `--cache-dir DIR` | Local no sistema de arquivos onde o yt-dlp pode armazenar algumas informações baixadas (como IDs de clientes e assinaturas) permanentemente. Por padrão, ${XDG_CACHE_HOME}/yt-dlp |
| `--no-cache-dir` | Desativa o cache do sistema de arquivos |
| `--rm-cache-dir` | Exclui todos os arquivos de cache do sistema de arquivos |
| `--write-thumbnail` | Escreve a imagem de miniatura no disco |
| `--no-write-thumbnail` | Não escreve a imagem de miniatura no disco (padrão) |
| `--write-all-thumbnails` | Escreve todos os formatos de miniatura no disco |
| `--list-thumbnails` | Lista as miniaturas disponíveis de cada vídeo. Simula a menos que `--no-simulate` seja usado |

# Opções de Atalho de Internet

| Argumento | Descrição |
|---|---|
| `--write-link` | Escreve um arquivo de atalho de internet, dependendo da plataforma atual (.url, .webloc ou .desktop). A URL pode ser armazenada em cache pelo SO |
| `--write-url-link` | Escreve um atalho de internet .url do Windows. O SO armazena a URL em cache com base no caminho do arquivo |
| `--write-webloc-link` | Escreve um atalho de internet .webloc do macOS |
| `--write-desktop-link` | Escreve um atalho de internet .desktop do Linux |

# Opções de Verbosidade e Simulação


| Argumento | Descrição |
|---|---|
| `-q, --quiet` | Ativa o modo silencioso. Se usado com `--verbose`, imprime o log para stderr |
| `--no-quiet` | Desativa o modo silencioso. (Padrão) |
| `--no-warnings` | Ignora avisos |
| `-s, --simulate` | Não baixa o vídeo e não escreve nada no disco |
| `--no-simulate` | Baixa o vídeo mesmo que opções de impressão/listagem sejam usadas |
| `--ignore-no-formats-error` | Ignora o erro "No video formats". Útil para extrair metadados, mesmo que os vídeos não estejam realmente disponíveis para download (experimental) |
| `--no-ignore-no-formats-error` | Lança erro quando não são encontrados formatos de vídeo para download (padrão) |
| `--skip-download` | Não baixa o vídeo, mas escreve todos os arquivos relacionados (Alias: --no-download) |
| `-O, --print [WHEN:]TEMPLATE` | Nome do campo ou template de saída para imprimir na tela, opcionalmente prefixado com quando imprimi-lo, separado por um ":". Valores suportados de "WHEN" são os mesmos de `--use-postprocessor` (padrão: vídeo). Implica `--quiet`. Implica `--simulate`, a menos que `--no-simulate` ou estágios posteriores de WHEN sejam usados. Esta opção pode ser usada várias vezes |
| `--print-to-file [WHEN:]TEMPLATE FILE` | Acrescenta o template dado ao arquivo. Os valores de WHEN e TEMPLATE são os mesmos de --print. FILE usa a mesma sintaxe que o template de saída. Esta opção pode ser usada várias vezes |
| `-j, --dump-json` | Silencioso, mas imprime informações JSON para cada vídeo. Simula a menos que `--no-simulate` seja usado. Veja "OUTPUT TEMPLATE" para uma descrição das chaves disponíveis |
| `-J, --dump-single-json` | Silencioso, mas imprime informações JSON para cada URL ou infojson passado. Simula a menos que --no-simulate seja usado. Se a URL se referir a uma playlist, as informações completas da playlist são impressas em uma única linha |
| `--force-write-archive` | Força a escrita de entradas de arquivo de download enquanto nenhum erro ocorrer, mesmo se -s ou outra opção de simulação for usada (Alias: `--force-download-archive`) |
| `--newline` | Exibe a barra de progresso como novas linhas |
| `--no-progress` | Não exibe a barra de progresso |
| `--progress` | Exibe a barra de progresso, mesmo em modo silencioso |
| `--console-title` | Exibe o progresso na barra de título do console |
| `--progress-template [TYPES:]TEMPLATE` | Template para saídas de progresso, opcionalmente prefixado com um dos "download:" (padrão), "download-title:" (o título do console), "postprocess:", ou "postprocess-title:". Os campos do vídeo são acessíveis sob a chave "info" e os atributos de progresso são acessíveis sob a chave "progress". Ex: --console-title --progress-template "download-title:%(info.id)s-%(progress.eta)s" |
| `--progress-delta SECONDS` | Tempo entre as saídas de progresso (padrão: 0) |
| `-v, --verbose` | Imprime várias informações de depuração |
| `--dump-pages` | Imprime páginas baixadas codificadas em base64 para depurar problemas (muito detalhado) |
| `--write-pages` | Escreve páginas intermediárias baixadas em arquivos no diretório atual para depurar problemas |
| `--print-traffic` | Exibe o tráfego HTTP enviado e recebido |

# Soluções Alternativas

| Argumento | Descrição |
|---|---|
| `--encoding ENCODING` | Força a codificação especificada (experimental) |
| `--legacy-server-connect` | Permite explicitamente conexão HTTPS com servidores que não suportam a renegociação segura RFC 5746 |
| `--no-check-certificates` | Suprime a validação do certificado HTTPS |
| `--prefer-insecure` | Usa uma conexão não criptografada para recuperar informações sobre o vídeo (Atualmente suportado apenas para YouTube) |
| `--add-headers FIELD:VALUE` | Especifica um cabeçalho HTTP personalizado e seu valor, separados por dois pontos ":". Você pode usar esta opção várias vezes |
| `--bidi-workaround` | Solução para terminais que não possuem suporte a texto bidirecional. Requer o executável bidiv ou fribidi no PATH |
| `--sleep-requests SECONDS` | Número de segundos para dormir entre solicitações durante a extração de dados |
| `--sleep-interval SECONDS` | Número de segundos para dormir antes de cada download. Este é o tempo mínimo para dormir quando usado junto com --max-sleep-interval (Alias: `--min-sleep-interval`) |
| `--max-sleep-interval SECONDS` | Número máximo de segundos para dormir. Só pode ser usado junto com `--min-sleep-interval` |
| `--sleep-subtitles SECONDS` | Número de segundos para dormir antes de cada download de legenda |

# Opções de Formato de Vídeo

| Argumento | Descrição |
|---|---|
| `-f, --format FORMAT` | Código do formato de vídeo, veja "FORMAT SELECTION" para mais detalhes |
| `-S, --format-sort SORTORDER` | Ordena os formatos pelos campos dados, veja "Sorting Formats" para mais detalhes |
| `--format-sort-force` | Força a ordem de classificação especificada pelo usuário a ter precedência sobre todos os campos, veja "Sorting Formats" para mais detalhes (Alias: `--S-force`) |
| `--no-format-sort-force` | Alguns campos têm precedência sobre a ordem de classificação especificada pelo usuário (padrão) |
| `--video-multistreams` | Permite que vários streams de vídeo sejam mesclados em um único arquivo |
| `--no-video-multistreams` | Apenas um stream de vídeo é baixado para cada arquivo de saída (padrão) |
| `--audio-multistreams` | Permite que vários streams de áudio sejam mesclados em um único arquivo |
| `--no-audio-multistreams` | Apenas um stream de áudio é baixado para cada arquivo de saída (padrão) |
| `--prefer-free-formats` | Prefere formatos de vídeo com contêineres livres em vez de não livres de mesma qualidade. Use com "-S ext" para preferir estritamente contêineres livres independentemente da qualidade |
| `--no-prefer-free-formats` | Não dá preferência especial a contêineres livres (padrão) |
| `--check-formats` | Certifique-se de que os formatos são selecionados apenas entre os realmente baixáveis |
| `--check-all-formats` | Verifique todos os formatos para saber se são realmente baixáveis |
| `--no-check-formats` | Não verifica se os formatos são realmente baixáveis |
| `-F, --list-formats` | Lista formatos disponíveis de cada vídeo. Simula a menos que `--no-simulate` seja usado |
| `--merge-output-format FORMAT` | Contêineres que podem ser usados ao mesclar formatos, separados por "/", ex: "mp4/mkv". Ignorado se nenhuma mesclagem for necessária. (Atualmente suportado: avi, flv, mkv, mov, mp4, webm) |

# Opções de Legenda

| Argumento | Descrição |
|---|---|
| `--write-subs` | Escreve o arquivo de legendas |
| `--no-write-subs` | Não escreve o arquivo de legendas (padrão) |
| `--write-auto-subs` | Escreve arquivo de legendas gerado automaticamente (Alias: `--write-automatic-subs`) |
| `--no-write-auto-subs` | Não escreve legendas geradas automaticamente (padrão) (Alias: `--no-write-automatic-subs`) |
| `--list-subs` | Lista legendas disponíveis de cada vídeo. Simula a menos que `--no-simulate` seja usado |
| `--sub-format FORMAT` | Formato da legenda; aceita preferências de formatos, por exemplo, "srt" ou "ass/srt/best" |
| `--sub-langs LANGS` | Idiomas das legendas a serem baixadas (podem ser regex) ou "all" separados por vírgulas, ex: --sub-langs "en.*,ja". Você pode prefixar o código do idioma com um "-" para excluí-lo dos idiomas solicitados, ex: --sub-langs all,-live_chat. Use `--list-subs` para uma lista de tags de idioma disponíveis |

# Opções de Autenticação

| Argumento | Descrição |
|---|---|
| `-u, --username USERNAME` | Login com este ID de conta |
| `-p, --password PASSWORD` | Senha da conta. Se esta opção for deixada de fora, o yt-dlp perguntará interativamente |
| `-2, --twofactor TWOFACTOR` | Código de autenticação de dois fatores |
| `-n, --netrc` | Usa dados de autenticação .netrc |
| `--netrc-location PATH` | Localização dos dados de autenticação .netrc; pode ser o caminho ou seu diretório contido. Padrão é ~/.netrc |
| `--netrc-cmd NETRC_CMD` | Comando para executar para obter as credenciais para um extractor |
| `--video-password PASSWORD` | Senha específica do vídeo |
| `--ap-mso MSO` | Identificador do operador de múltiplos sistemas Adobe Pass (TV provider), use `--ap-list-mso` para uma lista de MSOs disponíveis |
| `--ap-username USERNAME` | Login da conta do operador de múltiplos sistemas |
| `--ap-password PASSWORD` | Senha da conta do operador de múltiplos sistemas. Se esta opção for deixada de fora, o yt-dlp perguntará interativamente |
| `--ap-list-mso` | Lista todos os operadores de múltiplos sistemas suportados |
| `--client-certificate CERTFILE` | Caminho para o arquivo de certificado do cliente em formato PEM. Pode incluir a chave privada |
| `--client-certificate-key KEYFILE` | Caminho para o arquivo de chave privada para o certificado do cliente |
| `--client-certificate-password PASSWORD` | Senha para a chave privada do certificado do cliente, se criptografada. Se não fornecida, e a chave for criptografada, o yt-dlp perguntará interativamente |

# Opções de Pós-Processamento

| Argumento | Descrição |
|---|---|
| `-x, --extract-audio` | Converte arquivos de vídeo em arquivos apenas de áudio (requer ffmpeg e ffprobe) |
| `--audio-format FORMAT` | Formato para converter o áudio ao usar -x. (Atualmente suportado: best (padrão), aac, alac, flac, m4a, mp3, opus, vorbis, wav). Você pode especificar várias regras usando sintaxe semelhante a `--remux-video` |
| `--audio-quality QUALITY` | Especifica a qualidade do áudio do ffmpeg ao converter o áudio com -x. Insira um valor entre 0 (melhor) e 10 (pior) para VBR ou um bitrate específico, como 128K (padrão 5) |
| `--remux-video FORMAT` | Remuxa o vídeo em outro contêiner, se necessário (atualmente suportado: avi, flv, gif, mkv, mov, mp4, webm, aac, aiff, alac, flac, m4a, mka, mp3, ogg, opus, vorbis, wav). Se o contêiner de destino não suportar o codec de vídeo/áudio, o remux falhará. Você pode especificar várias regras; ex: "aac>m4a/mov>mp4/mkv" remuxará aac para m4a, mov para mp4 e qualquer outra coisa para mkv |
| `--recode-video FORMAT` | Re-encoda o vídeo em outro formato, se necessário. A sintaxe e os formatos suportados são os mesmos de `--remux-video` |
| `--postprocessor-args NAME:ARGS` | Passa esses argumentos para os pós-processadores. Especifica o nome do pós-processador/executável e os argumentos separados por dois pontos ":" para passar o argumento para o pós-processador/executável especificado. Pós-processadores suportados são: Merger, ModifyChapters, SplitChapters, ExtractAudio, VideoRemuxer, VideoConvertor, Metadata, EmbedSubtitle, EmbedThumbnail, SubtitlesConvertor, ThumbnailsConvertor, FixupStretched, FixupM4a, FixupM3u8, FixupTimestamp e FixupDuration. Executáveis suportados são: AtomicParsley, FFmpeg e FFprobe. Você também pode especificar "PP+EXE:ARGS" para passar os argumentos para o executável especificado apenas quando estiver sendo usado pelo pós-processador especificado. Adicionalmente, para ffmpeg/ffprobe, "_i"/"_o" pode ser anexado ao prefixo opcionalmente seguido por um número para passar o argumento antes do arquivo de entrada/saída especificado, ex: `--ppa "Merger+ffmpeg_i1:-v quiet"`. Você pode usar esta opção várias vezes para passar diferentes argumentos para diferentes pós-processadores. (Alias: `--ppa`) |
| `-k, --keep-video` | Mantém o arquivo de vídeo intermediário no disco após o pós-processamento |
| `--no-keep-video` | Exclui o arquivo de vídeo intermediário após o pós-processamento (padrão) |
| `--post-overwrites` | Sobrescreve arquivos pós-processados (padrão) |
| `--no-post-overwrites` | Não sobrescreve arquivos pós-processados |
| `--embed-subs` | Incorpora legendas no vídeo (somente para vídeos mp4, webm e mkv) |
| `--no-embed-subs` | Não incorpora legendas (padrão) |
| `--embed-thumbnail` | Incorpora a miniatura no vídeo como capa |
| `--no-embed-thumbnail` | Não incorpora a miniatura (padrão) |
| `--embed-metadata` | Incorpora metadados ao arquivo de vídeo. Também incorpora capítulos/infojson se presentes, a menos que --no-embed-chapters/--no-embed-info-json sejam usados (Alias: `--add-metadata`) |
| `--no-embed-metadata` | Não adiciona metadados ao arquivo (padrão) (Alias: `--no-add-metadata`) |
| `--embed-chapters` | Adiciona marcadores de capítulos ao arquivo de vídeo (Alias: `--add-chapters`) |
| `--no-embed-chapters` | Não adiciona marcadores de capítulos (padrão) (Alias: `--no-add-chapters`) |
| `--embed-info-json` | Incorpora o infojson como um anexo a arquivos de vídeo mkv/mka |
| `--no-embed-info-json` | Não incorpora o infojson como um anexo ao arquivo de vídeo |
| `--parse-metadata [WHEN:]FROM:TO` | Analisa metadados adicionais, como título/artista, de outros campos; veja "MODIFYING METADATA" para detalhes. Valores suportados de "WHEN" são os mesmos de `--use-postprocessor` (padrão: pre_process) |
| `--replace-in-metadata [WHEN:]FIELDS REGEX REPLACE` | Substitui texto em um campo de metadados usando o regex fornecido. Esta opção pode ser usada várias vezes. Valores suportados de "WHEN" são os mesmos de `--use-postprocessor` (padrão: pre_process) |
| `--xattrs` | Escreve metadados para os xattrs do arquivo de vídeo (usando padrões dublin core e xdg) |
| `--concat-playlist POLICY` | Concatena vídeos em uma playlist. Um dos valores: "never", "always" ou "multi_video" (padrão; apenas quando os vídeos formam um único show). Todos os arquivos de vídeo devem ter os mesmos codecs e número de streams para serem concatenáveis. O prefixo "pl_video:" pode ser usado com "`--paths`" e "`--output`" para definir o nome do arquivo de saída para os arquivos concatenados. Veja "OUTPUT TEMPLATE" para detalhes |
| `--fixup POLICY` | Corrige automaticamente falhas conhecidas do arquivo. Um dos valores: never (não faz nada), warn (apenas emite um aviso), detect_or_warn (o padrão; corrige o arquivo se possível, avisa caso contrário), force (tenta corrigir mesmo que o arquivo já exista) |
| `--ffmpeg-location PATH` | Localização do binário do ffmpeg; pode ser o caminho para o binário ou seu diretório contido |
| `--exec [WHEN:]CMD` | Executa um comando, opcionalmente prefixado com quando executá-lo, separado por dois pontos ":". Valores suportados de "WHEN" são os mesmos de `--use-postprocessor` (padrão: after_move). Mesma sintaxe que o template de saída pode ser usada para passar qualquer campo como argumentos para o comando. Se nenhum campo for passado, %(filepath,_filename|)q é anexado ao final do comando. Esta opção pode ser usada várias vezes |
| `--no-exec` | Remove qualquer `--exec` definido anteriormente |
| `--convert-subs FORMAT` | Converte as legendas para outro formato (atualmente suportado: ass, lrc, srt, vtt) (Alias: --convert-subtitles) |
| `--convert-thumbnails FORMAT` | Converte as miniaturas para outro formato (atualmente suportado: jpg, png, webp). Você pode especificar várias regras usando sintaxe semelhante a `--remux-video` |
| `--split-chapters` | Divide o vídeo em vários arquivos com base em capítulos internos. O prefixo "chapter:" pode ser usado com "--paths" e "--output" para definir o nome do arquivo de saída para os arquivos divididos. Veja "OUTPUT TEMPLATE" para detalhes |
| `--no-split-chapters` | Não divide o vídeo com base em capítulos (padrão) |
| `--remove-chapters REGEX` | Remove capítulos cujo título corresponda à expressão regular fornecida. A sintaxe é a mesma de --download-sections. Esta opção pode ser usada várias vezes |
| `--no-remove-chapters` | Não remove nenhum capítulo do arquivo (padrão) |
| `--force-keyframes-at-cuts` | Força keyframes nos cortes ao baixar/dividir/remover seções. Isso é lento devido à necessidade de re-encode, mas o vídeo resultante pode ter menos artefatos ao redor dos cortes |
| `--no-force-keyframes-at-cuts` | Não força keyframes ao redor dos capítulos ao cortar/dividir (padrão) |
| `--use-postprocessor NAME[:ARGS]` | Nome (diferencia maiúsculas de minúsculas) dos pós-processadores de plugin a serem habilitados, e (opcionalmente) argumentos a serem passados para ele, separados por dois pontos ":". ARGS são uma lista de NAME=VALUE delimitada por ponto e vírgula ";". O argumento "when" determina quando o pós-processador é invocado. Pode ser um dos valores: "pre_process" (após a extração do vídeo), "after_filter" (após o vídeo passar pelo filtro), "video" (após --format; antes de --print/--output), "before_dl" (antes de cada download de vídeo), "post_process" (após cada download de vídeo; padrão), "after_move" (após mover o arquivo de vídeo para seus locais finais), "after_video" (após baixar e processar todos os formatos de um vídeo), ou "playlist" (no final da playlist). Esta opção pode ser usada várias vezes para adicionar diferentes pós-processadores |


# Opções do SponsorBlock

| Argumento | Descrição |
|---|---|
| `--sponsorblock-mark CATS` | Categorias SponsorBlock para criar capítulos separados por vírgulas. Categorias disponíveis são sponsor, intro, outro, selfpromo, preview, filler, interaction, music_offtopic, poi_highlight, chapter, all e default (=all). Você pode prefixar a categoria com um "-" para excluí-la. Veja [1] para descrição das categorias. Ex: --sponsorblock-mark all,-preview [1] https://wiki.sponsor.ajay.app/w/Segment_Categories |
| `--sponsorblock-remove CATS` | Categorias SponsorBlock a serem removidas do arquivo de vídeo, separadas por vírgulas. Se uma categoria estiver presente em ambos mark e remove, remove terá precedência. A sintaxe e as categorias disponíveis são as mesmas de --sponsorblock-mark exceto que "default" refere-se a "all,-filler" e poi_highlight, chapter não estão disponíveis |
| `--sponsorblock-chapter-title TEMPLATE` | Um template de saída para o título dos capítulos do SponsorBlock criados por --sponsorblock-mark. Os únicos campos disponíveis são start_time, end_time, category, categories, name, category_names. Padrão é "[SponsorBlock]: %(category_names)l" |
| `--no-sponsorblock` | Desativa tanto --sponsorblock-mark quanto `--sponsorblock-remove` |
| `--sponsorblock-api URL` | Localização da API SponsorBlock, padrão é https://sponsor.ajay.app |

# Opções de Extrator

| Argumento | Descrição |
| --- | --- |
| `--extractor-retries RETRIES` | Número de tentativas para erros conhecidos do extrator (o padrão é 3), ou "infinito" |
| `--allow-dynamic-mpd` | Processar manifestos DASH dinâmicos (padrão) (Alias: `--no-ignore-dynamic-mpd`) |
| `--ignore-dynamic-mpd` | Não processar manifestos DASH dinâmicos (Alias: `--no-allow-dynamic-mpd`) |
| `--hls-split-discontinuity` | Dividir playlists HLS em diferentes formatos em descontinuidades, como pausas para anúncios |
| `--no-hls-split-discontinuity` | Não dividir playlists HLS em diferentes formatos em descontinuidades, como pausas para anúncios (padrão) |
| `--extractor-args IE_KEY:ARGS` | Passar argumentos ARGS para o extrator IE_KEY. Veja "ARGUMENTOS DO EXTRATOR" para detalhes. Você pode usar esta opção várias vezes para fornecer argumentos para diferentes extratores |