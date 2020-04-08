# npm

### 安装

```bash
# 安装
brew install nvm

# 在编辑页 保存以下配置
vim ~/.zshrc

# 待保存的配置
export NVM_DIR=~/.nvm
source $(brew --prefix nvm)/nvm.sh

# 重新激活配置
source ~/.zshrc

nvm -v

# 查看本地
nvm ls

# 线上可用版本
nvm ls-remote

nvm install 8.9.4
nvm use 8.9.4
nvm ls

# 使用 npm 安装 `json` 插件
npm install -g json
```