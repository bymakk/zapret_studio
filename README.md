<div align="center">

# <img src="https://cdn-icons-png.flaticon.com/128/5968/5968756.png" height=28 /> <a href="https://github.com/bymakk/">bymakk</a><a href="https://github.com/bymakk/zapret_studio">/zapret_studio</a> <img src="https://cdn-icons-png.flaticon.com/128/1384/1384060.png" height=28 />

Форк <a href="https://github.com/Flowseal/zapret-discord-youtube">Flowseal/zapret-discord-youtube</a> с расширенными списками под **Chaturbate**, **Stripchat**, **MyWebcamRoom**, **Camsoda** и связанные CDN/API.

**Апстрим:** <a href="https://github.com/Flowseal/zapret-discord-youtube">zapret-discord-youtube</a> · **Альтернатива:** <a href="https://github.com/bol-van/zapret-win-bundle">bol-van/zapret-win-bundle</a>  
**Ускорение Telegram Desktop (у Flowseal):** <a href="https://github.com/Flowseal/tg-ws-proxy">tg-ws-proxy</a>  
Поддержка оригинального разработчика zapret: <a href="https://github.com/bol-van/zapret?tab=readme-ov-file#%D0%BF%D0%BE%D0%B4%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D1%82%D1%8C-%D1%80%D0%B0%D0%B7%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D1%87%D0%B8%D0%BA%D0%B0">bol-van/zapret</a>

</div>

