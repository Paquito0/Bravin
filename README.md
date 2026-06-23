# Bravin

Otimizador de memória para navegadores Windows. Reduz o consumo de RAM dos processos do navegador periodicamente, liberando Working Set via `EmptyWorkingSet`.

## Funcionalidades

- Reduz memória do navegador em intervalos configuráveis (500ms a 2h)
- Limite de uso: só reduz se a memória ultrapassar o limite definido (MB)
- Processos estendidos: aplique a otimização a outros processos além do navegador
- Suporta Brave, Chrome, Edge, Firefox, Waterfox e qualquer navegador baseado em Chromium
- Modo seguro e incognito por navegador
- Atualização automática
- Multilíngue (Inglês, Português, Português do Brasil)
- Iniciar com o Windows (atalho na pasta Startup)

## Requisitos

- Windows Vista ou superior
- [AutoIt3](https://www.autoitscript.com/) (para compilar a partir do código fonte)

## Compilação

Abra `source/Bravin.au3` no SciTE4AutoIt3 e compile (F7). As diretivas `#AutoIt3Wrapper_*` no topo do arquivo controlam a compilação:

- Gera `Bravin.exe` (32-bit) e `Bravin_X64.exe` (64-bit) na raiz
- A versão incrementa automaticamente a cada compilação
- AU3Check é executado antes da compilação

Ou via linha de comando:

```
Aut2Exe /in source\Bravin.au3 /out Bravin.exe /x64
```

## Configuração

Edite `Bravin.ini` na raiz (modo portátil) ou em `%APPDATA%\Rizonesoft\Bravin\`:

```ini
[Bravin]
PortableEdition=1
BrowserPath=C:\Program Files\BraveSoftware\Brave-Browser\Application\brave.exe
BoostEnabled=1
Boost=500              ; intervalo em ms
LimitEnabled=1
ReduceLimit=10         ; MB - só reduz se acima disso
ExtendedProcs=         ; processos adicionais (separados por vírgula)
ShowNotifications=1
Language=pt-BR

[Safemode]
brave.exe=--disable-extensions
firefox.exe=-safe-mode

[Incognito]
brave.exe=-incognito
firefox.exe=-private-window
```

## Estrutura

```
Bravin/
├── source/
│   ├── Bravin.au3            # Código fonte principal
│   ├── Includes/             # Módulos (About, Update, Localization, etc.)
│   └── Resources/Icons/      # Ícones do aplicativo
├── Language/                 # Arquivos de idioma (.ini)
├── Bravin.exe                # Build 32-bit
├── Bravin_X64.exe            # Build 64-bit
└── Bravin.ini                # Configuração do usuário
```

## Licença

© 2024 Rizonesoft. Bravin by @marlonteixeira.
