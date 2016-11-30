# lua-argon2-ffi
[![Module Version][badge-version-image]][luarocks-argon2-ffi]
[![Build Status][badge-travis-image]][badge-travis-url]
[![Coverage Status][badge-coveralls-image]][badge-coveralls-url]

FFI binding of [Argon2] for LuaJIT.

While [lua-argon2] provides a PUC Lua binding through the Lua C API, this
module is a binding for the LuaJIT FFI, especially fit for use in
[ngx_lua]/[OpenResty].

### Prerequisites

The [Argon2] shared library must be compiled and available in your system.

Compatibility:
- Version `0.x` of this module is compatible with Argon2
  [`20151206`](https://github.com/P-H-C/phc-winner-argon2/releases/tag/20151206)
- Version `1.x` of this module is compatible with Argon2
  [`20160406`](https://github.com/P-H-C/phc-winner-argon2/releases/tag/20160406)
  and later.

See the [CI builds][badge-coveralls-url] for the status of the currently
supported versions.

### Install

This binding can be installed via [Luarocks](https://luarocks.org):

```
$ luarocks install argon2-ffi
```

Or simply by copying the `src/argon2.lua` file in your `LUA_PATH`.

### Usage

**Note**: lua-argon2-ffi uses the same API as [lua-argon2], to the exception of
the default settings capabilities of lua-argon2.

Encrypt:

```lua
local argon2 = require "argon2"
--- Prototype
-- local hash, err = argon2.encrypt(pwd, salt, opts)

--- Argon2i
local hash = assert(argon2.encrypt("password", "somesalt"))
-- hash is "$argon2i$m=12,t=2,p=1$c29tZXNhbHQ$ltrjNRFqTXmsHj++TFGZxg+zSg8hSrrSJiViCRns1HM"

--- Argon2d
local hash = assert(argon2.encrypt("password", "somesalt", {argon2d = true}))
-- hash is "$argon2d$m=12,t=2,p=1$c29tZXNhbHQ$mfklun4fYCbv2Hw0UnZZ56xAqWbjD+XRMSN9h6SfLe4"

-- Hashing options
local hash = assert(argon2.encrypt("password", "somesalt", {
  t_cost = 4,
  m_cost = 24,
  parallelism = 2,
  hash_len = 64
}))
-- hash is "$argon2i$v=19$m=24,t=4,p=2$c29tZXNhbHQ$NV3zeCzIhUhosd7jtTuifTEoPOb/aPAtO0oTYdZkWfNBXCglBgxVEiJy+tLG4j011vZRO3pnmG82Vc/C1B6Tzw"
```

Verify:

```lua
local argon2 = require "argon2"
--- Prototype
-- local ok, err = argon2.decrypt(hash, plain)

local hash = assert(argon2.encrypt("password", "somesalt"))
-- hash is an argon2i hash

assert(argon2.verify(hash, "password")) -- ok: true
assert(argon2.verify(hash, "passworld")) -- error: The password did not match
```

### Documentation

This module's API being the same as [lua-argon2]'s, the detailed documentation
is available at <http://thibaultcha.github.io/lua-argon2>.

### License

Work licensed under the MIT License. Please check
[P-H-C/phc-winner-argon2][Argon2] for license over Argon2 and the reference
implementation.

[Argon2]: https://github.com/P-H-C/phc-winner-argon2
[lua-argon2]: https://github.com/thibaultCha/lua-argon2
[luarocks-argon2-ffi]: http://luarocks.org/modules/thibaultcha/argon2-ffi

[ngx_lua]: https://github.com/openresty/lua-nginx-module
[OpenResty]: https://openresty.org

[badge-travis-url]: https://travis-ci.org/thibaultcha/lua-argon2-ffi
[badge-travis-image]: https://travis-ci.org/thibaultcha/lua-argon2-ffi.svg?branch=master
[badge-version-image]: https://img.shields.io/badge/version-1.0.0-blue.svg?style=flat
[badge-coveralls-url]: https://coveralls.io/github/thibaultcha/lua-argon2-ffi?branch=master
[badge-coveralls-image]: https://coveralls.io/repos/github/thibaultcha/lua-argon2-ffi/badge.svg?branch=master
