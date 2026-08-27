说明：
【vim.overrideCopy】: true，复制命令使用系统的ctrl+C
【"update.mode"】: "none" 这个可以关闭自动更新
【"chat.byokUtilityModelDefault"】: "mainAgent" 这是BYOK下agent的配置
【"chat.agentHost.byokModels.enabled"】: true,同上

怎么查看一个配置的默认值，json里，输入完key: 会自动弹出补全下拉框，会显示默认值，如果是实验性的配置，默认值可能会调整，所以最好显式写出来


at set1 
{
    "gopls": {
        "build.directoryFilters": ["-plugin"]
    },
    "[c]": {
        "editor.defaultFormatter": "xaver.clang-format",
        "editor.formatOnSave": true,
        "editor.quickSuggestions": {
            "comments": "on",
            "strings": "on",
            "other": "on"
        }
    },
    "[cpp]": {
        "editor.defaultFormatter": "xaver.clang-format",
        "editor.formatOnSave": true,
        "editor.quickSuggestions": {
            "comments": "on",
            "strings": "on",
            "other": "on"
        }
    },
    "[go]": {
        "editor.defaultFormatter": "golang.go",
        "editor.formatOnSave": true
    },
    "[javascript]": {
        "editor.defaultFormatter": "vscode.typescript-language-features"
    },
    "[json]": {
        "editor.defaultFormatter": "vscode.json-language-features"
    },
    "C_Cpp.clang_format_sortIncludes": false,
    "clang-format.fallbackStyle": "Google",
    "clang-format.executable": "/root/.vscode-server/extensions/ms-vscode.cpptools-1.32.2-linux-x64x/LLVM/bin/clang-format",
    "editor.minimap.maxColumn": 120,
    "editor.rulers": [
        80,
        100,
        120
    ],
    "remote.SSH.showLoginTerminal": false,
    "files.associations": {
        "exception": "cpp",
        "mutex": "cpp",
        "format": "cpp",
        "hash_map": "cpp",
        "hash_set": "cpp",
        "*.tcc": "cpp",
        "cctype": "cpp",
        "chrono": "cpp",
        "clocale": "cpp",
        "cmath": "cpp",
        "complex": "cpp",
        "condition_variable": "cpp",
        "csetjmp": "cpp",
        "csignal": "cpp",
        "cstdarg": "cpp",
        "cstdatomic": "cpp",
        "cstddef": "cpp",
        "cstdio": "cpp",
        "cstdlib": "cpp",
        "cstring": "cpp",
        "ctime": "cpp",
        "cwchar": "cpp",
        "cwctype": "cpp",
        "bitset": "cpp",
        "deque": "cpp",
        "list": "cpp",
        "unordered_map": "cpp",
        "unordered_set": "cpp",
        "vector": "cpp",
        "rope": "cpp",
        "slist": "cpp",
        "fstream": "cpp",
        "initializer_list": "cpp",
        "iomanip": "cpp",
        "iosfwd": "cpp",
        "iostream": "cpp",
        "istream": "cpp",
        "limits": "cpp",
        "new": "cpp",
        "ostream": "cpp",
        "numeric": "cpp",
        "ratio": "cpp",
        "sstream": "cpp",
        "stdexcept": "cpp",
        "streambuf": "cpp",
        "system_error": "cpp",
        "thread": "cpp",
        "tuple": "cpp",
        "type_traits": "cpp",
        "array": "cpp",
        "cfenv": "cpp",
        "cinttypes": "cpp",
        "cstdint": "cpp",
        "functional": "cpp",
        "hashtable": "cpp",
        "random": "cpp",
        "regex": "cpp",
        "utility": "cpp",
        "typeinfo": "cpp",
        "valarray": "cpp",
        "atomic": "cpp",
        "forward_list": "cpp",
        "future": "cpp",
        "scoped_allocator": "cpp",
        "typeindex": "cpp"
    },
    "explorer.confirmDragAndDrop": false,
    "workbench.editorAssociations": {
        "*.out": "default",
        "*.ipynb": "jupyter-notebook"
    },
    "editor.tabSize": 8,
    "editor.cursorStyle": "line",
    "editor.insertSpaces": false,
    "editor.lineNumbers": "on",
    "editor.wordSeparators": "/\\()\"':,.;<>~!@#$%^&*|+=[]{}`?-",
    "editor.renderControlCharacters": true,
    "editor.renderWhitespace": "all",
    "search.followSymlinks": false,
    "workbench.editor.showTabs": "multiple",
    "security.workspace.trust.untrustedFiles": "open",
    "git.ignoreLegacyWarning": true,
    "gitlens.advanced.messages": {
        "suppressGitVersionWarning": true
    },
    "go.useLanguageServer": true,
    "go.buildFlags": [
        "-gcflags=all=-l"
    ],
    "vim.useSystemClipboard": true,
    "vim.overrideCopy": true,
    "update.enableWindowsBackgroundUpdates": false,
    "terminal.integrated.enableMultiLinePasteWarning": "never",
    "terminal.integrated.initialHint": false,
    "gitlens.rebaseEditor.openOnPausedRebase": false,
    "files.autoSave": "off",
    "remote.autoForwardPortsSource": "hybrid",
    "terminal.integrated.profiles.linux": {
        "bash": {
            "path": "/usr/bin/bash"
        },
        "zsh": {
            "path": "/usr/bin/zsh"
        },
        "fish": {
            "path": "fish"
        },
        "tmux": {
            "path": "tmux",
            "icon": "terminal-tmux"
        },
        "pwsh": {
            "path": "pwsh",
            "icon": "terminal-powershell"
        }
    },
    "terminal.integrated.defaultProfile.linux": "bash",
    "terminal.integrated.profiles.windows": {
        "PowerShell": {
            "source": "PowerShell",
            "icon": "terminal-powershell"
        },
        "Command Prompt": {
            "path": [
                "${env:windir}\\Sysnative\\cmd.exe",
                "${env:windir}\\System32\\cmd.exe"
            ],
            "args": [],
            "icon": "terminal-cmd"
        },
        "Git Bash": {
            "path": "C:\\Program Files\\Git\\bin\\bash.exe",
            "args": [
                "--login",
                "-i"
            ]
        }
    },
    "terminal.integrated.defaultProfile.windows": "Git Bash",
    "chat.tools.terminal.autoApprove": {
        "git checkout": true
    },
    "chat.editing.confirmEditRequestRemoval": false,
    "chat.agentHost.byokModels.enabled": true,
    "chat.byokUtilityModelDefault": "mainAgent",
    "redhat.telemetry.enabled": false,
    "agents.voice.language": "zh"
}