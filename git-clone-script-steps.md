# git clone script steps
git clone脚本的基本步骤：
1. 设置shell脚本，通常是`set -eu` or `set -x`.可以根据提供的参数设置
2. 配置git 认证信息，如用户名密码方式，ssh公钥方式，ssl-ca方式
3. 配置git clone前是否需要清理
4. 检查是否配置HTTP/HTTPS代理
5. 配置safe.directory
6. git-init使用上面配置的参数进行clone
7. 进入CHECKOUT_DIR，使用相关git命令获取给一些变量赋值，如 RESULT_SHA="$(git rev-parse HEAD)"
