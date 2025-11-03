Техническое задание: проект игры «The Murk»
(версия v0.4)
1. Общее описание игры
Жанр/атмосфера: реалистичный survival + immersive‑sim; текстовая подача, одиночный опыт, «дневник на войне».
Платформа: Windows десктоп (версия для других платформ откладывается).
Целевая аудитория: игроки, интересующиеся глубокой текстовой атмосферой, выживанием, напряжённым миром.
Ключевая идея: движок игры — ИИ; игрок входит в мир, задание мира задаёт описание, далее ИИ генерирует события и взаимодействия.
Основные особенности:
Реалтайм (сутки = 20 мин), голод/жажда/температура.
Свободное описание мира игроком при запуске: жанр/тон/условия.
Сложности: Easy / Normal / Hard / Unreal.
Многоязычие (интерфейс + часть текста).
UI‑стиль: ретро‑терминал с зелёной подсветкой, эффектами сканлайнов, мерцания и т.п.
Используемый стек технологий: HTML + CSS + TypeScript + Vite (фронтенд) + Rust + Tauri v2 (бекенд/десктоп).
Генерация сюжета/событий: через LLM API (например, Google Gemini API) с учётом лимитов, с кешем, очередью, fallback‑режимом.
Документируем и передаём дизайн через JSON‑токены (design tokens) и глобальную систему дизайна.
2. Ключевые особенности
«Бесконечный сюжет»: нет жёсткого набора заранее заданных тегов‑механик; всё строится на строковых описаниях мира, памяти мира и генерации «на лету».
Создание мира: игрок вводит название мира, выбирает сложность, вводит (опционально) описание мира — этот текст задаёт правила‑канон для ИИ. Также есть переключатель «Строгий реализм».
Свободное текстовое описание мира → задаёт канон + память мира + стартовые условия.
Реалтайм‑механика событий + цикл: день/ночь, голод/жажда/температура + инциденты от ИИ.
Многоязычие: переключение локали без перезапуска мира, поддержка UI и текста на разных языках.
Дизайн‑совместимость: стилевое оформление ретро‑терминала, зелёный/чёрный, моноширинный шрифт, эффекты (мерцание, scanlines, glow) — оформлено в JSON design tokens.
3. Игровые механики
Статусы игрока: health, hunger, thirst, temperature.
Основные команды (текстовый ввод): «осмотреть», «обыскать», «прислушаться», «идти <к/на>», «укрыться», «использовать <предмет>», «лечиться», «инвентарь», «статус», «спать».
Инвентарь: вес/объём ограничения, стартовое снаряжение зависит от описания мира и сложности.
Бой/встречи: текстовые столкновения с рисками, зависят от экипировки, времени суток, погоды.
Управление ресурсами: пища, вода, тепло, инструменты — всё важно.
Генерация событий: ИИ создает «storylet»‑события на основе памяти мира и канона, с весами, пейзажем и пейсингом волн.
Прогрессия:
Краткосрочные цели: выжить ночь, найти укрытие, добыть пищу/воду.
Долгосрочные: адаптироваться к миру, раскрыть сюжет мира, сохранить состояние.
Сложности:
Easy / Normal / Hard / Unreal: влияют на частоту событий, урон/потери, лут‑коэффициент, подсказки.
Подсказки ввода: есть и могут быть отключены.
Строгий реализм: при включённом — нет фантастики, только реалистичные угрозы.
4. Игровой цикл и прогрессия
Цикл: Создание мира → старт игры → игрок начинает в мире → режим реалтайм: меняется время суток, состояние игрока обновляется, ИИ генерирует события → игрок реагирует → цикл продолжается.
Пейсинг: волны — тихий участок → накат событий → передышка. Сложность определяет параметры.
Интервал событий: зависит от сложности, например: Easy — большая передышка, Unreal — короткие интервалы.
Пример параметров сложности:
Параметр Easy Normal Hard Unreal Интервал крупных событий 90‑150с 60‑120с 45‑90с 30‑75с Потери (голод/жажда) −30% 0% +20% +40% Лут‑множитель ×1.35 ×1.0 ×0.85 ×0.7 Частота враждебных встреч ↓ базовая ↑ ↑↑ Подсказки ввода вкл вкл вкл выкл
Стартовые условия: зависят от описания мира и сложности (инвентарь, экипировка, время суток, локация).
5. Пользовательский интерфейс (UI/UX)
Общие принципы
Десктоп‑окно Windows: клавиатура + мышь.
Интерфейс в стиле ретро‑терминала: фон чёрный (#000000), текст зелёный (#0F0F0F/#00FF00 etc), моноширинный шрифт (например Courier New).
Эффекты: сканлайны, мерцание (flicker), лёгкое движение (sway‑эффект).
UI‑элементы: кнопки крупные, с зелёной подсветкой, моноширинный текст, обводки зелёные.
Экран создания мира: название + сложность + описание мира + реализм (галочка) + живое превью влияния описания.
Игровой экран: терминал‑лента сообщений, панель статусов, строка ввода, меню паузы (ESC).
Настройки: звук (громкость мастер/музыка/эффекты), видео (fullscreen), язык (русский / English), API‑ключ (Gemini).
Дизайн‑токены (см. раздел 7)
Цвета, типографика, спэйсинг, бордеры, эффекты заданы JSON‑спецификацией.
UI‑компоненты описаны (оболочка приложения, меню, список миров, терминал, панель ввода, настройки).
Анимации: тайпинг, курсор, плавные переходы меню.
Интернационализация (i18n)
Все UI‑тексты вынесены в внешние файлы (JSON/YAML) с поддержкой ICU MessageFormat.
Переключение языка на лету без перезапуска мира.
Поддержка LTR и (при будущем расширении) RTL.
Формат дат/чисел через Intl API или эквивалент.
6. Требования к арту и звуку
Арт
Минималистичный — нет сложной 3D‑графики; текстовая подача + фоны/иллюстрации по необходимости.
Терминальный стиль: зелёный на чёрном, моноширинный гос.
Элементы UI: светящиеся границы (glow), подсветка при наведении, прозрачные панели.
Звук
Атмосферические эффекты: ветер, дождь, скрип, шаги, эхо.
Звуки интерфейса: щелчки кнопок, печать текста, курсор мигания.
Музыка: минимальная, фоновая, подчёркивает одиночество и напряжение.
Ассеты:
mainMenuTheme.mp3 – фон меню
terminalAmbience.mp3 – фон во время игры
click.mp3 – навигация/кнопки
print.mp3 – звук печатающего текста
7. Техничес требования
Стек
Фронтенд: HTML + CSS + TypeScript + Vite.
Десктоп‑обёртка: Rust + Tauri v2 (фокус на Windows).
Генерация сюжета: LLM API (Gemini) с учётом лимитов, кеш, очередь, fallback механика.
Локализация: файлы сообщений, поддержка ICU, переключение языка.
Архитектура
Разделение: UI (фронтенд) ↔ движок игры + память мира ↔ API‑модуль (LLM) ↔ хранилище миров/сейвов.
Хранение данных: файлы/база (на диске) через Tauri/Rust.
Интеграция с Gemini API
Подсчёт запросов/токенов (rate‑limits).
Очередь задач → при приближении лимита → перевод в экономный режим → fallback на локальные шаблоны.
Запросы включают параметр локали.
Мониторинг использования, логирование.
Design tokens
JSON‑файл спецификации дизайна (пример далее).
Сборка и дистрибуция
Windows‑только таргет: .exe или .msi через Tauri.
CI/CD: настроено под Windows‑runner.
Версионирование: версия приложения‑отображается (например, v1.1.0 на стартовом экране).
8. Дизайн‑спецификация (design.json)
{
  "meta": {
    "name": "the-murk-terminal",
    "title": "The Murk Design System",
    "description": "Terminal‑inspired UI palette and component guidance for The Murk survival experience.",
    "version": "1.0.0",
    "theme": "terminal‑glow"
  },
  "foundations": {
    "palette": {
      "background": {
        "base": "#000000",
        "canvas": "#000000",
        "overlayStrong": "rgba(0, 0, 0, 0.9)",
        "overlaySoft": "rgba(0, 0, 0, 0.65)",
        "panel": "#000000"
      },
      "accent": {
        "primary": "#32FF32",
        "primaryBright": "#4ADE80",
        "primarySoft": "rgba(50, 255, 50, 0.35)",
        "primaryShadow": "rgba(50, 255, 50, 0.3)",
        "secondary": "#22C55E"
      },
      "neutrals": {
        "textHigh": "#E8FFE8",
        "textBase": "#4ADE80",
        "textMuted": "#15803D",
        "placeholder": "#166534",
        "divider": "#064E3B"
      },
      "status": {
        "warning": "#FACC15",
        "error": "#F87171",
        "errorBorder": "#B91C1C",
        "success": "#86EFAC",
        "info": "#22C55E"
      }
    },
    "typography": {
      "family": "Courier New, Courier, monospace",
      "titleCase": {
        "weight": 600,
        "letterSpacing": "0.25em"
      },
      "body": {
        "weight": 400,
        "size": "18px",
        "lineHeight": "1.6"
      },
      "small": {
        "weight": 400,
        "size": "14px",
        "lineHeight": "1.4"
      },
      "headline": {
        "weight": 700,
        "size": "60px",
        "lineHeight": "1.1"
      }
    },
    "spacing": {
      "xxs": "2px",
      "xs": "4px",
      "sm": "8px",
      "md": "16px",
      "lg": "24px",
      "xl": "32px",
      "xxl": "48px"
    },
    "radius": {
      "none": "0px",
      "sm": "6px",
      "md": "12px",
      "lg": "16px"
    },
    "borders": {
      "thin": "1px solid rgba(50, 255, 50, 0.45)",
      "strong": "2px solid rgba(50, 255, 50, 0.7)",
      "danger": "1px solid #B91C1C"
    },
    "shadows": {
      "glowSoft": "0 0 10px rgba(50, 255, 50, 0.3)",
      "glowStrong": "0 0 20px rgba(50, 255, 50, 0.5)",
      "panelInset": "inset 0 0 20px rgba(50, 255, 50, 0.5)"
    },
    "effects": {
      "scanlines": "repeating‑linear‑gradient(0deg, rgba(0,0,0,0.5), rgba(0,0,0,0.5) 1px, transparent 1px, transparent 3px)",
      "ambientGlow": "rgba(50, 255, 50, 0.25)",
      "flickerOpacity": [0.6, 0.8],
      "swayAngle": "0.35deg"
    },
    "states": {
      "hover": {
        "background": "rgba(20, 83, 45, 0.9)",
        "border": "rgba(34, 197, 94, 0.9)",
        "shadow": "0 0 12px rgba(50, 255, 50, 0.4)"
      },
      "focus": {
        "outline": "2px solid rgba(50, 255, 50, 0.7)",
        "shadow": "0 0 0 4px rgba(20, 83, 45, 0.35)"
      },
      "disabled": {
        "opacity": 0.5,
        "cursor": "not‑allowed"
      }
    }
  },
  "components": {
    "appShell": {
      "background": "#000000",
      "textColor": "#4ADE80",
      "scanlineOverlay": "enabled",
      "textShadow": "0 0 5px rgba(50, 255, 50, 0.4), 0 0 10px rgba(50, 255, 50, 0.3)",
      "maxContentWidth": "1100px"
    },
    "mainMenu": {
      "title": {
        "size": "72px",
        "color": "#86EFAC",
        "letterSpacing": "0.3em"
      },
      "panel": {
        "background": "transparent",
        "border": "1px solid rgba(21, 128, 61, 1)",
        "hoverBackground": "rgba(20, 83, 45, 0.55)"
      },
      "backgroundPattern": "repeating‑linear‑gradient(transparent 0, rgba(50, 255, 50, 0.1) 1px, rgba(50, 255, 50, 0.05) 2px, transparent 3px, transparent 10px)",
      "vignette": "inset 0 0 8rem 4rem #000000",
      "errorToast": {
        "background": "rgba(76, 5, 5, 0.4)",
        "border": "1px solid #B91C1C",
        "textColor": "#F87171"
      }
    },
    "worldSelection": {
      "panel": {
        "background": "#000000",
        "border": "2px solid #14532D",
        "glow": "0 0 20px rgba(50, 255, 50, 0.3)"
      },
      "list": {
        "itemActive": {
          "background": "rgba(20, 83, 45, 0.4)",
          "border": "1px solid rgba(34, 197, 94, 0.9)"
        },
        "itemIdle": {
          "background": "transparent",
          "border": "1px solid rgba(21, 128, 61, 0.8)"
        }
      }
    },
    "terminalDisplay": {
      "font": "Courier New, Courier, monospace",
      "textColor": "#4ADE80",
      "userTextColor": "#BBF7D0",
      "cursor": {
        "width": "8px",
        "height": "20px",
        "color": "#4ADE80"
      },
      "thinkingIndicator": {
        "color": "#22C55E",
        "frames": ["[   ]", "[.  ]", "[.. ]", "[...]", "[ ..]", "[  .]"]
      },
      "padding": "16px"
    },
    "inputBar": {
      "background": "#000000",
      "borderTop": "1px solid #14532D",
      "promptPrefix": ">",
      "placeholderColor": "#166534",
      "button": {
        "border": "1px solid #15803D",
        "textColor": "#4ADE80",
        "hoverBackground": "#14532D",
        "disabledOpacity": 0.5
      }
    },
    "settingsMenu": {
      "frame": {
        "background": "rgba(0, 0, 0, 0.9)",
        "border": "1px solid rgba(21, 128, 61, 0.9)",
        "radius": "12px",
        "shadow": "inset 0 0 20px rgba(50, 255, 50, 0.5)"
      },
      "sectionHeading": {
        "color": "#BAF2C0",
        "size": "24px"
      },
      "controlLabel": {
        "color": "#4ADE80",
        "size": "14px"
      },
      "slider": {
        "trackColor": "#14532D",
        "thumbColor": "#22C55E",
        "disabledOpacity": 0.4
      },
      "toggle": {
        "checkedBorder": "2px solid rgba(34, 197, 94, 0.9)",
        "checkedFill": "#4ADE80",
        "uncheckedBorder": "2px solid rgba(21, 128, 61, 1)"
      }
    },
    "modal": {
      "overlay": "rgba(0, 0, 0, 0.9)",
      "frame": {
        "background": "#000000",
        "border": "2px solid #32FF32",
        "shadow": "0 0 20px rgba(50, 255, 50, 0.5)",
        "padding": "32px"
      },
      "textColor": "#4ADE80"
    },
    "buttons": {
      "default": {
        "height": "48px",
        "border": "1px solid #15803D",
        "textColor": "#4ADE80",
        "hoverBackground": "#14532D",
        "activeShadow": "0 0 16px rgba(50, 255, 50, 0.35)"
      },
      "primary": {
        "border": "2px solid #22C55E",
        "background": "rgba(34, 197, 94, 0.1)",
        "textColor": "#BBF7D0"
      },
      "danger": {
        "border": "1px solid #B91C1C",
        "background": "rgba(120, 20, 20, 0.35)",
        "textColor": "#F87171",
        "hoverBackground": "rgba(120, 20, 20, 0.55)"
      }
    },
    "alerts": {
      "error": {
        "background": "rgba(40, 0, 0, 0.6)",
        "textColor": "#F87171",
        "border": "1px solid #B91C1C"
      },
      "warning": {
        "background": "rgba(87, 64, 0, 0.45)",
        "textColor": "#FACC15",
        "border": "1px solid rgba(250, 204, 21, 0.7)"
      },
      "success": {
        "background": "rgba(20, 83, 45, 0.35)",
        "textColor": "#86EFAC",
        "border": "1px solid rgba(34, 197, 94, 0.7)"
      }
    }
  },
  "audio": {
    "tracks": [
      {
        "id": "mainMenuTheme",
        "path": "assets/main_menu_theme.mp3",
        "usage": "Main menu ambience loop",
        "volumeDefault": 0.65
      },
      {
        "id": "terminalAmbience",
        "path": "assets/terminal_ambient.mp3",
        "usage": "In‑game background loop",
        "volumeDefault": 0.45
      },
      {
        "id": "click",
        "path": "assets/click.mp3",
        "usage": "Button and navigation feedback",
        "volumeDefault": 0.9
      },
      {
        "id": "print",
        "path": "assets/print.mp3",
        "usage": "Text typing feedback",
        "volumeDefault": 1.0
      }
    ]
  },
  "motion": {
    "flicker": {
      "duration": "0.15s",
      "timing": "steps(5, end)",
      "opacityRange": [0.6, 0.8]
    },
    "scanlineDrift": {
      "duration": "20s",
      "timing": "linear",
      "direction": "vertical"
    },
    "sway": {
      "duration": "7s",
      "timing": "ease‑in‑out",
      "angleRange": ["-0.35deg", "0.35deg"]
    },
    "typing": {
      "intervalMs": 30,
      "cursor": {
        "animation": "pulse",
        "color": "#4ADE80"
      }
    }
  }
}

Примечание: это базовый файл спецификации дизайна. Его можно расширять (добавлять темы «тёмная», «светлая», другие настройки) или разбивать на модули (colors.json, typography.json и т.д.) для удобства.Использование design tokens позволяет обеспечить единый источник правды для цветов/типографики/спейсинга — ключевая практика в системах дизайна. (uxpin.com)
9. План следующих шагов
Инициализация репозиториев: фронтенд + Tauri (как обсуждали).
Подключение design token файла — интеграция с CSS/SCSS/TypeScript (например, через Vite, экспорт CSS‑переменных).
Реализация UI‑экранов: главное меню, список миров, игровой терминал, строка ввода.
Интеграция локализации (i18n) + переключение языка.
Настройка очереди API‑вызовов, кеша, и механизмов защиты от лимитов.
Настройка сборки Windows .exe/.msi с Tauri.
Тестирование UX/дизайна, проверка читаемости закона шрифта/цветов/эффектов.
Подготовка документации: style guide (токены), README, и возможное расширение дизайна (темы/варианты).