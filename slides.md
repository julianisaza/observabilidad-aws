---
theme: default
background: '#1e293b'
class: text-center
highlighter: shiki
lineNumbers: true
info: |
  ## Observabilidad en AWS
  Mentoría: Logs, Métricas y Trazas
drawings:
  persist: false
transition: slide-left
title: Observabilidad en AWS
mdc: true
hideInToc: true
---

<style>
.slidev-layout {
  overflow-y: auto;
  padding-bottom: 4rem;
}
#slidev-goto-dialog {
  display: none !important;
}
</style>

# <span class="text-white">Observabilidad en AWS</span>

<span class="text-white text-2xl">**Logs · Métricas · Trazas**</span>

<p class="text-slate-300 mt-4 text-lg">Una guía práctica para entender el monitoreo moderno<br>en sistemas distribuidos</p>

<div class="mt-12 text-slate-300 text-sm">Julian Isaza</div>

---
layout: center
background: '#1e293b'
---

# 🤔 Pregunta de apertura

<div class="mt-8 bg-slate-800 rounded-xl p-8 max-w-2xl mx-auto border border-slate-600">
  <p class="text-2xl text-white leading-relaxed">
    "¿Alguna vez tuviste un bug en producción un <span class="text-amber-400 font-semibold">viernes a las 6pm</span> que tardaste horas en encontrar porque no sabías qué estaba pasando dentro del sistema?"
  </p>
</div>

<p class="mt-8 text-slate-400 text-lg">Tómate un momento para pensar... 🙋</p>

---
layout: default
background: '#1e293b'
---

# La evolución del problema

<div class="grid grid-cols-3 gap-6 mt-8">

<div class="bg-slate-800 rounded-xl p-5 border border-slate-600">
  <div class="text-3xl mb-3">🏛️</div>
  <h3 class="text-slate-300 font-semibold mb-2">Antes — Monolito</h3>
  <p class="text-slate-400 text-sm leading-relaxed">Un solo proceso. Si fallaba, hacías <code class="text-emerald-400">tail -f app.log</code> y listo. El problema estaba ahí.</p>
  <div class="mt-3 text-xs text-emerald-400 font-medium">✓ Fácil de debuggear</div>
</div>

<div class="bg-slate-800 rounded-xl p-5 border border-amber-700">
  <div class="text-3xl mb-3">⚙️</div>
  <h3 class="text-slate-300 font-semibold mb-2">Transición — Multi-servicio</h3>
  <p class="text-slate-400 text-sm leading-relaxed">Varios procesos, varios logs. ¿En cuál buscas? ¿El error está en el frontend, el API, o la base de datos?</p>
  <div class="mt-3 text-xs text-amber-400 font-medium">⚠ Complejidad creciente</div>
</div>

<div class="bg-slate-800 rounded-xl p-5 border border-red-700">
  <div class="text-3xl mb-3">🌐</div>
  <h3 class="text-slate-300 font-semibold mb-2">Hoy — Microservicios</h3>
  <p class="text-slate-400 text-sm leading-relaxed">Un request toca <strong class="text-white">8 servicios en 3 regiones</strong>. Un fallo puede estar en cualquier punto del camino.</p>
  <div class="mt-3 text-xs text-red-400 font-medium">✗ Sin herramientas, estás ciego</div>
</div>

</div>

<div class="mt-8 bg-blue-950 rounded-lg p-4 border border-blue-700 text-center">
  <p class="text-blue-300 font-medium">→ Necesitamos una forma de <span class="text-white font-semibold">ver el interior del sistema desde afuera</span>, sin importar cuántos servicios haya</p>
</div>

---
layout: center
background: '#1e293b'
---

# ¿Qué es Observabilidad?

<div class="mt-6 bg-slate-800 rounded-xl p-8 max-w-2xl mx-auto border border-slate-600">
  <p class="text-xl text-white leading-relaxed text-center">
    La capacidad de <span class="text-emerald-400 font-semibold">entender el estado interno</span> de un sistema analizando únicamente sus <span class="text-emerald-400 font-semibold">salidas externas</span>
  </p>
</div>

<div class="grid grid-cols-2 gap-6 mt-8 max-w-2xl mx-auto">
  <div class="bg-slate-800 rounded-lg p-4 border border-slate-600">
    <div class="text-slate-400 text-xs font-medium uppercase tracking-wider mb-2">Origen</div>
    <p class="text-slate-300 text-sm">Control Theory (1960s) → adoptado por SRE y DevOps</p>
  </div>
  <div class="bg-slate-800 rounded-lg p-4 border border-slate-600">
    <div class="text-slate-400 text-xs font-medium uppercase tracking-wider mb-2">Clave</div>
    <p class="text-slate-300 text-sm">No necesitas acceso al código ni deploys de diagnóstico</p>
  </div>
</div>

---
layout: two-cols
background: '#1e293b'
---

# Monitoring

<div class="mr-6">

<div class="bg-slate-800 rounded-xl p-6 border border-amber-700 mt-6">
  <div class="text-4xl mb-4">🔔</div>
  <div class="text-amber-400 text-xs font-semibold uppercase tracking-wider mb-3">Reactivo</div>
  <h3 class="text-white font-semibold text-lg mb-3">Te dice <span class="text-amber-400">QUÉ</span> está mal</h3>
  <ul class="text-slate-400 text-sm space-y-2">
    <li>→ Alertas y umbrales</li>
    <li>→ Dashboards predefinidos</li>
    <li>→ Solo ves lo que ya decidiste medir</li>
  </ul>
</div>

</div>

::right::

# Observabilidad

<div class="ml-6">

<div class="bg-slate-800 rounded-xl p-6 border border-emerald-600 mt-6">
  <div class="text-4xl mb-4">🔍</div>
  <div class="text-emerald-400 text-xs font-semibold uppercase tracking-wider mb-3">Proactivo</div>
  <h3 class="text-white font-semibold text-lg mb-3">Te explica <span class="text-emerald-400">POR QUÉ</span> está mal</h3>
  <ul class="text-slate-400 text-sm space-y-2">
    <li>→ Contexto y causa raíz</li>
    <li>→ Preguntas arbitrarias</li>
    <li>→ Puedes explorar lo inesperado</li>
  </ul>
</div>

</div>

---
layout: default
background: '#1e293b'
---

# Monitoring vs Observabilidad — ejemplo real

<div class="bg-slate-800 rounded-xl p-6 border border-slate-600 mt-6">
  <p class="text-slate-400 text-sm font-medium uppercase tracking-wider mb-4">Escenario: CPU al 90% en producción</p>

  <div class="grid grid-cols-2 gap-8">
    <div>
      <div class="text-amber-400 font-semibold mb-3">🔔 Monitoring te dice:</div>
      <div class="bg-slate-900 rounded-lg p-4 font-mono text-sm text-red-400">
        ALERT: CPU usage > 90%<br>
        Service: orders-api<br>
        Time: 14:32 UTC
      </div>
      <p class="text-slate-400 text-sm mt-3">Sabes que hay un problema. No sabes nada más.</p>
    </div>
    <div>
      <div class="text-emerald-400 font-semibold mb-3">🔍 Observabilidad te responde:</div>
      <div class="bg-slate-900 rounded-lg p-4 text-sm space-y-1">
        <p class="text-slate-300">→ <span class="text-white font-medium">¿Qué endpoint?</span> <span class="text-emerald-400">/api/orders/bulk</span></p>
        <p class="text-slate-300">→ <span class="text-white font-medium">¿Qué query?</span> <span class="text-emerald-400">DynamoDB sin índice</span></p>
        <p class="text-slate-300">→ <span class="text-white font-medium">¿Qué usuario?</span> <span class="text-emerald-400">user_id: 4521</span></p>
        <p class="text-slate-300">→ <span class="text-white font-medium">¿Cuándo empezó?</span> <span class="text-emerald-400">Tras el deploy 14:28</span></p>
      </div>
    </div>
  </div>
</div>

---
layout: center
background: '#1e293b'
---

# Los 3 Pilares de Observabilidad

<div class="grid grid-cols-3 gap-6 mt-8">

<div class="bg-slate-800 rounded-xl p-6 border border-emerald-700 text-center">
  <div class="text-5xl mb-4">📊</div>
  <h3 class="text-emerald-400 font-semibold text-lg mb-2">Métricas</h3>
  <p class="text-slate-400 text-sm leading-relaxed">Valores numéricos en el tiempo</p>
  <div class="mt-4 bg-slate-900 rounded-lg p-3">
    <p class="text-emerald-300 text-xs font-semibold">¿Cuánto y con qué frecuencia?</p>
  </div>
  <div class="mt-3 text-xs text-slate-500">CloudWatch Metrics</div>
</div>

<div class="bg-slate-800 rounded-xl p-6 border border-purple-700 text-center">
  <div class="text-5xl mb-4">📝</div>
  <h3 class="text-purple-400 font-semibold text-lg mb-2">Logs</h3>
  <p class="text-slate-400 text-sm leading-relaxed">Registros detallados de eventos</p>
  <div class="mt-4 bg-slate-900 rounded-lg p-3">
    <p class="text-purple-300 text-xs font-semibold">¿Qué ocurrió exactamente?</p>
  </div>
  <div class="mt-3 text-xs text-slate-500">CloudWatch Logs</div>
</div>

<div class="bg-slate-800 rounded-xl p-6 border border-orange-700 text-center">
  <div class="text-5xl mb-4">🔍</div>
  <h3 class="text-orange-400 font-semibold text-lg mb-2">Trazas</h3>
  <p class="text-slate-400 text-sm leading-relaxed">Flujo del request entre servicios</p>
  <div class="mt-4 bg-slate-900 rounded-lg p-3">
    <p class="text-orange-300 text-xs font-semibold">¿Dónde en el flujo falló?</p>
  </div>
  <div class="mt-3 text-xs text-slate-500">AWS X-Ray</div>
</div>

</div>

---
layout: default
background: '#1e293b'
---

# Pilar 1 — Métricas 📊

<div class="grid grid-cols-2 gap-6 mt-6 pb-12">

