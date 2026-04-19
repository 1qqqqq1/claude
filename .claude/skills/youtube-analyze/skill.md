---
name: youtube-analyze
description: "Fetches real data from YouTube Data API v3 and analyzes the channel's latest video. Provides metrics dashboard, strengths, weaknesses, comment sentiment, overall rating, and actionable recommendations. Use for: 'проанализируй последний видос', 'analyze my latest video', 'как прошёл последний ролик', 'stats on my channel', 'покажи статистику', 'сильные и слабые стороны видео'."
---

# YouTube Video Analyzer

Fetches live data from YouTube Data API v3 and delivers a full analysis of the channel's latest video.

## Config

- **Channel ID**: `UC7KczP-AJ3ztnFD9h1LWKow`
- **API Key**: read from env var `YOUTUBE_API_KEY`

## Step-by-Step Execution

### Step 1 — Channel Stats
```
GET https://www.googleapis.com/youtube/v3/channels?part=statistics,snippet&id=UC7KczP-AJ3ztnFD9h1LWKow&key={YOUTUBE_API_KEY}
```
Extract: `subscriberCount`, `viewCount`, `videoCount`, `title`

### Step 2 — Latest Video
```
GET https://www.googleapis.com/youtube/v3/search?part=snippet&channelId=UC7KczP-AJ3ztnFD9h1LWKow&maxResults=1&order=date&type=video&key={YOUTUBE_API_KEY}
```
Extract: `videoId`, `title`, `publishedAt`, `description`

### Step 3 — Video Statistics
```
GET https://www.googleapis.com/youtube/v3/videos?part=statistics,snippet,contentDetails&id={videoId}&key={YOUTUBE_API_KEY}
```
Extract: `viewCount`, `likeCount`, `commentCount`, `duration`, `tags`

### Step 4 — Top Comments
```
GET https://www.googleapis.com/youtube/v3/commentThreads?part=snippet&videoId={videoId}&maxResults=20&order=relevance&key={YOUTUBE_API_KEY}
```
Extract: top 20 comments with text and like count.

### Step 5 — Analysis Report

Using all fetched data, produce the report in this exact structure:

---

## 📊 Метрики

| Показатель | Значение | Оценка |
|-----------|---------|--------|
| Просмотры | {viewCount} | vs avg канала |
| Лайки | {likeCount} | — |
| Комментарии | {commentCount} | — |
| Длительность | {duration} | оптимально? |
| Like Rate | {likes/views}% | >2% = хорошо |
| Engagement Rate | {(likes+comments)/views}% | >3% = хорошо |
| Подписчики канала | {subscriberCount} | — |
| Avg views/видео | {totalViews/videoCount} | — |

## ✅ Сильные стороны
- Аргументированный список, основанный на метриках и комментариях

## ⚠️ Слабые стороны
- Аргументированный список с объяснением что именно проседает

## 💬 Настроение комментариев
- Краткое саммари топ-комментариев, повторяющиеся темы, тональность

## ⭐ Итоговая оценка: X/10
Разбивка по критериям: охват, вовлечённость, контент, удержание (если доступно).

## 🚀 Рекомендации
3-5 конкретных действий для улучшения следующего видео.

---

## Benchmarks (YouTube Shorts, малые каналы <10k подписчиков)

| Метрика | Слабо | Норма | Хорошо | Отлично |
|--------|-------|-------|--------|---------|
| Like Rate | <1% | 1-2% | 2-5% | >5% |
| Engagement Rate | <2% | 2-4% | 4-7% | >7% |
| Comments/Views | <0.1% | 0.1-0.5% | 0.5-1% | >1% |
| Views vs subscribers | <10% | 10-50% | 50-100% | >100% |
