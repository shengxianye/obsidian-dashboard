# React 18 新特性总结

## 并发特性 (Concurrent Features)
- `useTransition` — 标记非紧急更新，避免阻塞 UI
- `useDeferredValue` — 延迟渲染低优先级内容
- `Suspense` 支持 SSR 流式渲染

## Automatic Batching
React 18 自动合并多个 setState，减少不必要的重渲染：
```js
// 以前只在事件处理器中 batch，现在 setTimeout/Promise 中也会
setTimeout(() => {
  setCount(c => c + 1);
  setFlag(f => !f);
  // 只触发一次重渲染
}, 1000);
```

## 迁移注意
- `ReactDOM.render` → `ReactDOM.createRoot`
- StrictMode 双重渲染行为变化
