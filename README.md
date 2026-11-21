# Maža-Kaina OSM export (Литва)

Ежедневная выгрузка из OpenStreetMap для карты магазинов maza-kaina.lt (`/parduotuves/`): продуктовые магазины
и торговые центры, аптеки и промтовары, ориентиры (станции, автовокзалы, рынки, самоуправления, больницы, почта,
церкви, музеи) и автоматы приёма тары — только Литва, обрезка по официальной границе.

Копия [aeronavtik/mazacena-osm](https://github.com/aeronavtik/mazacena-osm) (выгрузка mazacena.lv) с литовскими
границей (`ISO3166-1=LT`), тегом `name:lt` и сетями (одежда NEW YORKER, книжные Pegasas и Humanitas, электроника,
зоотовары, агромагазины).

Зачем: без выгрузки сайт берёт данные OSM из общих серверов Overpass, а они часто перегружены (21.11.2026 все
четыре публичных сервера отвечали 502/504 даже на маленькие запросы). Тогда в карту не попадают сети без своего
источника (Maxima, Iki, Rimi, Norfa, аптеки, Senukai и др.), торговые центры, тароматы и ориентиры.

Каждую ночь (00:45 UTC) GitHub Actions:

1. скачивает свежую выгрузку Литвы с [Geofabrik](https://download.geofabrik.de/europe/lithuania.html) (~220 МБ);
2. вырезает Литву по границе и оставляет нужные объекты утилитой [osmium](https://osmcode.org/osmium-tool/);
3. `build.php` пишет файлы в формате ответа Overpass API (`food.json`, `other.json`, `places.json`, `meta.json`);
4. публикует их на GitHub Pages.

Если объектов подозрительно мало, выгрузка не публикуется, и на Pages остаются вчерашние файлы.

## Настройка (один раз)

Репозиторий: [aeronavtik-rgb/maza-kaina-osm](https://github.com/aeronavtik-rgb/maza-kaina-osm) (создан 21.11.2026).

1. Создайте на GitHub **публичный** репозиторий `maza-kaina-osm` и загрузите в него содержимое этой папки, включая `.github/`.
2. **Settings → Pages → Build and deployment → Source: GitHub Actions.**
3. **Actions → OSM export → Run workflow** — первый запуск вручную (около 3 минут).
4. Проверьте, что открывается `https://aeronavtik-rgb.github.io/maza-kaina-osm/meta.json`.

Сайт ищет выгрузку по этому адресу сам (`OsmData::exportUrl`, настройка `stores.osm_export` его меняет). Пока
адрес не отвечает, импорт идёт через Overpass раз в неделю; когда выгрузка появится — OSM обновляется каждую ночь.
