# UOFiddler-Rust 🗡️

> 🌐 **Language / Idioma**: [English (README.md)](README.md) | **Português**  
> 👤 **Autor**: Mauricio Nunes + DeepSeek & Gemini · [Colaboradores](#-colaboradores)  
> 📥 **Download**: [última release](https://github.com/mbnunes/UOFiddler-Rust/releases/latest) (Windows, Linux `.AppImage` / `.deb` / `.tar.gz`)  
> 🔗 **GitHub**: [https://github.com/mbnunes/UOFiddler-Rust](https://github.com/mbnunes/UOFiddler-Rust)

**UOFiddler** é uma reimplementação moderna, ultrarrápida e **100% nativa em Rust** do clássico **UOFiddler** para Ultima Online. Desenvolvido com **egui / eframe**, ele executa de forma estável, fluida e elegante no **Linux, Windows e macOS**, eliminando integralmente qualquer dependência de **.NET runtime**, **Mono**, **Wine** ou **Python**.

---

## 🤝 Como contribuir

Todo Pull Request deve cumprir os [critérios de aceite](CONTRIBUTING.md). O primeiro: **todo texto visível ao usuário deve ser localizado**; um PR sem isso é recusado. Agentes: veja o [AGENTS.md](AGENTS.md).

---

## ⚡ Destaques e Diferenciais do UOFiddler

A execução do UOFiddler-Rust original em C# via Wine ou Mono no Linux causa frequentes falhas de renderização, travamentos e problemas de case-sensitivity no sistema de arquivos. O **UOFiddler** soluciona tudo de forma definitiva:

1. **100% Rust Nativo**: Zero dependências de runtime. Inicialização instantânea (< 50 ms) e consumo mínimo de memória RAM.
2. **Dual-Client Engine (Clássico & Enhanced UOP)**: Alternância em tempo real entre o cliente **Clássico 2D** (`.mul`/`.idx`) e o **Enhanced Client 3D** (`.uop` Mythic MYP v5 / texturas DDS nativas).
3. **Interface Gráfica Acelerada por Hardware (egui/eframe)**: Navegação suave a 60+ FPS por todas as tabelas de **65.536 itens estáticos**, **16.384 land tiles**, **79 tabelas GameData** e estruturas multis complexas.
4. **Detecção Automática Inteligente**: Localiza automaticamente diretórios de instalação do UO Classic e Enhanced (`/opt/hd_jogos/`, `~/.wine/`, Lutris, Steam, pastas locais) com resolução de arquivos imune a maiúsculas/minúsculas.
5. **Navegação Universal por Teclado**: Suporte completo a navegação por setas (**Cima**, **Baixo**, **PageUp**, **PageDown**) com auto-scroll em todas as grades e listas de todas as abas.
6. **Áudio Nativo sem Drivers Extras**: Reprodução de áudio PCM WAV diretamente via `rodio` em servidores PipeWire, PulseAudio e ALSA.
7. **Barra Vertical de Zoom**: Visualização em tamanho real (1:1) com barra vertical de zoom (0.5x a 5.0x) nas abas de Itens, Land Tiles, TileData, Texturas e Animações.
8. **Mapa Isométrico e Editor de Mapa**: O mundo desenhado como no cliente do jogo (GPU, arte real, terreno inclinado), a partir de `mapN.mul` ou `mapNLegacyMUL.uop`, e um editor de mapa completo que **grava direto nos arquivos do cliente** (com backup `.bak`), para admins de shard.
9. **Interface multilíngue**: inglês e português do Brasil (idioma do sistema detectado na primeira execução, trocável nas opções); todo texto passa pelas tabelas de tradução em `crates/uo_gui/locales`, então um novo idioma é só mais um arquivo.

---

## 📑 Visão Geral Completa das 21 Abas (20, mais 1 experimental)

As abas ficam agrupadas em três categorias no topo da janela: **Arte**, **Dados** e **Mundo**.

| Aba | Descrição |
|---|---|
| **⚔️ Items** | Visualizador de itens estáticos em modos **Grade** e **Lista**. Alternância dinâmica entre arte clássica 2D e texturas DDS do Enhanced Client (`LegacyTexture.uop`). Testador de Hues (3.000 cores), inspetor do **TileData** do item (flags clássicas de 32 bits e High Seas de 64 bits) e ferramenta vertical de zoom. |
| **🌿 Land Tiles** | 16.384 terrenos com geometria de diamante (losango 44x44). Suporte a terrenos 2D clássicos e texturas 3D do Enhanced Client (`Texture.uop`), inspetor do **TileData** do terreno e slider de zoom. |
| **🖼️ Gumps** | Navegador para `gumpart.mul` e pacotes Mythic `gumpartLegacyMUL.uop`. Sobreposição das caixas de colisão de contêineres (71 caixas), busca e barra vertical de zoom. |
| **🎨 Hues** | Tabela completa com todas as 3.000 matizes de cores do Ultima Online. Inspetor de gradiente com amostragem RGB555 individual de 32 cores por hue. |
| **🧱 Textures** | Texturas de superfície 64x64 e 128x128 de `texmaps.mul`. Modos grade e lista com visualização 1:1 e zoom vertical. |
| **🎯 RadarCol** | Todas as 81.920 cores de radar do minimapa mapeadas para itens e terrenos. Visualização da cor RGB e da arte do tile em tamanho real. |
| **🎭 Animations** | Player de animações de criaturas com suporte a `anim1..6.mul` e `AnimationFrame1..6.uop` (mais de 46.000 entradas). Filtro completo de categorias (Monstros, Marinhos, Animais, Humanos/Equip, UOP Modernos), seleção dinâmica de ações com indicador `✓`, reprodução em 8 direções e exportação de spritesheets. |
| **💬 Cliloc** | Navegador de strings e textos localizados (`cliloc.enu`). Pesquisa rápida por ID numérico ou texto em mais de 100.000 entradas. |
| **🔊 Sounds** | Navegador e player de áudio PCM WAV para `sound.mul`. Reprodução nativa instantânea. |
| **🗺️ Map** | As seis facetas (Felucca, Trammel, Ilshenar, Malas, Tokuno, Ter Mur) a partir de `mapN.mul` ou `mapNLegacyMUL.uop`. Vista **isométrica** desenhada na GPU como no cliente (arte real, encostas com textura, corte de telhados e de altura, radar no canto), **Radar** e **Visão geral** da faceta (`facet0N.mul` / `Multimap.rle`), mapdif/stadif, presets e ir para. O botão **Edit map** abre o [editor de mapa](#-editor-de-mapa). |
| **🏰 Multis** | Estruturas multi-tile (casas, castelos, navios). Suporte a `multi.mul` clássico e `MultiCollection.uop` do Enhanced Client. Renderizador isométrico em tempo real com corte vertical Z e zoom pelo scroll do mouse. |
| **💡 Light** | Padrões de luz circulares e direcionais (`light.mul`) com visualizador de alto contraste. |
| **🔤 Fonts** | Fontes bitmap ASCII (`fonts.mul`) com campo de teste de digitação interativo. |
| **⭐ Skills** | Tabela de habilidades do jogo (`skills.mul`) com flags de botões de ação e títulos. |
| **🗣️ Speech** | Palavras-chave e gatilhos de comandos de voz (`speech.mul`). |
| **🎬 AnimData** | Tabela de taxas de quadros e intervalos de animação (`animdata.mul`) com pré-visualização estática do item. |
| **🎮 GameData** | Inspetor de configurações do cliente 3D Enhanced (`GameData.uop`). Decodifica todas as **79 tabelas de configuração** com modos **📊 Tabela Estruturada** e **📝 Texto Raw**, busca interna por linhas e exportador em lote para CSV. |
| **🧍 Paperdoll** *(experimental)* | Montador de paperdoll: a animação do corpo e o gump de paperdoll lado a lado, com slots de itens estilo mochila e hues (`Equipconv.def`, `gump.def`), qualquer item em qualquer camada e o personagem montado. Ainda está sendo finalizado, por isso fica **escondido a menos que o programa seja iniciado com `--experimental`** (veja [Recursos experimentais](#-recursos-experimentais)). |
| **🧩 GumpStudio** | Editor visual de gumps: fundos, imagens, itens, texto, HTML/cliloc, botões, checkboxes, radios, entradas de texto e regiões alfa, com a arte do cliente e as **fontes do UO** (`unifont*.mul`, `fonts.mul`). Salva e abre JSON e exporta para **RunUO (C#)**, **Sphere** e **POL**. Seletores visuais para imagens, itens, botões, checkboxes e radios, navegador de resizepics (com lista manual de válidos/inválidos), texto HTML, laços de repetição, favoritos e grupos. |
| **📡 Comunicação** | Referência estática dos pacotes do cliente (servidor para cliente e cliente para servidor) e dos comandos de layout de gump, com busca e filtros. |
| **📁 Arquivos** | Navegador de todos os arquivos do cliente: texto/def em tabela, localização IFF, navegador de UOP com entradas e prévia, visão hex, `Multimap.rle` e `facet0N.mul` como imagem, e o **conversor UOP ↔ MUL** (art, gumps, mapas). |

---

## 🗺️ Editor de Mapa

Abra a aba **Map** e clique em **Edit map** (ou F11). O editor toma o lugar do visualizador, na mesma aba:

- **Ferramentas**: Select (inspecionar e editar um tile e seus statics), Stamp, Terrain, Height, Eraser e Region, com tamanho e forma do pincel, prévia do traço e um **fantasma** transparente do static onde ele vai cair (empilha sobre mesas, pisos, balcões…).
- **Terreno com bordas automáticas**, lidas da arte ou de **conjuntos de bordas** editados no diálogo *Borders…* (guardados em `~/.config/uofiddler/map_borders.json`); pisos ganham as bordas por dentro do traço.
- **Faixa de alturas** para editar dentro de cavernas e andares de cima, e um Z de trabalho fixo.
- **Região**: copiar, recortar, colar, mover, apagar, preencher e modelos guardados.
- **Paleta** com o seletor visual de arte (categorias, busca, favoritos, recentes), desfazer/refazer e atalhos de teclado (aperte `?`).

**Gravação**: as alterações ficam pendentes até você clicar em **Save**, que grava nos arquivos da faceta na pasta do cliente: o terreno em `mapN.mul` / `mapNLegacyMUL.uop` (corrigido no lugar) e os statics em `staidxN.mul` / `staticsN.mul` (reescritos). Antes da primeira gravação de cada arquivo é feita uma cópia `.bak`. **Save as…** grava uma cópia completa dos arquivos da faceta em outra pasta, e as alterações pendentes podem ser exportadas e importadas em JSON. O editor edita o mapa base: patches `mapdif`/`stadif`, se a faceta tiver, continuam valendo por cima no cliente.

> Faça uma cópia da pasta do cliente antes de gravar, e feche o cliente do jogo enquanto grava.

---

## 🧪 Recursos Experimentais

As abas que ainda estão sendo finalizadas ficam escondidas, para que a ferramenta publicada mostre só o que está pronto. Inicie o programa com `--experimental` para exibi-las:

```bash
uo_gui --experimental                  # Windows: uo_gui.exe --experimental
./UOFiddler-<versão>-x86_64.AppImage --experimental
cargo run --release -p uo_gui -- --experimental
```

- Hoje é a aba **Paperdoll**. Ela aparece marcada como *(experimental)*, e os contadores de abas (20 abas, 21 com a flag) só a incluem com a flag.
- A flag é mantida quando o [atualizador automático](#atualizações) reinicia o programa. Para scripts e capturas de tela automatizadas, `UOFIDDLER_EXPERIMENTAL=1` faz o mesmo. Todas as variáveis de ambiente estão em [`docs/variaveis-de-ambiente.md`](docs/variaveis-de-ambiente.md).

---

## 📥 Download

Builds prontos de cada push na `master` são publicados na [página de Releases](https://github.com/mbnunes/UOFiddler-Rust/releases/latest):

| Sistema | Arquivo |
|---|---|
| Windows 10/11 (x64) | `uofiddler-<versão>-windows-x86_64.zip`: descompacte e rode o `uo_gui.exe` |
| Linux (qualquer distro) | `UOFiddler-<versão>-x86_64.AppImage`: `chmod +x` e rode |
| Debian / Ubuntu / Mint | `uofiddler_<versão>_amd64.deb`: `sudo apt install ./uofiddler_<versão>_amd64.deb` e abra o *UOFiddler* pelo menu |
| Linux (arquivo simples) | `uofiddler-<versão>-linux-x86_64.tar.gz`: extraia e rode `./uofiddler` |

Os builds de Linux precisam da glibc 2.30 ou mais nova (Ubuntu 20.04+, Debian 11+, Fedora 32+ e tudo mais recente).

### Atualizações

O UOFiddler verifica, em segundo plano, se a [última release](https://github.com/mbnunes/UOFiddler-Rust/releases/latest) tem um build mais novo que o seu (o número do build é embutido pelo CI; um `cargo build` local é o build 0, mostrado como "build de desenvolvimento"). Sem rede ou com o GitHub indisponível, nada acontece e o app abre normalmente.

- Havendo build mais novo, um aviso mostra a versão nova e as notas da release com **Atualizar agora**, **Lembrar depois** e **Ignorar esta versão**. Nada é baixado ou instalado sem o seu clique.
- **Atualizar agora** baixa o arquivo do seu sistema, confere o **SHA-256** com o `SHA256SUMS.txt` da release (arquivo que não confere nunca é instalado), guarda o executável antigo como `<nome>.old`, troca o executável e reinicia. Se a versão nova não abrir, a antiga é restaurada. As configurações são mantidas (ficam em `~/.config/uofiddler`).
- Atualiza o `.exe` do Windows (do `.zip`), o `.AppImage` e o binário do `.tar.gz` no Linux. No `.deb` (instalação do sistema) só abre a página da release.
- Em **Opções** há a caixa *Verificar atualizações automaticamente ao abrir* (desmarque para desligar a checagem) e o botão **Verificar atualizações**. *Ignorar esta versão* fica salvo no mesmo `settings.json` (`auto_update_check`, `ignored_update_build`).
- Linha de comando (sem janela): `uo_gui --version` mostra o número do build; `uo_gui --self-update <pasta>` roda o mesmo código de atualização contra uma pasta local organizada como uma release (zip/tar.gz/AppImage + `SHA256SUMS.txt`). É usado pelo workflow **Updater test** (GitHub: Actions > Updater test > Run workflow, só manual), que compila o build 1 e o build 2 no Windows e no Linux, atualiza um no outro e confere que um checksum adulterado é recusado.

---

## 🛠️ Como Compilar e Executar

### Pré-requisitos
- [Rust toolchain](https://rustup.rs/) (versão 1.75 ou superior recomendada)
- No Linux, só as bibliotecas de desktop de sempre para rodar (OpenGL e X11 ou Wayland); nada extra para compilar.

### Compilando e Executando
```bash
# Clone o repositório
git clone https://github.com/mbnunes/UOFiddler-Rust.git
cd UOFiddler-Rust

# Execute diretamente através do script inicializador
./run.sh

# Ou compile e execute com cargo
cargo run --release -p uo_gui
```

### Executando os Testes
```bash
cargo test -p uo_core --lib
cargo test -p uo_gui

# Os testes que leem arquivos reais do cliente rodam com UOFIDDLER_UO_DIR apontando para a pasta do cliente
UOFIDDLER_UO_DIR=/caminho/do/cliente cargo test -p uo_gui
```

`cargo test --workspace` também roda o teste de integração `benchmark_test`, que precisa de uma instalação do Enhanced Client (`GameData.uop`).

---

## 🏗️ Estrutura do Projeto

```
UOFiddler-Rust/
├── Cargo.toml                  # Manifesto do workspace Rust
├── crates/
│   ├── uo_core/                # Decodificadores de alta performance (100% Rust Seguro)
│   │   ├── src/
│   │   │   ├── art.rs          # Decodificação de Art clássica e DDS do EC
│   │   │   ├── animations.rs   # Decodificador de sequências de animação MUL e UOP
│   │   │   ├── gamedata.rs     # Parser de GameData.uop (79 tabelas e contêineres)
│   │   │   ├── multis.rs       # Renderizador isométrico de multis e coleções uop
│   │   │   ├── maps.rs         # Leitor de mapa (mapN.mul / mapNLegacyMUL.uop, statics, mapdif), radar
│   │   │   ├── map_edit.rs     # Alterações no mapa e a gravação nos arquivos do cliente
│   │   │   ├── uop_convert.rs  # Conversor UOP <-> MUL
│   │   │   ├── gump_studio.rs  # Modelo do GumpStudio e exportadores RunUO / Sphere / POL
│   │   │   ├── tiledata.rs     # Parser de TileData 32-bit e 64-bit High Seas
│   │   │   ├── gumps.rs        # Decodificador de Gumps clássicos e UOP
│   │   │   └── ...
│   └── uo_gui/                 # Interface gráfica nativa acelerada (egui / eframe)
│       └── src/
│           ├── main.rs         # Abas, virtualização, navegação por teclado, tema escuro
│           ├── map/            # Vista isométrica do mapa (GPU) e o editor de mapa
│           └── ...             # Abas Arquivos, Comunicação, Paperdoll e GumpStudio
├── docs/
│   ├── formats/                # Todos os formatos de arquivo do cliente, documentados
│   └── protocol/               # Referência de pacotes e de layout de gumps
├── packaging/linux/            # Empacotamento .deb / AppImage / .tar.gz (usado pelo CI)
├── Animationlist.xml           # Definições de nomes de criaturas e categorias
├── Multilist.xml               # Metadados e nomes das estruturas multis
├── run.sh                      # Inicializador universal
└── README.md
```

---

## 📚 Documentação

- [`docs/formats`](docs/formats/README.md): um documento por família de arquivos do cliente (MUL/IDX, mapas, tabelas, animações, contêineres UOP, `.def`, localização…), cobrindo todos os arquivos da pasta de um cliente 7.x.
- [`docs/variaveis-de-ambiente.md`](docs/variaveis-de-ambiente.md): todas as variáveis de ambiente e argumentos de linha de comando (`--experimental`, `UOFIDDLER_UO_DIR`, as de captura de tela, as do atualizador). Tradução para inglês em [`docs/en/environment-variables.md`](docs/en/environment-variables.md).
- [`docs/protocol`](docs/protocol/README.md): pacotes, comandos de layout de gump e fluxos (login, gumps, editor de casas, barcos). Tradução para inglês em [`docs/en`](docs/en/formats/README.md).

---

## 👥 Colaboradores

| | |
|---|---|
| [**Mauricio Nunes**](https://github.com/mbnunes) | Autor e mantenedor: a reescrita em Rust, os decodificadores do core e todo o app original. |
| [**Felipe Tuffani**](https://github.com/fetuffani) | Animações via `Body.def`/`Bodyconv.def`, as abas Arquivos e Comunicação, o montador de Paperdoll, o conversor UOP ↔ MUL, o GumpStudio e as fontes do UO, a documentação de formatos e protocolo, e o CI. |
| [**João Escribano**](https://github.com/joaoescribano) | Leitura de mapas UOP, o visualizador isométrico, o editor de mapa que grava nos arquivos do cliente e os pacotes de release. |

Contribuições e pull requests são bem-vindos!

---

## 📜 Licença
Código aberto sob a licença MIT.
