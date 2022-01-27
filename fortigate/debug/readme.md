

```
config vdom
edit VD03-VDOM-CLIENT
execute ping 10.10.10.1

diagnose debug flow filter <-------------------- показывает фильтр
```

==============================================================

```
diagnose debug flow filter addr 8.8.8.8
diagnose debug enable
diagnose debug flow trace start 100
diagnose sniffer packet any "host 10.10.10.1"
```

==============================================================

```
diagnose sniffer packet any "host 10.10.10.1"
```

```
diagnose debug disable
```
