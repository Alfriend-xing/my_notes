# vim-plugin
---

### 基本配置
```vim
set nocompatible "关闭与vi的兼容模式
syntax on    "语法高亮
set number "显示行号
set nowrap    "不自动折行
set showmatch    "显示匹配的括号
set scrolloff=3        "距离顶部和底部3行
set encoding=utf-8  "编码
set fenc=utf-8      "编码
set mouse=a        "启用鼠标
set hlsearch        "搜索高亮
```
### pep8配置
```vim
au Filetype python let python_highlight_all=1
au Filetype python set tabstop=4
au Filetype python set softtabstop=4
au Filetype python set shiftwidth=4
au Filetype python set textwidth=79
au Filetype python set expandtab
au Filetype python set autoindent
au Filetype python set fileformat=unix
autocmd Filetype python set foldmethod=indent
autocmd Filetype python set foldlevel=99
```
### 窗口移动：
```vim
vim的默认窗口移动方式是Ctrl+w Ctrl+hjkl，但是这样太过复杂，对此进行绑定：

nnoremap <C-J> <C-W><C-J>
nnoremap <C-K> <C-W><C-K>
nnoremap <C-L> <C-W><C-L>
nnoremap <C-H> <C-W><C-H>
```
### 插件
```vim
git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim
然后在vimrc中加入：

set nocompatible              " required
filetype off                  " required
" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')
" let Vundle manage Vundle, required
Plugin 'gmarik/Vundle.vim'
" Add all your plugins here (note older versions of Vundle used Bundle instead of Plugin)
" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
```
### 安装插件
```vim
:PluginInstall
或
vim +PluginInstall +qall
```
### 状态栏
```vim
vim-airline
Plugin 'bling/vim-airline'
```
### 编程提示
```vim
jedi-vim
Bundle 'davidhalter/jedi-vim'
```
### 使用tab键提示
```vim
Bundle 'ervandew/supertab'
```
### 格式化
```vim
Plugin 'Chiel92/vim-autoformat'
```
### 自动补全括号和引号
```vim
Plugin 'jiangmiao/auto-pairs'
```
### 缩进指示线
```vim
Plugin 'Yggdroot/indentLine'
```
### 语法检查 
```vim
Plugin 'scrooloose/syntastic'
https://blog.csdn.net/Demorngel/article/details/69053443
https://www.dreamxu.com/books/vim/plugin/syntastic.html
```
### 全局查找
```vim
Plugin 'ctrlpvim/ctrlp.vim'
```
### 调试
```vim
pdb，ipdb，logging
https://docs.python.org/2/howto/logging-cookbook.html
```
### 文件树
```vim
Plugin " scrooloose/nerdtree"
```
### 针对vim8的异步插件
```vim

```
### 识别python虚拟环境
```vim
vim-jedi可以自动识别 无需配置
" Add the virtualenv's site-packages to vim path
if has('python')
py << EOF
import os.path
import sys
import vim
if 'VIRTUAL_ENV' in os.environ:
    project_base_dir = os.environ['VIRTUAL_ENV']
    sys.path.insert(0, project_base_dir)
    activate_this = os.path.join(project_base_dir, 'bin/activate_this.py')
    execfile(activate_this, dict(__file__=activate_this))
EOF
endif
```



参考
https://zhuanlan.zhihu.com/p/30022074
https://www.cnblogs.com/linxiyue/p/7834817.html
https://www.zhihu.com/question/19655689
http://codingpy.com/article/vim-and-python-match-in-heaven/