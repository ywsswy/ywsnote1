https://code.visualstudio.com/docs/remote/faq#_can-i-run-vs-code-server-on-older-linux-distributions

升级远端服务器，或者设置vscode的环境变量，它支持了patchelf能力（加载一个高版本的sysroot（glibc），不使用系统的）

下载高版本glibc和patchelf工具，网上网盘有

在~/.bashrc里写上
source $HOME/<path_to_vscode-run-env>/set-env.sh

里面其实就是设置了三个环境变量
export VSCODE_SERVER_CUSTOM_GLIBC_LINKER="$SCRIPT_DIR/sysroot/lib/ld-linux-x86-64.so.2"
export VSCODE_SERVER_CUSTOM_GLIBC_PATH="$SCRIPT_DIR/sysroot/lib"
export VSCODE_SERVER_PATCHELF_PATH="$SCRIPT_DIR/patchelf/bin/patchelf"

然后vscode-ssh（普通连接，而不是容器）上服务器上，vscode自己应该就会自动执行patchelf了

此时应该是patch到了/root/.vscode-server/cli/servers/Stable-<commit_id>/server里，（ps xfu看运行的node应该是这个目录，而不是/root/.vscode-server/bin/<commit_id>里）

判断是否成功patchelf成功：
ldd node程序，能看到libc.so => 新目录而不是系统目录，就说明成功了；


普通ssh成功后，再试ssh-容器
- 如果有报错的vscode脚本（check-requirements.sh），就在里面加上source ~/.bashrc（因为vscode是以非交互式的非登陆shell形式起来的）
- 如果还是有报错，可能是还使用的/root/.vscode-server/bin/<commit_id>目录，这时候直接暴力rm -rf然后把/root/.vscode-server/cli/servers/Stable-<commit_id>/server软链接过来，（之所以能这么搞因为node二进制不在乎服务器的环境，只看客户端版本的commit_id）