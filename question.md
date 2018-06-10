1. 为什么要分开设置 4 次
```
unsigned char table[256];
  unsigned char *p = memset (table, 0, 64);
  memset (p + 64, 0, 64);
  memset (p + 128, 0, 64);
  memset (p + 192, 0, 64);
```