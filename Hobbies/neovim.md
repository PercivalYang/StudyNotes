记录自己在配置coc-vim中间的步骤  
## vim->nvim
vim中总会发生插件的报错问题，便换成了nvim,步骤如下：
- neovim的安装
    - stable 版本:
    ```shell
    sudo add-apt-repository ppa:neovim-ppa/stable
    sudo apt update
    sudo apt install -y neovim
    ```
    - unstable 版本:
    ```shell
    sudo add-apt-repository ppa:neovim-ppa/unstable
    sudo apt update
    sudo apt install -y neovim
    ```
- 创建vim配置文件夹，转移vimrc中的配置
    ```shell
    mkdir -p ~/.config/nvim
    cd ~/.config/nvim
    cp ~/.vimrc /.config/nvim/init.vim
    ```
- 为了方便和避免错误启动`vim`，可以设置别名：`alias vim=nvim`
# Plugings
## vim-plug
- vim的插件包管理器，可以快速安装和卸载插件；
- 仓库地址: https://github.com/junegunn/vim-plug；
- 安装指令(针对Linux/Unix下的Neovim，其他版本请参考[这里](https://github.com/junegunn/vim-plug#installation)): 
    ```shell
    sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
           https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
   ```
- 如何安装插件? 打开`init.vim`(对应vim的.vimrc)
    - `call plug#begin([PLUGIN_DIR])`: 插件列表块的开头，可以自定义插件安装路径`PLUGIN_DIR`
    - 安装插件时，通过`Plug`加上其github的仓库地址即可，插件安装时为自动Clone到`PLUGIN_DIR`处
    - `call plug#end()`: 插件列表的结尾
    ```shell
    call plug#begin()
    Plug 'preservim/nerdtree'
    call plug#end()
    ```
> 由于vim-plug是从github clone仓库的方式来安装插件，我在最初安装时,试过通过修改hosts、在plug.vim中采用镜像网站的方法，但都不适用于我，目前采用挂代理下载插件，推荐[Clash for Windows](https://github.com/Fndroid/clash_for_windows_pkg/releases).  
> 关于Clash for Windows的配置方法，和在终端的使用，请查看我的[这篇]()文章。
## coc-nvim
- 刚开始我以为这个只是个和YCM一样的自动补全插件，使用过后发现他类似Vscode中的Extensions Marketplace，可以理解为管理插件的插件
>vim-plug：我是什么？？
- 仓库地址: https://github.com/neoclide/coc.nvim
- 
## markdown-preview
- 用来实时预览markdown的插件;
- 仓库地址：https://github.com/iamcco/markdown-preview.nvim
- 前置需求：nodejs, yarn
- 安装指令：`Plug 'iamcco/markdown-preview.nvim', { 'do': 'cd app && yarn install' }`
- 配置：
    ```shell
    let g:mkdp_auto_start = 0  " 打开md文件自动开启preview(设置为1时开启)
    let g:mkdp_auto_close = 1  " 当切换到其他buffer时，preview 会自动关闭
    let g:mkdp_browser = '/usr/bin/microsoft-edge-stable' " 指定用来预览的浏览器路径,我这用的edge 
    let g:mkdp_echo_preview_url = 1 " 将preview对应的url输出到下方cmd line
    let g:mkdp_theme='dark' " 主题设置（dark or light）
    let g:mkdp_port = '[Port]' " 指定端口(要确保端口不被占用，否则会报错哦)
    let g:mkdp_open_ip = '[IP]' " 可实现在远程主机的vim上编写，同时在本地主机实时预览

    nmap <leader>s <Plug>MarkdownPreview " 开启preview,自动创建端口链接
    nmap <leader>t <Plug>MarkdownPreviewStop " 关闭当前preview
    nmap <leader>p <Plug>MarkdownPreviewToggle " 等同上面两个结合，开启或关闭preview
    ```
- 其他：
    - 关于远程预览的效果图，可以看[这里](https://github.com/iamcco/markdown-preview.nvim/pull/9)
    - Preview的主题可以自己导入，设置如下，主题可以去[Typora Theme Gallery](https://theme.typora.io/)里：
    ```shell
    " 指定你的markdown.css路径
    let g:mkdp_markdown_css = ''

    " 指定你的highlight.css路径
    let g:mkdp_highlight_css = ''
    " 注意上述css文件一定都要是绝对路径!
    ```
    
## NERD-Tree
- 目前只是用来在vim内预览文件目录，和打开文件夹中的文件，其他功能还未尝试
- 仓库地址：https://github.com/preservim/nerdtree
- 安装指令：`Plug 'preservim/nerdtree'`
- 配置：
    ```shell
    let mapleader=";" " 重映射leader,默认是\
    nnoremap <leader>n :NERDTreeFocus<CR> " 用来将光标切换到文件目录
    map <F2> :NERDTreeToggle<CR> " 按F2打开文件目录
    " 默认情况NerdTree需要手动关闭,这样你返回shell界面时还要再多:q一遍NerdTree
    " 下面是命令当NerdTree是目前唯一窗口时自动关闭
    autocmd BufEnter * if winnr('$') == 1 && exists('b:NERDTree') && b:NERDTree.isTabTree() | quit | endif
    ```
## Airline
- 
## Rainbow
