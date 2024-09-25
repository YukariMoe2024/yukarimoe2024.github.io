对照以下填写即可  
```systemD Service
[Unit]
Description=Service Name
After=network.target

[Service]
User=User Name
WorkingDirectory=/dir
ExecStart=/usr/bin/chromium
Restart=always

[Install]
WantedBy=multi-user.target
```