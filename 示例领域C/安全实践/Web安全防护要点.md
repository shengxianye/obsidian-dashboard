# Web 安全防护要点

## OWASP Top 10 重点关注

### 1. SQL 注入
- 使用参数化查询 / ORM
- 永远不拼接用户输入到 SQL

### 2. XSS（跨站脚本）
- 输出时 HTML 转义
- 使用 CSP（Content-Security-Policy）
- Cookie 设置 HttpOnly

### 3. CSRF（跨站请求伪造）
- 使用 CSRF Token
- SameSite Cookie 属性
- 关键操作二次确认

### 4. 认证与会话
- 密码使用 bcrypt/argon2 加密存储
- JWT 设置合理过期时间
- 敏感操作需 MFA

## 安全 Headers
```
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Strict-Transport-Security: max-age=31536000
Content-Security-Policy: default-src 'self'
```
