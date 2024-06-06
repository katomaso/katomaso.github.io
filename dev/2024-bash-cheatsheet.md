# Collection of useful bash things

### Replace (strip) string
```bash
${string//substring/replacement}
```

### Default value
```bash
X=${X:-default-value}  # x = (x or "default-value)
```

### Array length
```bash
if [ "${#COMP_WORDS[@]}" = "2" ];
```
