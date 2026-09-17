# Caso 2: Asistente Corporativo con IA

## 🎯 Objetivo

Sistema de asistente de IA para responder preguntas sobre documentación interna, análisis de documentos corporativos y asistencia en procesos operativos.

## 🔧 Configuración Técnica

### **Hardware Probado**
- **Instancia AWS**: `g7e.2xlarge`
- **GPU**: 1× NVIDIA RTX PRO 6000 Ada (48 GB VRAM)
- **CPU**: 8 vCPUs
- **RAM**: 64 GB

### **Modelo**
- **LLM**: `Qwen/Qwen2.5-Coder-32B-Instruct-AWQ`
- **Inference Server**: vLLM (OpenAI-compatible API)
- **Cuantización**: AWQ (4-bit)
- **VRAM Utilizada**: ~32 GB

## 📊 Resultados de Pruebas

### **Métricas de Rendimiento**

| Métrica | Valor |
|---------|-------|
| **Usuarios Concurrentes** | 30 ✅ |
| **Throughput** | ~150 tok/s |
| **Latency (TTFT)** | <2s first token |
| **Latency (p95)** | ~3.5s |
| **VRAM Peak** | 32 GB / 48 GB |
| **GPU Utilization** | ~65% |

### **Casos de Uso Validados**

1. ✅ **Búsqueda en Documentación Interna**
   - Respuestas contextuales sobre políticas y procedimientos
   - Integración con RAG (Retrieval Augmented Generation)

2. ✅ **Análisis de Documentos**
   - Resumen de contratos y acuerdos
   - Extracción de información clave

3. ✅ **Asistencia Operativa**
   - Guía paso a paso en procesos
   - Resolución de dudas frecuentes

## 🚀 Arquitectura del Sistema

```
Usuario → API Gateway → vLLM Server (GPU) → Base de Conocimientos (RAG)
                            ↓
                     Vector DB (Qdrant)
                            ↓
                    Documentos Corporativos
```

## 📦 Deploy en AWS

### **1. Levantar vLLM Server**

```bash
docker run -d \
  --gpus all \
  --name asistente-corporativo \
  -p 8000:8000 \
  -v /data/hf-cache:/root/.cache/huggingface \
  vllm/vllm-openai:latest \
  --model Qwen/Qwen2.5-Coder-32B-Instruct-AWQ \
  --gpu-memory-utilization 0.90 \
  --max-model-len 8192 \
  --max-num-seqs 32
```

### **2. Configurar Base de Conocimientos**

```bash
# Usar Open WebUI con Qdrant
docker-compose up -d qdrant open-webui
```

### **3. Probar el Sistema**

```bash
curl http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Qwen/Qwen2.5-Coder-32B-Instruct-AWQ",
    "messages": [
      {"role": "user", "content": "¿Cuál es la política de vacaciones?"}
    ]
  }'
```

## 💡 Mejores Prácticas

1. **Prompt Engineering**
   - Definir rol del asistente claramente
   - Usar ejemplos (few-shot learning)
   - Limitar scope a documentación corporativa

2. **RAG (Retrieval)**
   - Embeddings de calidad (multilingual)
   - Chunk size óptimo: 512 tokens
   - Top-K: 3-5 documentos relevantes

3. **Seguridad**
   - Autenticación en API
   - Rate limiting por usuario
   - Logs de auditoría

## 🎓 Ejemplo de Prompt

```python
SYSTEM_PROMPT = """Eres un asistente corporativo de [Nombre Empresa].

Tu rol es ayudar a empleados con:
- Políticas y procedimientos internos
- Análisis de documentos corporativos
- Guía en procesos operativos

Reglas:
1. Usa solo información de la documentación oficial
2. Si no sabes algo, di "No tengo esa información"
3. Sé conciso y profesional
4. Cita fuentes cuando sea posible

Base de conocimientos disponible: [lista de documentos]
"""
```

## 📈 Escalamiento

### **Para más de 30 usuarios:**

| Usuarios | Hardware | Modelo |
|----------|----------|--------|
| 30 | g7e.2xlarge (1 GPU) | Qwen-32B-AWQ |
| 60 | g7e.12xlarge (2 GPUs) | Qwen-32B-AWQ × 2 |
| 100+ | g7e.12xlarge (2 GPUs) | Qwen-14B-AWQ × 2 + Load Balancer |

## 🔍 Monitoreo

**Dashboards recomendados:**
- vLLM metrics (Prometheus)
- GPU utilization (nvidia-smi)
- Latency por endpoint
- Tasa de error / fallback

---

**Última actualización**: Sep 17, 2026  
**Estado**: ✅ Validado en producción  
**Contacto**: Equipo IA