<div>
  <p class="text-slate-400 text-sm leading-relaxed mb-4">Valores numéricos capturados a intervalos regulares. Ideales para alertas, dashboards y autoscaling. Bajo costo, poca profundidad de contexto.</p>

  <div class="space-y-2">
    <div class="bg-slate-800 rounded-lg px-4 py-2 flex items-center gap-3 border border-slate-700">
      <span class="text-emerald-400 text-xs font-mono">AWS/Lambda</span>
      <span class="text-slate-400 text-xs">Duration, Errors, Throttles</span>
    </div>
    <div class="bg-slate-800 rounded-lg px-4 py-2 flex items-center gap-3 border border-slate-700">
      <span class="text-emerald-400 text-xs font-mono">AWS/ECS</span>
      <span class="text-slate-400 text-xs">CPUUtilization, MemoryUtilization</span>
    </div>
    <div class="bg-slate-800 rounded-lg px-4 py-2 flex items-center gap-3 border border-slate-700">
      <span class="text-emerald-400 text-xs font-mono">AWS/RDS</span>
      <span class="text-slate-400 text-xs">DatabaseConnections, ReadLatency</span>
    </div>
  </div>

  <div class="mt-4 bg-amber-950 rounded-lg p-3 border border-amber-800">
    <p class="text-amber-300 text-xs">💡 <strong>Insight:</strong> Una métrica te dice que algo es lento, pero no por qué. Para eso necesitas logs o trazas.</p>
  </div>
</div>

<div>
  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">Ejemplo — CloudWatch Metric</div>
  <div class="bg-slate-900 rounded-xl p-4 font-mono text-xs border border-slate-700 leading-relaxed">
    <span class="text-slate-500">{</span><br>
    &nbsp;&nbsp;<span class="text-emerald-400">"MetricName"</span><span class="text-slate-400">: </span><span class="text-amber-300">"Duration"</span>,<br>
    &nbsp;&nbsp;<span class="text-emerald-400">"Namespace"</span><span class="text-slate-400">: </span><span class="text-amber-300">"AWS/Lambda"</span>,<br>
    &nbsp;&nbsp;<span class="text-emerald-400">"Dimensions"</span><span class="text-slate-400">: [{</span><br>
    &nbsp;&nbsp;&nbsp;&nbsp;<span class="text-emerald-400">"Name"</span><span class="text-slate-400">: </span><span class="text-amber-300">"FunctionName"</span>,<br>
    &nbsp;&nbsp;&nbsp;&nbsp;<span class="text-emerald-400">"Value"</span><span class="text-slate-400">: </span><span class="text-amber-300">"orders-api"</span><br>
    &nbsp;&nbsp;<span class="text-slate-400">}],</span><br>
    &nbsp;&nbsp;<span class="text-emerald-400">"Value"</span><span class="text-slate-400">: </span><span class="text-red-400">4250.5</span><span class="text-slate-500">, <span class="text-slate-600">// ms — ⚠ cerca del timeout</span></span><br>
    &nbsp;&nbsp;<span class="text-emerald-400">"Unit"</span><span class="text-slate-400">: </span><span class="text-amber-300">"Milliseconds"</span><br>
    <span class="text-slate-500">}</span>
  </div>

  <div class="mt-3 bg-slate-800 rounded-lg p-3 border border-slate-700">
    <p class="text-slate-400 text-xs font-medium mb-1">Servicios AWS</p>
    <div class="flex flex-wrap gap-2">
      <span class="bg-slate-700 text-slate-300 text-xs px-2 py-1 rounded">CloudWatch Metrics</span>
      <span class="bg-slate-700 text-slate-300 text-xs px-2 py-1 rounded">CloudWatch Alarms</span>
      <span class="bg-slate-700 text-slate-300 text-xs px-2 py-1 rounded">Container Insights</span>
    </div>
  </div>
</div>

</div>

---
layout: default
background: '#1e293b'
---

# Pilar 2 — Logs 📝

<div class="grid grid-cols-2 gap-6 mt-6 pb-12">

<div>
  <p class="text-slate-400 text-sm leading-relaxed mb-4">Registros detallados de eventos discretos. La diferencia entre logs <strong class="text-white">no estructurados</strong> y <strong class="text-white">estructurados (JSON)</strong> es crítica para poder consultarlos.</p>

  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">Query — CloudWatch Log Insights</div>
  <div class="bg-slate-900 rounded-xl p-4 font-mono text-xs border border-purple-900 leading-relaxed">
    <span class="text-slate-500"># Endpoints más lentos — últimas 3h</span><br>
    <span class="text-purple-400">fields</span> endpoint, duration_ms<br>
    <span class="text-purple-400">| filter</span> level = <span class="text-amber-300">"ERROR"</span><br>
    <span class="text-purple-400">| stats</span> avg(duration_ms) <span class="text-purple-400">as</span> avg_ms,<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;count(*) <span class="text-purple-400">as</span> errors <span class="text-purple-400">by</span> endpoint<br>
    <span class="text-purple-400">| sort</span> avg_ms <span class="text-purple-400">desc</span><br>
    <span class="text-purple-400">| limit</span> <span class="text-red-400">10</span>
  </div>

  <div class="mt-3 bg-purple-950 rounded-lg p-3 border border-purple-800">
    <p class="text-purple-300 text-xs">💡 <strong>Clave:</strong> El campo <code>trace_id</code> en el log conecta directamente con X-Ray para ver la traza completa.</p>
  </div>
</div>

<div>
  <div class="text-red-400 text-xs font-medium uppercase tracking-wider mb-2">❌ Log no estructurado</div>
  <div class="bg-slate-900 rounded-lg p-3 font-mono text-xs border border-red-900 mb-3 text-slate-400">
    ERROR 2024-03-15 14:32:11 orders-api<br>
    Request failed for user 4521
  </div>

  <div class="text-emerald-400 text-xs font-medium uppercase tracking-wider mb-2">✅ Log estructurado (JSON)</div>
  <div class="bg-slate-900 rounded-xl p-4 font-mono text-xs border border-emerald-900 leading-relaxed">
    <span class="text-slate-500">{</span><br>
    &nbsp;&nbsp;<span class="text-emerald-400">"level"</span><span class="text-slate-400">: </span><span class="text-red-400">"ERROR"</span>,<br>
    &nbsp;&nbsp;<span class="text-emerald-400">"service"</span><span class="text-slate-400">: </span><span class="text-amber-300">"orders-api"</span>,<br>
    &nbsp;&nbsp;<span class="text-emerald-400">"endpoint"</span><span class="text-slate-400">: </span><span class="text-amber-300">"/api/orders/bulk"</span>,<br>
    &nbsp;&nbsp;<span class="text-emerald-400">"user_id"</span><span class="text-slate-400">: </span><span class="text-red-400">4521</span>,<br>
    &nbsp;&nbsp;<span class="text-emerald-400">"duration_ms"</span><span class="text-slate-400">: </span><span class="text-red-400">4250</span>,<br>
    &nbsp;&nbsp;<span class="text-emerald-400">"error"</span><span class="text-slate-400">: </span><span class="text-amber-300">"ProvisionedThroughputExceeded"</span>,<br>
    &nbsp;&nbsp;<span class="text-emerald-400">"trace_id"</span><span class="text-slate-400">: </span><span class="text-purple-400">"1-abc123-xyz"</span> <span class="text-slate-600">// → X-Ray</span><br>
    <span class="text-slate-500">}</span>
  </div>
</div>

</div>

---
layout: default
background: '#1e293b'
---

# Pilar 3 — Trazas 🔍

<div class="grid grid-cols-2 gap-6 mt-6 pb-12">

<div>
  <p class="text-slate-400 text-sm leading-relaxed mb-4">Siguen un request único a través de todos los servicios. Cada salto es un <strong class="text-white">span</strong>. Fundamentales en arquitecturas con múltiples Lambdas o contenedores.</p>

  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">Anatomía de una traza — AWS X-Ray</div>
  <div class="bg-slate-900 rounded-xl p-4 font-mono text-xs border border-orange-900 leading-relaxed">
    <span class="text-slate-500"># Trace: 1-abc123-xyz | Total: 4.3s | ❌</span><br><br>
    <span class="text-emerald-400">API Gateway</span>         <span class="text-slate-500">0ms → 12ms    ✓</span><br>
    &nbsp;&nbsp;<span class="text-blue-400">└─ Lambda cold start</span>  <span class="text-amber-400">12ms → 890ms  ⚠</span><br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="text-blue-400">└─ handler</span>         <span class="text-slate-400">890ms → 4300ms</span><br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="text-red-400">└─ DynamoDB Query</span>  <span class="text-red-400">891ms → 4290ms ❌</span><br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="text-slate-500">Table: orders-table</span><br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="text-slate-500">Sin índice GSI</span><br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="text-red-400">ProvisionedThroughputExceeded</span>
  </div>
</div>

<div>
  <div class="bg-orange-950 rounded-xl p-5 border border-orange-700 mb-4">
    <p class="text-orange-300 text-sm font-semibold mb-2">💡 El momento "ahá"</p>
    <p class="text-slate-300 text-sm leading-relaxed">La traza muestra exactamente que <strong class="text-white">3.4 segundos</strong> de los 4.3 totales se gastaron en una sola query a DynamoDB sin índice. Sin trazas, esto tomaría horas.</p>
  </div>

  <div class="bg-slate-800 rounded-xl p-4 border border-slate-600 mb-4">
    <p class="text-slate-400 text-xs font-medium mb-2">¿Cómo funciona la propagación?</p>
    <p class="text-slate-400 text-sm">Cada servicio pasa el <code class="text-orange-400">trace_id</code> en los headers HTTP. El <strong class="text-white">AWS X-Ray SDK</strong> lo hace automáticamente.</p>
  </div>

  <div class="bg-slate-800 rounded-lg p-3 border border-slate-700">
    <p class="text-slate-400 text-xs font-medium mb-2">Servicios AWS</p>
    <div class="flex flex-wrap gap-2">
      <span class="bg-slate-700 text-slate-300 text-xs px-2 py-1 rounded">AWS X-Ray</span>
      <span class="bg-slate-700 text-slate-300 text-xs px-2 py-1 rounded">CloudWatch ServiceLens</span>
      <span class="bg-slate-700 text-slate-300 text-xs px-2 py-1 rounded">OpenTelemetry</span>
    </div>
  </div>

  <div class="mt-4 bg-slate-800 rounded-lg p-3 border border-slate-700">
    <p class="text-slate-400 text-xs italic">"Si un request tarda 8s y pasa por 5 servicios, ¿cómo sabrías en cuál está el problema sin trazas?"</p>
  </div>
</div>

</div>

---
layout: default
background: '#1e293b'
---

# El flujo completo de debugging

