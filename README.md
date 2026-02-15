# Architecture of the Top 1000 GitHub Repositories Parser

This parser is designed to collect information about the 1000 most-starred GitHub repositories and analyze the presence of key files: CODE_OF_CONDUCT, CONTRIBUTING, README, and the .github/workflows folder. The architecture is built around three sequential stages, taking into account GitHub API rate limits.

## Overall Structure
The parser consists of the following logical components:

1. Request Manager – forms HTTP requests to the GitHub API, manages headers (authorization token), and processes responses.

2. Metadata Collector – executes search queries to obtain the list of top 1000 repositories (uses the Search API).

3. Content Analyzer – for each repository, requests the list of files in the root directory (/contents) and checks for the target files.

4. Saving Module – aggregates the results into a pandas DataFrame and saves them to a CSV file.

All components work sequentially in a single thread; delays (time.sleep) are inserted between requests to comply with API limits (5000 requests per hour for authenticated users).

## Step‑by‑Step Process Description

1. Retrieving the Repository List

- A loop iterates over 10 pages (100 records per page).

- The Search API is used with the query q=stars:>10000&sort=stars&order=desc.

- After each page, a 5‑second pause is applied.

- The result is stored in the all_repos list.

2. Detailed Analysis of Each Repository

- For every item in the list, a request is sent to the Contents API (/repos/{full_name}/contents/).

- File presence is checked by name (case‑insensitive):

has_coc: any name containing CODE_OF_CONDUCT

has_contributing: contains CONTRIBUTING

has_readme: contains README

has_workflows: presence of the .github folder (workflows are usually inside it)

- From the repository metadata, the following are extracted: name, organization, language, stars, forks, open issues, age in days, description.

- A 1.5‑second pause is applied between requests to avoid exceeding the rate limit.

3. Saving the Results

- All collected records are accumulated in a list of dictionaries.

- After the loop finishes, a pandas DataFrame is created and saved as top_1000_os_rules.csv (UTF‑8 encoding).

## Sequence Diagram (PlantUML)

Below is a diagram illustrating the interaction of the parser components with the GitHub API and among themselves.

<img width="599" height="598" alt="image" src="https://github.com/user-attachments/assets/c8e29015-a496-43cb-9cfb-f9a67dbf5227" />

## Conclusion

The proposed architecture ensures reliable data collection about popular repositories while respecting GitHub API limits. Built‑in delays and error handling help avoid being blocked due to rate limits. The results are saved in a structured format ready for further analysis (for example, statistics on the prevalence of CODE_OF_CONDUCT and CONTRIBUTING files among top projects).

# Архитектура парсера топ-1000 репозиториев GitHub

Данный парсер предназначен для сбора информации о 1000 самых звёздных репозиториях GitHub и анализа наличия в них ключевых файлов: CODE_OF_CONDUCT, CONTRIBUTING, README и папки .github/workflows. Архитектура построена на последовательном выполнении трёх основных этапов с учётом ограничений GitHub API.

## Общая структура
Парсер состоит из следующих логических компонентов:

- Менеджер запросов – формирует HTTP-запросы к GitHub API, управляет заголовками (токен авторизации) и обрабатывает ответы.

- Сборщик метаданных – выполняет поисковые запросы для получения списка топ-1000 репозиториев (использует Search API).

- Анализатор содержимого – для каждого репозитория запрашивает список файлов в корневой директории (/contents) и проверяет наличие целевых файлов.

- Модуль сохранения – агрегирует результаты в pandas DataFrame и сохраняет в CSV-файл.

Все компоненты работают последовательно в одном потоке, между запросами вставляются задержки (time.sleep) для соблюдения лимитов API (5000 запросов в час для аутентифицированных пользователей).

## Пошаговое описание процесса
Получение списка репозиториев

Выполняется цикл по 10 страницам (по 100 записей на страницу).

Используется Search API с запросом q=stars:>10000&sort=stars&order=desc.

После каждой страницы – пауза 5 секунд.

Результат сохраняется в список all_repos.

Детальный анализ каждого репозитория

Для каждого элемента из списка отправляется запрос к Contents API (/repos/{full_name}/contents/).

Проверяется наличие файлов по именам (регистронезависимо):

has_coc: любое имя, содержащее CODE_OF_CONDUCT

has_contributing: содержит CONTRIBUTING

has_readme: содержит README

has_workflows: наличие папки .github (в ней обычно лежат workflows)

Из метаданных репозитория извлекаются: имя, организация, язык, звёзды, форки, открытые issues, возраст в днях, описание.

Между запросами пауза 1.5 секунды, чтобы не превысить лимит.

Сохранение результатов

Все собранные записи складываются в список словарей.

После завершения цикла создаётся pandas DataFrame и сохраняется в top_1000_os_rules.csv (кодировка UTF-8).

## Диаграмма последовательности (PlantUML)
Ниже представлена диаграмма, иллюстрирующая взаимодействие компонентов парсера с GitHub API и между собой.

<img width="604" height="595" alt="image" src="https://github.com/user-attachments/assets/d95a8b4f-25f6-4869-90bb-283636c196af" />

## Заключение
Предложенная архитектура обеспечивает надёжный сбор данных о популярных репозиториях с учётом ограничений GitHub API. Встроенные задержки и обработка ошибок позволяют избежать блокировки по лимитам. Результаты сохраняются в структурированном формате, готовом для дальнейшего анализа (например, статистики распространённости файлов CODE_OF_CONDUCT и CONTRIBUTING среди топ-проектов).
