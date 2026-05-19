# StreamGuide - مجمع الأفلام والمسلسلات

## 1. Concept & Vision

**StreamGuide** هو موقع عربي لعرض ومراجعة الأفلام والمسلسلات مع توجيه المشاهد للمصادر القانونية للمشاهدة. يشبه موقع JustWatch أو Rotten Tomatoes - يقدم:
- كتالوج شامل للأفلام والمسلسلات
- تقييمات ومراجعات
- توجيه للمستخدم للمصدر الأصلي (Netflix, Disney+, etc.)
- نظام اشتراكات للمحتوى المتميز

**الشعور**: موقع سينمائي أنيق مع ألوان داكنة سينمائية وتأثيرات بصرية جذابة.

## 2. Design Language

### Aesthetic Direction
- **ستايل**: Dark Cinema Theme - خلفية داكنة عميقة مع لمسات ذهبية
- **إلهام**: Netflix meets Rotten Tomatoes with Arabic elegance

### Color Palette
```
Primary:      #E50914 (Cinema Red)
Secondary:    #F5C518 (IMDb Gold)
Accent:       #FFD700 (Golden highlight)
Background:   #0D0D0D (Deep Black)
Surface:      #1A1A1A (Card Dark)
Surface-2:    #262626 (Hover State)
Text:         #FFFFFF (White)
Text-muted:   #A0A0A0 (Gray)
Success:      #22C55E (Available)
```

### Typography
```
Arabic: 'Noto Sans Arabic', 'Tajawal', sans-serif
English: 'Inter', 'Poppins', sans-serif
Display: 'Cairo', bold for headings
```

### Spatial System
- Base unit: 4px
- Section padding: 64px vertical
- Card gaps: 24px
- Border radius: 12px for cards, 8px for buttons

### Motion Philosophy
- Cards: Scale 1.02 + shadow lift on hover (200ms ease-out)
- Page transitions: Fade in (300ms)
- Modal: Slide up + fade (250ms)
- Skeleton loading for content

## 3. Layout & Structure

### Header (Fixed)
- Logo "StreamGuide" مع أيقونة فلم
- Navigation: الرئيسية، أفلام، مسلسلات، جديد
- Search bar (expandable)
- Login/Register buttons
- Subscription badge

### Hero Section
- Featured movie/series with large backdrop
- Gradient overlay with title, rating, description
- "Watch on [Source]" CTA button
- Auto-rotate between top picks

### Content Sections
- **Trending Now**: أفلام/مسلسلات trending
- **New Releases**: أحدث الإصدارات
- **Top Rated**: الأعلى تقييماً
- **Coming Soon**: قريباً
- **By Genre**: تصنيفات (أكشن، كوميدي، دراما، رعب، إلخ)

### Movie/Series Detail Page
- Large poster + backdrop
- Title, year, duration, genre
- IMDb + Rotten Tomatoes ratings
- Plot summary
- Cast & Crew
- **"Where to Watch" section** with source logos (Netflix, Disney+, etc.)
- Similar movies
- User reviews

### Search Results Page
- Real-time search
- Filters: نوع، سنة، تقييم، مصدر
- Grid layout with posters

### Subscription Page
- Plans: مجاني، شهري، سنوي
- Features comparison
- Payment integration ready

## 4. Features & Interactions

### Core Features
1. **عرض الكتالوج**: أفلام ومسلسلات مع صور وتقييمات
2. **نظام البحث**: بحث فوري مع اقتراحات
3. **صفحة التفاصيل**: معلومات كاملة + مصادر المشاهدة
4. **التصنيفات**: فلترة حسب النوع، السنة، التقييم
5. **نظام الاشتراكات**: خطط مجانية ومدفوعة
6. **المفضلة**: حفظ في قائمة المفضلة (localStorage)

### Interactions
- **Hover on card**: Scale up, show quick info overlay
- **Click on card**: Navigate to detail page
- **Watch button**: External link to streaming source
- **Search**: Debounced search with dropdown results
- **Filter**: Instant filtering with URL params

### Error States
- No results: "لم يتم العثور على نتائج"
- Loading: Skeleton cards
- Source unavailable: "غير متاح حالياً"

## 5. Component Inventory

### MovieCard
- Poster image (lazy loaded)
- Title + Year overlay
- Rating badge (top-right)
- Hover: scale + info panel
- States: default, hover, loading (skeleton)

### HeroSlider
- Full-width backdrop
- Gradient overlay
- Title, rating, description
- CTA button

### SearchBar
- Input with search icon
- Dropdown with live results
- Recent searches
- States: empty, typing, results, no-results

### RatingBadge
- IMDb style (yellow)
- Stars or number display
- Platform indicator

### SourceButton
- Source logo (Netflix, Disney+, etc.)
- Price if applicable
- "Watch Now" text

### SubscriptionCard
- Plan name
- Price
- Features list
- CTA button
- Popular badge (for recommended)

### GenreTag
- Colored pill
- Icon optional
- Clickable for filtering

## 6. Technical Approach

### Stack
- React 18 + TypeScript
- Vite for build
- Tailwind CSS for styling
- React Router for navigation
- Lucide React icons
- Local data (mock JSON) for content

### Data Model
```typescript
interface Movie {
  id: string;
  title: string;
  titleEn: string;
  year: number;
  type: 'movie' | 'series';
  poster: string;
  backdrop: string;
  rating: number;
  imdbRating: number;
  rottenTomatoes?: number;
  duration: string;
  genres: string[];
  plot: string;
  cast: string[];
  director: string;
  sources: StreamSource[];
  releaseDate: string;
  isNew?: boolean;
  isTrending?: boolean;
}

interface StreamSource {
  name: string; // Netflix, Disney+, etc.
  logo: string;
  url: string;
  price?: string;
  available: boolean;
}

interface SubscriptionPlan {
  id: string;
  name: string;
  price: number;
  period: 'month' | 'year';
  features: string[];
  recommended?: boolean;
}
```

### Routing
```
/                 - Home (Hero + Sections)
/movies           - Movies catalog
/series           - Series catalog
/search?q=        - Search results
/movie/:id        - Movie detail
/series/:id       - Series detail
/subscription     - Subscription plans
/favorites        - User favorites
```

### State Management
- React Context for global state (favorites, user)
- LocalStorage for persistence
- URL params for filters