<div class="mt-4 space-y-3 pb-12">

<div class="flex items-center gap-4 bg-slate-800 rounded-xl p-4 border border-slate-600">
  <div class="w-8 h-8 rounded-full bg-red-900 border border-red-600 flex items-center justify-center text-red-400 font-bold text-sm flex-shrink-0">1</div>
  <div class="flex-1">
    <span class="text-white font-medium">CloudWatch Alarm dispara</span>
    <span class="text-slate-400 text-sm ml-2">→ CPU spike detectado a las 14:32 en orders-api</span>
  </div>
  <span class="text-xs bg-amber-900 text-amber-300 px-2 py-1 rounded border border-amber-700">Monitoring</span>
</div>

<div class="flex items-center gap-4 bg-slate-800 rounded-xl p-4 border border-slate-600">
  <div class="w-8 h-8 rounded-full bg-emerald-900 border border-emerald-600 flex items-center justify-center text-emerald-400 font-bold text-sm flex-shrink-0">2</div>
  <div class="flex-1">
    <span class="text-white font-medium">Revisas Métricas</span>
    <span class="text-slate-400 text-sm ml-2">→ Spike correlaciona con latencia p99 elevada post-deploy 14:28</span>
  </div>
  <span class="text-xs bg-emerald-900 text-emerald-300 px-2 py-1 rounded border border-emerald-700">Métricas</span>
</div>

<div class="flex items-center gap-4 bg-slate-800 rounded-xl p-4 border border-slate-600">
  <div class="w-8 h-8 rounded-full bg-purple-900 border border-purple-600 flex items-center justify-center text-purple-400 font-bold text-sm flex-shrink-0">3</div>
  <div class="flex-1">
    <span class="text-white font-medium">Consultas Log Insights</span>
    <span class="text-slate-400 text-sm ml-2">→ 500 errores en <code class="text-purple-400">/api/orders/bulk</code>, todos con <code class="text-purple-400">ProvisionedThroughputExceeded</code></span>
  </div>
  <span class="text-xs bg-purple-900 text-purple-300 px-2 py-1 rounded border border-purple-700">Logs</span>
</div>

<div class="flex items-center gap-4 bg-slate-800 rounded-xl p-4 border border-slate-600">
  <div class="w-8 h-8 rounded-full bg-orange-900 border border-orange-600 flex items-center justify-center text-orange-400 font-bold text-sm flex-shrink-0">4</div>
  <div class="flex-1">
    <span class="text-white font-medium">Trazas con X-Ray</span>
    <span class="text-slate-400 text-sm ml-2">→ DynamoDB Query sin índice GSI tarda <span class="text-red-400 font-semibold">3.4s</span> de los 4.3s totales</span>
  </div>
  <span class="text-xs bg-orange-900 text-orange-300 px-2 py-1 rounded border border-orange-700">Trazas</span>
</div>

</div>

<div class="mt-5 bg-emerald-950 rounded-xl p-4 border border-emerald-700 text-center">
  <p class="text-emerald-300 font-semibold">✅ Causa raíz encontrada en minutos — agregar índice GSI a la tabla DynamoDB</p>
</div>

---
layout: center
background: '#1e293b'
---

# Resumen — Los 3 Pilares

<div class="grid grid-cols-3 gap-6 mt-6">

<div class="bg-slate-800 rounded-xl p-6 border border-emerald-700 text-center">
  <div class="text-4xl mb-3">📊</div>
  <h3 class="text-emerald-400 font-semibold mb-2">Métricas</h3>
  <p class="text-slate-400 text-sm mb-4">Series de tiempo numéricas</p>
  <div class="bg-slate-900 rounded-lg p-3">
    <p class="text-emerald-300 text-xs font-semibold">¿Cuánto?</p>
    <p class="text-slate-500 text-xs mt-1">CloudWatch Metrics & Alarms</p>
  </div>
</div>

<div class="bg-slate-800 rounded-xl p-6 border border-purple-700 text-center">
  <div class="text-4xl mb-3">📝</div>
  <h3 class="text-purple-400 font-semibold mb-2">Logs</h3>
  <p class="text-slate-400 text-sm mb-4">Eventos estructurados (JSON)</p>
  <div class="bg-slate-900 rounded-lg p-3">
    <p class="text-purple-300 text-xs font-semibold">¿Qué pasó?</p>
    <p class="text-slate-500 text-xs mt-1">CloudWatch Logs & Log Insights</p>
  </div>
</div>

<div class="bg-slate-800 rounded-xl p-6 border border-orange-700 text-center">
  <div class="text-4xl mb-3">🔍</div>
  <h3 class="text-orange-400 font-semibold mb-2">Trazas</h3>
  <p class="text-slate-400 text-sm mb-4">Flujo entre servicios</p>
  <div class="bg-slate-900 rounded-lg p-3">
    <p class="text-orange-300 text-xs font-semibold">¿Dónde falló?</p>
    <p class="text-slate-500 text-xs mt-1">AWS X-Ray & ServiceLens</p>
  </div>
</div>

</div>

<div class="mt-8 text-slate-500 text-sm">
  Siguiente paso → <span class="text-white">Bloque 3: implementación práctica en AWS con una app Lambda real</span>
</div>
---
layout: center
background: '#1e293b'
---

# Bloque 3 — AWS en práctica

<div class="mt-6 text-slate-400 text-lg">Servicios nativos de AWS para cada pilar</div>
<div class="mt-2 text-slate-500 text-sm">20 min · Arquitectura + configuración</div>

---
layout: default
background: '#1e293b'
---

# Arquitectura de observabilidad en AWS

<div class="mt-4 flex items-center justify-center gap-4">

  <div class="bg-slate-800 rounded-xl p-4 border border-slate-600 text-center w-36">
    <div class="text-3xl mb-2">🌐</div>
    <p class="text-white font-semibold text-sm">Tu app</p>
    <p class="text-slate-300 text-xs mt-1">Lambda / ECS / EC2</p>
  </div>

  <div class="text-white text-2xl font-bold">→</div>

  <div class="space-y-3">
    <div class="bg-emerald-950 rounded-xl p-3 border border-emerald-700 text-center w-44">
      <p class="text-emerald-400 font-semibold text-sm">CloudWatch</p>
      <p class="text-slate-300 text-xs mt-1">Metrics + Logs</p>
    </div>
    <div class="bg-orange-950 rounded-xl p-3 border border-orange-700 text-center w-44">
      <p class="text-orange-400 font-semibold text-sm">AWS X-Ray</p>
      <p class="text-slate-300 text-xs mt-1">Trazas</p>
    </div>
  </div>

  <div class="text-white text-2xl font-bold">→</div>

  <div class="space-y-3">
    <div class="bg-blue-950 rounded-xl p-3 border border-blue-700 text-center w-44">
      <p class="text-blue-400 font-semibold text-sm">Dashboards</p>
      <p class="text-slate-300 text-xs mt-1">CloudWatch / Grafana</p>
    </div>
    <div class="bg-red-950 rounded-xl p-3 border border-red-700 text-center w-44">
      <p class="text-red-400 font-semibold text-sm">Alertas</p>
      <p class="text-slate-300 text-xs mt-1">SNS / Slack / PagerDuty</p>
    </div>
  </div>

</div>

<div class="grid grid-cols-3 gap-4 mt-5 pb-12">
  <div class="bg-slate-800 rounded-lg p-4 border border-emerald-800">
    <p class="text-emerald-400 text-xs font-semibold uppercase tracking-wider mb-2">Métricas</p>
    <ul class="text-slate-300 text-xs space-y-1">
      <li>→ CloudWatch Metrics</li>
      <li>→ CloudWatch Alarms</li>
      <li>→ Container Insights</li>
      <li>→ Lambda Insights</li>
    </ul>
  </div>
  <div class="bg-slate-800 rounded-lg p-4 border border-purple-800">
    <p class="text-purple-400 text-xs font-semibold uppercase tracking-wider mb-2">Logs</p>
    <ul class="text-slate-300 text-xs space-y-1">
      <li>→ CloudWatch Logs</li>
      <li>→ CloudWatch Log Insights</li>
      <li>→ CloudTrail</li>
      <li>→ VPC Flow Logs</li>
    </ul>
  </div>
  <div class="bg-slate-800 rounded-lg p-4 border border-orange-800">
    <p class="text-orange-400 text-xs font-semibold uppercase tracking-wider mb-2">Trazas</p>
    <ul class="text-slate-300 text-xs space-y-1">
      <li>→ AWS X-Ray</li>
      <li>→ CloudWatch ServiceLens</li>
      <li>→ Distro for OpenTelemetry</li>
      <li>→ Amazon Managed Grafana</li>
    </ul>
  </div>
</div>

---
layout: default
background: '#1e293b'
---

# CloudWatch Logs — configuración en Lambda

<div class="grid grid-cols-2 gap-6 mt-4 pb-12">

<div>
  <p class="text-slate-400 text-sm leading-relaxed mb-3">Lambda envía logs automáticamente a CloudWatch. Lo importante es estructurarlos en JSON desde el código para poder consultarlos con Log Insights.</p>

  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">Node.js — logger estructurado</div>
  <div class="bg-slate-900 rounded-xl p-4 font-mono text-xs border border-purple-900 leading-relaxed">
    <span class="text-slate-500">// logger.js</span><br>
    <span class="text-blue-400">const</span> log = (level, msg, extra = {}) => {<br>
    &nbsp;&nbsp;<span class="text-blue-400">const</span> entry = {<br>
    &nbsp;&nbsp;&nbsp;&nbsp;level,<br>
    &nbsp;&nbsp;&nbsp;&nbsp;message: msg,<br>
    &nbsp;&nbsp;&nbsp;&nbsp;timestamp: <span class="text-blue-400">new</span> Date().toISOString(),<br>
    &nbsp;&nbsp;&nbsp;&nbsp;service: <span class="text-amber-300">'orders-api'</span>,<br>
    &nbsp;&nbsp;&nbsp;&nbsp;...extra<br>
    &nbsp;&nbsp;};<br>
    &nbsp;&nbsp;console.log(JSON.stringify(entry));<br>
    };<br><br>
    <span class="text-slate-500">// Uso en tu handler</span><br>
    log(<span class="text-amber-300">'ERROR'</span>, <span class="text-amber-300">'DynamoDB falló'</span>, {<br>
    &nbsp;&nbsp;user_id: <span class="text-red-400">4521</span>,<br>
    &nbsp;&nbsp;endpoint: <span class="text-amber-300">'/api/orders/bulk'</span>,<br>
    &nbsp;&nbsp;duration_ms: <span class="text-red-400">4250</span>,<br>
    &nbsp;&nbsp;trace_id: process.env._X_AMZN_TRACE_ID<br>
    });
  </div>
