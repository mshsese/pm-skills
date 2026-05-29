---
name: pm-shadcn
description: Next.js + Tailwind 프로젝트에서 프론트엔드 UI를 구현할 때 사용. shadcn/ui 컴포넌트를 우선 활용해 inline 스타일 없이 일관된 디자인으로 구현한다. 서일이앤씨 ERP, SNUH 인트라넷 등 기존 프로젝트에 UI 컴포넌트를 추가하거나 개선할 때 트리거.
---

# pm-shadcn — shadcn/ui 프론트엔드 구현 가이드

## 0. 핵심 원칙

- **inline style 금지** — `style={{ color: 'red' }}` 대신 shadcn 컴포넌트 + Tailwind 클래스 사용
- **shadcn 우선** — 버튼·카드·테이블·팝업 등 있는 컴포넌트는 직접 짜지 말고 shadcn 가져와서 씀
- **기술 스택 확인** — Next.js + TypeScript + Tailwind CSS 프로젝트에만 적용

---

## 1. 최초 세팅 (프로젝트에 처음 추가할 때)

프로젝트 폴더에서 아래 순서로 실행:

```bash
# 1단계: shadcn 초기화 (처음 한 번만)
npx shadcn@latest init
# 질문 나오면: Default → New York → Zinc → 엔터

# 2단계: 필요한 컴포넌트 추가
npx shadcn@latest add button card table badge select dialog toast
```

설치 후 `src/components/ui/` 폴더에 파일들이 생김. 이제 내 코드에서 바로 import해서 씀.

---

## 2. 자주 쓰는 컴포넌트 치트시트

### 버튼
```tsx
import { Button } from "@/components/ui/button"

<Button>기본 버튼</Button>
<Button variant="outline">테두리 버튼</Button>
<Button variant="destructive">삭제 버튼</Button>
<Button size="sm">작은 버튼</Button>
```

### 카드 (프로젝트 현황, 통계 카드 등)
```tsx
import { Card, CardHeader, CardTitle, CardContent } from "@/components/ui/card"

<Card>
  <CardHeader>
    <CardTitle>프로젝트 현황</CardTitle>
  </CardHeader>
  <CardContent>
    내용
  </CardContent>
</Card>
```

### 배지 (상태 표시: 진행중, 완료, 입찰중 등)
```tsx
import { Badge } from "@/components/ui/badge"

<Badge>진행중</Badge>
<Badge variant="outline">입찰중</Badge>
<Badge variant="secondary">완료</Badge>
```

### 셀렉트 (담당자 선택, 상태 변경)
```tsx
import { Select, SelectTrigger, SelectValue, SelectContent, SelectItem } from "@/components/ui/select"

<Select onValueChange={(v) => setStatus(v)}>
  <SelectTrigger>
    <SelectValue placeholder="상태 선택" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="진행중">진행중</SelectItem>
    <SelectItem value="완료">완료</SelectItem>
  </SelectContent>
</Select>
```

### 테이블 (나라장터 공고 목록, 프로젝트 목록)
```tsx
import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from "@/components/ui/table"

<Table>
  <TableHeader>
    <TableRow>
      <TableHead>공고명</TableHead>
      <TableHead>마감일</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    <TableRow>
      <TableCell>도로 보수 공사</TableCell>
      <TableCell>D-3</TableCell>
    </TableRow>
  </TableBody>
</Table>
```

### 다이얼로그 (팝업, 확인창)
```tsx
import { Dialog, DialogTrigger, DialogContent, DialogHeader, DialogTitle } from "@/components/ui/dialog"

<Dialog>
  <DialogTrigger asChild>
    <Button>상세 보기</Button>
  </DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>프로젝트 상세</DialogTitle>
    </DialogHeader>
    내용
  </DialogContent>
</Dialog>
```

### 토스트 (성공/실패 알림)
```tsx
import { useToast } from "@/hooks/use-toast"

const { toast } = useToast()

toast({ title: "등록 완료", description: "프로젝트가 생성되었습니다." })
toast({ title: "오류", variant: "destructive", description: "다시 시도해주세요." })
```

---

## 3. 기존 프로젝트에 적용할 때 순서

1. `npx shadcn@latest init` 실행 (이미 했으면 건너뜀)
2. 필요한 컴포넌트만 `npx shadcn@latest add [컴포넌트명]` 으로 추가
3. 기존 inline style → shadcn 컴포넌트로 교체
4. `npm run build`로 에러 없는지 확인

---

## 4. 현재 프로젝트별 적용 포인트

| 프로젝트 | 교체 우선순위 |
|---------|------------|
| 서일이앤씨 ERP | `ErpDemoWorkspace.tsx` inline 스타일 → Card, Badge, Button, Table |
| SNUH 인트라넷 | 동일 스택이므로 동일 방식 적용 가능 |

---

## 5. 컴포넌트 목록 전체 확인

```bash
npx shadcn@latest add --help
```

또는 shadcn/ui 공식 사이트에서 전체 컴포넌트 확인 가능.
