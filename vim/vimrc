call plug#begin('~/.vim/plugged')
	" Theme and airline
	Plug 'morhetz/gruvbox'
	Plug 'itchyny/lightline.vim'
	" to comment stuff
	Plug 'tpope/vim-commentary'
	"auto close brackets
	Plug 'Raimondi/delimitMate'
	" Latex auto-compsetion:
	Plug 'lervag/vimtex'
	" Add Parentesis on a world 
	Plug 'tpope/vim-surround'
	" Float terminal
	Plug 'voldikss/vim-floaterm'

call plug#end()

colorscheme gruvbox
set background=dark
set belloff=all
set tabstop=4
" set mouse=a
set title
set hlsearch
set incsearch
set nocompatible
set number                      
set laststatus=2
let g:lightline={'colorscheme':'gruvbox'}
syntax on

nnoremap   <silent>   <F7>    :FloatermNew<CR>
tnoremap   <silent>   <F7>    <C-\><C-n>:FloatermNew<CR>
nnoremap   <silent>   <F8>    :FloatermPrev<CR>
tnoremap   <silent>   <F8>    <C-\><C-n>:FloatermPrev<CR>
nnoremap   <silent>   <F9>    :FloatermNext<CR>
tnoremap   <silent>   <F9>    <C-\><C-n>:FloatermNext<CR>
nnoremap   <silent>   <F12>   :FloatermToggle<CR>
tnoremap   <silent>   <F12>   <C-\><C-n>:FloatermToggle<CR>