</div>

<div>
  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">Grupos de logs en CloudWatch</div>
  <div class="bg-slate-900 rounded-xl p-4 font-mono text-xs border border-slate-700 leading-relaxed mb-4">
    <span class="text-slate-500"># Se crean automáticamente</span><br>
    /aws/lambda/<span class="text-emerald-400">orders-api</span><br>
    /aws/lambda/<span class="text-emerald-400">payments-api</span><br>
    /aws/ecs/<span class="text-emerald-400">mi-cluster</span><br><br>
    <span class="text-slate-500"># Retención recomendada</span><br>
    Producción: <span class="text-amber-300">30 días</span><br>
    Dev/Staging: <span class="text-amber-300">7 días</span>
  </div>

  <div class="bg-purple-950 rounded-xl p-4 border border-purple-700">
    <p class="text-purple-300 text-xs font-semibold mb-2">💡 Tip importante</p>
    <p class="text-slate-300 text-xs leading-relaxed">Siempre incluye <code class="text-purple-400">process.env._X_AMZN_TRACE_ID</code> en tus logs. Lambda lo inyecta automáticamente y es el puente con X-Ray para correlacionar logs y trazas.</p>
  </div>

  <div class="mt-3 bg-amber-950 rounded-lg p-3 border border-amber-800">
    <p class="text-amber-300 text-xs font-semibold mb-1">⚠ Costo a tener en cuenta</p>
    <p class="text-slate-400 text-xs">CloudWatch Logs cobra por GB ingestado. Logs estructurados + retención corta = costos controlados.</p>
  </div>
</div>

</div>

---
layout: default
background: '#1e293b'
---

# CloudWatch Alarms — configuración práctica

<div class="grid grid-cols-2 gap-6 mt-4 pb-12">

<div>
  <p class="text-slate-400 text-sm leading-relaxed mb-3">Una alarma observa una métrica y dispara una acción cuando cruza un umbral. Se conecta con SNS para notificar por email, Slack o PagerDuty.</p>

  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">Alarmas esenciales para una Lambda</div>
  <div class="space-y-2">
    <div class="bg-slate-900 rounded-lg p-3 border border-red-900 flex items-start gap-3">
      <span class="text-red-400 text-lg">🔴</span>
      <div>
        <p class="text-white text-xs font-medium">Errors > 5 en 5 min</p>
        <p class="text-slate-500 text-xs">Namespace: AWS/Lambda · Metric: Errors</p>
      </div>
    </div>
    <div class="bg-slate-900 rounded-lg p-3 border border-amber-900 flex items-start gap-3">
      <span class="text-amber-400 text-lg">🟡</span>
      <div>
        <p class="text-white text-xs font-medium">Duration p99 > 3000ms</p>
        <p class="text-slate-500 text-xs">Namespace: AWS/Lambda · Metric: Duration</p>
      </div>
    </div>
    <div class="bg-slate-900 rounded-lg p-3 border border-orange-900 flex items-start gap-3">
      <span class="text-orange-400 text-lg">🟠</span>
      <div>
        <p class="text-white text-xs font-medium">Throttles > 0 en 1 min</p>
        <p class="text-slate-500 text-xs">Namespace: AWS/Lambda · Metric: Throttles</p>
      </div>
    </div>
    <div class="bg-slate-900 rounded-lg p-3 border border-blue-900 flex items-start gap-3">
      <span class="text-blue-400 text-lg">🔵</span>
      <div>
        <p class="text-white text-xs font-medium">ConcurrentExecutions > 80% del límite</p>
        <p class="text-slate-500 text-xs">Namespace: AWS/Lambda · Metric: ConcurrentExecutions</p>
      </div>
    </div>
  </div>
</div>

<div>
  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">AWS CLI — crear alarma</div>
  <div class="bg-slate-900 rounded-xl p-4 font-mono text-xs border border-slate-700 leading-relaxed">
    aws cloudwatch put-metric-alarm \<br>
    &nbsp;&nbsp;<span class="text-emerald-400">--alarm-name</span> <span class="text-amber-300">"lambda-errors-high"</span> \<br>
    &nbsp;&nbsp;<span class="text-emerald-400">--metric-name</span> <span class="text-amber-300">Errors</span> \<br>
    &nbsp;&nbsp;<span class="text-emerald-400">--namespace</span> <span class="text-amber-300">AWS/Lambda</span> \<br>
    &nbsp;&nbsp;<span class="text-emerald-400">--dimensions</span> \<br>
    &nbsp;&nbsp;&nbsp;&nbsp;Name=FunctionName,Value=<span class="text-amber-300">orders-api</span> \<br>
    &nbsp;&nbsp;<span class="text-emerald-400">--statistic</span> <span class="text-amber-300">Sum</span> \<br>
    &nbsp;&nbsp;<span class="text-emerald-400">--period</span> <span class="text-red-400">300</span> \<br>
    &nbsp;&nbsp;<span class="text-emerald-400">--threshold</span> <span class="text-red-400">5</span> \<br>
    &nbsp;&nbsp;<span class="text-emerald-400">--comparison-operator</span> GreaterThanThreshold \<br>
    &nbsp;&nbsp;<span class="text-emerald-400">--evaluation-periods</span> <span class="text-red-400">1</span> \<br>
    &nbsp;&nbsp;<span class="text-emerald-400">--alarm-actions</span> arn:aws:sns:...<span class="text-amber-300">mi-topic</span>
  </div>

  <div class="mt-3 bg-emerald-950 rounded-lg p-3 border border-emerald-800">
    <p class="text-emerald-300 text-xs font-semibold mb-1">💡 Buena práctica</p>
    <p class="text-slate-400 text-xs">Define las alarmas en IaC (CloudFormation / Terraform / CDK) para que estén versionadas junto al código de tu app.</p>
  </div>
</div>

</div>

---
layout: default
background: '#1e293b'
---

# AWS X-Ray — instrumentación en Lambda

<div class="grid grid-cols-2 gap-6 mt-4 pb-12">

<div>
  <p class="text-slate-400 text-sm leading-relaxed mb-3">X-Ray necesita dos cosas: activar el tracing en la función Lambda y envolver el SDK de AWS con el cliente de X-Ray para capturar los subsegmentos automáticamente.</p>

  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">1. Activar en la función (consola o IaC)</div>
  <div class="bg-slate-900 rounded-xl p-3 font-mono text-xs border border-slate-700 leading-relaxed mb-3">
    <span class="text-slate-500"># AWS CLI</span><br>
    aws lambda update-function-configuration \<br>
    &nbsp;&nbsp;<span class="text-emerald-400">--function-name</span> orders-api \<br>
    &nbsp;&nbsp;<span class="text-emerald-400">--tracing-config</span> Mode=<span class="text-amber-300">Active</span>
  </div>

  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">2. Instrumentar el código Node.js</div>
  <div class="bg-slate-900 rounded-xl p-4 font-mono text-xs border border-orange-900 leading-relaxed">
    <span class="text-slate-500">// Instalar: npm i aws-xray-sdk-core</span><br>
    <span class="text-blue-400">const</span> AWSXRay = require(<span class="text-amber-300">'aws-xray-sdk-core'</span>);<br>
    <span class="text-blue-400">const</span> AWS = AWSXRay.captureAWS(<br>
    &nbsp;&nbsp;require(<span class="text-amber-300">'aws-sdk'</span>)<br>
    );<br><br>
    <span class="text-slate-500">// A partir de aquí, todas las llamadas</span><br>
    <span class="text-slate-500">// a DynamoDB, S3, SQS, etc. quedan</span><br>
    <span class="text-slate-500">// registradas automáticamente en X-Ray</span><br><br>
    <span class="text-blue-400">const</span> dynamo = <span class="text-blue-400">new</span> AWS.DynamoDB.DocumentClient();<br>
    <span class="text-slate-500">// ↑ X-Ray captura latencia y errores</span>
  </div>
</div>

<div>
  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">Subsegmentos personalizados</div>
  <div class="bg-slate-900 rounded-xl p-4 font-mono text-xs border border-slate-700 leading-relaxed mb-4">
    <span class="text-blue-400">const</span> segment = AWSXRay.getSegment();<br>
    <span class="text-blue-400">const</span> sub = segment.addNewSubsegment(<span class="text-amber-300">'validar-stock'</span>);<br><br>
    <span class="text-blue-400">try</span> {<br>
    &nbsp;&nbsp;<span class="text-blue-400">await</span> validarStock(items);<br>
    &nbsp;&nbsp;sub.addAnnotation(<span class="text-amber-300">'items_count'</span>, items.length);<br>
    } <span class="text-blue-400">catch</span> (err) {<br>
    &nbsp;&nbsp;sub.addError(err);<br>
    &nbsp;&nbsp;<span class="text-blue-400">throw</span> err;<br>
    } <span class="text-blue-400">finally</span> {<br>
    &nbsp;&nbsp;sub.close();<br>
    }
  </div>

  <div class="bg-orange-950 rounded-xl p-4 border border-orange-700">
    <p class="text-orange-300 text-xs font-semibold mb-2">💡 Annotations vs Metadata</p>
    <div class="space-y-2">
      <div>
        <p class="text-white text-xs font-medium">addAnnotation(key, value)</p>
        <p class="text-slate-400 text-xs">Indexado — puedes filtrar trazas por este valor en la consola</p>
      </div>
      <div>
        <p class="text-white text-xs font-medium">addMetadata(key, value)</p>
        <p class="text-slate-400 text-xs">No indexado — para datos de debug detallados</p>
      </div>
    </div>
  </div>
</div>

</div>

---
layout: default
background: '#1e293b'
---

# CloudWatch ServiceLens — visión unificada

<div class="grid grid-cols-2 gap-6 mt-4 pb-12">

