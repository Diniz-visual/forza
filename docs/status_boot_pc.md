# Status do boot no PC (Linux)

![PRESS START](../../screenshots/screenshot_1791831003773.png)
![Menu principal](../../screenshots/screenshot_1791832335384.png)
![Jogabilidade: HUD e velocímetro respondendo, cena 3D preta](../../screenshots/screenshot_1791834749711.png)

## O que funciona
- Intros → PRESS START → tela de VIDEO (1ª execução, perfil ForzaProfile salvo) → menu SINGLE PLAYER → vídeo da campanha → carregamento do mundo → **jogabilidade**.
- Entrada pelo teclado (driver MnK): Return/X = Start, Space = A, E = acelerar, Q = frear, WASD = direção. Segurar E levou o carro a 31 mph (o velocímetro e o minimapa respondem).
- Fibras do XAPI mapeadas para os ganchos `rexcrt` (tabela `[rexcrt]` no fim do `overrides.toml`; ela precisa continuar sendo a última tabela).
- Codegen: 0 Unresolved, 0 REX_FATAL. Roda por mais de 10 min sem crash.
- Script: `FORZAH_ARGS='...' tools/rodar_ate_menu.sh` (chega ao Single Player sozinho).

## Pendências
- **Cena 3D preta.** Só o HUD aparece. No lavapipe, os pipelines continuam sendo compilados por minutos; com `async_shader_compilation`, os draws usam um placeholder até a compilação terminar. Próximo teste: `--async_shader_compilation=false` e esperar mais. Depois, verificar os render targets e as resolves da cena.
- "Resolve region is empty": **provavelmente benigno.** O jogo usa predicated tiling (faixas de 512+208 ou 256×3). Cada faixa emite as resolves de todos os retângulos, e só a do próprio tile tem área. As vazias seriam as dos outros tiles, e o Xenia upstream se comporta igual (mesma lógica em `GetResolveInfo`).
- Testado com `--gpu_allow_invalid_fetch_constants=true --vulkan_async_skip_incomplete_frames=false`.
- Depois: build do Switch.
