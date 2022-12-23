# Localizing URLs with Svelte

As good SEO practice, a website should not serve multiple languages from a single URL. So one thing you want to do is to figure out how to generate separate content for different languages. There weren't many good official documents that cover how to do this in SvelteKit. 

The goal is to create a set of URLs that are prefixed by the language:

```
/en/area/niseko/
/ja/area/niseko/
/en/
/ja/
```

First, add in svelte-i18n to the build:

```
pnpm install --save-dev svelte-i18n
```

Next, setup the svelte-i18n as you documented, by creating a `src/lib/i18n.ts`:

```
import { browser } from '$app/environment'
import { init, register } from 'svelte-i18n'

import en from '../locales/en';
import ja from '../locales/ja';

const defaultLocale = 'en'

register('en', () => {
  return new Promise((resolve, reject) => {
    resolve(en);
  });
})

register('ja', () => {
  return new Promise((resolve, reject) => {
    resolve(ja);
  });
})

init({
  fallbackLocale: defaultLocale,
  initialLocale: browser ? window.navigator.language : defaultLocale,
})
```

And also your localization strings per language in `src/locales/{lang}.ts`

```
export const en = {
  key: 'The message'
}
```

Next, move your site to `src/routes/[lang]/`. This will now give `+page.ts` and `+layout.ts` the `param.lang`. We can then use this to set up the current locale. Implement a `+layout.ts` in the root of the route `src/routes/+layout.ts` that will read the language from params:

```
export const prerender = true;
export const trailingSlash = 'always';

import '$lib/i18n' // Import to initialize. Important :)
import { locale, waitLocale } from 'svelte-i18n'
import type { LayoutLoad } from './$types'

export const load: LayoutLoad = async ({ params }) => {
  if (params.lang) {
    locale.set(params.lang)
  }

  await waitLocale()
}
```

After this, you should be able to use the regular svelte-i18n convenience method to use `$_(stringKey)` to get the localized name. If you need to get the current language, there probably is an API, but I just added a `{'lang': 'en}` to the locale file and read the language through that.