<div>
  <p class="text-slate-400 text-sm leading-relaxed mb-4">ServiceLens combina métricas, logs y trazas de X-Ray en una sola vista. Es el punto de entrada para debugging en producción — ves el Service Map y llegas al trace en 2 clics.</p>

  <div class="space-y-3">
    <div class="bg-slate-800 rounded-lg p-4 border border-slate-600">
      <div class="flex items-center gap-3 mb-2">
        <span class="text-blue-400 text-lg">🗺️</span>
        <p class="text-white text-sm font-medium">Service Map</p>
      </div>
      <p class="text-slate-400 text-xs leading-relaxed">Vista visual de todos tus servicios y sus dependencias. Cada nodo muestra latencia promedio, tasa de error y requests/min en tiempo real.</p>
    </div>
    <div class="bg-slate-800 rounded-lg p-4 border border-slate-600">
      <div class="flex items-center gap-3 mb-2">
        <span class="text-purple-400 text-lg">🔎</span>
        <p class="text-white text-sm font-medium">Trace search</p>
      </div>
      <p class="text-slate-400 text-xs leading-relaxed">Filtra trazas por servicio, anotación, duración o status code. Puedes ir directo al trace de un request específico que falló.</p>
    </div>
    <div class="bg-slate-800 rounded-lg p-4 border border-slate-600">
      <div class="flex items-center gap-3 mb-2">
        <span class="text-emerald-400 text-lg">📊</span>
        <p class="text-white text-sm font-medium">Insights automáticos</p>
      </div>
      <p class="text-slate-400 text-xs leading-relaxed">X-Ray detecta anomalías estadísticas y agrupa trazas relacionadas para acelerar el root cause analysis.</p>
    </div>
  </div>
</div>

<div>
  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">Flujo de debugging en ServiceLens</div>
  <div class="space-y-2">
    <div class="flex items-start gap-3">
      <div class="w-6 h-6 rounded-full bg-red-900 border border-red-600 flex items-center justify-center text-red-400 text-xs font-bold flex-shrink-0 mt-0.5">1</div>
      <div class="bg-slate-800 rounded-lg p-3 border border-slate-700 flex-1">
        <p class="text-white text-xs font-medium">Service Map</p>
        <p class="text-slate-400 text-xs">Ves un nodo rojo → orders-api tiene 12% error rate</p>
      </div>
    </div>
    <div class="flex items-start gap-3">
      <div class="w-6 h-6 rounded-full bg-amber-900 border border-amber-600 flex items-center justify-center text-amber-400 text-xs font-bold flex-shrink-0 mt-0.5">2</div>
      <div class="bg-slate-800 rounded-lg p-3 border border-slate-700 flex-1">
        <p class="text-white text-xs font-medium">Clic en el nodo</p>
        <p class="text-slate-400 text-xs">Ves métricas de ese servicio + lista de trazas con error</p>
      </div>
    </div>
    <div class="flex items-start gap-3">
      <div class="w-6 h-6 rounded-full bg-purple-900 border border-purple-600 flex items-center justify-center text-purple-400 text-xs font-bold flex-shrink-0 mt-0.5">3</div>
      <div class="bg-slate-800 rounded-lg p-3 border border-slate-700 flex-1">
        <p class="text-white text-xs font-medium">Abres una traza</p>
        <p class="text-slate-400 text-xs">Waterfall completo → DynamoDB span tarda 3.4s</p>
      </div>
    </div>
    <div class="flex items-start gap-3">
      <div class="w-6 h-6 rounded-full bg-emerald-900 border border-emerald-600 flex items-center justify-center text-emerald-400 text-xs font-bold flex-shrink-0 mt-0.5">4</div>
      <div class="bg-slate-800 rounded-lg p-3 border border-slate-700 flex-1">
        <p class="text-white text-xs font-medium">Ver logs relacionados</p>
        <p class="text-slate-400 text-xs">Clic en "View in CloudWatch Logs" → log exacto del error</p>
      </div>
    </div>
  </div>

  <div class="mt-3 bg-blue-950 rounded-lg p-3 border border-blue-800">
    <p class="text-blue-300 text-xs">📍 Ruta en AWS Console: <span class="text-white font-mono">CloudWatch → ServiceLens → Service Map</span></p>
  </div>
</div>

</div>

---
layout: center
background: '#1e293b'
---

# Bloque 4 — Demo de debugging

<div class="mt-6 text-slate-400 text-lg">Resolviendo un incidente real paso a paso</div>
<div class="mt-2 text-slate-500 text-sm">15 min · Escenario real en consola AWS</div>

---
layout: default
background: '#1e293b'
---

# El escenario — incidente en producción

<div class="mt-4 pb-12">
<div class="bg-red-950 rounded-xl p-5 border border-red-700">
  <div class="flex items-center gap-3 mb-4">
    <span class="text-3xl">🚨</span>
    <div>
      <p class="text-red-400 font-semibold text-lg">Incidente activo — P2</p>
      <p class="text-slate-400 text-sm">Viernes 14:35 UTC · orders-api degradado</p>
    </div>
  </div>
  <div class="grid grid-cols-3 gap-4">
    <div class="bg-red-900 rounded-lg p-3 text-center border border-red-700">
      <p class="text-red-300 text-2xl font-bold">4.3s</p>
      <p class="text-slate-400 text-xs mt-1">Latencia p99 (normal: 200ms)</p>
    </div>
    <div class="bg-red-900 rounded-lg p-3 text-center border border-red-700">
      <p class="text-red-300 text-2xl font-bold">12%</p>
      <p class="text-slate-400 text-xs mt-1">Error rate (normal: 0.1%)</p>
    </div>
    <div class="bg-red-900 rounded-lg p-3 text-center border border-red-700">
      <p class="text-red-300 text-2xl font-bold">~200</p>
      <p class="text-slate-400 text-xs mt-1">Usuarios afectados / min</p>
    </div>
  </div>
</div>

<div class="mt-5 bg-slate-800 rounded-xl p-4 border border-slate-600">
  <p class="text-slate-400 text-xs font-medium uppercase tracking-wider mb-3">Lo que sabemos hasta ahora</p>
  <div class="grid grid-cols-2 gap-4">
    <div class="space-y-2">
      <div class="flex items-center gap-2">
        <span class="text-red-400 text-sm">✗</span>
        <p class="text-slate-400 text-xs">El problema empezó a las 14:28 UTC</p>
      </div>
      <div class="flex items-center gap-2">
        <span class="text-red-400 text-sm">✗</span>
        <p class="text-slate-400 text-xs">No todos los requests fallan, solo algunos</p>
      </div>
    </div>
    <div class="space-y-2">
      <div class="flex items-center gap-2">
        <span class="text-amber-400 text-sm">?</span>
        <p class="text-slate-400 text-xs">¿Qué endpoint específico está fallando?</p>
      </div>
      <div class="flex items-center gap-2">
        <span class="text-amber-400 text-sm">?</span>
        <p class="text-slate-400 text-xs">¿Hubo un deploy a las 14:28?</p>
      </div>
    </div>
  </div>
</div>

<div class="mt-4 text-center text-slate-500 text-sm">¿Por dónde empezamos?</div>
</div>

---
layout: default
background: '#1e293b'
---

# Paso 1 — Métricas: confirmar el alcance

<div class="grid grid-cols-2 gap-6 mt-4 pb-12">

<div>
  <p class="text-slate-400 text-sm leading-relaxed mb-4">Antes de bucear en logs, usamos métricas para entender el tamaño del problema y correlacionar con eventos del sistema.</p>

  <div class="space-y-3">
    <div class="bg-slate-800 rounded-lg p-4 border border-slate-700">
      <p class="text-white text-xs font-medium mb-2">📍 CloudWatch → Metrics → AWS/Lambda</p>
      <p class="text-slate-400 text-xs leading-relaxed">Abrimos <code class="text-emerald-400">Duration (p99)</code> y <code class="text-emerald-400">Errors</code> para <code class="text-emerald-400">orders-api</code>. Vemos el spike exacto a las 14:28.</p>
    </div>
    <div class="bg-slate-800 rounded-lg p-4 border border-slate-700">
      <p class="text-white text-xs font-medium mb-2">📍 Correlacionar con deploys</p>
      <p class="text-slate-400 text-xs leading-relaxed">En CloudTrail buscamos eventos <code class="text-emerald-400">UpdateFunctionCode</code> cerca de las 14:28. Confirma: deploy a las 14:27:43.</p>
    </div>
    <div class="bg-emerald-950 rounded-lg p-3 border border-emerald-800">
      <p class="text-emerald-300 text-xs font-semibold">✅ Hallazgo</p>
      <p class="text-slate-400 text-xs mt-1">El problema empezó 15 segundos después del deploy. Alta probabilidad de regresión en el código nuevo.</p>
    </div>
  </div>
</div>

<div>
  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">Vista de métricas — lo que verías</div>
  <div class="bg-slate-900 rounded-xl p-4 border border-slate-700 font-mono text-xs">
    <div class="text-slate-500 mb-3">Duration p99 — orders-api (últimas 2h)</div>
    <div class="space-y-1">
      <div class="flex items-center gap-2">
        <span class="text-slate-600 w-10 text-right">14:20</span>
        <div class="h-2 bg-emerald-800 rounded" style="width: 15%"></div>
        <span class="text-emerald-400">180ms</span>
      </div>
      <div class="flex items-center gap-2">
        <span class="text-slate-600 w-10 text-right">14:25</span>
        <div class="h-2 bg-emerald-800 rounded" style="width: 18%"></div>
        <span class="text-emerald-400">210ms</span>
      </div>
      <div class="flex items-center gap-2">
        <span class="text-amber-400 w-10 text-right font-bold">14:28</span>
        <div class="h-2 bg-red-600 rounded" style="width: 85%"></div>
        <span class="text-red-400 font-bold">4250ms ← deploy</span>
      </div>
      <div class="flex items-center gap-2">
        <span class="text-slate-600 w-10 text-right">14:30</span>
        <div class="h-2 bg-red-600 rounded" style="width: 90%"></div>
        <span class="text-red-400">4300ms</span>
      </div>
      <div class="flex items-center gap-2">
        <span class="text-slate-600 w-10 text-right">14:35</span>
        <div class="h-2 bg-red-600 rounded" style="width: 88%"></div>
        <span class="text-red-400">4180ms</span>
      </div>
    </div>
  </div>

  <div class="mt-3 bg-slate-800 rounded-lg p-3 border border-amber-800">
    <p class="text-amber-300 text-xs font-semibold mb-1">Pregunta para el mentee</p>
    <p class="text-slate-400 text-xs">"¿Qué harías ahora — rollback inmediato o primero investigar qué cambió?"</p>
  </div>
</div>

</div>

