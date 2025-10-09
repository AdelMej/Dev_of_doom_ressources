# 🧱 1️⃣ Basic systemd commands

| Command                                | What it does                    |
| -------------------------------------- | ------------------------------- |
| `sudo systemctl start myapp.service`   | Start service now               |
| `sudo systemctl stop myapp.service`    | Stop service                    |
| `sudo systemctl restart myapp.service` | Restart service                 |
| `sudo systemctl reload myapp.service`  | Reload config (if supported)    |
| `sudo systemctl enable myapp.service`  | Auto-start on boot              |
| `sudo systemctl disable myapp.service` | Disable auto-start              |
| `systemctl status myapp.service`       | Show service status/log summary |
| `journalctl -u myapp.service`          | Show logs for service           |
| `journalctl -fu myapp.service`         | Follow live logs                |
| `systemctl list-units --type=service`  | List active services            |

# ⚙️ 2️⃣ Basic .service template
```ini
# /etc/systemd/system/serverhub.service
[Unit]
Description=ServerHub Flask App
After=network.target

[Service]
User=serverhub
WorkingDirectory=/home/serverhub/app
ExecStart=/home/serverhub/venv/bin/python app.py
Environment="FLASK_ENV=production"
Environment="FLASK_SECRET_KEY=super_secret_key"
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Then:
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now serverhub.service
```

# 📦 3️⃣ Using a .env file for secrets

Instead of putting secrets directly in the service file:

```ini
EnvironmentFile=/home/serverhub/.env
```

Then in .env:
```ini
FLASK_SECRET_KEY=your_super_secret_key
DATABASE_URL=postgresql://serverhub:pass@localhost/db
```

✅ Safe, portable, and cleaner.

# 🧩 4️⃣ Auto-restart & crash recovery
| Setting              | Description                      |
| -------------------- | -------------------------------- |
| `Restart=always`     | Restart even if exited normally  |
| `Restart=on-failure` | Only restart on error exit       |
| `RestartSec=5`       | Wait 5 seconds before restarting |

Systemd will automatically restart your Flask app if it crashes 💪

# 🪵 5️⃣ Logging

View logs:
```bash
journalctl -u serverhub.service
```

Follow live logs:

```bash
journalctl -fu serverhub.service
```

Clear logs (if testing):
```bash
sudo journalctl --vacuum-time=1d
```

# 🧠 6️⃣ Common [Unit] dependencies
| Directive                     | Meaning                       |
| ----------------------------- | ----------------------------- |
| `After=network.target`        | Wait until network is up      |
| `Requires=postgresql.service` | Start DB before app           |
| `PartOf=multi-user.target`    | Runs under normal system boot |

# 🔐 7️⃣ Security Hardening (Optional but pro)

You can sandbox your app a bit:
```ini
ProtectSystem=full
ProtectHome=true
NoNewPrivileges=true
PrivateTmp=true
```

👉 This limits what your Flask app can mess with outside its directory.

# 🧰 8️⃣ Troubleshooting Tips
| Symptom                      | Try this                              |
| ---------------------------- | ------------------------------------- |
| Service won’t start          | `sudo systemctl status myapp.service` |
| Logs missing                 | `journalctl -u myapp.service`         |
| Changed .service not applied | `sudo systemctl daemon-reload`        |
| App exits immediately        | Add `Restart=always`                  |
| Env vars not loading         | Check `EnvironmentFile` path          |

# ⚡ 9️⃣ Pro Tips

- Test manually before systemd:
```bash
sudo -u serverhub /home/serverhub/venv/bin/python /home/serverhub/app/app.py
```

- Keep .service files clean and readable.
- Always daemon-reload after edits.
- Use sudo systemctl cat yourapp.service to confirm what’s loaded.

# 🧙‍♂️ 10️⃣ Bonus: Quick aliasing

You can make short aliases in your shell:

```bash
alias logs='journalctl -fu serverhub.service'
alias restart='sudo systemctl restart serverhub.service'
```

Now you can debug like:

```bash
logs
 ```

and boom, live logs.

# ⚡ TL;DR Summary
| Goal           | Command                         |
| -------------- | ------------------------------- |
| Start/stop     | `sudo systemctl start/stop app` |
| Enable on boot | `sudo systemctl enable app`     |
| See logs       | `journalctl -u app -f`          |
| Reload configs | `sudo systemctl daemon-reload`  |
| Add secrets    | `EnvironmentFile=/path/to/.env` |
