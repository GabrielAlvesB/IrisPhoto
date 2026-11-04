# Iris Photo — instructions for agents

## LEIA PRIMEIRO: o Iris Photo é uma cópia modificada do PhotoCraft

**Esta seção vale mais que todo o resto (e que o AGENTS.md).** O AGENTS.md e a pasta `docs/` vieram do PhotoCraft; onde eles contradizerem esta seção, esta seção vence.

O Iris Photo é uma cópia (fork) do [PhotoCraft](https://github.com/storytold/photocraft), da equipe ArtCraft, adaptada para o ecossistema Iris por **Gabriel Alves Tech**. O repositório é `GabrielAlvesB/IrisPhoto` (remote `origin`); o PhotoCraft original é o remote `upstream`, usado **só para trazer atualizações**: nunca envie nada para ele. O IrisWord, o IrisGrid, o IrisDeck e o IrisVector são as cópias irmãs do WordCraft, do GridCraft, do DeckCraft e do VectorCraft e seguem as mesmas regras.

Todos os ajustes listados abaixo foram feitos de propósito. **Nenhuma atualização vinda do PhotoCraft pode desfazê-los.** Ao trazer uma versão nova, o trabalho é incorporar as melhorias do PhotoCraft **mantendo o Iris Photo como ele é**. Em qualquer conflito, fica o do Iris Photo. Na dúvida, pergunte ao dono antes de mudar.

### O que é do Iris Photo e não pode ser revertido

1. **Nome visível "Iris Photo"** (com espaço) em tudo que o usuário vê: título da janela, Sobre, tela inicial, menus, mensagens, traduções (chaves **e** traduções dos `.tsv` em `crates/ui-egui/src/i18n/`), nomes de perfis de cor embutidos, metadados gravados (`Creator` de EPS/DPX, LUT `.cube`), README, instaladores, pacotes. Onde um caminho não pode ter espaço, o nome é `IrisPhoto` (`IrisPhotoData`, `IrisPhoto.portable`, `IrisPhoto.app`, `Software\IrisPhoto`).
2. **"PhotoCraft" fica** só onde é crédito, licença ou compatibilidade de arquivo: `LICENSE-MIT`, `NOTICE`, `ATTRIBUTION.md`, `crates/ui-egui/src/i18n/LICENSE-translations.txt`, as frases "PhotoCraft contributors"/"PhotoCraft authors" (inclusive `LegalCopyright` em `apps/photocraft*/build.rs`), o link "Based on PhotoCraft" (`help.upstream`, `crates/ui-egui/src/links.rs`), o marcador XMP `x:xmptk="PhotoCraft"` (`crates/io/src/camera_raw_xmp.rs`: é como o app reconhece as configurações Camera Raw que ele mesmo gravou), as chaves `PhotoCraftVersion`/`PhotoCraftBuildCommit` do `Info.plist`, o formato `.pcraft` e os bytes de fixtures de teste. A busca por um VectorCraft instalado em `apps/photocraft/src/external_svg.rs` (editor externo de SVG) também fica.
3. **ID do app `com.iris.irisphoto`** (nunca `ai.storyteller.photocraft`): `packaging/linux/com.iris.irisphoto.*`, `packaging/linux/flatpak/com.iris.irisphoto*.yml`, `packaging/macos/Info.plist.in`, os ícones em `assets/app-icon/hicolor/*/apps/`.
4. **Ícone do Iris Photo** (uma foto, montanhas e sol, entre marcas de corte; gradiente `#00b1cc` → `#0083f4`, o mesmo desenho do Workspace do Iris, matiz 205): a fonte é `assets/app-icon/photocraft.svg` (cópia em `photocraft-small.svg`); todo PNG, `.ico` e `.icns` de `assets/app-icon/` é gerado dela (veja `assets/app-icon/README.md`). Nunca aceite ícones do PhotoCraft (a raposa de nove caudas) por cima destes. A cópia da raposa em `crates/io/src/svg/clip-path-tile.svg` é só caso de teste do importador de SVG.
5. **Sem marca ArtCraft**: sem `docs/brand/`, sem logos ArtCraft, sem links para `getartcraft.com` ou `discord.gg/artcraft`, sem botão "Join us on Discord" (tirado da tela inicial e do Sobre), sem os itens de Ajuda `help.discord`, `help.website` e `help.artcraftWebsite` (e sem as linhas deles nas traduções), sem a seção "Crafting Apps" no README, sem selos SonarCloud/CodeRabbit. O menu Ajuda tem `help.github` (Iris Photo), `help.upstream` (crédito ao original) e `help.reportIssue`. Se uma atualização trouxer menções novas à ArtCraft ou à suíte Crafting Apps na interface, retire-as: a licença de marca da ArtCraft exige isso de forks.
6. **Atualizador** (`UPDATE_APP` em `crates/ui-egui/src/lib.rs`): `name: "Iris Photo"`, `repo: "GabrielAlvesB/IrisPhoto"` (exatamente assim: o atualizador compara os links diferenciando maiúsculas), `developer: "Gabriel Alves Tech"`, `accent` `#008ce8`. `RELEASES` em `crates/engine/src/cmd/updates_cmds.rs` aponta para o mesmo repositório. Nunca volte para `craft_update::suite::PHOTOCRAFT` puro: o app passaria a baixar o PhotoCraft por cima do Iris Photo.
7. **Instalador do Windows** (`packaging/windows/`): `UpgradeCode` `864206C8-E7B3-4383-91B3-9921396A74F1` (em `photocraft.wxs` e `install-policy.json`), GUID do componente `PhotocraftApp` `52B7EAFD-CEE3-469D-B6F4-E55EA1F04FC6` (nunca os do PhotoCraft, senão um instalador substitui o outro), ProgIds `IrisPhoto.Document`/`IrisPhoto.PhotoshopDocument`/`IrisPhoto.Image`, fabricante `Gabriel Alves Tech`, pasta `Programs\Iris\Iris Photo`, chave de registro `Software\IrisPhoto`, links do Painel de Controle para `GabrielAlvesB/IrisPhoto`. Em `apps/photocraft*/build.rs`, `CompanyName` é `Gabriel Alves Tech`.
8. **Pastas de dados próprias**: no Windows `%APPDATA%\IrisPhoto` (recuperação em `%LOCALAPPDATA%\IrisPhoto\Recovery`), no macOS `Application Support/IrisPhoto` e a pasta portátil `IrisPhotoData` (`apps/photocraft/src/app_dirs.rs`), para não misturar com um PhotoCraft instalado. No Linux a pasta continua `~/.config/photocraft` (nome interno).
9. **CI próprio**: `.github/workflows/ci.yml` é autocontido (sem segredos, token só de leitura, actions fixadas por SHA). **Nunca restaure** os workflows do PhotoCraft (`storytold/craft-common-actions@...`, SonarCloud, release, FreeBSD, nix, packaging-lint, perf-nightly, docs, windows-arm64), nem `sonar-project.properties` ou `.coderabbit.yaml`: eles rodam código de outra organização e repassam segredos. O Dependabot roda semanalmente (`America/Sao_Paulo`). As instruções de CI do AGENTS.md e de `docs/` são do PhotoCraft e **não valem** aqui.
10. **Segurança**: `cargo audit` precisa terminar sem vulnerabilidades (os avisos de crates "unmaintained"/"yanked" são aceitos). Na criação do fork foram atualizados `rustls-webpki` (≥ 0.103.16, RUSTSEC-2026-0382) e `webpki-root-certs` (≥ 1.0.10, RUSTSEC-2026-0383); `tokio` ≥ 1.54.1 e `zip` ≥ 9. Não volte para versões antigas. O servidor de controle (`--control`) só escuta em `127.0.0.1`, exige token e fica desligado sem a flag; não mude isso.
11. **Nomes internos mantidos de propósito**: crates `photocraft-*`, binários `photocraft.exe` e `photocraft-cli`, pastas `apps/photocraft*`, arquivos `assets/app-icon/photocraft.*`, `packaging/windows/photocraft.wxs`, a extensão `.pcraft`, variáveis `PHOTOCRAFT_*`, identificadores Rust (`PhotocraftApp`, `PhotocraftMcp`) e os nomes dos arquivos de release (`photocraft-<versão>-windows-<arq>.msi`, `...-portable.zip`). O atualizador e o Workspace do Iris (`../Iris/src/shared/types/workspace.types.ts`) procuram exatamente esses nomes. Não renomeie.

### Pendências conhecidas

- O fundo do DMG do macOS (`packaging/macos/dmg/background.tiff`) ainda tem o ícone antigo embutido; regenere com `packaging/macos/dmg/generate.py` num Mac antes de publicar uma versão para macOS.
- `docs/` e `book/` são a documentação técnica do PhotoCraft com o nome trocado; links para issues `storytold/photocraft#...` ali e em comentários do código são referências históricas e ficam.

### Como trazer uma atualização do PhotoCraft

1. `git fetch upstream --tags` e escolha a tag nova (por exemplo `v0.21.0`). Faça o merge numa branch: `git switch -c update-photocraft-0.21.0 && git merge v0.21.0`.
2. Resolva os conflitos mantendo **tudo** da lista acima. Arquivos removidos no Iris Photo (`docs/brand/`, workflows antigos, `sonar-project.properties`, `.coderabbit.yaml`) continuam removidos.
3. Procure o que entrou de novo com a marca antiga e adapte: `git grep -n -e PhotoCraft -e ArtCraft -e getartcraft -e discord -e ai.storyteller -e "storytold/photocraft" -e "Learning Machines"`. Texto visível novo vira "Iris Photo" (nas chaves **e** nas traduções dos `.tsv`, senão a tradução deixa de casar); créditos e os marcadores do item 2 ficam como estão.
4. Arquivos novos em `.github/workflows/` vindos do PhotoCraft: apague. Ícones ou imagens novas de marca: não adote.
5. Se a revisão de `craft-libs` mudou, confira se `craft_update::app::App` ainda aceita os campos do `UPDATE_APP`.
6. Rode `cargo audit`, `cargo xtask assets` e os testes (`cargo test --workspace --no-fail-fast`). Nesta máquina, use `CARGO_BUILD_JOBS=4` (com o paralelismo padrão o Windows fica sem memória virtual, os error 1455).
7. **Versão**: a versão do Iris Photo (`[workspace.package]` no `Cargo.toml`) precisa ser **maior que a da última release do Iris Photo**, senão o atualizador não oferece a nova. Rode `cargo update -w` depois de mudar.
8. Mostre ao dono o que mudou antes de fazer merge na `main` e de publicar release.

### Como publicar uma release

1. Versão nova no `Cargo.toml`, `cargo update -w`, commit e push na `main`.
2. Rode `packaging/windows/package.ps1 -Arch x64` num processo `powershell.exe` separado, com a saída indo para arquivo (no PowerShell 5.1, redirecionar dentro do mesmo processo transforma avisos do cargo em erro fatal). Precisa do WiX 5 (`dotnet tool install --global wix --version 5.0.2`). Saída em `dist/release/`.
3. Gere `dist/release/SHA256SUMS.txt` no formato `hash  nome` (dois espaços, sem `*`). O Workspace do Iris não instala sem ele.
4. `gh release create v<versão> -R GabrielAlvesB/IrisPhoto --title "Iris Photo v<versão>" --notes-file <notas.md> --latest` com o `.msi`, o `-portable.zip` e o `SHA256SUMS.txt`. Notas em português.
5. Os builds não têm assinatura Authenticode, então o próprio Iris Photo só avisa da versão nova e abre a página da release; a instalação automática acontece pelo Workspace do Iris.

## O resto

Para arquitetura, convenções de código e verificação, siga o [AGENTS.md](AGENTS.md) (do PhotoCraft), respeitando a seção acima.