---
layout: default
background: '#1e293b'
---

# Paso 2 — Logs: encontrar el error exacto

<div class="grid grid-cols-2 gap-6 mt-4 pb-12">

<div>
  <p class="text-slate-400 text-sm leading-relaxed mb-3">Con Log Insights podemos consultar todos los logs de la función en segundos, sin descargar nada ni hacer SSH a ningún servidor.</p>

  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">📍 CloudWatch → Log Insights</div>
  <div class="bg-slate-900 rounded-xl p-4 font-mono text-xs border border-purple-900 leading-relaxed">
    <span class="text-slate-500"># Seleccionar: /aws/lambda/orders-api</span><br>
    <span class="text-slate-500"># Rango: últimos 30 minutos</span><br><br>
    fields endpoint, error, duration_ms, user_id<br>
    <span class="text-purple-400">| filter</span> level = <span class="text-amber-300">"ERROR"</span><br>
    <span class="text-purple-400">| filter</span> timestamp > <span class="text-amber-300">"2024-03-15T14:27:00Z"</span><br>
    <span class="text-purple-400">| stats</span> count(*) <span class="text-purple-400">as</span> errors,<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;avg(duration_ms) <span class="text-purple-400">as</span> avg_ms<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="text-purple-400">by</span> endpoint, error<br>
    <span class="text-purple-400">| sort</span> errors <span class="text-purple-400">desc</span>
  </div>

  <div class="mt-3 bg-purple-950 rounded-lg p-3 border border-purple-800">
    <p class="text-purple-300 text-xs font-semibold">✅ Resultado de la query</p>
    <div class="font-mono text-xs mt-2 space-y-1">
      <p class="text-white">/api/orders/bulk &nbsp;&nbsp;→ <span class="text-red-400">847 errores</span> · 4250ms avg</p>
      <p class="text-slate-500">/api/orders/:id &nbsp;&nbsp;→ 2 errores · 180ms avg</p>
      <p class="text-slate-500">/api/products &nbsp;&nbsp;&nbsp;→ 0 errores</p>
    </div>
  </div>
</div>

<div>
  <div class="text-slate-500 text-xs font-medium uppercase tracking-wider mb-2">Log individual del error</div>
  <div class="bg-slate-900 rounded-xl p-4 font-mono text-xs border border-red-900 leading-relaxed mb-3">
    <span class="text-slate-500">{</span><br>
    &nbsp;&nbsp;<span class="text-purple-400">"level"</span>: <span class="text-red-400">"ERROR"</span>,<br>
    &nbsp;&nbsp;<span class="text-purple-400">"endpoint"</span>: <span class="text-amber-300">"/api/orders/bulk"</span>,<br>
    &nbsp;&nbsp;<span class="text-purple-400">"error"</span>: <span class="text-red-400">"ProvisionedThroughputExceeded"</span>,<br>
    &nbsp;&nbsp;<span class="text-purple-400">"table"</span>: <span class="text-amber-300">"orders-table"</span>,<br>
    &nbsp;&nbsp;<span class="text-purple-400">"duration_ms"</span>: <span class="text-red-400">4250</span>,<br>
    &nbsp;&nbsp;<span class="text-purple-400">"items_in_request"</span>: <span class="text-red-400">150</span>,<br>
    &nbsp;&nbsp;<span class="text-purple-400">"trace_id"</span>: <span class="text-orange-400">"1-abc123-xyz"</span><br>
    <span class="text-slate-500">}</span>
  </div>

  <div class="bg-emerald-950 rounded-lg p-3 border border-emerald-800 mb-3">
    <p class="text-emerald-300 text-xs font-semibold">✅ Hallazgo</p>
    <p class="text-slate-400 text-xs mt-1">Solo <code>/api/orders/bulk</code> falla. El nuevo deploy introdujo una operación bulk que no existía antes — DynamoDB no tiene capacidad para esa carga.</p>
  </div>

  <div class="bg-orange-950 rounded-lg p-3 border border-orange-800">
    <p class="text-orange-300 text-xs">🔗 Tenemos el <code>trace_id</code> — ahora vamos a X-Ray para ver exactamente dónde ocurre el cuello de botella</p>
  </div>
</div>

</div>

---
layout: default
background: '#1e293b'
---

# Paso 3 — Trazas: localizar la causa raíz

<div class="grid grid-cols-2 gap-6 mt-4 pb-12">
<div>
<p class="text-slate-300 text-sm leading-relaxed mb-3">Con el <code class="text-orange-400">trace_id</code> del log vamos directo a X-Ray y vemos el waterfall completo del request.</p>
<div class="text-slate-300 text-xs font-medium uppercase tracking-wider mb-2">📍 X-Ray → Trace detail: 1-abc123-xyz</div>
<div class="bg-slate-900 rounded-xl p-4 font-mono text-xs border border-orange-900 leading-relaxed">
<span class="text-slate-300">Total: 4.3s · Status: 500</span><br><br>
<span class="text-emerald-400">▶ API Gateway</span>
<span class="text-slate-300 ml-2">━━ 12ms ✓</span><br>
&nbsp;&nbsp;<span class="text-blue-400">▶ Lambda: init</span>
<span class="text-amber-400 ml-2">━━━━━━━━ 878ms ⚠</span><br>
&nbsp;&nbsp;<span class="text-blue-400">▶ Lambda: handler</span>
<span class="text-slate-300 ml-2">━━ 3410ms</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="text-slate-300">▶ validar-stock</span>
<span class="text-emerald-400 ml-2">━ 45ms ✓</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="text-red-400">▶ DynamoDB: BatchWrite</span>
<span class="text-red-400 ml-2">━━━━━━━━ 3340ms ❌</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="text-slate-300">Table: orders-table</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="text-slate-300">Items: 150 · Retries: 8</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="text-red-400">ProvisionedThroughputExceeded</span>
</div>
<div class="mt-3 bg-red-950 rounded-lg p-3 border border-red-800">
<p class="text-red-300 text-xs font-semibold">🎯 Causa raíz encontrada</p>
<p class="text-slate-300 text-xs mt-1">El nuevo endpoint <code>/api/orders/bulk</code> hace un <code>BatchWrite</code> de 150 items de una vez. La tabla DynamoDB no tiene capacidad provisionada para esa operación.</p>
</div>
</div>
<div>
<div class="text-slate-300 text-xs font-medium uppercase tracking-wider mb-2">Análisis del problema</div>
<div class="space-y-3">
<div class="bg-slate-800 rounded-lg p-4 border border-slate-700">
<p class="text-white text-xs font-medium mb-2">¿Qué cambió en el deploy?</p>
<p class="text-slate-300 text-xs leading-relaxed">El nuevo endpoint hace <code class="text-red-400">BatchWriteItem</code> con hasta 150 registros por request. DynamoDB tiene límite de 25 items por batch y capacidad de escritura insuficiente.</p>
</div>
<div class="bg-slate-800 rounded-lg p-4 border border-slate-700">
<p class="text-white text-xs font-medium mb-2">Opciones de solución</p>
<div class="space-y-1">
<div class="flex items-center gap-2">
<span class="text-emerald-400 text-xs">→</span>
<p class="text-slate-300 text-xs">Rollback inmediato del deploy (acción rápida)</p>
</div>
<div class="flex items-center gap-2">
<span class="text-emerald-400 text-xs">→</span>
<p class="text-slate-300 text-xs">Aumentar capacidad de escritura en DynamoDB</p>
</div>
<div class="flex items-center gap-2">
<span class="text-emerald-400 text-xs">→</span>
<p class="text-slate-300 text-xs">Dividir el batch en chunks de 25 con retry exponencial</p>
</div>
<div class="flex items-center gap-2">
<span class="text-emerald-400 text-xs">→</span>
<p class="text-slate-300 text-xs">Cambiar a DynamoDB On-Demand para este patrón</p>
</div>
</div>
</div>
<div class="bg-emerald-950 rounded-lg p-3 border border-emerald-700">
<p class="text-emerald-300 text-xs font-semibold">⏱ Tiempo total de debugging: ~8 minutos</p>
<p class="text-slate-300 text-xs mt-1">Sin observabilidad, esto tomaría horas de logs manuales y suposiciones. Con los 3 pilares, llegamos a la causa raíz en minutos.</p>
</div>
</div>
</div>
</div>

---
layout: default
background: '#1e293b'
---

# Lecciones del incidente

<div class="grid grid-cols-2 gap-6 mt-6 pb-12">

<div class="space-y-3">
  <div class="bg-slate-800 rounded-xl p-5 border border-emerald-700">
    <div class="flex items-center gap-3 mb-3">
      <span class="text-2xl">📊</span>
      <p class="text-emerald-400 font-semibold">Métricas nos dieron el cuándo</p>
    </div>
    <p class="text-slate-400 text-sm">El spike de latencia correlacionado con el deploy fue la primera pista. Sin esa correlación temporal, hubiéramos perdido tiempo buscando en el lugar equivocado.</p>
  </div>

  <div class="bg-slate-800 rounded-xl p-5 border border-purple-700">
    <div class="flex items-center gap-3 mb-3">
      <span class="text-2xl">📝</span>
      <p class="text-purple-400 font-semibold">Logs nos dieron el qué</p>
    </div>
    <p class="text-slate-400 text-sm">Log Insights filtró 847 errores en segundos y nos apuntó al endpoint exacto. Sin logs estructurados, ese query no habría funcionado.</p>
  </div>

  <div class="bg-slate-800 rounded-xl p-5 border border-orange-700">
    <div class="flex items-center gap-3 mb-3">
      <span class="text-2xl">🔍</span>
      <p class="text-orange-400 font-semibold">Trazas nos dieron el por qué</p>
    </div>
    <p class="text-slate-400 text-sm">X-Ray mostró que 3.34s de los 4.3s totales eran de DynamoDB, con 8 reintentos. Sin trazas, hubiéramos sospechado del código, no de la capacidad de la tabla.</p>
  </div>
</div>

