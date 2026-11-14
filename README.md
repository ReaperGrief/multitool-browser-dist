# MultiToolBrowser — канал обновлений

Здесь лежат только собранные APK и манифест обновлений. Исходников в этом репозитории нет.

## Как работает автообновление

Приложение запрашивает:

```
https://github.com/ReaperGrief/multitool-browser-dist/releases/latest/download/version.json
```

Поля манифеста:

| Поле | Значение |
| --- | --- |
| `versionCode` | целое, сравнивается с установленным |
| `versionName` | строка для показа пользователю |
| `apk` | имя файла ассета в этом же релизе |
| `sha256` | контрольная сумма APK |
| `size` | размер в байтах |
| `notes` | что нового, одна строка |
| `date` | дата сборки, `YYYY-MM-DD` |

Если `versionCode` больше установленного, приложение скачивает

```
https://github.com/ReaperGrief/multitool-browser-dist/releases/latest/download/<apk>
```

проверяет SHA-256 и передаёт файл системному установщику Android. Подпись у всех сборок одна, поэтому обновление встаёт поверх без потери данных.

## Как выпустить новую версию

```powershell
cd D:\multitool-browser
powershell -ExecutionPolicy Bypass -File .\build.ps1
```

Затем обновить `version.json` (versionCode, versionName, apk, sha256, size, notes, date) и:

```powershell
gh release create v<версия> "out\MultiToolBrowser-<версия>.apk" "dist\version.json" `
  --repo ReaperGrief/multitool-browser-dist `
  --title "MultiToolBrowser <версия>" --notes "<что нового>"
```

Ссылки `releases/latest/download/...` всегда указывают на последний опубликованный релиз, поэтому в приложении ничего менять не нужно.
