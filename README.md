# primevpn-config (GitHub Pages repo)

Репозиторий, который раздаёт зашифрованный список серверов для PrimeVPN-клиента.

---

## Структура

```
/
└── servers.json     ← единственный нужный файл
```

**GitHub Pages** раздаёт `servers.json` по адресу:

```
https://YOUR_USERNAME.github.io/primevpn-config/servers.json
```

---

## Как обновить IP-адреса серверов

### Шаг 1. Отредактируйте `tools/update_servers.py` (или `.ps1`) в проекте клиента

```python
AES_KEY_HEX = "ваш-64-символьный-hex-ключ"

SERVERS = {
    "moscow":     "новый.ip.адрес",
    "khabarovsk": "новый.ip.адрес",
    "tokyo":      "новый.ip.адрес",
}
```

### Шаг 2. Запустите скрипт

```bash
# Python
python tools/update_servers.py

# или PowerShell (без pip)
.\tools\update_servers.ps1
```

Скрипт сгенерирует `github-pages/servers.json`.

### Шаг 3. Скопируйте файл в этот репозиторий и запушьте

```bash
cp ../primevpn-client/github-pages/servers.json ./servers.json
git add servers.json
git commit -m "Update servers $(date +%Y-%m-%d)"
git push
```

Через 30–60 секунд GitHub Pages раздаст новый файл.

---

## Безопасность

| Что                | Защита                                                 |
| ------------------ | ------------------------------------------------------ |
| URL публичный      | Не страшно — содержимое зашифровано AES-256            |
| Ключ AES           | Хранится только в бинарнике клиента (XOR + ConfuserEx) |
| IP-адреса серверов | Видны только после расшифровки с правильным ключом     |
| Перехват трафика   | Только шифртекст — без ключа бесполезен                |

---

## Настройка GitHub Pages

1. Создайте новый **публичный** репозиторий, например `primevpn-config`
2. Перейдите: Settings → Pages → Source → `main` branch → `/ (root)`
3. Сохраните, подождите ~1 минуту
4. Проверьте: `curl https://kulikov-dev.github.io/prm-config/servers.json`
