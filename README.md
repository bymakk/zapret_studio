<div align="center">

# <img src="https://cdn-icons-png.flaticon.com/128/5968/5968756.png" height=28 /> <a href="https://github.com/bymakk/">bymakk</a><a href="https://github.com/bymakk/zapret-cb-sc">/zapret-cb-sc</a> <img src="https://cdn-icons-png.flaticon.com/128/1384/1384060.png" height=28 />

Форк <a href="https://github.com/Flowseal/zapret-discord-youtube">Flowseal/zapret-discord-youtube</a> с расширенными списками под **Chaturbate**, **Stripchat**, **MyWebcamRoom**, **Camsoda** и связанные CDN/API.

**Апстрим:** <a href="https://github.com/Flowseal/zapret-discord-youtube">zapret-discord-youtube</a> · **Альтернатива:** <a href="https://github.com/bol-van/zapret-win-bundle">bol-van/zapret-win-bundle</a>  
**Ускорение Telegram Desktop (у Flowseal):** <a href="https://github.com/Flowseal/tg-ws-proxy">tg-ws-proxy</a>  
Поддержка оригинального разработчика zapret: <a href="https://github.com/bol-van/zapret?tab=readme-ov-file#%D0%BF%D0%BE%D0%B4%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D1%82%D1%8C-%D1%80%D0%B0%D0%B7%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D1%87%D0%B8%D0%BA%D0%B0">bol-van/zapret</a>

</div>

