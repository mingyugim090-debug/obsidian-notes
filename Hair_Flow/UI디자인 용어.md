
# AI 헤어 스타일링 서비스 레퍼런스 가이드

  

## 1. 프론트엔드 UI 용어 & 디자인 패턴

  

### 1-1. 디자인 시스템 용어

  

| 용어 | 설명 | 활용 |
| **Glassmorphism** | 반투명 유리 효과 (blur + 투명도) | 고급스러운 카드, 모달, 오버레이 |

| **Neumorphism** | 부드러운 돌출/함몰 효과 | 버튼, 입력 필드 |

| **Design Token** | 색상·폰트·간격 등 디자인 변수 | CSS 변수(`--primary-500`), Tailwind extend |

| **Luxury Aesthetic** | 골드/크림/차콜 배색 + 세리프 폰트 | 프리미엄 SaaS, 뷰티 서비스 |

| **Micro-interaction** | 호버, 클릭 시 미세한 피드백 애니메이션 | `whileHover`, `whileTap` (Framer Motion) |

| **Skeleton UI** | 콘텐츠 로딩 중 회색 플레이스홀더 | API 응답 대기 상태 |

| **Progressive Disclosure** | 정보를 단계적으로 표시 | AI 분석 → 추천 → 레시피 워크플로우 |

  

### 1-2. Luxury Salon 테마 CSS 클래스 패턴

  

```css

/* 글래스모피즘 카드 */

.glass-luxury {

background: rgba(247, 243, 239, 0.7); /* --cream + 투명도 */

backdrop-filter: blur(12px);

-webkit-backdrop-filter: blur(12px);

border: 1px solid rgba(212, 179, 127, 0.2); /* --gold 테두리 */

}

  

/* 다크 모드 글래스모피즘 */

.glass-luxury-dark {

background: rgba(44, 41, 38, 0.8); /* --charcoal */

backdrop-filter: blur(12px);

border: 1px solid rgba(212, 179, 127, 0.15);

}

  

/* 골드 그림자 */

.shadow-luxury {

box-shadow: 0 4px 24px rgba(212, 179, 127, 0.15),

0 1px 4px rgba(61, 43, 31, 0.1);

}

  

/* 호버 시 골드 글로우 */

.hover-gold-glow:hover {

box-shadow: 0 0 0 2px rgba(212, 179, 127, 0.4),

0 4px 24px rgba(212, 179, 127, 0.2);

}

.hover-gold-glow::before {

pointer-events: none; /* ⚠️ 클릭 이벤트 막히는 버그 방지 필수 */

}

  

/* 섹션 라벨 (대문자 + 넓은 자간) */

.section-label {

font-family: 'Noto Sans KR', sans-serif;

font-size: 11px;

font-weight: 500;

letter-spacing: 4px;

text-transform: uppercase;

color: var(--warm-brown);

}

  

/* 세리프 제목 폰트 */

.font-heading {

font-family: 'Cormorant Garamond', serif;

font-weight: 300;

letter-spacing: 1px;

}

```

  

### 1-3. 색상 팔레트 (Luxury Salon)

  

```css

:root {

--cream: #F7F3EF; /* 라이트 배경 */

--charcoal: #2C2926; /* 다크 배경, 메인 텍스트 */

--gold: #D4B37F; /* 강조색, CTA, 아이콘 */

--gold-light: #E5CC9E; /* 하이라이트, 호버 */

--warm-brown: #9A7E6D; /* 서브 텍스트, 보더 */

--soft-beige: #EDE4DA; /* 카드 배경 */

--deep-brown: #3D2B1F; /* 다크 강조 */

}

```

  

### 1-4. Framer Motion 애니메이션 패턴

  

```tsx

// 페이지 진입 애니메이션

const pageVariants = {

initial: { opacity: 0, y: 20 },

animate: { opacity: 1, y: 0 },

exit: { opacity: 0, y: -20 }

};

  

// 카드 호버 효과

<motion.div whileHover={{ scale: 1.02, y: -2 }} whileTap={{ scale: 0.98 }}>

  

// 스태거 리스트 (아이템 순차 등장)

const containerVariants = {

animate: { transition: { staggerChildren: 0.1 } }

};

const itemVariants = {

initial: { opacity: 0, x: -20 },

animate: { opacity: 1, x: 0 }

};

  

// 이미지 로딩 shimmer

<motion.div

animate={{ backgroundPosition: ['200% 0', '-200% 0'] }}

transition={{ duration: 1.5, repeat: Infinity }}

className="bg-gradient-to-r from-soft-beige via-cream to-soft-beige bg-[length:200%_100%]"

/>

```

  

