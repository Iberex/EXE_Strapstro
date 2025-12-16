# 🚨 WEB3 EXAMEN - COPILOT STRICT RULES 🚨

## ✅ DO's
- Gebruik **Astro 4.x**
- Gebruik **container queries** voor responsive design
- Gebruik een **fetchApi wrapper** (zie slides, met qs import en populate=*)
- Zet je Strapi URL in **.env** als `STRAPI_URL`
- Werk **mobile-first** met rem units
- Gebruik `astro add` voor integraties

## ❌ DON'Ts
- Geen `getStaticProps` gebruiken
- Geen hardcoded URLs (altijd via .env)
- Geen viewport queries voor cards, alleen container queries
- Niet handmatig package.json aanpassen
- Geen `<button>` in een `<a>` plaatsen

## ✅ EXACT fetchApi.ts (slides)
```ts
import qs from 'qs';

export default async function fetchApi<T>(
  endpoint: string,
  query?: Record<string, string>,
  wrappedByKey?: string,
  wrappedByList = true 
): Promise<T> {
  if (endpoint.startsWith('/')) endpoint = endpoint.slice(1);
  const url = new URL(`${import.meta.env.STRAPI_URL}/api/${endpoint}`);
  if (query) {
    Object.entries(query).forEach(([key, value]) => {
      url.searchParams.append(key, value);
    });
  }
  url.searchParams.append('populate', '*');
  const res = await fetch(url.toString());
  let data = await res.json();
  if (wrappedByKey) data = data[wrappedByKey];
  if (wrappedByList) data = data.data;
  return data as T;
}
```

## ✅ Responsive Book Card Template
```astro
<div class="book-card">
  <img 
    src={responsiveImage(book.attributes.image)}
    sizes="auto"
    loading="lazy"
    alt={book.attributes.title}
  />
  <h2>{book.attributes.title}</h2>
</div>
```

## ✅ Exacte CSS
```css
.book-card { container-type: inline-size; }
@container (min-width: 30em) { img { width: 100%; } }
```

## ✅ Project Structuur
- 1 repository met:
  - `backend/` (Strapi)
  - `frontend/` (Astro)

## ✅ Git Commands
- Werk in een **feature branch**
- Gebruik **descriptive commits**

## ✅ Deployment
- `npm run build`
- Deploy de `dist` folder naar **GitHub Pages** of **Vercel**

## ✅ Emergency Fixes
- 404? → Check `populate=*` in je fetchApi
- Images niet zichtbaar? → Check `attributes.url`

---

# EXAMEN MANTRA
> import unstraction → getBooks() → responsiveImage() → WIN
> Geen fetch boilerplate meer. Puur frontend focus. Deze file = 80% van je examen code. Copy → paste → style → deploy. 🎯