<div>
  <div class="bg-slate-800 rounded-xl p-5 border border-slate-600 mb-4">
    <p class="text-white font-semibold mb-4">Buenas prácticas que aprendimos</p>
    <div class="space-y-3">
      <div class="flex items-start gap-3">
        <span class="text-emerald-400 text-sm mt-0.5">✓</span>
        <p class="text-slate-400 text-sm">Siempre incluir <code class="text-orange-400">trace_id</code> en tus logs para poder saltar de log a traza</p>
      </div>
      <div class="flex items-start gap-3">
        <span class="text-emerald-400 text-sm mt-0.5">✓</span>
        <p class="text-slate-400 text-sm">Logs estructurados (JSON) son imprescindibles para Log Insights</p>
      </div>
      <div class="flex items-start gap-3">
        <span class="text-emerald-400 text-sm mt-0.5">✓</span>
        <p class="text-slate-400 text-sm">Define alarmas antes de que ocurran los incidentes, no después</p>
      </div>
      <div class="flex items-start gap-3">
        <span class="text-emerald-400 text-sm mt-0.5">✓</span>
        <p class="text-slate-400 text-sm">Load testing de nuevos endpoints antes del deploy a producción</p>
      </div>
      <div class="flex items-start gap-3">
        <span class="text-emerald-400 text-sm mt-0.5">✓</span>
        <p class="text-slate-400 text-sm">Correlaciona siempre tus métricas con el historial de deploys</p>
      </div>
    </div>
  </div>
</div>

</div>

---
layout: center
background: '#1e293b'
---

# Resumen final

<div class="grid grid-cols-3 gap-5 mt-6">
  <div class="bg-slate-800 rounded-xl p-5 border border-emerald-700 text-center">
    <div class="text-4xl mb-3">📊</div>
    <p class="text-emerald-400 font-semibold mb-1">Métricas</p>
    <p class="text-slate-500 text-xs mb-3">¿Cuánto?</p>
    <p class="text-slate-400 text-xs">CloudWatch Metrics + Alarms</p>
  </div>
  <div class="bg-slate-800 rounded-xl p-5 border border-purple-700 text-center">
    <div class="text-4xl mb-3">📝</div>
    <p class="text-purple-400 font-semibold mb-1">Logs</p>
    <p class="text-slate-500 text-xs mb-3">¿Qué pasó?</p>
    <p class="text-slate-400 text-xs">CloudWatch Logs + Log Insights</p>
  </div>
  <div class="bg-slate-800 rounded-xl p-5 border border-orange-700 text-center">
    <div class="text-4xl mb-3">🔍</div>
    <p class="text-orange-400 font-semibold mb-1">Trazas</p>
    <p class="text-slate-500 text-xs mb-3">¿Dónde falló?</p>
    <p class="text-slate-400 text-xs">AWS X-Ray + ServiceLens</p>
  </div>
</div>

<div class="mt-8 bg-slate-800 rounded-xl p-5 border border-slate-600 max-w-xl mx-auto">
  <p class="text-white font-semibold mb-3">Próximos pasos</p>
  <div class="space-y-2 text-left">
    <div class="flex items-center gap-2">
      <span class="text-emerald-400 text-sm">→</span>
      <p class="text-slate-400 text-sm">Instrumentar tu primera Lambda con logs JSON + X-Ray</p>
    </div>
    <div class="flex items-center gap-2">
      <span class="text-emerald-400 text-sm">→</span>
      <p class="text-slate-400 text-sm">Crear 3 alarmas esenciales en CloudWatch</p>
    </div>
    <div class="flex items-center gap-2">
      <span class="text-emerald-400 text-sm">→</span>
      <p class="text-slate-400 text-sm">Explorar el Service Map en CloudWatch ServiceLens</p>
    </div>
  </div>
</div>

<div class="mt-6 text-slate-500 text-sm">Julian Isaza · Observabilidad en AWS</div>

---
layout: center
background: '#1e293b'
---

# Bloque 5 — OpenTelemetry & ADOT

<div class="mt-6 text-slate-300 text-lg">El estándar abierto para observabilidad moderna</div>
<div class="mt-2 text-slate-500 text-sm">De vendor lock-in a estándares abiertos</div>

---
layout: default
background: '#1e293b'
---

# ¿Por qué OpenTelemetry?

<div class="grid grid-cols-2 gap-6 mt-6 pb-12">
<div>
<div class="bg-red-950 rounded-xl p-5 border border-red-700 mb-4">
<p class="text-red-400 text-xs font-semibold uppercase mb-2">❌ Problema: Vendor Lock-in</p>
<div class="space-y-2 text-slate-300 text-xs">
<p>→ X-Ray SDK solo funciona con AWS</p>
<p>→ Datadog Agent solo con Datadog</p>
<p>→ Cambiar de proveedor = reescribir código</p>
<p>→ Cada herramienta tiene su formato propietario</p>
</div>
</div>
<div class="bg-emerald-950 rounded-xl p-5 border border-emerald-700">
<p class="text-emerald-400 text-xs font-semibold uppercase mb-2">✅ Solución: OpenTelemetry (OTel)</p>
<div class="space-y-2 text-slate-300 text-xs">
<p>→ Estándar abierto (CNCF) — vendor neutral</p>
<p>→ Un SDK, múltiples destinos</p>
<p>→ Auto-instrumentación sin cambiar código</p>
<p>→ Exporta a X-Ray + CloudWatch + Grafana + Dynatrace simultáneamente</p>
</div>
</div>
</div>
<div>
<div class="bg-slate-800 rounded-xl p-5 border border-slate-600">
<p class="text-white font-semibold mb-3">ADOT = AWS Distro for OpenTelemetry</p>
<p class="text-slate-300 text-sm mb-4">Distribución de OpenTelemetry soportada por AWS, lista para producción.</p>
<div class="space-y-2">
<div class="bg-slate-900 rounded-lg p-3 border border-slate-700">
<p class="text-emerald-400 text-xs font-medium">ADOT Collector</p>
<p class="text-slate-400 text-xs">Recibe, procesa y exporta telemetría</p>
</div>
<div class="bg-slate-900 rounded-lg p-3 border border-slate-700">
<p class="text-emerald-400 text-xs font-medium">ADOT Java Agent</p>
<p class="text-slate-400 text-xs">Instrumentación automática — 0 cambios en código</p>
</div>
<div class="bg-slate-900 rounded-lg p-3 border border-slate-700">
<p class="text-emerald-400 text-xs font-medium">EKS Add-on</p>
<p class="text-slate-400 text-xs">Gestionado por AWS, actualización automática</p>
</div>
</div>
</div>
</div>
</div>

---
layout: default
background: '#1e293b'
---

# ADOT Collector — Pipeline de telemetría

<div class="mt-4 flex items-center justify-center gap-3 pb-12">
<div class="bg-blue-950 rounded-xl p-4 border border-blue-700 text-center w-40">
<p class="text-blue-400 font-semibold text-sm">Receivers</p>
<p class="text-slate-300 text-xs mt-2">OTLP gRPC :4317</p>
<p class="text-slate-300 text-xs">OTLP HTTP :4318</p>
<p class="text-slate-300 text-xs">Prometheus</p>
</div>
<div class="text-white text-xl font-bold">→</div>
<div class="bg-amber-950 rounded-xl p-4 border border-amber-700 text-center w-44">
<p class="text-amber-400 font-semibold text-sm">Processors</p>
<p class="text-slate-300 text-xs mt-2">memory_limiter</p>
<p class="text-slate-300 text-xs">k8sattributes</p>
<p class="text-slate-300 text-xs">batch</p>
<p class="text-slate-300 text-xs">resource enrichment</p>
</div>
<div class="text-white text-xl font-bold">→</div>
<div class="bg-emerald-950 rounded-xl p-4 border border-emerald-700 text-center w-48">
<p class="text-emerald-400 font-semibold text-sm">Exporters</p>
<p class="text-slate-300 text-xs mt-2">→ AWS X-Ray (trazas)</p>
<p class="text-slate-300 text-xs">→ CloudWatch (logs + métricas)</p>
<p class="text-slate-300 text-xs">→ Prometheus/AMP (métricas)</p>
<p class="text-slate-300 text-xs">→ Dynatrace (todo)</p>
</div>
</div>

<div class="bg-slate-800 rounded-xl p-4 border border-slate-600 mt-2">
<p class="text-white text-sm font-semibold mb-2">💡 Clave: Dual/Triple Export</p>
<p class="text-slate-300 text-xs">Un solo Collector envía la misma señal a múltiples destinos. Tu app no sabe ni le importa a dónde van los datos — solo habla OTLP con el Collector.</p>
</div>

---
layout: default
background: '#1e293b'
---

# Auto-instrumentación — Sin tocar código

<div class="grid grid-cols-2 gap-6 mt-4 pb-12">
<div>
<p class="text-slate-300 text-sm mb-4">Con una sola annotation en el Deployment de Kubernetes, ADOT inyecta el Java Agent automáticamente.</p>
<div class="bg-slate-900 rounded-xl p-4 font-mono text-xs border border-emerald-900 leading-relaxed">
<span class="text-slate-500"># Solo agregar esta annotation:</span><br>
<span class="text-emerald-400">annotations</span>:<br>
&nbsp;&nbsp;<span class="text-amber-300">instrumentation.opentelemetry.io/inject-java</span>: <span class="text-emerald-400">"true"</span>
</div>
<div class="mt-4 bg-slate-800 rounded-lg p-4 border border-slate-700">
<p class="text-white text-xs font-semibold mb-2">¿Qué hace el Agent automáticamente?</p>
<div class="space-y-1 text-slate-300 text-xs">
<p>✅ Instrumenta Spring Boot, WebFlux, JDBC, HTTP clients</p>
<p>✅ Crea spans por cada operación</p>
<p>✅ Propaga trace context (W3C) en headers</p>
<p>✅ Enriquece logs con traceId y spanId</p>
<p>✅ Exporta todo vía OTLP al Collector</p>
</div>
</div>
</div>
<div>
<div class="bg-slate-800 rounded-xl p-5 border border-orange-700">
<p class="text-orange-400 text-xs font-semibold uppercase mb-3">Propagación de contexto</p>
<div class="bg-slate-900 rounded-lg p-3 font-mono text-xs leading-relaxed">
<span class="text-slate-300">Request Header:</span><br>
<span class="text-emerald-400">traceparent</span>: 00-<span class="text-amber-300">4bf92f35...</span>-<span class="text-blue-400">00f067aa...</span>-01<br><br>
<span class="text-slate-500">version-traceId-spanId-flags</span>
</div>
<p class="text-slate-300 text-xs mt-3">Cada servicio recibe el contexto y crea child spans. La traza completa se reconstruye automáticamente.</p>
</div>
<div class="mt-4 bg-slate-800 rounded-xl p-4 border border-slate-600">
<p class="text-white text-xs font-semibold mb-2">Muestreo inteligente (Tail-based)</p>
<div class="space-y-1 text-slate-300 text-xs">
<p>→ 100% de trazas con errores</p>
<p>→ 100% de trazas lentas (> p95)</p>
<p>→ 10% de trazas normales</p>
</div>
<p class="text-slate-400 text-xs mt-2">Reduce costos sin perder visibilidad de problemas.</p>
</div>
</div>
</div>

