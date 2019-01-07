# kong-plugin-ram
## Resource Access Management on top of Kong

[![][https://camo.githubusercontent.com/9e4fe7914c7357861223aa535d7ca9858253c96e/68747470733a2f2f6b6f6e6768712e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031382f30352f6b6f6e672d6c6f676f2d6769746875622d726561646d652e706e67]][https://konghq.com]

安装resty(/usr/bin/resty)
```
yum install -y yum-utils
yum-config-manager --add-repo https://openresty.org/package/centos/openresty.repo
yum install -y openresty  openresty-resty
rpm -ql openresty-resty
```

安装dev(DEV_ROCKS = "busted 2.0.rc13" "luacheck 0.20.0" "lua-llthreads2 0.1.5")
```
yum install -y  lua luarocks git openssl-devel
git clone https://github.com/Kong/kong
cd kong
git checkout 1.0.0
luarocks make
make dev
luarocks list
```

静态检查(linting 使用luacheck )
```
make lint
```

单元测试（Unit Testing）
解决：编译的默认路径/usr/local/share/lua/5.1/socket.lua->实际安装路径/usr/share/lua/5.1/socket.lua不匹配问题(其中';;'代表编译的默认路径)
```
export LUA_PATH="/usr/share/lua/5.1/?.lua;/usr/share/lua/5.1/?/init.lua;;"
export LUA_CPATH="/usr/lib64/lua/5.1/?.so;;"
make test
```

集成测试 Integration Testing
```
sed -i 's/lua_package_/# lua_package_/g' ./spec/kong_tests.conf
echo  'lua_package_path=/usr/share/lua/5.1/?.lua;/usr/share/lua/5.1/?/init.lua;./spec/fixtures/custom_plugins/?.lua;;' >> ./spec/kong_tests.conf
echo  'lua_package_cpath=/usr/lib64/lua/5.1/?.so;;' >> ./spec/kong_tests.conf
```

插件测试 Plugins Testing
```
make test-plugins
```
