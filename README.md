## redis-cli key values hints

```c
/* Add the actual keys' values hint instead of only hint word "key" for GET command.
 *
 * This method depends on:
 * 1) The hint value to be "key"
 * 2) SCAN command(since: v2.8.0) with pattern[PREFIX*]
 * 
 * The default hints_keys_values is '0' (OFF).
 * The default hints_keys_values_count is 5 keys.
 */
 ```


-------------------------------------------------------------

<center>

### [The PR HERE](https://github.com/redis/redis/pull/12215)

Here is a screencast demo:


https://github.com/khaledalam/redis-cli-key-values-hints/assets/8682067/89aba5ae-0784-4a3e-ac80-ed3e36e46499



</center>


It's like chating with server while typing before hint Enter but it works like that only if:
- User allow that by adding `:set hints-keys-values` in CLI preferences `~/.redisclirc` file  (i just changed the default value of this mechanism to be OFF)
- GET command is used
- Original hint value is the word `key`

### Example:
if we have stored pairs values like this:<br/>

<small>_MSET a test1 aa test2 aab test3 aaz test4 abc test5 aah test6 baa test7 bab test8 bbb test9 bbz test10_</small>

```
[
    "a":    "test1",
    "aa":   "test2",
    "aab":  "test3",
    "aaz":  "test4",
    "abc":  "test5",
    "aah":  "test6",
    "baa":  "test7",
    "bab":  "test8",
    "bbb":  "test9",
    "bbz":  "test10",
]
```


While writing in redis-cli `GET aa` the hint will be like this:

$ GET aa <span style="color:gray;">key: [ aa, aah, aaz, aab, ]</span>
<img width="368" alt="redis-key-values-hints1" src="https://github.com/redis/redis/assets/8682067/838b2fbb-1e27-429d-9524-e673591f01d8">

While writing in redis-cli `GET b` the hint will be like this:

$ GET b <span style="color:gray;">key: [ bbb, bba, bab, bbz, ]</span>
<img width="365" alt="redis-key-values-hints2" src="https://github.com/redis/redis/assets/8682067/e5340ccd-2dc1-4920-a5a5-68918e13972f">

While writing in redis-cli `GET ba` the hint will be like this:

$ GET ba <span style="color:gray;">key: [ baa, bab, ]</span>
<img width="309" alt="redis-key-values-hints3" src="https://github.com/redis/redis/assets/8682067/c38fecbb-be23-4b7c-9cce-b9cd6f28b91b">
## User preferences

- Added the ability of setting the actual key' values hint ON and OFF using `:set hints_keys_values` in `~/.redisclirc` file. (by default it's OFF)
- Added the ability of setting number of actual key' values items count (default: 5, max: 20) using `:set hints_keys_values_count X` in `~/.redisclirc` file.


