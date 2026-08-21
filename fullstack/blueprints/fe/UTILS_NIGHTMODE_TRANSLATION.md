
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


## Spaguetti code and bad decissions about Translation and NotificationService

Shared UI is not fully stateless anymore
Translation is too app-specific / hard-coded
App state/services/tools like translation should be consumed by app-owned components, not generic scaffold components

target architecture:
shared-domain
  pure types, contracts, DTO-independent primitives

shared-ui
  stateless visual primitives and scaffold components
  no translation store
  no notification service
  no app-specific labels
  no business rules

shared-features-translation
  app-level/feature-level state and translation lookup
  can be used by app components and feature containers
Good, this confirms the diagnosis.


## What is exactly going on?

`shared-ui` imports the translation feature directly:

```plain text
libs/shared/ui/src/components/... 
import { useTranslation } from '@monorepo/shared-features-translation';
```
So Nx sees:

```plain text
shared-ui --> shared-features-translation
```

Then something in the translation dependency path points back to `shared-ui`.

Because translation depends on shared-domain, and shared-domain depends on shared-ui:

```plain text
shared-features-translation
  --> shared-domain
      --> shared-ui
```
So the full cycle is probably:

```plain text
shared-ui
  --> shared-features-translation
      --> shared-domain
          --> shared-ui
```


```plain text
libs/shared/shared-infrastructure/src/notifications/notificationService.ts
import { toasterMessagesStyles } from '@monorepo/shared/ui';
```


That one may not be part of this exact cycle, but architecturally it is also inverted:

```plain text
shared-infrastructure --> shared-ui
```


Infrastructure should not depend on UI styling.

The clean design is:

```plain text
apps/features call useTranslation()
apps/features pass translated labels into shared-ui components
shared-ui renders labels only
```


So `shared-ui` should receive strings via props, not call `useTranslation()` itself.

Example:

```typescript
<EntityModal
  closeLabel={commonLabels.btnClose}
  // other props...
/>
```


Instead of `EntityModal` importing `useTranslation()` internally.


And because translation depends on domain:

```plain text
shared-features-translation --> shared-domain --> shared-ui
```

That is why the cycle survives the rollbacks.

---

# About `notificationService`

This import is also bad:

```typescript
import { toasterMessagesStyles } from '@monorepo/shared/ui';
```


in:

```plain text
libs/shared/shared-infrastructure/src/notifications/notificationService.ts
```


A notification service should probably emit a notification with semantic data:

```typescript
type: 'success' | 'error' | 'warning'
```


Then the UI layer decides the styles.

So yes, rolling it back is reasonable, but it is likely **not the main reason** for the reported cycle.

Fix the direction of dependencies:

```plain text
domain -> no UI imports
translation -> domain only
ui -> domain allowed, but ideally no translation
app/features -> translation + UI composition
```
