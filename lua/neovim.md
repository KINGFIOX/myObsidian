假设我们有这样的文件目录
```Shell
~/.config/nvim
|-- after/
|-- ftplugin/
|-- lua/
|  |-- myluamodule.lua
|  |-- other_modules/
|     |-- anothermodule.lua
|     |-- init.lua
|-- plugin/
|-- syntax/
|-- init.vim
```
在主入口init.lua中写入
```lUa
require('other_modules') -- loads other_modules/init.lua
```
就表示加载oterh_modules里面的init.lUa

### neovim-lua的require只会执行一次，并不会反复执行
假如想要执行多次的话
```lua
package.loaded['myluamodule'] = nil
require('myluamodule')    -- read and execute the module again from disk
```
先释放，再读取








