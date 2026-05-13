---
theme: default
background: '#0f172a'
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
---

# Observabilidad en AWS

**Logs · Métricas · Trazas**

<p class="text-slate-400 mt-4 text-lg">Una guía práctica para entender el monitoreo moderno<br>en sistemas distribuidos</p>

<div class="mt-12 text-slate-500 text-sm">DevOps Journey · Bloque 1 & 2</div>

---
layout: center
background: '#0f172a'
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
background: '#0f172a'
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
background: '#0f172a'
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
background: '#0f172a'
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
background: '#0f172a'
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
background: '#0f172a'
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
background: '#0f172a'
---

# Pilar 1 — Métricas 📊

<div class="grid grid-cols-2 gap-6 mt-6">

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
background: '#0f172a'
---

# Pilar 2 — Logs 📝

<div class="grid grid-cols-2 gap-6 mt-6">

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
background: '#0f172a'
---

# Pilar 3 — Trazas 🔍

<div class="grid grid-cols-2 gap-6 mt-6">

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
background: '#0f172a'
---

# El flujo completo de debugging

<div class="mt-4 space-y-3">

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
background: '#0f172a'
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