> [!CAUTION]
>
> ### Источник сборки
> Считайте доверенной только копию из этого репозитория: **[bymakk/zapret-cb-sc](https://github.com/bymakk/zapret-cb-sc)**.  
> Сторонние репаки и «зеркала» могут содержать подменённые бинарники — проверяйте, что запускаете.

> [!WARNING]
>
> ### АНТИВИРУСЫ
> WinDivert может вызвать реакцию антивируса.  
> WinDivert — инструмент перехвата и фильтрации трафика, необходимый для работы zapret (аналог iptables/NFQUEUE под Windows).  
> Он может использоваться и легитимными, и вредоносными программами; **сам по себе не является вирусом**.  
> Драйвер `WinDivert64.sys` подписан для загрузки в 64-битное ядро Windows.
>
> **Выдержка из [`readme.md`](https://github.com/bol-van/zapret-win-bundle/blob/master/readme.md#%D0%B0%D0%BD%D1%82%D0%B8%D0%B2%D0%B8%D1%80%D1%83%D1%81%D1%8B) репозитория [bol-van/zapret-win-bundle](https://github.com/bol-van/zapret-win-bundle)**
>
> Некоторые антивирусы относят WinDivert к повышенному риску. Детект часто содержит `WinDivert` или `Not-a-virus:RiskTool.Multi.WinDivert`.  
> При проблемах добавьте папку с zapret в исключения или отключите детектирование PUA, если понимаете риски.

> [!IMPORTANT]
>
> Бинарные файлы в папке [`bin`](./bin) взяты из [zapret-win-bundle/zapret-winws](https://github.com/bol-van/zapret-win-bundle/tree/master/zapret-winws). Сверяйте хэши при сомнениях.

## ⚙️ Использование

1. **Включите Secure DNS** (часто критично для YouTube и части сервисов)
   - Chrome: «Использовать безопасный DNS», выбрать поставщика (не «по умолчанию от провайдера»).
   - Firefox: DNS через HTTPS, например `https://dns.google/dns-query` (Cloudflare у части провайдеров может быть недоступен).
   - Windows 11: см. [How to enable DNS over HTTPS on Windows 11](https://www.howtogeek.com/765940/how-to-enable-dns-over-https-on-windows-11/).

2. **Скачайте архив**
   - **Code → Download ZIP** на странице репозитория, или  
   - прямая ссылка на архив ветки `main`: [main.zip](https://github.com/bymakk/zapret-cb-sc/archive/refs/heads/main.zip)

3. У скачанного архива в **Свойствах** при необходимости включите **«Разблокировать»** (для встроенного в Windows архиватора).

4. Распакуйте в путь **без кириллицы и спецсимволов**, желательно корень диска, например `C:\zapret-cb-sc` (пробелы в пути иногда ломают сервис/батники).

5. Запустите нужный **`general*.bat` от имени администратора** или настройте автозапуск через **`service.bat`**.

## ✨ Отличия этого форка от апстрима

- Расширен **`lists/list-general-user.txt`**: домены для кам-сайтов и CDN (в т.ч. по данным [urlscan.io — Quickstart](https://docs.urlscan.io/guides/quickstart)).
- Добавлен **`lists/list-ipset-host-skip.txt`**: снижает риск **`ERR_CONNECTION_RESET`** на Camsoda из-за двойного срабатывания hostlist + ipset.
- Перед стартом **`general*.bat`** вызывается **`service.bat prepare_winws`**: останавливается предыдущий `winws.exe`, сервис `zapret` (если был), чистится WinDivert — удобно переключать пресеты.
- После запуска — **`verify_winws`**: проверка, что `winws.exe` поднялся, и контроль TCP timestamps.
- Пункты **`Update IPSet List`**, **`Update Hosts File`**, **`Check for Updates`** в `service.bat` берут данные из **этого** репозитория (ветка `main`, папка [`.service`](./.service)).

## ℹ️ Краткие описания файлов

- **[`general.bat`](./general.bat) … [`general (ALT).bat`](./general%20(ALT).bat) и др.** — ручной запуск стратегии.  
  **Пробуйте разные `ALT` / `FAKE` / `SIMPLE`**, пока не найдёте рабочую комбинацию на своём провайдере.

- **[`service.bat`](./service.bat)** — установка в автозапуск и сервисные функции:
  - **`Install Service`** — установка выбранной стратегии как службы Windows
  - **`Remove Services`** — удаление службы `zapret`, остановка `winws`, очистка WinDivert
  - **`Check Status`** — статус служб и процесса обхода
  - **`Game Filter`** — расширенный фильтр портов для игр/UDP-TCP; **после смены — перезапуск стратегии**
  - **`IPSet Filter`** — режимы работы с `ipset-all.txt` (`none` / `loaded` / `any`)
  - **`Auto-Update Check`** — вкл/выкл фоновой проверки обновлений
  - **`Update IPSet List`** — подтягивание актуального списка из [`.service/ipset-service.txt`](https://raw.githubusercontent.com/bymakk/zapret-cb-sc/main/.service/ipset-service.txt)
  - **`Update Hosts File`** — проверка/подсказка по `hosts` из [`.service/hosts`](https://raw.githubusercontent.com/bymakk/zapret-cb-sc/main/.service/hosts) (веб Telegram / голос Discord у апстрима)
  - **`Check for Updates`** — сравнение с [`.service/version.txt`](https://raw.githubusercontent.com/bymakk/zapret-cb-sc/main/.service/version.txt), открытие [архива main.zip](https://github.com/bymakk/zapret-cb-sc/archive/refs/heads/main.zip)
  - **`Run Diagnostics`** — типовые причины сбоев
  - **`Run Tests`** — [`utils/test zapret.ps1`](./utils/test%20zapret.ps1)

## ☑️ Распространённые вопросы

### После запуска `general*` «ничего не происходит»

Должен появиться процесс **`winws.exe`** (часто свёрнут в трей). Если нет — см. обсуждения апстрима, например [#522](https://github.com/Flowseal/zapret-discord-youtube/issues/522).

### Не работает веб Telegram / голос Discord

Запустите **`service.bat` → Update Hosts File** и следуйте подсказкам (копирование блока в системный `hosts` от администратора).

### Обход не работает / перестал работать

> [!IMPORTANT]
> Стратегии со временем «протухают» из-за DPI/инфраструктуры. Пробуйте другие `general*`, меняйте параметры по документации [bol-van/zapret — nfqws](https://github.com/bol-van/zapret/blob/master/docs/readme.md#nfqws).

- **`Run Diagnostics`**
- Убедитесь, что домен/IP есть в **`list-general-user.txt`** / **`ipset-all.txt`**
- Для Camsoda при сбросе соединения проверьте наличие **`list-ipset-host-skip.txt`** и актуальность списков

### Как полностью переустановить

1. Сохраните свои правки в `*-user.txt` и кастомные списки.  
2. Перезагрузка ПК (по желанию).  
3. **service.bat** → **Remove Services**.  
4. **`Run Diagnostics`** — устраните замечания.  
5. Удалите папку с проектом.  
6. Скачайте свежий ZIP с **[GitHub](https://github.com/bymakk/zapret-cb-sc)** и распакуйте в новую папку без проблемного пути.  
7. Снова подберите рабочий `general*`, при необходимости **`Install Service`**.

### Не работает игра с включённым запретом

В **`service.bat`**: **`Game Filter` = disabled**, **`IPSet Filter` = none** для диагностики.

### Античит ругается на WinDivert

См. [windivert-hide](https://github.com/bol-van/zapret-win-bundle/tree/master/windivert-hide).

### Не работает <img src="https://cdn-icons-png.flaticon.com/128/1384/1384060.png" height=18 /> YouTube

Secure DNS, отключение агрессивных блокировщиков, перебор стратегий. См. [#251](https://github.com/Flowseal/zapret-discord-youtube/discussions/251).

### Не работает <img src="https://cdn-icons-png.flaticon.com/128/5968/5968756.png" height=18 /> Discord

Secure DNS, перебор стратегий, браузерный https://discord.com/app. См. [#252](https://github.com/Flowseal/zapret-discord-youtube/discussions/252).

### Не нашли ответа

Создайте issue в **[bymakk/zapret-cb-sc](https://github.com/bymakk/zapret-cb-sc/issues)**. Вопросы по «чистому» апстриму — в [Flowseal/zapret-discord-youtube](https://github.com/Flowseal/zapret-discord-youtube/issues).

## 🗒️ Добавление адресов

- **`lists/list-general-user.txt`** — домены для hostlist (поддомены обычно покрываются шаблоном списка)
- **`lists/list-exclude-user.txt`** — исключения доменов из hostlist-правил
- **`lists/list-ipset-host-skip.txt`** — хосты, для которых **не** применяется второй слой правил по **ipset** (важно для части сценариев Camsoda)
- **`lists/ipset-all.txt`** — IP и подсети для ipset-правил
- **`lists/ipset-exclude-user.txt`** — исключения по IP/подсетям  
Файлы **`*-user.txt`** и **`list-ipset-host-skip.txt`** при отсутствии создаются/дополняются при первом запуске через логику `service.bat`.

## 🛑 Остановка обхода (вручную)

```bat
taskkill /IM winws.exe /F
```

Или полностью, как в меню **`Remove Services`**:

```bat
net stop zapret
sc delete zapret
taskkill /IM winws.exe /F
net stop WinDivert
sc delete WinDivert
```

## ⭐ Поддержка

Поставьте :star: репозиторию **[bymakk/zapret-cb-sc](https://github.com/bymakk/zapret-cb-sc)**.  
Оригинальный zapret: [поддержать bol-van](https://github.com/bol-van/zapret?tab=readme-ov-file#%D0%BF%D0%BE%D0%B4%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D1%82%D1%8C-%D1%80%D0%B0%D0%B7%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D1%87%D0%B8%D0%BA%D0%B0).

<a href="https://star-history.com/#bymakk/zapret-cb-sc&Date">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=bymakk/zapret-cb-sc&type=Date&theme=dark" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=bymakk/zapret-cb-sc&type=Date" />
   <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=bymakk/zapret-cb-sc&type=Date" />
 </picture>
</a>

## ⚖️ Лицензирование

В корне этого репозитория отдельный `LICENSE` может отсутствовать; базовая сборка наследуется от **[Flowseal/zapret-discord-youtube](https://github.com/Flowseal/zapret-discord-youtube)** (там указана [MIT](https://github.com/Flowseal/zapret-discord-youtube/blob/main/LICENSE.txt)). Уточняйте условия при распространении своих изменений.

## 🩷 Благодарности

[![Contributors](https://contrib.rocks/image?repo=bymakk/zapret-cb-sc)](https://github.com/bymakk/zapret-cb-sc/graphs/contributors)

Отдельная благодарность **[Flowseal](https://github.com/Flowseal)** за [zapret-discord-youtube](https://github.com/Flowseal/zapret-discord-youtube) и **[bol-van](https://github.com/bol-van)** за [zapret](https://github.com/bol-van/zapret) и [zapret-win-bundle](https://github.com/bol-van/zapret-win-bundle).
