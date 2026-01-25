# 新婚礼邀请页面测试数据

## 快速填充测试数据

使用以下API接口填充测试数据（请将 `USER_ID` 替换为实际的userId）：

### 1. 设置通用配置（婚礼信息）
```bash
curl -X POST "http://localhost:8080/api/wedding-invitation/upsertCommonConfig?userId=USER_ID" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "张三 & 李四",
    "date": "2026年2月14日",
    "time": "12:00",
    "hotel": "某某大酒店",
    "detail": "我们诚挚邀请您参加我们的婚礼",
    "videoUrl": "https://your-qiniu-domain.com/music.mp3"
  }'
```

### 2. 添加封面照片
```bash
curl -X POST "http://localhost:8080/api/wedding-invitation/upsertResource?userId=USER_ID" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "wedding-cover",
    "url": "https://your-qiniu-domain.com/cover.jpg",
    "sort": 1
  }'
```

### 3. 添加爱情故事（3-5张）

#### 初遇
```bash
curl -X POST "http://localhost:8080/api/wedding-invitation/upsertResource?userId=USER_ID" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "love-story",
    "url": "https://your-qiniu-domain.com/first-meet.jpg",
    "date": "2018年5月20日",
    "desc": "初遇：在图书馆的第一次相遇，阳光正好，微风不燥",
    "sort": 1
  }'
```

#### 第一次约会
```bash
curl -X POST "http://localhost:8080/api/wedding-invitation/upsertResource?userId=USER_ID" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "love-story",
    "url": "https://your-qiniu-domain.com/first-date.jpg",
    "date": "2018年6月1日",
    "desc": "第一次约会：一起去看海，你说海风吹得很温柔",
    "sort": 2
  }'
```

#### 求婚瞬间
```bash
curl -X POST "http://localhost:8080/api/wedding-invitation/upsertResource?userId=USER_ID" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "love-story",
    "url": "https://your-qiniu-domain.com/proposal.jpg",
    "date": "2025年12月25日",
    "desc": "求婚：在圣诞节那天，我单膝跪地，你笑着点头",
    "sort": 3
  }'
```

### 4. 查询RSVP统计
```bash
curl "http://localhost:8080/api/wedding-invitation/getRsvpStats?userId=USER_ID"
```

### 5. 提交RSVP回执（测试）
```bash
curl -X POST "http://localhost:8080/api/wedding-invitation/submitRSVP?userId=USER_ID" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "测试用户",
    "peopleCount": 2,
    "attend": true,
    "message": "祝你们新婚快乐，百年好合！"
  }'
```

## 数据库字段说明

### resources 表新增字段
- `date` (varchar(50)): 日期，用于爱情故事
- `desc` (text): 描述，用于爱情故事

### presents 表新增字段
- `attend` (boolean): 是否出席，默认 true
- `message` (text): 祝福留言

## 注意事项

1. **图片资源**：建议将图片上传到七牛云或对象存储，然后使用URL
2. **图片尺寸**：
   - 封面照片：建议 800×800px 或以上
   - 爱情故事照片：建议 800×600px 或以上
3. **音乐格式**：支持 mp3, m4a 等格式
4. **userId**：确保所有操作使用相同的userId
