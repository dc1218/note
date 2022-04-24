[toc]
编译配置：
cc main.c \`pkg-config --cflags lua5.3\` \`pkg-config --libs lua5.3\`

# luaAPI入栈操作：  
void lua_pushnil(lua_State* L) <br> --将常量nil入栈  
void lua_pushboolean (lua_State *L, int b)<br> --将b作为boolean值入栈  
void lua_pushnumber (lua_State *L, lua_Number n)<br> --将为n的浮点数入栈 
void lua_pushinteger(lua_State *L, lua_Integer n)<br> --将为n的整数入栈  
const char *lua_pushfstring(lua_State *L, const char *fmt, ...) <br> --将长度为"len"，非字面形式的字符串"s"入栈。Lua对这个字符串做一个内部副本（或是复用一个副本），因此"s"处的内存在函数返回后，可以释放掉或是立刻重用于其它用途。字符串内可以是任意二进制数据，包括'\0'。函数返回内部副本的指针。  
const char *lua_pushstring(lua_State *L, const char *s)<br> --等价于"lua_pushfstring()"。不过是用"va_list"接收参数，而不是用可变数量的实际参数。

# skynet 简介
```
git clone https://github.com/cloudwu/skynet.git
cd skynet
make linux     #make 'PLATFORM'  # PLATFORM can be linux, macosx, freebsd now
```
# skynet API
## skynet.call