# An Example OpenResty / Lua authentication manager
This is an example project demonstrating how to build an authentication management system entirely within nginx using LUA and redis. Users will authenticate against a `/login` route and then be permitted to access other routes that are being proxied. 

h2. Overview
#`conf/example.conf`
- This is an example config illustrating how to setup a reverse ssl proxy and use the auth manager to require authentication to those routes.  Once a user posts the appropriate credentials to `/login` a unique cookie with some cyphertext is stored in their browser. In the future when a user tries to access a protected website an internal lua `authByGetRewrite` method may be invoked specifying the credentials needed to access the URL. 
#`lua/AuthManager.lua`
- This is the actual authentication manger implementation in LUA. There's tons of room for improvement but at a very high level it basically serves to take each inbound request, determine which permissions are needed, determine if the user has those permissions, and then either allow access or reject the request.
- 
h2. Installation
More work probably needs to be done to this before it can be used in any serious capacity but if you want to fool around with it follow the following steps:
# wget http://openresty.org/download/ngx_openresty-1.7.10.1.tar.gz
# tar xfz ngx_openresty-1.7.10.1.tar.gz
# cd ngx_openresty-1.7.10.1
# PATH=/sbin/:/usr/sbin:$PATH ./configure --with-pcre-jit --with-ipv6
# make -j2
# sudo make install
# cd ~/src
# wget http://keplerproject.github.io/luarocks/releases/luarocks-2.2.1.tar.gz
# tar xfz luarocks-2.2.1.tar.gz
# cd luarocks-2.2.1
# ./configure --prefix=/usr/local/openresty/luajit --with-lua=/usr/local/openresty/luajit/ --lua-suffix=jit-2.1.0-alpha --with-lua-include=/usr/local/openresty/luajit/include/luajit-2.1
# make
# sudo make install
# /usr/local/openresty/luajit/luajit/bin/luarocks install luafilesystem

you should then be able to spin up your nginx server and create users by visiting https://www.example.com/createUser?username=?&password=?&secret=?

