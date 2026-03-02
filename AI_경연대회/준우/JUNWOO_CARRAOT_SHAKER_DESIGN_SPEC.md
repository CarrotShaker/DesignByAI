# 📱 당근마켓 클론 - 디자인 & 개발 명세서

> Android Compose 클론코딩 학습을 위한 전체 명세서

---

## 목차

1. [Design System](#1-design-system)
2. [화면 구조 Overview](#2-화면-구조-overview)
3. [Bottom Navigation Bar](#3-bottom-navigation-bar)
4. [홈 화면 (HomeScreen)](#4-홈-화면-homescreen)
5. [상품 상세 화면 (ProductDetailScreen)](#5-상품-상세-화면-productdetailscreen)
6. [좋아요 인터랙션](#6-좋아요-인터랙션-like-interaction)
7. [Bottom Sheet](#7-bottom-sheet)
8. [Dialog](#8-dialog)
9. [컴포넌트 체크리스트](#9-컴포넌트-체크리스트)
10. [프로젝트 구조](#10-프로젝트-구조)
11. [AI 프롬프트 템플릿](#11-ai-프롬프트-템플릿)
12. [추가 UX 고려사항](#12-추가-ux-고려사항)

---

## 1. Design System

### 1.1 Color Token

| Token Name | Hex | 용도 |
|------------|-----|------|
| `primary` | `#FF6F0F` | 당근 오렌지, CTA, 강조 |
| `primaryVariant` | `#E55A00` | Pressed 상태 |
| `background` | `#FFFFFF` | 앱 배경 |
| `surface` | `#FFFFFF` | 카드, 시트 배경 |
| `surfaceVariant` | `#F8F8F8` | 구분 영역 배경 |
| `onPrimary` | `#FFFFFF` | Primary 위 텍스트 |
| `onBackground` | `#212121` | 기본 텍스트 |
| `onSurface` | `#212121` | Surface 위 텍스트 |
| `onSurfaceVariant` | `#8C8C8C` | 보조 텍스트, 비활성 아이콘 |
| `divider` | `#F0F0F0` | 구분선 |
| `border` | `#E5E5E5` | 테두리 |
| `error` | `#F44336` | 에러, 삭제 |
| `scrim` | `#000000` (50%) | 딤 배경 |

### 1.2 Typography Token

| Token Name | Size | Weight | Line Height | 용도 |
|------------|------|--------|-------------|------|
| `headlineLarge` | 24sp | Bold | 32sp | 화면 제목 |
| `headlineMedium` | 20sp | Bold | 28sp | 섹션 제목 |
| `titleLarge` | 18sp | Bold | 24sp | 다이얼로그 제목 |
| `titleMedium` | 16sp | SemiBold | 22sp | 상품명, 강조 텍스트 |
| `bodyLarge` | 16sp | Normal | 24sp | 본문 |
| `bodyMedium` | 14sp | Normal | 20sp | 설명 텍스트 |
| `labelLarge` | 16sp | SemiBold | 20sp | 버튼 텍스트 |
| `labelMedium` | 14sp | Medium | 18sp | 태그, 라벨 |
| `caption` | 12sp | Normal | 16sp | 부가 정보 |

### 1.3 Spacing & Sizing

```
Base Unit: 4dp

Spacing Scale:
  xs:   4dp
  sm:   8dp
  md:   12dp
  lg:   16dp
  xl:   20dp
  xxl:  24dp
  xxxl: 32dp

Border Radius:
  small:  4dp
  medium: 8dp
  large:  12dp
  xlarge: 16dp

Icon Size:
  small:  16dp
  medium: 20dp
  large:  24dp
  xlarge: 32dp

Touch Target: 최소 48dp
```

---

## 2. 화면 구조 Overview

```
┌─────────────────────────────────┐
│         TopAppBar               │
├─────────────────────────────────┤
│                                 │
│         Content Area            │
│      (Screen별 콘텐츠)           │
│                                 │
│                   ┌───────────┐ │
│                   │ + 글쓰기  │ │  ← FAB (홈 화면)
│                   └───────────┘ │
├─────────────────────────────────┤
│       BottomNavigationBar       │
└─────────────────────────────────┘
```

---

## 3. Bottom Navigation Bar

### 3.1 구조

| Index | Label | Icon | 구현 범위 |
|-------|-------|------|----------|
| 0 | 홈 | home | ✅ 전체 구현 |
| 1 | 동네생활 | article | ⬜ 빈 화면 |
| 2 | 내 근처 | location | ⬜ 빈 화면 |
| 3 | 채팅 | chat | ⬜ 빈 화면 |
| 4 | 나의 당근 | person | ⬜ 빈 화면 |

### 3.2 디자인 스펙

```
Height: 56dp (+ Safe Area)
Background: surface (#FFFFFF)
Border Top: 1dp, #E5E5E5

Item:
  - Icon: 24dp
  - Label: 10sp
  - Spacing (icon-label): 4dp

Selected:
  - Icon Color: primary (#FF6F0F)
  - Label Color: primary (#FF6F0F)
  - Font Weight: Bold

Unselected:
  - Icon Color: onSurfaceVariant (#8C8C8C)
  - Label Color: onSurfaceVariant (#8C8C8C)
  - Font Weight: Normal
```

### 3.3 인터랙션

- 탭 전환: 즉시 전환 (애니메이션 없음)
- 현재 탭 재클릭: 스크롤 맨 위로 이동
- Badge: 채팅 탭에 읽지 않은 메시지 수 표시 가능

---

## 4. 홈 화면 (HomeScreen)

### 4.1 TopAppBar

```
┌─────────────────────────────────────────┐
│  [역삼동 ▼]                    🔔   ☰   │
└─────────────────────────────────────────┘

Height: 56dp
Left: 동네 선택 (Dropdown)
Right: 알림, 메뉴 아이콘
Background: surface (#FFFFFF)
```

### 4.2 콘텐츠 구조 (Vertical 중심 + 가끔 Horizontal)

```
┌─────────────────────────────────────────┐
│  🔍 검색창                               │
├─────────────────────────────────────────┤
│  ┌─────────┐                            │
│  │  Image  │  에어팟 프로 2세대          │  ← Product
│  │         │  역삼동 · 3분 전            │
│  │         │  180,000원            ♡ 2  │
│  └─────────┘                            │
├─────────────────────────────────────────┤
│  ┌─────────┐                            │
│  │  Image  │  자전거 팝니다              │  ← Product
│  │         │  삼성동 · 10분 전           │
│  │         │  50,000원             ♡ 5  │
│  └─────────┘                            │
├─────────────────────────────────────────┤
│  ┌─────────┐                            │
│  │  Image  │  아이폰 14 케이스           │  ← Product
│  │         │  청담동 · 1시간 전          │
│  │         │  15,000원             ♡ 0  │
│  └─────────┘                            │
├─────────────────────────────────────────┤
│  ┌─────────┐                            │
│  │  Image  │  무선 키보드                │  ← Product
│  │         │  ...                       │
│  └─────────┘                            │
├─────────────────────────────────────────┤
│  ┌─────────┐                            │
│  │  Image  │  원피스 새상품              │  ← Product
│  │         │  ...                       │
│  └─────────┘                            │
├─────────────────────────────────────────┤
│                                         │
│  ⭐ 인기 카테고리                더보기 > │  ← Section Header
│ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐    │
│ │디지털│ │가구  │ │의류  │ │도서  │ ──▶ │  ← Horizontal
│ └──────┘ └──────┘ └──────┘ └──────┘    │
│                                         │
├─────────────────────────────────────────┤
│  ┌─────────┐                            │
│  │  Image  │  닌텐도 스위치              │  ← Product
│  │         │  ...                       │
│  └─────────┘                            │
├─────────────────────────────────────────┤
│  ┌─────────┐                            │
│  │  Image  │  소파 급처                  │  ← Product
│  └─────────┘                            │
├─────────────────────────────────────────┤
│           ... 상품 계속 ...              │
├─────────────────────────────────────────┤
│                                         │
│  🏷️ 이 카테고리는 어때요?        더보기 > │  ← Section Header
│ ┌──────┐ ┌──────┐ ┌──────┐             │
│ │ 추천1 │ │ 추천2 │ │ 추천3 │ ──▶        │  ← Horizontal
│ └──────┘ └──────┘ └──────┘             │
│                                         │
├─────────────────────────────────────────┤
│  ┌─────────┐                            │
│  │  Image  │  겨울 패딩                  │  ← Product
│  └─────────┘                            │
│           ... 계속 ...                   │
│                                         │
│                   ┌─────────────┐       │
│                   │  + 글쓰기   │       │  ← FAB (우하단 고정)
│                   └─────────────┘       │
└─────────────────────────────────────────┘
         ↕ 전체 Vertical Scroll
```

### 4.3 데이터 모델

```kotlin
sealed class HomeFeedItem {
    
    data class Product(
        val id: String,
        val title: String,
        val price: Int,
        val location: String,
        val timeAgo: String,
        val imageUrl: String,
        val likeCount: Int,
        val isLiked: Boolean = false
    ) : HomeFeedItem()
    
    data class HorizontalSection(
        val type: HorizontalSectionType,
        val title: String,
        val titleIcon: ImageVector? = null,
        val showMoreButton: Boolean = true,
        val items: List<HorizontalItem>
    ) : HomeFeedItem()
}

enum class HorizontalSectionType {
    CATEGORY,           // 인기 카테고리
    RECOMMENDATION      // 이 카테고리는 어때요?
}

data class HorizontalItem(
    val id: String,
    val title: String,
    val iconUrl: String? = null,
    val imageUrl: String? = null
)
```

### 4.4 피드 구성 비율

| 아이템 타입 | 비율 | 설명 |
|------------|------|------|
| Product | ~90% | 메인 콘텐츠 |
| HorizontalSection | ~10% | 5~10개 상품마다 1번 정도 |

### 4.5 Product List Item 스펙

```
┌─────────────────────────────────────────────────┐
│ ┌─────────┐                                     │
│ │  Image  │  상품명 (최대 2줄)                    │
│ │         │  동네 · 시간                         │
│ │ 100x100 │  가격                          ♡ 3  │
│ └─────────┘                                     │
└─────────────────────────────────────────────────┘

Container:
  - Padding: 16dp (horizontal), 12dp (vertical)
  - Background: surface (#FFFFFF)

Image:
  - Size: 100 x 100dp
  - Radius: medium (8dp)
  - Placeholder: surfaceVariant

Title:
  - Typography: titleMedium (16sp, SemiBold)
  - Color: onSurface (#212121)
  - MaxLines: 2
  - Overflow: Ellipsis

Location · Time:
  - Typography: caption (12sp)
  - Color: onSurfaceVariant (#8C8C8C)
  - Format: "역삼동 · 3분 전"

Price:
  - Typography: titleMedium (16sp, Bold)
  - Color: onSurface (#212121)
  - Format: 천 단위 콤마

Like:
  - Icon: 16dp
  - Color: onSurfaceVariant (#8C8C8C)
  - Count: caption (12sp)
  - Position: 우측 하단

Divider:
  - Height: 1dp
  - Color: divider (#F0F0F0)
  - Margin: horizontal 16dp
```

#### 4.5.1 터치 피드백 (Pressed State)

리스트 아이템을 탭했을 때의 시각적 피드백 정의:

```
Ripple Effect:
  - Color: #000000 (Black)
  - Opacity: 10%
  - Type: Bounded (아이템 영역 내)

또는 Background 변경 방식:
  - Default: surface (#FFFFFF)
  - Pressed: surfaceVariant (#F8F8F8)
  - Duration: 즉시 적용, 150ms fade out

Compose 구현:
  Modifier.clickable(
      interactionSource = remember { MutableInteractionSource() },
      indication = rememberRipple(bounded = true, color = Color.Black.copy(alpha = 0.1f))
  ) { ... }
```

#### 4.5.2 텍스트 안전 영역 (Text Safe Area)

긴 텍스트가 우측 요소를 침범하지 않도록 보장:

```
┌─────────────────────────────────────────────────┐
│ ┌─────────┐                                     │
│ │  Image  │  [────── Text Area ──────]  [Like]  │
│ │         │                             ← 16dp →│
│ │ 100x100 │  ↑ 최소 16dp 간격 보장              │
│ └─────────┘                                     │
└─────────────────────────────────────────────────┘

규칙:
  - 좋아요 영역(♡ + Count)은 고정 너비로 우측 배치
  - 좋아요 영역 Margin Start: 최소 16dp 확보
  - 텍스트 영역: 남은 공간에서 flex (weight = 1)
  - 텍스트가 길어지면 Ellipsis 처리, 절대 좋아요 영역 침범 금지

Layout 구조:
  Row {
      Column(modifier = Modifier.weight(1f)) {  // 텍스트 영역
          Text(title, maxLines = 2, overflow = Ellipsis)
          Text(locationTime)
          Text(price)
      }
      Spacer(modifier = Modifier.width(16.dp))  // 최소 간격 보장
      LikeBadge(count)  // 고정 너비
  }

이점:
  - 작은 화면(360dp 이하)에서도 UI 깨짐 방지
  - 긴 상품명/지역명에서도 레이아웃 안정성 확보
```

### 4.6 Section Header 스펙

```
┌─────────────────────────────────────────────────┐
│  ⭐ 인기 카테고리                      더보기 > │
└─────────────────────────────────────────────────┘

Container:
  - Padding: horizontal 16dp
  - Margin Bottom: 12dp
  - Layout: Row (SpaceBetween, CenterVertically)

Left (Title Area):
  - Icon: 20dp (optional)
  - Icon Margin End: 8dp
  - Text: titleMedium (16sp, SemiBold)
  - Color: onSurface (#212121)

Right (더보기 버튼):
  - Text: "더보기"
  - Typography: bodyMedium (14sp)
  - Color: onSurfaceVariant (#8C8C8C)
  - Chevron Icon: navigate_next, 16dp
  - Icon Margin Start: 4dp
  - Touch Area: 최소 48dp height
  - Clickable: true (Ripple)
```

### 4.7 Horizontal Section 스펙

```
┌─────────────────────────────────────────────────┐
│  ⭐ 인기 카테고리                      더보기 > │  ← Section Header
│ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐    │
│ │  Icon  │ │  Icon  │ │  Icon  │ │  Icon  │ ▶  │
│ │ 디지털 │ │ 가구   │ │ 의류   │ │ 도서   │    │
│ └────────┘ └────────┘ └────────┘ └────────┘    │
└─────────────────────────────────────────────────┘

Section Container:
  - Padding Top: 16dp
  - Padding Bottom: 12dp
  - Background: surfaceVariant (#F8F8F8)

LazyRow:
  - Content Padding: horizontal 16dp
  - Item Spacing: 12dp
  - Scroll: Free scroll (no snap)

Category Item:
  - Width: 72dp
  - Icon Container: 48dp, circular, surface (#FFFFFF)
  - Icon: 24dp
  - Label: caption (12sp), center
  - Spacing (icon-label): 8dp
```

### 4.8 FAB (Floating Action Button) 스펙

```
┌─────────────────┐
│  ✏️ + 글쓰기    │
└─────────────────┘

Position:
  - Anchor: 우하단 (End, Bottom)
  - Margin End: 16dp
  - Margin Bottom: 16dp (BottomNav 위)

Extended FAB:
  - Height: 56dp
  - Padding Horizontal: 20dp
  - Background: primary (#FF6F0F)
  - Radius: 16dp (또는 full round 28dp)
  - Elevation: 6dp

Icon:
  - Icon: add (plus)
  - Size: 24dp
  - Color: onPrimary (#FFFFFF)
  - Margin End: 8dp

Text:
  - Text: "글쓰기"
  - Typography: labelLarge (16sp, SemiBold)
  - Color: onPrimary (#FFFFFF)

States:
  - Default: primary (#FF6F0F)
  - Pressed: primaryVariant (#E55A00)
  - Elevation Pressed: 12dp
```

#### FAB 인터랙션

- 클릭: 글쓰기 화면으로 이동
- 스크롤 반응 (선택사항):
  - 아래로 스크롤: FAB 축소 (아이콘만) 또는 숨김
  - 위로 스크롤: FAB 확장 (아이콘 + 텍스트)
  - Animation: 300ms, EaseInOut
- 그림자: 스크롤 시에도 항상 위에 떠있음

#### Compose 구현 힌트

```kotlin
Scaffold(
    floatingActionButton = {
        ExtendedFloatingActionButton(
            onClick = { /* navigate to write */ },
            icon = {
                Icon(
                    imageVector = Icons.Default.Add,
                    contentDescription = "글쓰기"
                )
            },
            text = { Text("글쓰기") },
            containerColor = Primary,
            contentColor = OnPrimary,
            expanded = isExpanded  // 스크롤 상태에 따라
        )
    },
    floatingActionButtonPosition = FabPosition.End
) { ... }
```

### 4.9 스크롤 UX

- 전체: LazyColumn (수직 스크롤)
- HorizontalSection 내부: LazyRow (수평 스크롤)
- Pull to Refresh: 지원 (SwipeRefresh)
- Infinite Scroll: 하단 도달 시 추가 로딩
- 스크롤 위치: 탭 전환 후 복귀 시 유지

---

## 5. 상품 상세 화면 (ProductDetailScreen)

### 5.1 전체 구조

```
┌─────────────────────────────────────────┐
│  ←                              ⋮  공유  │  ← TopBar (투명/스크롤시 변경)
├─────────────────────────────────────────┤
│ ┌─────────────────────────────────────┐ │
│ │                                     │ │
│ │         Product Images              │ │  ← HorizontalPager
│ │           (1:1 비율)                 │ │
│ │                                     │ │
│ │                    ● ○ ○ ○          │ │  ← Indicator
│ └─────────────────────────────────────┘ │
├─────────────────────────────────────────┤
│  ┌────┐                                 │
│  │ 👤 │  닉네임                          │  ← 판매자 프로필
│  └────┘  역삼동 · 매너온도 36.5°         │
├─────────────────────────────────────────┤
│                                         │
│  상품 제목                              │  ← 18sp, Bold
│  카테고리 · 3시간 전                     │  ← 14sp, Gray
│                                         │
│  상품 설명 텍스트가 여기에 표시됩니다.     │  ← 16sp
│  여러 줄로 작성될 수 있습니다.            │
│  ...                                    │
│                                         │
├─────────────────────────────────────────┤
│  조회 32 · 관심 5 · 채팅 2               │  ← 12sp, Gray
├─────────────────────────────────────────┤
│                                         │
│  이 판매자의 다른 상품            더보기 >│  ← Section Header
│ ┌────────┐ ┌────────┐ ┌────────┐       │
│ │  상품  │ │  상품  │ │  상품  │  ────▶ │  ← Horizontal Scroll
│ └────────┘ └────────┘ └────────┘       │
│                                         │
└─────────────────────────────────────────┘
         ↕ Scroll

┌─────────────────────────────────────────┐
│  ♡    │   ₩150,000        [ 채팅하기 ] │  ← Fixed Bottom Bar
└─────────────────────────────────────────┘
```

### 5.2 이미지 영역

```
Aspect Ratio: 1:1
Pager: HorizontalPager
Indicator:
  - Position: 하단 중앙
  - Active: primary (#FF6F0F)
  - Inactive: #DDDDDD, 50% opacity
  - Size: 6dp (active 8dp)
  - Spacing: 6dp
```

### 5.3 Bottom Action Bar

```
┌───────────────────────────────────────────────────┐
│  ♡(♥)   │         ₩150,000        [  채팅하기  ] │
└───────────────────────────────────────────────────┘

Height: 64dp (+ Safe Area)
Background: surface (#FFFFFF)
Border Top: 1dp, border (#E5E5E5)
Padding: horizontal 16dp

Like Button (Left):
  - Touch Area: 48 x 48dp
  - Icon: 24dp
  - Unliked: outline, onSurfaceVariant (#8C8C8C)
  - Liked: filled, primary (#FF6F0F)

Divider:
  - Width: 1dp
  - Height: 32dp
  - Color: border (#E5E5E5)
  - Margin: horizontal 16dp

Price:
  - Typography: titleLarge (18sp, Bold)
  - Flex: 1 (남은 공간 차지)
  - "가격 제안 가능" 라벨: caption (12sp), onSurfaceVariant

CTA Button (Right):
  - Width: 100dp
  - Height: 44dp
  - Background: primary (#FF6F0F)
  - Text: labelLarge (16sp, Bold), onPrimary (#FFFFFF)
  - Radius: medium (8dp)
```

---

## 6. 좋아요 인터랙션 (Like Interaction)

### 6.1 상태 정의

```kotlin
enum class LikeState {
    UNLIKED,    // 빈 하트
    LIKED       // 채워진 하트
}
```

### 6.2 비주얼 스펙

```
Unliked:
  - Icon: heart_outline
  - Color: onSurfaceVariant (#8C8C8C)

Liked:
  - Icon: heart_filled
  - Color: primary (#FF6F0F)
```

### 6.3 애니메이션 스펙

```
UNLIKED → LIKED:
  1. Scale: 1.0 → 1.3 → 1.0
  2. Duration: 300ms
  3. Easing: Spring (DampingRatioMediumBouncy)
  4. Color: Gray → Orange (simultaneous)

LIKED → UNLIKED:
  1. Scale: 1.0 → 0.8 → 1.0
  2. Duration: 200ms
  3. Color: Orange → Gray

Haptic Feedback: LightImpact (optional)
```

### 6.4 Compose 구현 힌트

```kotlin
@Composable
fun LikeButton(
    isLiked: Boolean,
    onToggle: () -> Unit,
    modifier: Modifier = Modifier
) {
    var isAnimating by remember { mutableStateOf(false) }
    
    val scale by animateFloatAsState(
        targetValue = if (isAnimating) 1.3f else 1f,
        animationSpec = spring(
            dampingRatio = Spring.DampingRatioMediumBouncy,
            stiffness = Spring.StiffnessLow
        ),
        finishedListener = { isAnimating = false }
    )

    val tint by animateColorAsState(
        targetValue = if (isLiked) Primary else OnSurfaceVariant,
        animationSpec = tween(200)
    )

    IconButton(
        onClick = {
            isAnimating = true
            onToggle()
        },
        modifier = modifier
    ) {
        Icon(
            imageVector = if (isLiked) Icons.Filled.Favorite 
                          else Icons.Outlined.FavoriteBorder,
            contentDescription = if (isLiked) "좋아요 취소" else "좋아요",
            tint = tint,
            modifier = Modifier.scale(scale)
        )
    }
}
```

---

## 7. Bottom Sheet

### 7.1 사용 케이스

- 상품 메뉴 (신고, 숨기기, 공유)
- 정렬 옵션 선택
- 동네 범위 설정

### 7.2 디자인 스펙

```
┌─────────────────────────────────────────┐
│              ━━━━ (Handle)              │
├─────────────────────────────────────────┤
│                                         │
│   🚨  이 게시글 신고하기                 │
│                                         │
├ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┤
│                                         │
│   🙈  이 게시글 숨기기                   │
│                                         │
├ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┤
│                                         │
│   📤  공유하기                          │
│                                         │
└─────────────────────────────────────────┘

Sheet Container:
  - Background: surface (#FFFFFF)
  - Top Radius: xlarge (16dp)
  - Max Height: 화면의 90%

Drag Handle:
  - Width: 40dp
  - Height: 4dp
  - Radius: 2dp
  - Color: #DDDDDD
  - Margin Top: 12dp
  - Margin Bottom: 8dp

Menu Item:
  - Height: 56dp
  - Padding Horizontal: 20dp
  - Icon: 24dp, onSurfaceVariant
  - Icon Margin End: 16dp
  - Text: bodyLarge (16sp), onSurface
  - Ripple: true

Divider:
  - Height: 1dp
  - Color: divider (#F0F0F0)
  - Margin Horizontal: 20dp

Scrim:
  - Color: scrim (#000000, 50%)
```

### 7.3 인터랙션

```
열기:
  - Trigger: 버튼 클릭
  - Animation: Slide up from bottom
  - Duration: 300ms
  - Easing: EaseOut

닫기:
  - Scrim 영역 탭
  - 아래로 스와이프 (threshold: 50%)
  - 메뉴 아이템 선택
  - Back 버튼/제스처

드래그:
  - Handle 및 Sheet 전체 영역
  - Velocity threshold for dismiss
```

### 7.4 Compose 구현 힌트

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun ProductMenuBottomSheet(
    isVisible: Boolean,
    onDismiss: () -> Unit,
    onReport: () -> Unit,
    onHide: () -> Unit,
    onShare: () -> Unit
) {
    val sheetState = rememberModalBottomSheetState()
    
    if (isVisible) {
        ModalBottomSheet(
            onDismissRequest = onDismiss,
            sheetState = sheetState,
            shape = RoundedCornerShape(topStart = 16.dp, topEnd = 16.dp),
            dragHandle = { BottomSheetDefaults.DragHandle() }
        ) {
            Column {
                BottomSheetMenuItem(
                    icon = Icons.Default.Report,
                    text = "이 게시글 신고하기",
                    onClick = {
                        onReport()
                        onDismiss()
                    }
                )
                // ... more items
            }
        }
    }
}
```

---

## 8. Dialog

### 8.1 사용 케이스

- 삭제 확인 (Destructive)
- 로그아웃 확인
- 신고 완료 알림 (Single Button)
- 네트워크 에러

### 8.2 Confirm Dialog 스펙

```
        ┌─────────────────────────────┐
        │                             │
        │      정말 삭제할까요?         │
        │                             │
        │  삭제한 게시글은 복구할 수     │
        │  없어요.                     │
        │                             │
        ├──────────────┬──────────────┤
        │    취소      │     삭제      │
        └──────────────┴──────────────┘

Container:
  - Width: 280dp (또는 화면의 80%)
  - Background: surface (#FFFFFF)
  - Radius: xlarge (16dp)
  - Elevation: 24dp

Content:
  - Padding: 24dp (top, horizontal)
  - Padding Bottom: 0

Title:
  - Typography: titleLarge (18sp, Bold)
  - Color: onSurface (#212121)
  - Alignment: Center
  - Margin Bottom: 12dp

Body:
  - Typography: bodyMedium (14sp)
  - Color: onSurfaceVariant (#666666)
  - Alignment: Center
  - Margin Bottom: 24dp

Actions Container:
  - Height: 52dp
  - Border Top: 1dp, divider (#E5E5E5)
  - Layout: Row (equal width)

Cancel Button:
  - Typography: labelLarge (16sp)
  - Color: onSurfaceVariant (#666666)
  - Background: Transparent
  - Border Right: 1dp, divider

Confirm Button:
  - Typography: labelLarge (16sp, SemiBold)
  - Color: primary (#FF6F0F) 또는 error (#F44336) for destructive
  - Background: Transparent

Scrim:
  - Color: scrim (#000000, 50%)
```

### 8.3 인터랙션

```
열기:
  - Animation: Scale (0.9 → 1.0) + Fade In
  - Duration: 200ms
  - Easing: EaseOut

닫기:
  - 버튼 클릭
  - Scrim 탭 (dismissOnClickOutside = true인 경우)
  - Back 버튼/제스처

접근성:
  - 열릴 때 Title에 포커스
  - 버튼 간 Tab 이동 가능
```

### 8.4 Compose 구현 힌트

```kotlin
@Composable
fun KarrotConfirmDialog(
    title: String,
    message: String,
    confirmText: String,
    onConfirm: () -> Unit,
    onDismiss: () -> Unit,
    isDestructive: Boolean = false
) {
    Dialog(onDismissRequest = onDismiss) {
        Surface(
            shape = RoundedCornerShape(16.dp),
            color = MaterialTheme.colorScheme.surface,
            tonalElevation = 24.dp
        ) {
            Column(
                horizontalAlignment = Alignment.CenterHorizontally
            ) {
                // Title
                Text(
                    text = title,
                    style = MaterialTheme.typography.titleLarge,
                    modifier = Modifier.padding(top = 24.dp)
                )
                
                // Message
                Text(
                    text = message,
                    style = MaterialTheme.typography.bodyMedium,
                    color = MaterialTheme.colorScheme.onSurfaceVariant,
                    textAlign = TextAlign.Center,
                    modifier = Modifier.padding(
                        horizontal = 24.dp,
                        vertical = 12.dp
                    )
                )
                
                Divider()
                
                // Buttons
                Row(
                    modifier = Modifier.height(52.dp)
                ) {
                    TextButton(
                        onClick = onDismiss,
                        modifier = Modifier.weight(1f)
                    ) {
                        Text("취소")
                    }
                    
                    VerticalDivider()
                    
                    TextButton(
                        onClick = onConfirm,
                        modifier = Modifier.weight(1f)
                    ) {
                        Text(
                            text = confirmText,
                            color = if (isDestructive) 
                                MaterialTheme.colorScheme.error 
                            else 
                                MaterialTheme.colorScheme.primary
                        )
                    }
                }
            }
        }
    }
}
```

---

## 9. 컴포넌트 체크리스트

### 9.1 구현 필요 컴포넌트

| 컴포넌트 | 우선순위 | 상태 |
|---------|---------|------|
| KarrotTheme (Color, Typography, Shape) | P0 | ⬜ |
| BottomNavigationBar | P0 | ⬜ |
| TopAppBar (Home) | P0 | ⬜ |
| ProductListItem | P0 | ⬜ |
| SectionHeader (더보기 포함) | P0 | ⬜ |
| HorizontalSectionView | P0 | ⬜ |
| CategoryChip | P1 | ⬜ |
| FAB (글쓰기) | P0 | ⬜ |
| ProductDetailScreen | P1 | ⬜ |
| TopAppBar (Detail) | P1 | ⬜ |
| ImagePager with Indicator | P1 | ⬜ |
| SellerProfileCard | P1 | ⬜ |
| BottomActionBar | P1 | ⬜ |
| LikeButton (with Animation) | P1 | ⬜ |
| BottomSheet | P1 | ⬜ |
| ConfirmDialog | P1 | ⬜ |
| EmptyScreen (탭별 빈 화면) | P2 | ⬜ |

### 9.2 컴포넌트별 체크포인트

각 컴포넌트 구현 시 확인:

- [ ] 상태 정의 (State)
- [ ] 파라미터 정의 (Props/Parameters)
- [ ] Preview 작성 (@Preview)
- [ ] 접근성 (contentDescription)
- [ ] 다크모드 대응 (선택)
- [ ] 인터랙션/애니메이션

---

## 10. 프로젝트 구조

```
📁 carrot-clone/
├── 📁 docs/
│   ├── DESIGN_SPEC.md              ← 이 문서
│   ├── prompts/
│   │   ├── prompt-home.md
│   │   └── prompt-detail.md
│   └── screenshots/
│
├── 📁 app/src/main/java/.../
│   ├── ui/
│   │   ├── theme/
│   │   │   ├── Color.kt
│   │   │   ├── Type.kt
│   │   │   ├── Shape.kt
│   │   │   └── Theme.kt
│   │   │
│   │   ├── components/
│   │   │   ├── BottomNavBar.kt
│   │   │   ├── ProductListItem.kt
│   │   │   ├── SectionHeader.kt
│   │   │   ├── HorizontalSection.kt
│   │   │   ├── LikeButton.kt
│   │   │   ├── KarrotFAB.kt
│   │   │   ├── BottomSheet.kt
│   │   │   └── ConfirmDialog.kt
│   │   │
│   │   └── screens/
│   │       ├── home/
│   │       │   ├── HomeScreen.kt
│   │       │   └── HomeViewModel.kt
│   │       ├── detail/
│   │       │   ├── DetailScreen.kt
│   │       │   └── DetailViewModel.kt
│   │       └── empty/
│   │           └── EmptyScreen.kt
│   │
│   ├── data/
│   │   └── model/
│   │       ├── Product.kt
│   │       └── HomeFeedItem.kt
│   │
│   └── navigation/
│       └── NavGraph.kt
│
└── README.md
```

---

## 11. AI 프롬프트 템플릿

### 11.1 Design Token 추출용

```
당근마켓 앱의 UI를 분석해서 Design System Token을 정의해줘.

출력 형식:
1. Color Palette (Hex + 용도)
2. Typography Scale (size, weight, lineHeight)
3. Spacing System
4. Border Radius
5. Elevation/Shadow

Jetpack Compose의 MaterialTheme 형태로 Kotlin 코드도 제공해줘.
```

### 11.2 컴포넌트 구현용

```
[컴포넌트명]을 Jetpack Compose로 구현해줘.

요구사항:
- Material3 기반
- 아래 Design Token 사용: [토큰 정보]
- 상태: [필요한 상태들]

산출물:
1. 컴포넌트 구조 설명
2. data class (필요시)
3. Composable 함수 코드
4. Preview 코드
```

### 11.3 화면 구현용

```
당근마켓의 [화면명]을 Jetpack Compose로 클론코딩해줘.

요구사항:
- Material3 + 커스텀 Theme
- ViewModel + StateFlow
- LazyColumn/LazyRow 활용
- 명세서 기반 구현: [명세 첨부]

산출물:
1. Screen Composable
2. ViewModel
3. State/Event 정의
4. Preview
```

---

## 12. 추가 UX 고려사항

### 12.1 로딩 상태

- Skeleton UI: 리스트 초기 로딩
- Shimmer: 이미지 로딩
- CircularProgress: 액션 처리 중
- SwipeRefresh Indicator: Pull to Refresh

### 12.2 에러/빈 상태

- Empty State: 일러스트 + 메시지 + CTA
- Error State: 메시지 + 재시도 버튼
- Network Error: 오프라인 안내

### 12.3 접근성

- contentDescription: 모든 아이콘, 이미지
- 터치 영역: 최소 48dp
- 색상 대비: 4.5:1 이상
- 스크린 리더 순서 고려
- 포커스 관리 (다이얼로그, 바텀시트)

---

## 📝 버전 히스토리

| 버전 | 날짜 | 변경 내용 |
|-----|------|----------|
| 1.0 | - | 초기 버전 작성 |

---

> **Note**: 이 명세서는 Android Compose 클론코딩 학습을 위한 참고 자료입니다.
> 실제 당근마켓 앱과 세부 사항이 다를 수 있습니다.
