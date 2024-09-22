由'privileges for schema public'可知
由于role 'yukari'和schema 'public'是有依赖的
使用数据库语句
```SQL
REVOKE ALL PRIVILEGES ON SCHEMA public from yukari;
```
移除role'yukari'依赖的schema 'public'即可
![IMG_20240922_185911_091](https://github.com/user-attachments/assets/031372dc-f0bc-4a4d-976e-2d8765b73d67)