> [!CAUTION]
>
> ### Источник сборки
> Считайте доверенной только копию из этого репозитория: **[bymakk/zapret_studio](https://github.com/bymakk/zapret_studio)**.  
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
   - прямая ссылка на архив ветки `main`: [main.zip](https://github.com/bymakk/zapret_studio/archive/refs/heads/main.zip)

3. У скачанного архива в **Свойствах** при необходимости включите **«Разблокировать»** (для встроенного в Windows архиватора).

4. Распакуйте в путь **без кириллицы и спецсимволов**, желательно корень диска, например `C:\zapret_studio` (пробелы в пути иногда ломают сервис/батники).

5. Запустите нужный **`general*.bat` от имени администратора** или настройте автозапуск через **`service.bat`**.

## ✨ Отличия этого форка от апстрима

- Расширен **`lists/list-general-user.txt`**: домены для кам-сайтов и CDN (в т.ч. по данным [urlscan.io — Quickstart](https://docs.urlscan.io/guides/quickstart)).
- Добавлен **`lists/list-ipset-host-skip.txt`**: снижает риск **`ERR_CONNECTION_RESET`** на Camsoda из-за двойного срабатывания hostlist + ipset.
- Перед стартом **`general*.bat`** вызывается **`service.bat prepare_winws`**: останавливается предыдущий `winws.exe`, сервис `zapret` (если был), чистится WinDivert — удобно переключать пресеты.
- После запуска — **`verify_winws`**: проверка, что `winws.exe` поднялся, и контроль TCP timestamps.
- Пункты **`Update IPSet List`**, **`Update Hosts File`**, **`Check for Updates`** в `service.bat` берут данные из **этого** репозитория (ветка `main`, папка [`.service`](./.service)).
- **Автообновление списков при запуске** — при старте любого `general*.bat` вызывается `service.bat self_update`: базовые списки и `ipset-all` (база Flowseal ∪ ваши IP) подтягиваются из форка. Подробности — в разделе [Автообновление списков](#auto-update).

## ℹ️ Краткие описания файлов

- **[`general.bat`](./general.bat) … [`general (ALT).bat`](./general%20(ALT).bat) и др.** — ручной запуск стратегии.  
  **Пробуйте разные `ALT` / `FAKE` / `SIMPLE`**, пока не найдёте рабочую комбинацию на своём провайдере.

- **[`service.bat`](./service.bat)** — установка в автозапуск и сервисные функции:
  - **`Install Service`** — установка выбранной стратегии как службы Windows
  - **`Remove Services`** — удаление службы `zapret`, остановка `winws`, очистка WinDivert
  - **`Check Status`** — статус служб и процесса обхода
  - **`Game Filter`** — расширенный фильтр портов для игр/UDP-TCP; **после смены — перезапуск стратегии**
  - **`IPSet Filter`** — режимы работы с `ipset-all.txt` (`none` / `loaded` / `any`)
  - **`Auto-Update Check`** — вкл/выкл фоновой проверки версии
  - **`Auto-Update Lists`** — вкл/выкл автообновления списков при каждом запуске стратегии (см. раздел ниже)
  - **`Update Lists Now`** — принудительно обновить базовые списки и пересобрать `ipset-all` прямо сейчас
  - **`Update IPSet List`** — пересобрать `ipset-all.txt` = база Flowseal (`.service/ipset-base.txt`) ∪ ваши IP (`lists/ipset-user.txt`)
  - **`Update Hosts File`** — проверка/подсказка по `hosts` из [`.service/hosts`](https://raw.githubusercontent.com/bymakk/zapret_studio/main/.service/hosts) (веб Telegram / голос Discord у апстрима)
  - **`Check for Updates`** — сравнение с [`.service/version.txt`](https://raw.githubusercontent.com/bymakk/zapret_studio/main/.service/version.txt), открытие [архива main.zip](https://github.com/bymakk/zapret_studio/archive/refs/heads/main.zip)
  - **`Run Diagnostics`** — типовые причины сбоев

<a id="hosts-telegram-discord"></a>

## 💬 Веб-Telegram и голос Discord: файл `hosts`

Иногда **веб-версия Telegram** не открывается или **зависает на «Подключение…»**, а в **Discord** бесконечно крутится подключение к **голосовому чату**, хотя сам клиент или сайт в целом живы. Часть таких случаев лечится **записями в системном файле `hosts`**: скрипт сравнивает ваш `hosts` с актуальным шаблоном из репозитория ([`.service/hosts`](https://raw.githubusercontent.com/bymakk/zapret_studio/main/.service/hosts)) и подсказывает, что добавить.

**Что сделать**

1. Запустите **[`service.bat`](./service.bat)** от имени администратора.
2. Выберите пункт **`Update Hosts File`** в меню.
3. Дождитесь проверки. Если скрипт сообщит, что `hosts` **не совпадает** с рекомендуемым вариантом, откроется **Блокнот** с **готовым блоком строк** — это не весь файл `hosts`, а только то, что нужно **добавить или заменить** у вас.
4. **Скопируйте весь текст** из этого окна Блокнота (**Ctrl+A**, затем **Ctrl+C**).
5. Откройте **настоящий** системный `hosts`:
   - Путь по умолчанию: `C:\Windows\System32\drivers\etc\hosts`  
   - Удобно: в меню скрипта после проверки часто открывается **папка** с этим файлом — откройте **`hosts`** редактором **от имени администратора** (ПКМ по «Блокнот» → «Запуск от имени администратора» → Файл → Открыть → укажите `hosts`).
6. **В конец файла** вставьте скопированный блок (**Ctrl+V**).  
   Если вы **уже добавляли** похожие строки для Telegram/Discord раньше — лучше **удалите старый блок** и вставьте новый целиком, чтобы не было дублей и конфликтов.
7. Сохраните файл (**Ctrl+S**). Если сохранение не удаётся — вы открыли `hosts` **без прав администратора**; закройте редактор и повторите шаг 5.
8. Перезапустите браузер / Discord (при необходимости — ПК) и проверьте снова. Убедитесь, что файл на диске **действительно обновился** (дата изменения, содержимое).

Если после этого проблема остаётся, дополнительно проверьте **Secure DNS**, рабочую стратегию **`general*.bat`** и пункт **`Run Diagnostics`** в `service.bat`.

<a id="auto-update"></a>

## 🔄 Автообновление списков при запуске (Этап A)

При старте любого **`general*.bat`** после `prepare_winws`/`load_user_lists` вызывается **`service.bat self_update`**, который обновляет списки из форка **до** запуска `winws.exe`.

**Что обновляется**
- **Базовые списки** из манифеста [`.service/manifest.txt`](./.service/manifest.txt): `list-general.txt`, `list-google.txt`, `list-exclude.txt`, `ipset-exclude.txt` — тянутся из форка (`raw.githubusercontent` с фолбэком на `cdn.jsdelivr.net`).
- **`lists/ipset-all.txt`** пересобирается как **база Flowseal ∪ ваши IP** (только в режиме `IPSet Filter = loaded`):
  - **база** — `.service/ipset-base.txt` форка (наполняется GitHub Action [`sync-upstream`](./.github/workflows/sync-upstream.yml) из Flowseal `.service/ipset-service.txt`); если её ещё нет, клиент берёт базу напрямую из Flowseal;
  - **ваши IP** — [`lists/ipset-user.txt`](./lists/ipset-user.txt) (обход Chaturbate / Stripchat / Lovense и т. д. — правьте этот файл).

**Что НЕ трогается** (остаётся локальным): `list-general-user.txt`, `list-exclude-user.txt`, `ipset-exclude-user.txt`, `list-ipset-host-skip.txt`, `ipset-user.txt`.

**Где сводятся апстрим и ваши добавки** — в **форке** (`bymakk/zapret_studio`): база синхронизируется кнопкой **Sync fork**/Action, ваши коммиты ложатся сверху. Клиент только скачивает готовое — git у пользователя не нужен.

**Надёжность (fail-open)**
- Перед обновлением — быстрый пробник доступности репозитория; если GitHub недоступен (типично за DPI) — **берутся локальные списки, `winws` стартует без задержки**.
- Любая ошибка скачивания/битый файл (HTML/пусто) → файл **не** заменяется, используется текущая копия.
- Тротлинг: не чаще раза в **6 часов** (метка `utils/last_update.txt`); кнопка **`Update Lists Now`** игнорирует тротлинг.

**Управление**
- Меню `service.bat` → **`Auto-Update Lists`** (вкл/выкл, по умолчанию **вкл** — файл-флаг [`utils/auto_update.enabled`](./utils)).
- Полностью отключить сетевые проверки — переменная окружения `NO_UPDATE_CHECK`.

> [!NOTE]
> С базой Flowseal `ipset-all.txt` становится большим (десятки тысяч IPv4/IPv6). Это и есть «базовый список айпи Flowseal + ваши». Если нужен только кам-фокус — держите `IPSet Filter` вне режима `loaded`, либо переключите источник базы в [`service.bat`](./service.bat) (`:rebuild_ipset`).

## ☑️ Распространённые вопросы

### После запуска `general*` «ничего не происходит»

Должен появиться процесс **`winws.exe`** (часто свёрнут в трей). Если нет — см. обсуждения апстрима, например [#522](https://github.com/Flowseal/zapret-discord-youtube/issues/522).

### Не работает веб Telegram / голос Discord

Пошаговая инструкция — в разделе **[Веб-Telegram и голос Discord: файл `hosts`](#hosts-telegram-discord)** выше.

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
6. Скачайте свежий ZIP с **[GitHub](https://github.com/bymakk/zapret_studio)** и распакуйте в новую папку без проблемного пути.  
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

Создайте issue в **[bymakk/zapret_studio](https://github.com/bymakk/zapret_studio/issues)**. Вопросы по «чистому» апстриму — в [Flowseal/zapret-discord-youtube](https://github.com/Flowseal/zapret-discord-youtube/issues).

## 🗒️ Добавление адресов

- **`lists/list-general-user.txt`** — домены для hostlist; в winws/zapret строка **без** префикса `^` матчит и сам хост, и **все поддомены**. Во всех `general*.bat` задано `--hostlist-domains=…` для зон **chaturbate.com**, **stripchat.com**, **livejasmin.com**, **jasmin.com**, **dditsadn.com**, **dditscdn.com**, **dcbosf.com**, **hotjar.com**, **hotjar.io**, **hcaptcha.com**, **scarabresearch.com** (часть зон подтверждена сканами [urlscan.io](https://docs.urlscan.io/pages/api-intro); **emarsys** — только явный хост в `list-general-user.txt`). Перенос из `zapret-strategy-nfqws` без гигантских Instagram/Facebook/Telegram CIDR. Полный перечень поддоменов заранее не исчерпать — при новом хосте дописывайте строку в этот файл.
- **`lists/list-exclude-user.txt`** — исключения доменов из hostlist-правил
- **`lists/list-ipset-host-skip.txt`** — хосты, для которых **не** применяется второй слой правил по **ipset** (важно для части сценариев Camsoda)
- **`lists/ipset-user.txt`** — **ваши** IP/подсети (Chaturbate, Stripchat, Lovense и т. д.); попадают в `ipset-all` при пересборке
- **`lists/ipset-all.txt`** — активный ipset; при включённом автообновлении **генерируется** = база Flowseal ∪ `ipset-user.txt`. Правьте `ipset-user.txt`, а не этот файл
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

Поставьте :star: репозиторию **[bymakk/zapret_studio](https://github.com/bymakk/zapret_studio)**.  
Оригинальный zapret: [поддержать bol-van](https://github.com/bol-van/zapret?tab=readme-ov-file#%D0%BF%D0%BE%D0%B4%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D1%82%D1%8C-%D1%80%D0%B0%D0%B7%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D1%87%D0%B8%D0%BA%D0%B0).

<a href="https://star-history.com/#bymakk/zapret_studio&Date">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=bymakk/zapret_studio&type=Date&theme=dark" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=bymakk/zapret_studio&type=Date" />
   <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=bymakk/zapret_studio&type=Date" />
 </picture>
</a>

## ⚖️ Лицензирование

В корне этого репозитория отдельный `LICENSE` может отсутствовать; базовая сборка наследуется от **[Flowseal/zapret-discord-youtube](https://github.com/Flowseal/zapret-discord-youtube)** (там указана [MIT](https://github.com/Flowseal/zapret-discord-youtube/blob/main/LICENSE.txt)). Уточняйте условия при распространении своих изменений.

## 🩷 Благодарности

[![Contributors](https://contrib.rocks/image?repo=bymakk/zapret_studio)](https://github.com/bymakk/zapret_studio/graphs/contributors)

Отдельная благодарность **[Flowseal](https://github.com/Flowseal)** за [zapret-discord-youtube](https://github.com/Flowseal/zapret-discord-youtube) и **[bol-van](https://github.com/bol-van)** за [zapret](https://github.com/bol-van/zapret) и [zapret-win-bundle](https://github.com/bol-van/zapret-win-bundle).
