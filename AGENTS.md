# Bravin — RAM Limiter para Windows

## Linguagem & Toolchain

- **Linguagem:** AutoIt3 (não C/C++ nem C#)
- **Entrypoint único:** `source/Bravin.au3`
- **Includes:** `source/Includes/*.au3` (13 arquivos)
- **Compilador:** Aut2Exe (AutoIt3Wrapper) via SciTE4AutoIt3 ou linha de comando
  - Diretivas de compilação estão no topo de `Bravin.au3` (linhas 5–195)
  - Gera `Bravin.exe` (32-bit) e `Bravin_X64.exe` (64-bit) na raiz
- **Sem build scripts** (package.json, Makefile, etc.) — a compilação é controlada pelas diretivas `#AutoIt3Wrapper_*` no próprio fonte

## Configuração

- `Bravin.ini` na raiz (portable mode = default) ou `%APPDATA%\Rizonesoft\Bravin\Bravin.ini`
- Formato INI padrão do AutoIt (`IniRead`/`IniWrite`)
- Chaves principais: `BrowserPath`, `BoostEnabled`, `Boost` (ms), `LimitEnabled`, `ReduceLimit` (MB), `ExtendedProcs`, `ProcessPriority`, `ShowNotifications`, `Language`
- Seções `[Safemode]` e `[Incognito]` com flags por navegador (ex: `brave.exe=--disable-extensions`)
- `PortableEdition=1` = usa INI local; `=0` = usa `%APPDATA%`

## Arquitetura

- **`_StartCore()`** — ponto de partida: cria tray icon, registra `AdlibRegister("_ClearProcessesWorkingSet", $g_iBoostMill)`, loop `Sleep(55)`
- **`_StartCoreGui()`** — janela principal (GUI) com perfil do navegador, uso de memória, opções
- **`_ClearProcessesWorkingSet()`** — core: lista PIDs do processo alvo, soma WorkingSet via `_WinAPI_GetProcessMemoryInfo()`, se > limite chama `_WinAPI_EmptyWorkingSet()`
- **`_ClearExtendedProcs()`** — varre `ExtendedProcs` (separados por vírgula) e aplica mesma lógica
- **Singleton** via `_Singleton()` — apenas uma instância
- **Auto-elevação:** se 32-bit em Windows 64-bit, executa `Bravin_X64.exe`
- **Idiomas:** `Language/*.ini` (pt-BR, pt, + English hardcoded como default)
- **Startup:** usa atalho em `%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\` (não usa Task Scheduler nem registro)
- **Update:** download de `.ru` file de `https://cdn2.rizonesoft.com/update/`, compara build number

## Comandos

Não há comandos de dev — o projeto é compilado diretamente no SciTE ou via Aut2Exe CLI. Nenhum framework de teste, linter ou typechecker.

## Peculiaridades

- `#AutoIt3Wrapper_UseX64=Y` + `#AutoIt3Wrapper_Compile_both=Y` → compila duas archs
- `#AutoIt3Wrapper_Res_FileVersion_AutoIncrement=Y` → versão incrementa automaticamente
- `#AutoIt3Wrapper_Run_AU3Check=Y` + `#AutoIt3Wrapper_AU3Check_Stop_OnWarning=Y` → checagem ao compilar
- `Opt("GUIOnEventMode", 1)` — todo o app usa OnEvent mode, não MessageLoop
- `Opt("MustDeclareVars", 1)` — todas variáveis precisam ser declaradas
- `#NoTrayIcon` no início evita tray icon duplicado
- Donation nag: se `DonateName` difere de `@ComputerName` ou build mudou, mostra tela de doação ao fechar
- ProcessPriority 5 (Realtime) é bloqueado a menos que `SaveRealtime=1`
