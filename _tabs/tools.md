---
layout: page
icon: fas fa-tools
order: 2
---

<div class="container-fluid px-3 px-lg-5">
  <div class="row">
    <div class="col-12">
      <div class="mb-4">
        <h1 class="display-4 fw-bold text-center mb-3">🛠️ 工具箱</h1>
        <p class="lead text-center text-muted">实用的在线工具集合，让您的工作更加高效便捷</p>
      </div>
    </div>
  </div>
  
  <div class="row g-4">
    <!-- 彩票选号助手 -->
    <div class="col-12 col-md-6 col-lg-4">
      <div class="card h-100 shadow-sm border-0 tool-card">
        <div class="card-body d-flex flex-column">
          <div class="text-center mb-3">
            <div class="tool-icon mb-3">
              <i class="fas fa-dice fa-3x text-primary"></i>
            </div>
            <h5 class="card-title fw-bold">彩票选号助手</h5>
            <p class="card-text text-muted">智能生成彩票号码，支持双色球、大乐透等多种彩票类型，让您的选号更加科学合理。</p>
          </div>
          <div class="mt-auto">
            <div class="d-grid">
              <a href="/lottery-generator/" class="btn btn-primary btn-lg">
                <i class="fas fa-external-link-alt me-2"></i>立即使用
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Markdown预览器 -->
    <div class="col-12 col-md-6 col-lg-4">
      <div class="card h-100 shadow-sm border-0 tool-card">
        <div class="card-body d-flex flex-column">
          <div class="text-center mb-3">
            <div class="tool-icon mb-3">
              <i class="fab fa-markdown fa-3x text-success"></i>
            </div>
            <h5 class="card-title fw-bold">Markdown预览器</h5>
            <p class="card-text text-muted">实时预览Markdown文档，支持语法高亮、表格、代码块等丰富格式，写作更加便捷。</p>
          </div>
          <div class="mt-auto">
            <div class="d-grid">
              <a href="/markdown-preview/" class="btn btn-success btn-lg">
                <i class="fas fa-external-link-alt me-2"></i>立即使用
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 颜色转换工具 -->
    <div class="col-12 col-md-6 col-lg-4">
      <div class="card h-100 shadow-sm border-0 tool-card">
        <div class="card-body d-flex flex-column">
          <div class="text-center mb-3">
            <div class="tool-icon mb-3">
              <i class="fas fa-palette fa-3x text-info"></i>
            </div>
            <h5 class="card-title fw-bold">颜色转换工具</h5>
            <p class="card-text text-muted">RGB与十六进制颜色码相互转换，支持透明度调节，提供完整的透明度对应表。</p>
          </div>
          <div class="mt-auto">
            <div class="d-grid">
              <a href="/color-converter/" class="btn btn-info btn-lg">
                <i class="fas fa-external-link-alt me-2"></i>立即使用
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 占位卡片 - 为未来工具预留 -->
    <div class="col-12 col-md-6 col-lg-4">
      <div class="card h-100 shadow-sm border-0 tool-card coming-soon">
        <div class="card-body d-flex flex-column">
          <div class="text-center mb-3">
            <div class="tool-icon mb-3">
              <i class="fas fa-plus-circle fa-3x text-secondary"></i>
            </div>
            <h5 class="card-title fw-bold text-muted">更多工具</h5>
            <p class="card-text text-muted">更多实用工具正在开发中，敬请期待...</p>
          </div>
          <div class="mt-auto">
            <div class="d-grid">
              <button class="btn btn-outline-secondary btn-lg" disabled>
                <i class="fas fa-clock me-2"></i>敬请期待
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
/* 使用更高特异性的选择器和!important来确保样式优先级 */
.container-fluid .row .col-12 .card.tool-card,
.container-fluid .card.tool-card {
  transition: all 0.3s ease !important;
  border-radius: 15px !important;
  overflow: hidden !important;
  background-color: #ffffff !important;
  border: 1px solid #dee2e6 !important;
}

.container-fluid .row .col-12 .card.tool-card:hover,
.container-fluid .card.tool-card:hover {
  transform: translateY(-5px) !important;
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15) !important;
  background-color: #f8f9fa !important;
}

.container-fluid .tool-icon {
  transition: transform 0.3s ease !important;
}

.container-fluid .card.tool-card:hover .tool-icon {
  transform: scale(1.1) !important;
}

.container-fluid .card.tool-card.coming-soon {
  opacity: 0.7 !important;
}

.container-fluid .card.tool-card.coming-soon:hover {
  transform: none !important;
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15) !important;
}

/* 按钮容器布局优化 */
.container-fluid .card.tool-card .mt-auto {
  margin-top: auto !important;
  padding: 0 1rem 1rem 1rem !important; /* 左右和底部间距 */
}

.container-fluid .card.tool-card .d-grid {
  margin: 0 !important; /* 移除默认margin */
}

/* 按钮样式强化 */
.container-fluid .card.tool-card .btn {
  border-radius: 8px !important;
  font-weight: 600 !important;
  font-size: 1rem !important;
  padding: 0.75rem 1.5rem !important;
  transition: all 0.3s ease !important;
  border: none !important;
  text-decoration: none !important;
  width: 100% !important; /* 确保按钮拉伸到容器宽度 */
}

.container-fluid .card.tool-card .btn:hover {
  transform: translateY(-2px) !important;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2) !important;
  text-decoration: none !important;
}

/* 主要按钮样式 */
.container-fluid .card.tool-card .btn-primary {
  background-color: #0d6efd !important;
  color: #ffffff !important;
}