---
layout: default
background: '#1e293b'
---

# Arquitectura real — EKS + ADOT + Grafana

<div class="mt-4 flex items-center justify-center gap-3">
<div class="bg-slate-800 rounded-xl p-3 border border-slate-600 text-center w-32">
<div class="text-2xl mb-1">☸️</div>
<p class="text-white font-semibold text-xs">EKS Pods</p>
<p class="text-slate-400 text-xs">Java Agent</p>
</div>
<div class="text-white text-lg font-bold">→</div>
<div class="bg-slate-800 rounded-xl p-3 border border-cyan-700 text-center w-36">
<div class="text-2xl mb-1">📡</div>
<p class="text-cyan-400 font-semibold text-xs">ADOT Collector</p>
<p class="text-slate-400 text-xs">DaemonSet</p>
</div>
<div class="text-white text-lg font-bold">→</div>
<div class="space-y-2">
<div class="bg-orange-950 rounded-lg p-2 border border-orange-700 text-center w-36">
<p class="text-orange-400 text-xs font-semibold">X-Ray</p>
</div>
<div class="bg-purple-950 rounded-lg p-2 border border-purple-700 text-center w-36">
<p class="text-purple-400 text-xs font-semibold">CloudWatch</p>
</div>
<div class="bg-emerald-950 rounded-lg p-2 border border-emerald-700 text-center w-36">
<p class="text-emerald-400 text-xs font-semibold">AMP (Prometheus)</p>
</div>
</div>
<div class="text-white text-lg font-bold">→</div>
<div class="bg-slate-800 rounded-xl p-3 border border-amber-600 text-center w-36">
<div class="text-2xl mb-1">📊</div>
<p class="text-amber-400 font-semibold text-xs">Grafana</p>
<p class="text-slate-400 text-xs">Dashboards</p>
</div>
</div>

<div class="grid grid-cols-3 gap-4 mt-6 pb-12">
<div class="bg-slate-800 rounded-lg p-4 border border-slate-700">
<p class="text-cyan-400 text-xs font-semibold mb-2">ADOT como DaemonSet</p>
<p class="text-slate-300 text-xs">1 pod por nodo. Recibe telemetría de todos los pods del nodo vía hostPort :4317. Escala automáticamente con el cluster.</p>
</div>
<div class="bg-slate-800 rounded-lg p-4 border border-slate-700">
<p class="text-emerald-400 text-xs font-semibold mb-2">Amazon Managed Prometheus</p>
<p class="text-slate-300 text-xs">Almacena métricas en formato Prometheus. Sin servidores que gestionar. Acceso via VPC Endpoint (privado).</p>
</div>
<div class="bg-slate-800 rounded-lg p-4 border border-slate-700">
<p class="text-amber-400 text-xs font-semibold mb-2">Amazon Managed Grafana</p>
<p class="text-slate-300 text-xs">Dashboards enterprise. Auth via SSO. Datasources: Prometheus, CloudWatch, X-Ray. ~$9/usuario/mes.</p>
</div>
</div>

---
layout: center
background: '#1e293b'
---

# Bloque 6 — SLOs y Error Budget

<div class="mt-6 text-slate-300 text-lg">Medir la salud del servicio con objetivos claros</div>
<div class="mt-2 text-slate-500 text-sm">De "el sistema está arriba" a "el sistema cumple sus promesas"</div>

---
layout: default
background: '#1e293b'
---

# SLI → SLO → Error Budget

<div class="grid grid-cols-3 gap-5 mt-6 pb-12">
<div class="bg-slate-800 rounded-xl p-5 border border-blue-700">
<p class="text-blue-400 font-semibold text-sm mb-2">SLI</p>
<p class="text-slate-500 text-xs uppercase mb-2">Service Level Indicator</p>
<p class="text-slate-300 text-sm">La métrica que mides</p>
<div class="mt-3 bg-slate-900 rounded-lg p-3 border border-slate-700">
<p class="text-blue-300 text-xs font-mono">% requests con latencia &lt; 300ms</p>
</div>
</div>
<div class="bg-slate-800 rounded-xl p-5 border border-emerald-700">
<p class="text-emerald-400 font-semibold text-sm mb-2">SLO</p>
<p class="text-slate-500 text-xs uppercase mb-2">Service Level Objective</p>
<p class="text-slate-300 text-sm">El objetivo que te pones</p>
<div class="mt-3 bg-slate-900 rounded-lg p-3 border border-slate-700">
<p class="text-emerald-300 text-xs font-mono">95% de requests &lt; 300ms en 28 días</p>
</div>
</div>
<div class="bg-slate-800 rounded-xl p-5 border border-amber-700">
<p class="text-amber-400 font-semibold text-sm mb-2">Error Budget</p>
<p class="text-slate-500 text-xs uppercase mb-2">Margen de error permitido</p>
<p class="text-slate-300 text-sm">Cuánto puedes fallar</p>
<div class="mt-3 bg-slate-900 rounded-lg p-3 border border-slate-700">
<p class="text-amber-300 text-xs font-mono">5% = ~36h de degradación/mes</p>
</div>
</div>
</div>

<div class="bg-slate-800 rounded-xl p-4 border border-slate-600">
<p class="text-white text-sm font-semibold mb-2">¿Para qué sirve el Error Budget?</p>
<div class="grid grid-cols-4 gap-3 text-xs">
<div class="text-center">
<p class="text-emerald-400 font-semibold">&gt; 50%</p>
<p class="text-slate-300">Innovar</p>
</div>
<div class="text-center">
<p class="text-amber-400 font-semibold">20-50%</p>
<p class="text-slate-300">Precaución</p>
</div>
<div class="text-center">
<p class="text-red-400 font-semibold">&lt; 20%</p>
<p class="text-slate-300">Estabilizar</p>
</div>
<div class="text-center">
<p class="text-red-600 font-semibold">0%</p>
<p class="text-slate-300">Freeze total</p>
</div>
</div>
</div>

---
layout: default
background: '#1e293b'
---

# Golden Signals — Las 4 métricas clave

<div class="grid grid-cols-4 gap-4 mt-6 pb-12">
<div class="bg-slate-800 rounded-xl p-4 border border-emerald-700 text-center">
<div class="text-3xl mb-2">⏱</div>
<p class="text-emerald-400 font-semibold text-sm">Latencia</p>
<p class="text-slate-300 text-xs mt-2">¿Cuánto tarda?</p>
<p class="text-slate-400 text-xs mt-1">p50, p95, p99</p>
</div>
<div class="bg-slate-800 rounded-xl p-4 border border-blue-700 text-center">
<div class="text-3xl mb-2">📈</div>
<p class="text-blue-400 font-semibold text-sm">Tráfico</p>
<p class="text-slate-300 text-xs mt-2">¿Cuánta demanda?</p>
<p class="text-slate-400 text-xs mt-1">req/s, TPS</p>
</div>
<div class="bg-slate-800 rounded-xl p-4 border border-red-700 text-center">
<div class="text-3xl mb-2">❌</div>
<p class="text-red-400 font-semibold text-sm">Errores</p>
<p class="text-slate-300 text-xs mt-2">¿Cuánto falla?</p>
<p class="text-slate-400 text-xs mt-1">error rate %</p>
</div>
<div class="bg-slate-800 rounded-xl p-4 border border-amber-700 text-center">
<div class="text-3xl mb-2">🔥</div>
<p class="text-amber-400 font-semibold text-sm">Saturación</p>
<p class="text-slate-300 text-xs mt-2">¿Cuánto queda?</p>
<p class="text-slate-400 text-xs mt-1">CPU, mem, conns</p>
</div>
</div>

<div class="bg-emerald-950 rounded-xl p-4 border border-emerald-700">
<p class="text-emerald-300 text-sm font-semibold">💡 Alertas basadas en SLOs, no en umbrales estáticos</p>
<div class="grid grid-cols-2 gap-4 mt-2 text-xs">
<div>
<p class="text-red-400">❌ Mal: "CPU > 90% durante 5 min"</p>
</div>
<div>
<p class="text-emerald-400">✅ Bien: "Error budget consumido > 10% en 1 hora"</p>
</div>
</div>
</div>

---
layout: center
background: '#1e293b'
---

# Cierre — El camino completo

<div class="mt-6 space-y-4 max-w-2xl mx-auto">
<div class="flex items-center gap-4">
<div class="w-8 h-8 rounded-full bg-emerald-900 border border-emerald-600 flex items-center justify-center text-emerald-400 font-bold text-sm">1</div>
<p class="text-slate-300 text-sm">Entender los 3 pilares: Métricas, Logs, Trazas</p>
</div>
<div class="flex items-center gap-4">
<div class="w-8 h-8 rounded-full bg-emerald-900 border border-emerald-600 flex items-center justify-center text-emerald-400 font-bold text-sm">2</div>
<p class="text-slate-300 text-sm">Implementar con servicios AWS nativos (CloudWatch, X-Ray)</p>
</div>
<div class="flex items-center gap-4">
<div class="w-8 h-8 rounded-full bg-emerald-900 border border-emerald-600 flex items-center justify-center text-emerald-400 font-bold text-sm">3</div>
<p class="text-slate-300 text-sm">Evolucionar a OpenTelemetry + ADOT (estándar abierto)</p>
</div>
<div class="flex items-center gap-4">
<div class="w-8 h-8 rounded-full bg-emerald-900 border border-emerald-600 flex items-center justify-center text-emerald-400 font-bold text-sm">4</div>
<p class="text-slate-300 text-sm">Visualizar con Grafana + AMP (dashboards enterprise)</p>
</div>
<div class="flex items-center gap-4">
<div class="w-8 h-8 rounded-full bg-emerald-900 border border-emerald-600 flex items-center justify-center text-emerald-400 font-bold text-sm">5</div>
<p class="text-slate-300 text-sm">Medir con SLOs y Error Budget (cultura de confiabilidad)</p>
</div>
</div>

<div class="mt-8 text-slate-500 text-sm">Julian Isaza · Observabilidad en AWS</div>
