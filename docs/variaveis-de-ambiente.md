# Variáveis de ambiente e argumentos de linha de comando

Quase nada aqui é para o uso diário: as variáveis `UOFIDDLER_*` existem para capturas de tela e testes automatizados. As úteis para quem usa o programa são `UOFIDDLER_UO_DIR`, `--experimental`, as de idioma e `HOME`.

## Argumentos de linha de comando

| Argumento | O que faz |
|---|---|
| `--experimental` | Mostra as abas ainda em acabamento (hoje, o **Paperdoll**). Sem ele elas ficam escondidas. O atualizador repassa os argumentos ao reiniciar, então a flag sobrevive a uma atualização. `./run.sh --experimental` também funciona. |
| `--version` | Imprime o número do build (`0` num build de desenvolvimento) e sai, sem abrir janela. |
| `--self-update <pasta>` | Roda o código de atualização contra uma pasta local organizada como um release (zip, tar.gz ou AppImage + `SHA256SUMS.txt`) e sai, sem janela. Se a pasta não for informada, usa `UOF_UPDATE_SOURCE`. Serve ao teste do atualizador. |

## Para quem usa o programa

| Variável | O que faz |
|---|---|
| `UOFIDDLER_UO_DIR` | Pasta do cliente do Ultima Online **só para esta execução**. Tem prioridade sobre a pasta salva nas opções e sobre a detecção automática. |
| `UOFIDDLER_EXPERIMENTAL` | Qualquer valor equivale a `--experimental`. Existe para scripts e capturas de tela. |
| `LC_ALL`, `LC_MESSAGES`, `LANG` | Escolhem o idioma da interface na primeira execução (`pt_...` -> português, qualquer outro -> inglês). Vale a primeira que estiver definida e não vazia, nessa ordem. Depois, a escolha feita nas opções prevalece. |
| `HOME` (e `USERPROFILE`) | Base da pasta de configuração `~/.config/uofiddler` (`settings.json`, `gumpstudio_resizepics.txt`, favoritos do GumpStudio, bordas do editor de mapa) e das pastas onde o programa procura o cliente sozinho. O editor de mapa e o GumpStudio usam `USERPROFILE` quando `HOME` não existe (Windows). |
| `APPIMAGE` | Definida pelo próprio AppImage com o caminho do arquivo. O atualizador a usa para saber **qual arquivo substituir**. Ao reiniciar o programa, ele remove `APPDIR`, `APPIMAGE`, `ARGV0` e `OWD` do ambiente do novo processo, para que o novo AppImage monte a si mesmo em vez de reaproveitar a montagem do antigo. |

## Capturas de tela e testes automatizados

Servem para abrir o programa já num estado e fotografar, sem ninguém na frente da tela. Combine com `UOFIDDLER_UO_DIR`.

| Variável | Formato | O que faz |
|---|---|---|
| `UOFIDDLER_START_TAB` | `<nome da aba>` (`Items`, `Map`, `Paperdoll`, `GumpStudio`...) | Aba inicial. Sem diferenciar maiúsculas; uma aba experimental só vale junto de `--experimental`. |
| `UOFIDDLER_MAP` | `<radar\|live\|overview>,<faceta>,<x>,<y>[,nodiff][,zoom=<f>][,maxz=<z>][,noroofs][,grid]` | Estado da aba Mapa: modo (`live` é o isométrico), faceta 0 a 5, coordenadas, mapdif desligado (`nodiff`), zoom, corte de altura, sem telhados e grade. |
| `UOFIDDLER_EDITOR` | `<ferramenta>[/x,y][/opções...]` | Abre o editor de mapa na ferramenta, com o cursor no tile e ações opcionais (paleta, ajuda, altura, pincel...). Só vale junto de `UOFIDDLER_MAP`. A lista completa está no comentário de `MapEditor::focus` (`crates/uo_gui/src/map/editor/state.rs`). Exemplo: `select/1480,1620/palette`. |
| `UOFIDDLER_PICKER` | `<tipo>[/<id>[/<busca>]]` | Abre o seletor de arte do mapa ao iniciar. Exemplos: `static/0x0F51/dagger`; para matizes, `hue/1153/0x1F03` mostra o hue 1153 numa túnica. |
| `UOFIDDLER_OPEN` | `<nome do arquivo>` | Aba Arquivos: abre esse arquivo ao iniciar. |
| `UOFIDDLER_GS_SELECT` | `<índice do elemento>` | GumpStudio: seleciona o elemento do gump de exemplo ao iniciar. |
| `UOFIDDLER_PAPERDOLL` | `<plate\|common\|robe>[,<corpo 0-5>]` | Paperdoll: veste um conjunto pronto (armadura de placas, roupas comuns, manto) e, opcionalmente, troca o corpo (0 humano ♂ ... 5 gárgula ♀). |
| `UOFIDDLER_PAPERDOLL_ITEMS` | `0x2662,0x1539,...` | Paperdoll: veste exatamente esses itens. |
| `UOFIDDLER_SCREENSHOT` | `<arquivo.png>[,<segundos>]` | Salva uma captura da janela depois do atraso (5 s se omitido) e **fecha o programa**. |

## Build, CI e atualizador

| Variável | O que faz |
|---|---|
| `UOF_BUILD` | Lida pelo `build.rs`: número do build embutido no executável (o CI usa `github.run_number`). Sem ela o build é 0, mostrado como "build de desenvolvimento". É o que o atualizador compara com o release mais recente. |
| `UOF_UPDATE_SOURCE` | Pasta de release usada por `--self-update` quando o argumento não é informado. |
| `UOF_UPDATE_TEST_ARCH` | Só vale com `--self-update`: finge a arquitetura (`x86_64`) para ensaiar `packaging/test-updater.sh` em máquinas que não são x86_64 (os releases só têm arquivos x86_64). A interface gráfica nunca a lê. |
| `UOF_UPDATE_ACK` | **Interna.** O processo antigo a define com o caminho de um arquivo ao iniciar o novo; o novo escreve nele quando sua janela abre, e só então o antigo sai. Se isso não acontecer a tempo, o executável antigo é restaurado. |
| `SKIP_APPIMAGE` | Usada por `packaging/test-updater.sh`: pula a parte do AppImage. |
| `GH_TOKEN`, `REPO`, `OLD_TAG` | Definidas no workflow `updater-live.yml` (manual): token do GitHub, repositório e a tag do build antigo a ser atualizado (padrão `build-29`). |

## Testes e ferramentas de desenvolvimento

| Variável | O que faz |
|---|---|
| `UPDATE_I18N_BASELINE` | Ao rodar `cargo test -p uo_gui`, regrava `crates/uo_gui/i18n_baseline.txt` com as contagens atuais de textos fixos, desde que nenhuma tenha aumentado (o teste de i18n é uma "catraca": a contagem nunca pode subir). |
| `UO_DIR` | Pasta do cliente lida pelos exemplos de `crates/uo_core/examples` (`anim_probe`, `equip_audit`, `mapdif_check`...). Padrão: `/uo`. |
| `UOFIDDLER_UO_DIR` | Nos testes com arquivos reais (editor de mapa, seletor), é a pasta do cliente; sem ela esses testes são pulados. |