### 1-5. 이미지 업로드 UX 패턴

  

```tsx

// 5면 사진 업로드 레이아웃 (앞/뒤/좌/우/윗)

const VIEWS = [

{ id: 'front', label: '앞', icon: '👤', required: true },

{ id: 'back', label: '뒤', icon: '🔄', required: true },

{ id: 'left', label: '좌', icon: '◁', required: false },

{ id: 'right', label: '우', icon: '▷', required: false },

{ id: 'top', label: '위', icon: '↑', required: false },

];

  

// 모바일 카메라/앨범 선택

<input

type="file"

accept="image/*"

capture="user" // 전면 카메라

// capture="environment" // 후면 카메라

// capture 속성 없음 // 앨범 선택

/>

```

  

### 1-6. 모바일 최적화 체크리스트

  

- 터치 타겟 최소 `44px × 44px`

- `-webkit-tap-highlight-color: transparent` (모바일 탭 하이라이트 제거)

- 폰트 크기 최소 `16px` (iOS 자동 확대 방지)

- `safe-area-inset` 처리 (`padding-bottom: env(safe-area-inset-bottom)`)

- 이미지는 `next/image` + `sizes` 속성으로 반응형 최적화

  

---

  

## 2. AI 이미지 생성 모델 선택 가이드

  

### 핵심 원칙


> **기존 사진을 편집하고 싶다 → Kontext Pro (image-to-image)**

> **새 이미지를 얼굴 참조로 생성하고 싶다 → IP-Adapter / InstantID**

> **텍스트만으로 생성하고 싶다 → Flux Pro / SDXL**

  
### 2-2. 용도별 추천 모델 플로우

  

```

[헤어스타일 변경 시뮬레이션]

고객 사진 있음 → fal-ai/flux-pro/kontext (guidance_scale: 4)

고객 사진 없음 → fal-ai/flux-pro/v1.1 (text-to-image 폴백)

  

[완전 새로운 스타일 포트폴리오]

참조 얼굴 → fal-ai/instantid (새 배경, 새 조명)

```

  

## 6. 서비스 아키텍처 패턴


### 6-1. AI 분석 워크플로우


```

[Step 1] 사진 업로드

└── Supabase Storage 저장 (버킷: customer-photos)

└── 서명된 URL 생성 (5분 유효, Fal.ai 접근용)

  

[Step 2] 이미지 분석 (GPT-4o Vision)

└── 이미지 → Base64 또는 URL로 Vision API 전달

└── 분석 결과 → consultations.five_view_analysis (JSONB)

  

[Step 3] 스타일 추천 (GPT-4o)

└── five_view_analysis → 스타일 3-5개 추천

└── 각 스타일마다 imagePrompt 생성

└── Kontext Pro로 병렬 이미지 생성 (Promise.all)

└── 생성 이미지 → Supabase Storage 캐싱

  

[Step 4] 레시피 생성 (GPT-4o)

└── 선택한 스타일 + 고객 상태 → 단계별 시술 레시피



```

  

### 6-2. Fal.ai CDN 프록시 패턴

  
```typescript

// Supabase Storage URL → Fal CDN URL 변환 (필수)

// Fal.ai는 Supabase 서명된 URL에 직접 접근 불가

async function uploadToFalCdn(imageUrl: string): Promise<string> {

const response = await fetch(imageUrl);

const blob = await response.blob();

const file = new File([blob], 'input.jpg', { type: 'image/jpeg' });

return await fal.storage.upload(file); // Fal CDN URL 반환

}

```

  

### 6-3. 이미지 영구 저장 패턴

  
```typescript

// Fal.ai 임시 URL → Supabase Storage 영구 저장

// Fal 생성 이미지 URL은 만료됨 → 즉시 캐싱 필요

async function cacheGeneratedImage(

supabase: SupabaseClient,

temporaryUrl: string,

storagePath: string // 예: 'ai-generated/style/customer-id-timestamp.jpg'

): Promise<string> {

const response = await fetch(temporaryUrl);

const blob = await response.blob();

const { data } = await supabase.storage

.from('customer-photos')

.upload(storagePath, blob, { contentType: 'image/jpeg' });

return supabase.storage.from('customer-photos').getPublicUrl(data.path).data.publicUrl;

}

```

  
