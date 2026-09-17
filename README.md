# 💼 Casos 2 y 3: Asistente Corporativo + Programación

Sistema de asistentes de IA para casos de uso corporativo (Caso 2) y generación de código (Caso 3).

## 📋 Casos de Uso

### **Caso 2: Asistente Corporativo**
- Respuesta a preguntas sobre documentación interna
- Análisis de documentos corporativos
- Asistencia en procesos operativos
- Base de conocimientos empresarial

**Modelo**: Qwen2.5-Coder-32B-Instruct

### **Caso 3: Asistente de Programación**
- Generación de código
- Revisión y debugging
- Explicación de código existente
- Refactoring y optimización

**Modelo**: Qwen2.5-Coder-32B-Instruct / Qwen3-Coder-30B-A3B (MoE)

## 📊 Métricas (Pruebas AWS g7e.2xlarge)

| Métrica | Valor |
|---------|-------|
| **Usuarios Concurrentes** | 30 |
| **Throughput** | ~150 tok/s |
| **Latency (p95)** | <2s first token |
| **VRAM** | ~32 GB (Coder-32B AWQ) |

## 🚀 Deploy

```bash
git clone https://github.com/shell931/caso-2-3-asis-prog.git
cd caso-2-3-asis-prog

# TODO: Agregar scripts de deploy
```

## 📁 Estructura (Planeada)

```
caso-2-3-asis-prog/
├── caso-2-asistente/
│   ├── docker-compose.yml
│   └── workers/
└── caso-3-codigo/
    ├── docker-compose.yml
    └── workers/
```

## 🔧 Tecnologías

- **LLM**: Qwen2.5-Coder-32B-Instruct-AWQ
- **LLM (MoE)**: Qwen3-Coder-30B-A3B
- **Inference**: vLLM
- **Deploy**: Docker Compose + AWS EC2

## 📖 Documentación

> 📝 **Nota**: Este repo está en construcción inicial. La documentación se agregará progresivamente.

---

**Última actualización**: Sep 17, 2026  
**Estado**: 🚧 En construcción
