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
.tool-card {
  transition: all 0.3s ease;
  border-radius: 15px;
  overflow: hidden;
  background-color: #ffffff;
  border: 1px solid #dee2e6;
}

.tool-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
  background-color: #f8f9fa;
}

.tool-icon {
  transition: transform 0.3s ease;
}

.tool-card:hover .tool-icon {
  transform: scale(1.1);
}

.coming-soon {
  opacity: 0.7;
}

.coming-soon:hover {
  transform: none;
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
}

/* 深色模式支持 */
@media (prefers-color-scheme: dark) {
  .tool-card {
    background-color: #1e1e1e;
    border-color: #495057;
    color: #ffffff;
  }
  
  .tool-card:hover {
    background-color: #2d3748;
    box-shadow: 0 0.5rem 1rem rgba(255, 255, 255, 0.1);
  }
  
  .text-muted {
    color: #adb5bd !important;
  }
}

[data-bs-theme="dark"] .tool-card {
  background-color: #1e1e1e;
  border-color: #495057;
  color: #ffffff;
}

[data-bs-theme="dark"] .tool-card:hover {
  background-color: #2d3748;
  box-shadow: 0 0.5rem 1rem rgba(255, 255, 255, 0.1);
}

[data-bs-theme="dark"] .text-muted {
  color: #adb5bd !important;
}

/* 响应式优化 */
@media (max-width: 768px) {
  .display-4 {
    font-size: 2rem;
  }
  
  .tool-card {
    margin-bottom: 1rem;
  }
}

@media (max-width: 576px) {
  .container-fluid {
    padding-left: 1rem;
    padding-right: 1rem;
  }
  
  .tool-icon i {
    font-size: 2rem;
  }
}
</style>