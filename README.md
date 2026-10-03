# 🍷🍽️ DishKit — Интерактивное планшетное меню ресторанного класса для Android

[![Android](https://img.shields.io/badge/Platform-Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)](https://developer.android.com/)
[![Java 11](https://img.shields.io/badge/Language-Java%2011-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)](https://www.oracle.com/java/)
[![Firebase](https://img.shields.io/badge/Backend-Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com/)
[![Design: Glassmorphism](https://img.shields.io/badge/Design-Glassmorphism%20%26%20M3-600018?style=for-the-badge)](https://m3.material.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](LICENSE)

**DishKit** — флагманское нативное Android-приложение для настольных планшетов и интерактивных терминалов в ресторанах, лаунж-барах и кафе. 

Приложение сочетает премиальный визуальный стиль с эффектами **Glassmorphism (матовое полупрозрачное стекло)**, глубокую винно-золотую цветовую палитру (Burgundy & Gold), фоновые текстуры зала, моментальную синхронизацию меню через **Google Cloud Firestore**, предзаказную корзину гостя с подсчетом суммы в сомах (сом / KGS) и скрытую Kiosk-панель управления заведением.

---

## 📌 Содержание
- [🌟 Отличия DishKit от DishKit Lite](#-отличия-dishkit-от-dishkit-lite)
- [✨ Ключевые возможности](#-ключевые-возможности)
  - [Режим гостя (Интерактивное меню ресторанного класса)](#режим-гостя-интерактивное-меню-ресторанного-класса)
  - [Секретный Kiosk-доступ в админку (5 тапов)](#секретный-kiosk-доступ-в-админку-5-тапов)
  - [Панель управления рестораном (Admin Panel)](#панель-управления-рестораном-admin-panel)
- [🎨 Дизайн и стилизация Glassmorphism](#-дизайн-и-стилизация-glassmorphism)
- [🏗 Архитектура проекта](#-архитектура-проекта)
- [📂 Структура каталогов](#-структура-каталогов)
- [🗄 Схема данных Firebase Firestore](#-схема-данных-firebase-firestore)
- [⚙️ Технологический стек](#️-технологический-стек)
- [🚀 Быстрый старт и установка](#-быстрый-старт-и-установка)
- [🔒 Настройка правил безопасности Firebase](#-настройка-правил-безопасности-firebase)
- [👤 Автор и контакты](#-автор-и-контакты)

---

## 🌟 Отличия DishKit от DishKit Lite

| Параметр | **DishKit** (Основная версия) | **DishKit Lite** |
| :--- | :--- | :--- |
| **Визуальный стиль** | Премиум **Glassmorphism**, полупрозрачные карточки со свечением | Минималистичный плоский Flat Material |
| **Цветовая палитра** | Благородный винный бордо (`#600018`) и теплое золото (`#D99026`) | Морской бирюзовый (Teal `#2C6D7E`) |
| **Фоновые текстуры** | Атмосферные фоны зала ресторана (`bg.jpg`, `bg_categori.jpg`) | Сплошная заливка фона |
| **Оформление диалогов** | Кастомный фон диалогов с мягкими тенями (`my_custom_dialog_background`) | Стандартные диалоги Material |
| **Назначение** | Рестораны высокой кухни, лаунж-бары, стейк-хаусы, премиум-кафе | Кофейни, бистро, фаст-фуд, терминалы самообслуживания |

---

## ✨ Ключевые возможности

### 🍽 Режим гостя (Интерактивное меню ресторанного класса)
- **Ландшафтный интерфейс для планшетов:** Идеально подходит для 10-дюймовых и 12-дюймовых настольных планшетов в чехлах-подставках.
- **Двухпанельная навигация:** Слева меню категорий с эффектом матового стекла, справа трехколоночная сетка блюд с плавным скроллом.
- **Подробные карточки блюд:** Высококачественные фотографии (кэширование через **Glide 4.16.0**), название, развернутый состав, граммовка и крупный ценник в сомах (сом).
- **Корзина гостя (Guest Cart):**
  - Клиентский расчет общей суммы в оперативной памяти (`CartManager` Singleton).
  - Быстрое управление количеством блюд (+ / -) и диалог подтверждения перед очисткой.
  - Без создания промежуточных черновиков в базе данных.
- **Реалтайм-управление отображением:** Администратор может удаленно включить или выключить показ фотографий, описаний блюд, цен или корзины — интерфейс всех планшетов перестраивается моментально через вебсокеты Firestore!

### 🔐 Секретный Kiosk-доступ в админку (5 тапов)
Чтобы гости не могли выйти из приложения или изменить настройки:
- На экране нет видимых кнопок входа в панель администратора.
- Вход активируется **5 быстрыми тапами по логотипу ресторана в левом верхнем углу** за **1.5 секунды** (`REQUIRED_CLICKS = 5`, `CLICK_TIMEOUT_MS = 1500`).
- При распознавании жеста открывается окно аутентификации **Firebase Auth**.

### 🛠 Панель управления рестораном (Admin Panel)
Удобный интерфейс с тремя вкладками на **ViewPager2 + TabLayout**:
1. **Блюда (`DishesFragment`):**
   - Добавление новых позиций с выбором категории и загрузкой фото в Firebase Storage (`dish_images/`).
   - Переключение видимости в меню в один клик.
   - Сортировка по порядковому номеру (`order`) или по алфавиту.
2. **Категории (`CategoryManagementFragment`):**
   - Создание иерархических категорий и подкатегорий (`parentId`).
   - Валидация дубликатов в одной ветке.
   - Каскадное удаление родительской категории вместе со всеми вложенными подкатегориями через единый Firestore-пакет (`runBatch`).
3. **Настройки (`SettingsFragment`):**
   - Название заведения и загрузка официального логотипа (`logos/`).
   - Переключатели видимости: показ изображений, составов, ценников и корзины.

---

## 🎨 Дизайн и стилизация Glassmorphism

В DishKit применены специальные стили и ресурсы:
- `glass_background` (`#33FFFFFF`) — полупрозрачная подложка с эффектом матового стекла.
- `glass_outline` (`#1AFFFFFF`) — деликатная обводка границ карточек.
- `@drawable/bg` — фоновое изображение ресторанного зала.
- `@drawable/bg_categori` — текстурированный фон навигационной панели категорий.
- Закругление углов карточек `app:cardCornerRadius="16dp"` с мягким возвышением `app:cardElevation="4dp"`.

---

## 🏗 Архитектура проекта

Архитектурный шаблон: **MVVM (Model-View-ViewModel)** + Repository Pattern + Reactive LiveData.

```
                    +----------------------------------------+
                    |          Firebase Cloud Services       |
                    |  (Firestore, Storage, Authentication)  |
                    +-------------------+--------------------+
                                        | Real-time snapshotListener / Uploads
                                        v
                    +----------------------------------------+
                    |             MenuRepository             |
                    | (Слушатели Firestore, кэш, запросы)   |
                    +-------------------+--------------------+
                                        | LiveData
                                        v
                    +----------------------------------------+
                    |             AdminViewModel             |
                    | (Бизнес-логика, валидация, состояние)  |
                    +-------------------+--------------------+
                                        |
                 +----------------------+----------------------+
                 | LiveData                                    | LiveData
                 v                                             v
+----------------------------------+          +----------------------------------+
|           MainActivity           |          |        AdminPanelActivity        |
|  - CategoryListAdapter (Слева)   |          |  - DishesFragment                |
|  - UserMenuAdapter (Сетка 3 кол) |          |  - CategoryManagementFragment    |
|  - GuestCartDialogFragment       |          |  - SettingsFragment              |
|  - CartManager (In-Memory RAM)   |          |  - AdminMenuAdapter              |
+----------------------------------+          +----------------------------------+
```

---

## 📂 Структура каталогов

```
kgz.senior.dishkit/
├── model/
│   ├── AppSettings.java            # Глобальные параметры заведения и тумблеры видимости
│   ├── Category.java               # Модель категории (id, name, order, parentId)
│   ├── MenuItem.java               # Модель блюда (id, name, description, price, imageUrl, order, isVisible)
│   └── CartItem.java               # Элемент корзины гостя (блюдо + количество + сумма)
├── repository/
│   ├── CartManager.java            # Потокобезопасный Singleton управления корзиной гостя
│   └── MenuRepository.java         # Репозиторий доступа к Firestore и Firebase Storage
├── utils/
│   └── Constants.java              # Константы коллекций, документов и путей Storage
├── view/
│   ├── MainActivity.java           # Главный экран (Glassmorphic меню + 5-tap секретный триггер)
│   ├── CategoryListAdapter.java    # Адаптер вертикального списка категорий
│   ├── UserMenuAdapter.java        # Адаптер трехколоночной сетки блюд
│   ├── GuestCartDialogFragment.java# Модальное окно корзины с подсчетом стоимости
│   ├── GuestCartAdapter.java       # Адаптер позиций в корзине
│   └── admin/
│       ├── LoginActivity.java      # Экран входа администратора
│       ├── AdminPanelActivity.java # Контейнер админ-панели (TabLayout + ViewPager2)
│       ├── DishesFragment.java     # Управление блюдами, фото и сортировкой
│       ├── CategoryManagementFragment.java # Управление категориями и подкатегориями
│       ├── SettingsFragment.java   # Настройки заведения и глобальные тумблеры
│       ├── AdminMenuAdapter.java   # Адаптер блюд для админки
│       ├── CategoryManagementAdapter.java # Адаптер категорий
│       └── AdminPagerAdapter.java  # ViewPager2 адаптер вкладок админ-панели
└── viewmodel/
    └── AdminViewModel.java         # Главная ViewModel управления состоянием
```

---

## 🗄 Схема данных Firebase Firestore

### 1. Коллекция `menu_items`
```json
{
  "id": "dish_101",
  "name": "Шашлык из баранины",
  "description": "Сочные кусочки баранины с маринованным луком и горячей лепешкой",
  "category": "Горячие блюда",
  "categoryId": "cat_hot_01",
  "price": 480.0,
  "imageUrl": "https://firebasestorage.googleapis.com/.../dish_images/shashlik.jpg",
  "order": 1,
  "isVisible": true
}
```

### 2. Коллекция `categories`
```json
{
  "id": "cat_hot_01",
  "name": "Горячие блюда",
  "order": 1,
  "parentId": null
}
```

### 3. Документ `config/main_settings`
```json
{
  "cafeName": "Чайхана Нават",
  "logoUrl": "https://firebasestorage.googleapis.com/.../logos/logo.png",
  "isImageVisible": true,
  "isDescriptionVisible": true,
  "isPriceVisible": true,
  "isGuestCartVisible": true,
  "adminDishSortType": "ORDER"
}
```

---

## ⚙️ Технологический стек

- **Язык разработки:** Java 11
- **Целевая платформа:** Android (Min SDK 24, Target SDK 36, Compile SDK 36)
- **Сборка:** Gradle Kotlin DSL (`build.gradle.kts`)
- **База данных:** Google Cloud Firestore (реалтайм-слушатели)
- **Хранилище медиа:** Firebase Storage (`dish_images/`, `logos/`)
- **Аутентификация:** Firebase Authentication (Email/Password)
- **Аналитика:** Firebase Analytics
- **Загрузка фото:** Bumptech Glide 4.16.0
- **Компоненты UI:** Google Material Design 3 (Glassmorphism & MaterialCardView)

---

## 🚀 Быстрый старт и установка

### 1. Клонирование репозитория
```bash
git clone https://github.com/toctosunov/DishKit.git
cd DishKit
```

### 2. Настройка Firebase
1. Создайте проект в [Firebase Console](https://console.firebase.google.com/).
2. Включите **Authentication** (способ входа Email/Password).
3. Создайте базу данных **Cloud Firestore** и хранилище **Firebase Storage**.
4. Добавьте Android-приложение с package name:
   ```
   kgz.senior.dishkit
   ```
5. Скачайте файл **`google-services.json`** и скопируйте в каталог:
   ```
   DishKit/app/google-services.json
   ```

### 3. Сборка и запуск
1. Откройте проект в **Android Studio**.
2. Дождитесь завершения Gradle Sync.
3. Подключите планшет (или запустите эмулятор планшета в альбомной ориентации).
4. Запустите проект (`Shift + F10`).

---

## 🔒 Настройка правил безопасности Firebase

Правила для **Cloud Firestore** (`firestore.rules`):
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /menu_items/{item} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /categories/{category} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /config/{setting} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

Правила для **Firebase Storage** (`storage.rules`):
```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /logos/{allPaths=**} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /dish_images/{allPaths=**} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

---

## 👤 Автор и контакты

- **Разработчик:** Мирбек Токтосунов ([@toctosunov](https://github.com/toctosunov))
- **Email:** toktosunovmirbek75@gmail.com
- **Репозиторий проекта:** [https://github.com/toctosunov/DishKit](https://github.com/toctosunov/DishKit)
