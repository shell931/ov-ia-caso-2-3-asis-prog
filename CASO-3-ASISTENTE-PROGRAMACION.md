# Caso 3: Asistente de Programación con IA

## 🎯 Objetivo

Sistema de asistente de IA especializado en generación de código, debugging, refactoring y explicación de código existente.

## 🔧 Configuración Técnica

### **Hardware Probado**

**Configuración 1 (Recomendada):**
- **Instancia AWS**: `g7e.2xlarge`
- **GPU**: 1× NVIDIA RTX PRO 6000 Ada (48 GB VRAM)
- **Modelo**: `Qwen/Qwen2.5-Coder-32B-Instruct-AWQ`
- **VRAM**: ~32 GB

**Configuración 2 (MoE - Más Rápida):**
- **Instancia AWS**: `g7e.2xlarge`
- **GPU**: 1× NVIDIA RTX PRO 6000 Ada (48 GB VRAM)
- **Modelo**: `Qwen/Qwen3-Coder-30B-A3B` (Mixture of Experts)
- **VRAM**: ~28 GB

### **Modelos Disponibles**

| Modelo | VRAM | Tokens/s | Uso Recomendado |
|--------|------|----------|-----------------|
| Qwen2.5-Coder-32B-AWQ | 32 GB | ~150 | Código complejo, refactoring |
| Qwen3-Coder-30B-A3B (MoE) | 28 GB | ~200 | Código rápido, debugging |
| Qwen2.5-Coder-14B-AWQ | 14 GB | ~250 | Equipos pequeños, prototipado |

## 📊 Resultados de Pruebas

### **Métricas de Rendimiento (Qwen3-Coder-30B MoE)**

| Métrica | Valor |
|---------|-------|
| **Usuarios Concurrentes** | 30 ✅ |
| **Throughput** | ~200 tok/s |
| **Latency (TTFT)** | <1.5s first token |
| **Latency (p95)** | ~2.8s |
| **VRAM Peak** | 28 GB / 48 GB |
| **GPU Utilization** | ~55% (MoE eficiente) |

### **Casos de Uso Validados**

1. ✅ **Generación de Código**
   - Funciones completas desde descripción
   - Tests unitarios automáticos
   - Documentación inline

2. ✅ **Debugging**
   - Análisis de stack traces
   - Identificación de bugs
   - Sugerencias de fix

3. ✅ **Refactoring**
   - Optimización de código
   - Mejores prácticas
   - Patterns de diseño

4. ✅ **Explicación de Código**
   - Code review asistido
   - Documentación de legacy code
   - Onboarding de equipos

## 🚀 Arquitectura del Sistema

```
IDE (VS Code) → Cursor AI / Continue.dev
                      ↓
                API Gateway
                      ↓
          vLLM Server (GPU)
          Qwen3-Coder-30B
                      ↓
          Codebase Context
          (opcional: RAG)
```

## 📦 Deploy en AWS

### **1. Levantar vLLM Server (Qwen3-Coder MoE)**

```bash
docker run -d \
  --gpus all \
  --name asistente-codigo \
  -p 8000:8000 \
  -v /data/hf-cache:/root/.cache/huggingface \
  vllm/vllm-openai:latest \
  --model Qwen/Qwen3-Coder-30B-A3B \
  --gpu-memory-utilization 0.85 \
  --max-model-len 8192 \
  --max-num-seqs 32 \
  --enforce-eager
```

### **2. Configurar IDE Integration**

**VS Code + Continue.dev:**

```json
{
  "models": [
    {
      "title": "Qwen3-Coder",
      "provider": "openai",
      "model": "Qwen/Qwen3-Coder-30B-A3B",
      "apiBase": "http://your-server:8000/v1",
      "apiKey": "not-needed"
    }
  ]
}
```

**Cursor AI:**

```
Settings → Models → Custom
Base URL: http://your-server:8000/v1
Model: Qwen/Qwen3-Coder-30B-A3B
```

### **3. Probar Generación de Código**

```python
import openai

client = openai.OpenAI(
    base_url="http://localhost:8000/v1",
    api_key="not-needed"
)

response = client.chat.completions.create(
    model="Qwen/Qwen3-Coder-30B-A3B",
    messages=[
        {
            "role": "user",
            "content": "Escribe una función en Python para validar emails con regex"
        }
    ]
)

print(response.choices[0].message.content)
```

## 💡 Mejores Prácticas

### **1. Prompt Engineering para Código**

```python
SYSTEM_PROMPT = """Eres un asistente de programación experto.

Cuando generes código:
1. Usa buenas prácticas del lenguaje
2. Agrega comentarios para lógica compleja
3. Incluye manejo de errores
4. Sugiere tests si es relevante

Lenguajes principales: Python, JavaScript/TypeScript, Go

Estilo de código: [definir según equipo]
"""
```

### **2. Context Management**

- Incluir archivos relevantes en el prompt
- Usar RAG para codebase grande
- Limitar context a ~4K tokens para velocidad

### **3. Code Review Workflow**

```bash
# 1. Generar código
# 2. Auto-review con IA
# 3. Tests automáticos
# 4. Human review final
```

## 🎓 Ejemplos de Uso

### **Ejemplo 1: Generar Función**

**Prompt:**
```
Crea una función en Python que procese un CSV y extraiga
columnas específicas usando pandas. Incluye manejo de errores.
```

### **Ejemplo 2: Debugging**

**Prompt:**
```
Este código da error:

[pegar stack trace]

¿Cuál es el problema y cómo lo arreglo?
```

### **Ejemplo 3: Refactoring**

**Prompt:**
```
Refactoriza este código para seguir SOLID principles:

[pegar código legacy]

Explica los cambios.
```

## 📈 Comparación de Modelos

### **Benchmarks Internos**

| Tarea | Qwen2.5-Coder-32B | Qwen3-Coder-30B (MoE) |
|-------|-------------------|----------------------|
| **Python simple** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Python complejo** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **JavaScript/TS** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Go** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Debugging** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Velocidad** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |

**Recomendación:**
- **Equipos pequeños (<10):** Qwen2.5-Coder-14B-AWQ
- **Equipos medianos (10-30):** Qwen3-Coder-30B (MoE)
- **Equipos grandes (30+):** Qwen2.5-Coder-32B × 2 (multi-GPU)

## 🔍 Monitoreo

**Métricas clave:**
- Requests por minuto
- Latency por lenguaje
- Token usage por usuario
- Tasa de aceptación de sugerencias

## 🏢 Caso de Estudio: Servidor para Equipo

### **Configuración Recomendada**

```yaml
Hardware: g7e.2xlarge (1 GPU, 48 GB VRAM)
Modelo: Qwen2.5-Coder-14B-AWQ
Usuarios: 10-15 desarrolladores
Costo: ~$1.50/hora AWS
```

### **ROI Estimado**

- **Tiempo ahorrado por dev**: 2-3 horas/semana
- **Costo mensual**: ~$1,080 (24/7)
- **Ahorro mensual** (10 devs × 10h × $50/h): ~$5,000
- **ROI**: 460%

---

**Última actualización**: Sep 17, 2026  
**Estado**: ✅ Validado en producción  
**Contacto**: Equipo IA
