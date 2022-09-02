# issabel improve Call Waiting
Call Waiting on issabel has the problem that the caller cannot understand that he is waiting behind the line.

For solved this problem you must edit `[macro-dial-one]`

This macro used for call extensions to extsions and outside to extensions.

1. Make sure the call waiting is enabled on your extensions

2. open `/etc/asterisk/extensions_additional.conf` and search for `[macro-dial-one]` then copy all this macro

3. open `/etc/asterisk/extensions_override_issabelpbx.conf` paste `[macro-dial-one]` here

4. search for this line `exten => s,n(godial),Dial(${DSTRING},${ARG1},${D_OPTIONS})`  delete it and copy and paste code below instead of it

``` astereisk
;--== improve Call Waiting ==--;

exten => s,n(godial),GotoIf($["${EXTENSION_STATE(${DEXTEN})}" = "INUSE"]?nextinuse:nextnotuse)
exten => s,n(nextinuse),Playback(custom/ext-inused-pr)
exten => s,n,Set(D_OPTIONS=${D_OPTIONS}m)

exten => s,n(nextnotuse),Dial(${DSTRING},${ARG1},${D_OPTIONS})

;--== end of improve Call Waiting ==--;

```

6. upload sound `ext-inused-pr` to your issabelpbx. (this sound in persion language)
```astereisk
System Recordings → Upload recording

```

7. reload the asterisk
```
asterisk -rx "reload"
```