" 区域buffer的复制粘贴
function! HOpen(what_to_open)
    let [type,name] = a:what_to_open
    if type=='buffer'
        exec 'buffer '.name
    else
        exec 'edit '.name
    end 
endfunction

function! HYankWindow()
    let g:window = winnr()
    let g:buffer = bufnr('%')
    let g:bufhidden = &bufhidden
endfunction

function! HPasteWindow()
    let old_buffer = bufnr('%')
    call HOpen(['buffer',g:buffer])
    let g:buffer = old_buffer
    let &bufhidden = g:bufhidden
endfunction

nnoremap <c-w>y :call HYankWindow()<cr>
nnoremap <c-w>p :call HPasteWindow()<cr>