.container-fluid .card.tool-card .btn-primary:hover {
  background-color: #0b5ed7 !important;
  color: #ffffff !important;
}

/* 成功按钮样式 */
.container-fluid .card.tool-card .btn-success {
  background-color: #198754 !important;
  color: #ffffff !important;
}

.container-fluid .card.tool-card .btn-success:hover {
  background-color: #157347 !important;
  color: #ffffff !important;
}

/* 信息按钮样式 */
.container-fluid .card.tool-card .btn-info {
  background-color: #0dcaf0 !important;
  color: #000000 !important;
}

.container-fluid .card.tool-card .btn-info:hover {
  background-color: #31d2f2 !important;
  color: #000000 !important;
}

/* 次要按钮样式（敬请期待） */
.container-fluid .card.tool-card .btn-outline-secondary {
  background-color: transparent !important;
  border: 2px solid #6c757d !important;
  color: #6c757d !important;
}

.container-fluid .card.tool-card .btn-outline-secondary:hover {
  background-color: #6c757d !important;
  color: #ffffff !important;
}

.container-fluid .card.tool-card .btn-outline-secondary:disabled {
  background-color: transparent !important;
  border-color: #6c757d !important;
  color: #6c757d !important;
  opacity: 0.65 !important;
  cursor: not-allowed !important;
}

.container-fluid .card.tool-card .btn-outline-secondary:disabled:hover {
  transform: none !important;
  box-shadow: none !important;
  background-color: transparent !important;
  border-color: #6c757d !important;
  color: #6c757d !important;
}

/* 图标颜色强化 */
.container-fluid .card.tool-card .text-primary {
  color: #0d6efd !important;
}

.container-fluid .card.tool-card .text-success {
  color: #198754 !important;
}

.container-fluid .card.tool-card .text-info {
  color: #0dcaf0 !important;
}

.container-fluid .card.tool-card .text-secondary {
  color: #6c757d !important;
}

/* 深色模式支持 - 使用更高特异性 */
@media (prefers-color-scheme: dark) {
  .container-fluid .row .col-12 .card.tool-card,
  .container-fluid .card.tool-card {
    background-color: #1e1e1e !important;
    border-color: #495057 !important;
    color: #ffffff !important;
  }
  
  .container-fluid .row .col-12 .card.tool-card:hover,
  .container-fluid .card.tool-card:hover {
    background-color: #2d3748 !important;
    box-shadow: 0 0.5rem 1rem rgba(255, 255, 255, 0.1) !important;
  }
  
  .container-fluid .card.tool-card .text-muted {
    color: #adb5bd !important;
  }
  
  /* 深色模式下的按钮样式 */
  .container-fluid .card.tool-card .btn-info {
    background-color: #0dcaf0 !important;
    color: #000000 !important;
  }
  
  .container-fluid .card.tool-card .btn-info:hover {
    background-color: #31d2f2 !important;
    color: #000000 !important;
  }
  
  .container-fluid .card.tool-card .btn-outline-secondary {
    border-color: #adb5bd !important;
    color: #adb5bd !important;
  }
  
  .container-fluid .card.tool-card .btn-outline-secondary:hover {
    background-color: #adb5bd !important;
    color: #000000 !important;
  }
  
  .container-fluid .card.tool-card .btn-outline-secondary:disabled {
    border-color: #6c757d !important;
    color: #6c757d !important;
  }
}

[data-bs-theme="dark"] .container-fluid .row .col-12 .card.tool-card,
[data-bs-theme="dark"] .container-fluid .card.tool-card {
  background-color: #1e1e1e !important;
  border-color: #495057 !important;
  color: #ffffff !important;
}

[data-bs-theme="dark"] .container-fluid .row .col-12 .card.tool-card:hover,
[data-bs-theme="dark"] .container-fluid .card.tool-card:hover {
  background-color: #2d3748 !important;
  box-shadow: 0 0.5rem 1rem rgba(255, 255, 255, 0.1) !important;
}

[data-bs-theme="dark"] .container-fluid .card.tool-card .text-muted {
  color: #adb5bd !important;
}

/* data-bs-theme="dark" 下的按钮样式 */
[data-bs-theme="dark"] .container-fluid .card.tool-card .btn-info {
  background-color: #0dcaf0 !important;
  color: #000000 !important;
}

[data-bs-theme="dark"] .container-fluid .card.tool-card .btn-info:hover {
  background-color: #31d2f2 !important;
  color: #000000 !important;
}

[data-bs-theme="dark"] .container-fluid .card.tool-card .btn-outline-secondary {
  border-color: #adb5bd !important;
  color: #adb5bd !important;
}

[data-bs-theme="dark"] .container-fluid .card.tool-card .btn-outline-secondary:hover {
  background-color: #adb5bd !important;
  color: #000000 !important;
}

[data-bs-theme="dark"] .container-fluid .card.tool-card .btn-outline-secondary:disabled {
  border-color: #6c757d !important;
  color: #6c757d !important;
}

/* 响应式优化 */
@media (max-width: 768px) {
  .container-fluid .display-4 {
    font-size: 2rem !important;
  }
  
  .container-fluid .card.tool-card {
    margin-bottom: 1rem !important;
  }
}

@media (max-width: 576px) {
  .container-fluid {
    padding-left: 1rem !important;
    padding-right: 1rem !important;
  }
  
  .container-fluid .tool-icon i {
    font-size: 2rem !important;
  }
}
</style>