# 考虑到需要使用vim

当前文件搜索
alt f

全局搜索
alt shift f

快速打开文件
alt p

可以打开>keyboard shortcuts然后快速复制如下快捷键设置

// Place your key bindings in this file to override the defaults
[
	{
		"key": "alt+f",
		"command": "actions.find",
		"when": "editorFocus || editorIsOpen"
	},
	{
		"key": "ctrl+f",
		"command": "-actions.find",
		"when": "editorFocus || editorIsOpen"
	},
	{
		"key": "shift+alt+f",
		"command": "-notebook.formatCell",
		"when": "editorHasDocumentFormattingProvider && editorTextFocus && inCompositeEditor && notebookEditable && !editorReadonly && activeEditor == 'workbench.editor.notebook'"
	},
	{
		"key": "shift+alt+f",
		"command": "-editor.action.formatDocument",
		"when": "editorHasDocumentFormattingProvider && editorTextFocus && !editorReadonly && !inCompositeEditor"
	},
	{
		"key": "shift+alt+f",
		"command": "-editor.action.formatDocument.none",
		"when": "editorTextFocus && !editorHasDocumentFormattingProvider && !editorReadonly"
	},
	{
		"key": "shift+alt+f",
		"command": "-notebook.format",
		"when": "notebookEditable && !editorTextFocus && activeEditor == 'workbench.editor.notebook'"
	},
	{
		"key": "shift+alt+f",
		"command": "-filesExplorer.findInFolder",
		"when": "explorerResourceIsFolder && filesExplorerFocus && foldersViewVisible && !inputFocus"
	},
	{
		"key": "shift+alt+f",
		"command": "-search.action.restrictSearchToFolder",
		"when": "folderMatchWithResourceFocus && searchViewletVisible"
	},
	{
		"key": "shift+alt+f",
		"command": "workbench.action.findInFiles"
	},
	{
		"key": "ctrl+shift+f",
		"command": "-workbench.action.findInFiles"
	},
	{
		"key": "shift+alt+f",
		"command": "workbench.view.search",
		"when": "workbench.view.search.active && neverMatch =~ /doesNotMatch/"
	},
	{
		"key": "ctrl+shift+f",
		"command": "-workbench.view.search",
		"when": "workbench.view.search.active && neverMatch =~ /doesNotMatch/"
	},
	{
		"key": "shift+alt+f",
		"command": "workbench.action.terminal.searchWorkspace",
		"when": "terminalFocus && terminalProcessSupported && terminalTextSelected"
	},
	{
		"key": "ctrl+shift+f",
		"command": "-workbench.action.terminal.searchWorkspace",
		"when": "terminalFocus && terminalProcessSupported && terminalTextSelected"
	},
	{
		"key": "alt+m",
		"command": "workbench.action.togglePanel"
	},
	{
		"key": "ctrl+j",
		"command": "-workbench.action.togglePanel"
	},
	{
		"key": "shift+alt+m",
		"command": "workbench.action.toggleMaximizedPanel"
	},
	{
		"key": "ctrl+p",
		"command": "-workbench.action.quickOpen"
	},
	{
		"key": "alt+w",
		"command": "-toggleSearchEditorWholeWord",
		"when": "inSearchEditor && searchInputBoxFocus"
	},
	{
		"key": "alt+w",
		"command": "-workbench.action.terminal.toggleFindWholeWord",
		"when": "terminalFindVisible && terminalHasBeenCreated || terminalFindVisible && terminalProcessSupported"
	},
	{
		"key": "alt+w",
		"command": "-toggleFindWholeWord",
		"when": "editorFocus"
	},
	{
		"key": "alt+w",
		"command": "-toggleSearchWholeWord",
		"when": "searchViewletFocus"
	},
	{
		"key": "alt+w",
		"command": "workbench.action.closeActiveEditor"
	},
	{
		"key": "ctrl+f4",
		"command": "-workbench.action.closeActiveEditor"
	},
	{
		"key": "ctrl+f",
		"command": "-notebook.find",
		"when": "notebookEditorFocused && !editorFocus && activeEditor == 'workbench.editor.interactive' || notebookEditorFocused && !editorFocus && activeEditor == 'workbench.editor.notebook'"
	},
	{
		"key": "ctrl+f",
		"command": "-settings.action.search",
		"when": "inSettingsEditor"
	},
	{
		"key": "ctrl+f",
		"command": "-workbench.action.terminal.focusFind",
		"when": "terminalFindFocused && terminalHasBeenCreated || terminalFindFocused && terminalProcessSupported || terminalFocusInAny && terminalHasBeenCreated || terminalFocusInAny && terminalProcessSupported"
	},
	{
		"key": "ctrl+f",
		"command": "-repl.action.filter",
		"when": "inDebugRepl && textInputFocus"
	},
	{
		"key": "ctrl+f",
		"command": "-problems.action.focusFilter",
		"when": "focusedView == 'workbench.panel.markers.view'"
	},
	{
		"key": "ctrl+f",
		"command": "-commentsFocusFilter",
		"when": "focusedView == 'workbench.panel.comments'"
	},
	{
		"key": "ctrl+f",
		"command": "-editor.action.extensioneditor.showfind",
		"when": "!editorFocus && activeEditor == 'workbench.editor.extension'"
	},
	{
		"key": "ctrl+f",
		"command": "-editor.action.webvieweditor.showFind",
		"when": "webviewFindWidgetEnabled && !editorFocus && activeEditor == 'WebviewEditor'"
	},
	{
		"key": "ctrl+f",
		"command": "-keybindings.editor.searchKeybindings",
		"when": "inKeybindings"
	},
	{
		"key": "ctrl+e",
		"command": "-workbench.action.quickOpen"
	},
	{
		"key": "ctrl+e",
		"command": "-editor.action.toggleScreenReaderAccessibilityMode",
		"when": "accessibilityHelpIsShown"
	},
	{
		"key": "ctrl+e",
		"command": "-workbench.action.quickOpenNavigateNextInFilePicker",
		"when": "inFilesPicker && inQuickOpen"
	},
	{
		"key": "alt+p",
		"command": "-keybindings.editor.toggleSortByPrecedence",
		"when": "inKeybindings"
	},
	{
		"key": "alt+p",
		"command": "-togglePreserveCase",
		"when": "editorFocus"
	},
	{
		"key": "alt+p",
		"command": "-toggleSearchPreserveCase",
		"when": "searchViewletFocus"
	},
	{
		"key": "alt+p",
		"command": "workbench.action.quickOpen"
	}
]