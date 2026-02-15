# 📊 GitHub Top 1000 Repositories Parser

[![Kaggle](https://img.shields.io/badge/Kaggle-Dataset-blue?style=for-the-badge&logo=kaggle)](https://www.kaggle.com/datasets/felkan228/metadata-of-the-top-1000-github-repositories)
[![License: CC0-1.0](https://img.shields.io/badge/License-CC0_1.0-lightgrey.svg?style=for-the-badge)](http://creativecommons.org/publicdomain/zero/1.0/)

---

## 🌎 Language / Язык
* [English Version](#-english-version)
* [Русская Версия](#-русская-версия)

---

## 🇬🇧 English Version

### 🏗️ Architecture
This parser is designed to collect information about the 1000 most-starred GitHub repositories and analyze the presence of key community files. The architecture is built around three sequential stages, strictly respecting **GitHub API rate limits**.

### 🧩 Overall Structure
The parser consists of four logical components:

* **Request Manager** 🛰️ – Handles HTTP requests, manages authorization headers, and processes API responses.
* **Metadata Collector** 🔎 – Executes search queries via the `Search API` to fetch the top 1000 repositories.
* **Content Analyzer** 🛠️ – Scans the root directory (`/contents`) of each repository for target files.
* **Saving Module** 💾 – Aggregates data into a `pandas.DataFrame` and exports it to CSV.

> **Note:** The system operates in a single thread with `time.sleep` delays to stay within the 5000 requests/hour limit for authenticated users.

### 🔄 Step‑by‑Step Process

1.  **Retrieving the Repository List**
    * Iterates through 10 pages (100 records per page).
    * Query: `q=stars:>10000&sort=stars&order=desc`.
    * A **5-second pause** is applied after each page request.
2.  **Detailed Analysis**
    * Requests the `/contents/` endpoint for each repository.
    * **File checks (case-insensitive):** `CODE_OF_CONDUCT`, `CONTRIBUTING`, `README`, and `.github` folder.
    * **Metadata extraction:** Name, organization, language, stars, forks, open issues, age, and description.
    * A **1.5-second pause** is applied between repository requests.
3.  **Data Export**
    * Accumulates records in a list of dictionaries.
    * Saves final data as `top_1000_os_rules.csv` (UTF-8).

### 📊 Sequence Diagram
<img width="599" height="598" alt="Sequence Diagram EN" src="https://github.com/user-attachments/assets/c8e29015-a496-43cb-9cfb-f9a67dbf5227" />

---

## 🇷🇺 Русская Версия

### 🏗️ Архитектура
Парсер предназначен для сбора данных о 1000 самых популярных репозиториях GitHub. Система анализирует наличие файлов инфраструктуры (Open Source standards), работая в три этапа с соблюдением лимитов **GitHub API**.

### 🧩 Общая структура
Логика разделена на четыре модуля:

* **Менеджер запросов** 🛰️ – формирование HTTP-запросов, управление токенами и обработка ответов.
* **Сборщик метаданных** 🔎 – получение списка топ-1000 через `Search API`.
* **Анализатор содержимого** 🛠️ – проверка корня репозитория (`/contents`) на наличие целевых файлов.
* **Модуль сохранения** 💾 – агрегация данных в `pandas.DataFrame` и экспорт в CSV.

> **Важно:** Для соблюдения лимитов (5000 запр/час) используются задержки `time.sleep` между итерациями.

### 🔄 Пошаговое описание процесса

1.  **Получение списка репозиториев**
    * Цикл по 10 страницам (по 100 записей).
    * Запрос: `q=stars:>10000&sort=stars&order=desc`.
    * Пауза **5 секунд** после каждой страницы.
2.  **Детальный анализ**
    * Запрос к `Contents API` для каждого проекта.
    * **Проверка файлов:** `has_coc`, `has_contributing`, `has_readme`, `has_workflows` (папка `.github`).
    * **Метаданные:** имя, организация, язык, звёзды, форки, задачи, возраст и описание.
    * Пауза **1.5 секунды** между репозиториями.
3.  **Сохранение**
    * Данные собираются в список словарей.
    * Итоговый файл: `top_1000_os_rules.csv` (кодировка UTF-8).

### 📊 Диаграмма последовательности
<img width="604" height="595" alt="Sequence Diagram RU" src="https://github.com/user-attachments/assets/d95a8b4f-25f6-4869-90bb-283636c196af" />

---

## 🔗 Links / Ссылки
* **Kaggle Dataset:** [Metadata of the Top 1000 GitHub Repositories](https://www.kaggle.com/datasets/felkan228/metadata-of-the-top-1000-github-repositories)
* **License:** [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/)

---

> **Conclusion:** This architecture provides a robust way to analyze community standards across the most influential software projects in the world.  
> **Заключение:** Предложенная архитектура обеспечивает надежный сбор данных для анализа стандартов сообщества в крупнейших IT-проектах мира.
