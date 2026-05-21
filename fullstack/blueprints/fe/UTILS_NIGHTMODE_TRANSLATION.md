
Short answer: **partially yes, but not everything should live in `libs/shared/feature`.**

For **translation** and **night mode**, I would use `libs/shared/feature` for the **React-facing feature integration**, but split reusable pieces across the existing shared layers.

## Recommended location

```plain text
libs/shared/domain
libs/shared/application
libs/shared/infrastructure
libs/shared/feature
libs/shared/ui
```


## 1. Translation / i18n

I would **not** put the entire translation system only in `libs/shared/feature`.

Recommended split:

```plain text
libs/shared/domain
  translation types, locale types, message key contracts

libs/shared/application
  translation state/use-cases, language switching logic

libs/shared/infrastructure
  translation resource loading, localStorage persistence, browser locale detection

libs/shared/feature
  TranslationProvider, useTranslation hook, React integration

libs/shared/ui
  optional language selector UI component
```


So `libs/shared/feature` is a good place for:

```plain text
TranslationProvider
useTranslation
useLocale
I18nBoundary / I18nProvider composition
```


But not necessarily for:

```plain text
locale contracts
storage adapters
translation file loaders
language detection
generic translation types
```


Those are better in `domain`, `application`, or `infrastructure`.

---

## 2. Night mode / theme mode

Same idea: **do not put everything only in `libs/shared/feature`**.

Recommended split:

```plain text
libs/shared/domain
  ThemeMode type, theme contracts

libs/shared/application
  theme mode state/use-cases, toggle logic

libs/shared/infrastructure
  localStorage persistence, system preference detection

libs/shared/feature
  ThemeProvider, useThemeMode hook, React integration

libs/shared/ui
  ThemeToggle, visual controls, theme-aware reusable components/styles
```


So `libs/shared/feature` is a good place for:

```plain text
ThemeProvider
useTheme
useThemeMode
ThemeModeProvider
```


But not necessarily for:

```plain text
theme tokens
CSS variables
storage persistence
system dark-mode detection
button/switch UI
```


Those should likely be split out.

---

## Why not put everything in `libs/shared/feature`?

Because `feature` should usually mean:

> reusable feature-level composition for React screens/apps.

If you put everything there, it becomes a mixed bucket containing:

- domain types
- browser APIs
- persistence logic
- React providers
- visual components
- CSS/theme tokens

That makes reuse harder later.

For future projects, modularity improves if each concern is reusable independently.

---

## Suggested structure

Something like this would be cleaner:

```plain text
libs/shared/domain/src/i18n
  locale.types.ts
  translation.types.ts

libs/shared/application/src/i18n
  locale-store.ts
  i18n.service.ts

libs/shared/infrastructure/src/i18n
  browser-locale-detector.ts
  translation-loader.ts
  locale-storage.ts

libs/shared/feature/src/i18n
  TranslationProvider.tsx
  useTranslation.ts
  useLocale.ts
  index.ts

libs/shared/ui/src/i18n
  LanguageSelector.tsx
```


And for theme/night mode:

```plain text
libs/shared/domain/src/theme
  theme-mode.types.ts
  theme.types.ts

libs/shared/application/src/theme
  theme-store.ts
  theme.service.ts

libs/shared/infrastructure/src/theme
  theme-storage.ts
  system-theme-detector.ts

libs/shared/feature/src/theme
  ThemeProvider.tsx
  useTheme.ts
  useThemeMode.ts
  index.ts

libs/shared/ui/src/theme
  ThemeToggle.tsx
```


---

## Final recommendation

Yes, **`libs/shared/feature` should contain the reusable React feature layer** for both translation and night mode.

But the whole implementation should be layered like this:

| Concern | Best location |
|---|---|
| Types/contracts | `libs/shared/domain` |
| State/use-case logic | `libs/shared/application` |
| Browser/localStorage/resource loading | `libs/shared/infrastructure` |
| React providers/hooks | `libs/shared/feature` |
| Buttons/selectors/visual components/styles | `libs/shared/ui` |

So the answer is:

> Build the React-facing feature modules in `libs/shared/feature`, but keep their lower-level contracts, logic, infrastructure, and visual components in the corresponding shared libraries.