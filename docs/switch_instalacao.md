# Forza Horizon no Nintendo Switch (forzah.nro) — instalação

Build experimental, **ainda não testada em hardware**.

## Cartão SD
```
sdmc:/switch/forzah/forzah.nro
sdmc:/switch/forzah/game_root/      <- arquivos extraídos do SEU Forza Horizon (Xbox 360, 4D5309C9)
    default.xex
    xmediafacade_default.xex
    speechfacade_default.xex
    ... (todo o conteúdo do disco/GoD, ~6,8 GB)
```
Abra pelo Homebrew Menu **em modo de título** (segure R ao abrir um jogo), não pelo álbum (applet tem pouca RAM).

## Limitações conhecidas
- No PC a cena 3D saiu preta (HUD e entrada funcionam); pode ocorrer o mesmo aqui.
- Shaders são traduzidos/compilados em tempo de execução: os primeiros minutos tendem a travar.
- Desempenho no Switch Lite desconhecido.

## Recompilar
`tools/switch/preparar_ambiente.sh` → `tools/switch/build_mesa_nvk_host.sh` (no mesa-switch) → `tools/switch/build_nro.sh`.
