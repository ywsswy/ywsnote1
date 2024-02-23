说明：
【vim.overrideCopy】: true，复制命令使用系统的ctrl+C

{
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
    "C_Cpp.clang_format_sortIncludes": false,
    "clang-format.fallbackStyle": "Google",
    "clang-format.executable": "/root/.vscode-server/extensions/ms-vscode.cpptools-1.19.2-linux-x64/LLVM/bin/clang-format",
    "editor.minimap.maxColumn": 120,
    "editor.rulers": [
        80,
        100,
        120
    ],
    "remote.SSH.showLoginTerminal": false,
    "files.exclude": {
        "**/.git": true,
        "**/.svn": true,
        "**/.hg": true,
        "**/CVS": true,
        "**/.DS_Store": true,
        "**/tmp": true,
        "**/node_modules": true,
        "**/bower_components": true,
        "**/dist": true
    },
    "files.watcherExclude": {
        "**/.git/objects/**": true,
        "**/.git/subtree-cache/**": true,
        "**/node_modules/**": true,
        "**/tmp/**": true,
        "**/bower_components/**": true,
        "**/dist/**": true
    },
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
    "[javascript]": {
        "editor.defaultFormatter": "vscode.typescript-language-features"
    },
    "security.workspace.trust.untrustedFiles": "open",
    "[json]": {
        "editor.defaultFormatter": "vscode.json-language-features"
    },
    "git.ignoreLegacyWarning": true,
    "terminal.integrated.enableMultiLinePasteWarning": false,
    "[go]": {
        "editor.defaultFormatter": "golang.go",
        "editor.formatOnSave": true
    },
    "gitlens.advanced.messages": {
        "suppressGitVersionWarning": true
    },
    "go.useLanguageServer": true,
    "go.buildFlags": [
        "-gcflags=all=-l"
    ],
    "vim.useSystemClipboard": true,
    "vim.overrideCopy": true,
    "terminal.integrated.shellIntegration.enabled": false
}