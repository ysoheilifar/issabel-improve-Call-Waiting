# issabel improve Call Waiting
Call Waiting on issabel has the problem that the caller cannot understand that he is waiting behind the line.

1. Make sure the call waiting is enabled on your extensions

2. open `/etc/asterisk/extensions_additional.conf` and search for `[macro-dial-one]` then copy all this macro

3. open `/etc/asterisk/extensions_override_issabelpbx.conf` paste `[macro-dial-one]` here

4. search for this line `exten => s,n(godial),Dial(${DSTRING},${ARG1},${D_OPTIONS})`  delet it and edit it like below

``` astereisk
exten => s,n,Set(D_OPTIONS=${D_OPTIONS}I)

;--== improve Call Waiting ==--;

exten => s,n(godial),GotoIf($["${EXTENSION_STATE(${DEXTEN})}" = "INUSE"]?nextinuse:nextnotuse)
exten => s,n(nextinuse),Playback(custom/ext-inused-pr)
exten => s,n,Set(D_OPTIONS=${D_OPTIONS}m(moh))

exten => s,n(nextnotuse),Dial(${DSTRING},${ARG1},${D_OPTIONS})

;--== end of improve Call Waiting ==--;

exten => s,n,ExecIf($["${DIALSTATUS}"="ANSWER" & "${CALLER_DEST}"!=""]?MacroExit())
```
5. reload the asterisk
```
asterisk -rx "reload"
```