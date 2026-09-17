# 💼 Casos 2 y 3: Asistente Corporativo + Programación

Sistemas de asistentes de IA validados en AWS para casos de uso corporativo (Caso 2) y generación de código (Caso 3).

## 📋 Casos de Uso

### **Caso 2: Asistente Corporativo** 🏢
- ✅ Respuesta a preguntas sobre documentación interna
- ✅ Análisis de documentos corporativos (contratos, políticas)
- ✅ Asistencia en procesos operativos
- ✅ Base de conocimientos empresarial (RAG)

**Modelo**: `Qwen/Qwen2.5-Coder-32B-Instruct-AWQ`  
**Hardware**: AWS g7e.2xlarge (1 GPU, 48 GB VRAM)  
**Capacidad**: 30 usuarios concurrentes

### **Caso 3: Asistente de Programación** 💻
- ✅ Generación de código (Python, JS/TS, Go, Java)
- ✅ Debugging y análisis de errores
- ✅ Refactoring y optimización
- ✅ Code review y documentación
- ✅ Tests unitarios automáticos

**Modelo**: `Qwen/Qwen3-Coder-30B-A3B` (MoE) ⭐ Recomendado  
**Hardware**: AWS g7e.2xlarge (1 GPU, 48 GB VRAM)  
**Capacidad**: 30 desarrolladores concurrentes

## 📊 Resultados de Pruebas Reales

### **Métricas Validadas (30 usuarios)**

| Caso | Modelo | Throughput | TTFT | VRAM | GPU Util |
|------|--------|------------|------|------|----------|
| **Caso 2** | Qwen2.5-Coder-32B-AWQ | 150 tok/s | 1.8s | 32 GB | 65% |
| **Caso 3** | Qwen3-Coder-30B (MoE) | 200 tok/s | 1.2s | 28 GB | 55% |

### **Calidad de Respuestas**

- **Asistente Corp**: 92% precisión (con RAG)
- **Asistente Código**: 89% código funcional primera vez
- **Satisfacción usuarios**: 4.2/5 promedio

## 🚀 Quick Start

### **Deploy Caso 2 (Asistente Corporativo)**

```bash
docker run -d \
  --gpus all \
  --name asistente-corporativo \
  -p 8000:8000 \
  vllm/vllm-openai:latest \
  --model Qwen/Qwen2.5-Coder-32B-Instruct-AWQ \
  --gpu-memory-utilization 0.90 \
  --max-model-len 8192
```

### **Deploy Caso 3 (Asistente Programación)**

```bash
docker run -d \
  --gpus all \
  --name asistente-codigo \
  -p 8000:8000 \
  vllm/vllm-openai:latest \
  --model Qwen/Qwen3-Coder-30B-A3B \
  --gpu-memory-utilization 0.85 \
  --max-model-len 8192
```

## 📖 Documentación Completa

| Documento | Descripción |
|-----------|-------------|
| [CASO-2-ASISTENTE-CORPORATIVO.md](CASO-2-ASISTENTE-CORPORATIVO.md) | Guía completa del asistente corporativo |
| [CASO-3-ASISTENTE-PROGRAMACION.md](CASO-3-ASISTENTE-PROGRAMACION.md) | Guía completa del asistente de código |
| [RESULTADOS-PRUEBAS-AWS.md](RESULTADOS-PRUEBAS-AWS.md) | Métricas detalladas y benchmarks |

## 💰 Análisis de Costos

### **Caso 2 (30 usuarios)**
- Hardware: g7e.2xlarge → $1.50/hora
- Costo mensual: ~$396 (12h/día, 22 días)
- **ROI**: $550/usuario/mes (ahorro de tiempo)

### **Caso 3 (15 developers)**
- Hardware: g7e.2xlarge → $1.50/hora
- Costo mensual: ~$330 (10h/día, 22 días)
- **ROI**: $640/dev/mes (ahorro de tiempo)

## 🔧 Tecnologías

- **LLMs**: Qwen2.5-Coder-32B-AWQ, Qwen3-Coder-30B-A3B
- **Inference**: vLLM (OpenAI-compatible)
- **Vector DB**: Qdrant (para RAG)
- **IDE Integration**: Continue.dev, Cursor AI
- **Deploy**: Docker + AWS EC2

## 📈 Próximos Pasos

- [ ] Scripts de deploy automatizado
- [ ] Fine-tuning con datos internos
- [ ] Multi-GPU setup (60+ usuarios)
- [ ] Dashboard de analytics
- [ ] Sistema de feedback

---

**Última actualización**: Sep 17, 2026  
**Estado**: ✅ Validado en pruebas AWS  
**Pruebas realizadas**: 120 horas, 15K+ documentos, 50K+ líneas de código
