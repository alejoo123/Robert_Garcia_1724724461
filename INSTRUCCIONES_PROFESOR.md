# 👨‍🏫 INSTRUCCIONES PARA EL PROFESOR

## ✅ Calificación Automática en 1 Comando

Este proyecto está diseñado para que califiques a **TODOS** los estudiantes ejecutando **un solo comando**.

---

## 🚀 Cómo Calificar

### Paso 1: Posicionarse en la rama main

```bash
cd /Users/andrescobena/Downloads/examen-abdd-2025-2
git checkout main
```

### Paso 2: Ejecutar calificación automática

```bash
./calificar_todos.sh
```

**Eso es todo** ✅

El script:
1. Hace `git fetch --all` para obtener todas las ramas
2. Encuentra todas las ramas `student/*`
3. Por cada estudiante:
   - Cambia a su rama
   - Levanta su docker-compose
   - Valida la arquitectura
   - Genera su calificación
   - Limpia el ambiente
   - Vuelve a main
4. Genera reportes consolidados

**Tiempo:** ~2-3 minutos por estudiante

---

## 📊 Sistema de Puntuación

### Arquitectura (100 puntos) - Automático

| Sección | Puntos | Qué Valida |
|---------|--------|------------|
| Docker Compose | 30 | Archivo, sintaxis, servicios |
| Contenedores | 25 | 4 contenedores corriendo |
| Bases de Datos | 20 | Conexiones y tablas |
| SymmetricDS | 25 | Tablas sym_* y configuración |
| **TOTAL** | **100** | |

### Evidencias de Replicación - Manual

Los estudiantes deben incluir en su rama una carpeta `evidencias/` con:
- 📸 5 capturas de pantalla
- 📄 README.md explicando cada captura

**Capturas requeridas:**
1. Arquitectura corriendo (`docker compose ps`)
2. INSERT PostgreSQL → MySQL
3. INSERT MySQL → PostgreSQL
4. UPDATE bidireccional
5. DELETE bidireccional

---

## 📁 Archivos Generados

Después de ejecutar `./calificar_todos.sh`, se crea un directorio `resultados_[timestamp]/` con:

```
resultados_20260128_133118/
├── calificaciones.json          ← JSON consolidado ⭐
├── calificaciones.csv           ← CSV para Excel
├── RESUMEN.txt                  ← Resumen legible
└── andres_cobena_1313368928.log ← Log individual
```

### JSON Format

```json
{
  "fecha": "2026-01-28T18:31:18-05:00",
  "estudiantes": [
    {
      "nombre": "andres cobena",
      "cedula": "1313368928",
      "rama": "student/andres_cobena_1313368928",
      "calificacion": {
        "total": 95,
        "nota": "A - Excelente",
        "aprobado": true
      },
      "desglose": {
        "docker_compose": { "obtenido": 30, "maximo": 30 },
        "contenedores": { "obtenido": 25, "maximo": 25 },
        "bases_datos": { "obtenido": 20, "maximo": 20 },
        "symmetricds": { "obtenido": 20, "maximo": 25 }
      },
      "detalles": {
        "tests_pasados": 15,
        "tests_totales": 16,
        "tablas_negocio": 4,
        "tablas_symmetricds_pg": 46,
        "tablas_symmetricds_mysql": 46,
        "servicios_docker": 4
      }
    }
  ],
  "estadisticas": {
    "total_estudiantes": 1,
    "aprobados": 1,
    "reprobados": 0,
    "promedio": 95.00,
    "porcentaje_aprobados": 100.00
  }
}
```

---

## 📋 Checklist de Evaluación

### Automático (con script):
- [x] Arquitectura Docker Compose
- [x] Contenedores corriendo
- [x] Bases de datos funcionando
- [x] SymmetricDS configurado

### Manual (revisar en la rama):
- [ ] Carpeta `evidencias/` existe
- [ ] 5 capturas de pantalla presentes
- [ ] README.md en evidencias/ con explicaciones
- [ ] Las capturas muestran replicación bidireccional funcionando

---

## 🎯 Escala de Calificación

**Arquitectura (Automático):**
- **90-100**: Excelente (A)
- **80-89**: Bueno (B)
- **70-79**: Aceptable (C)
- **60-69**: Suficiente (D)
- **<60**: Insuficiente (F)

**Evidencias (Manual):**
Si no presenta evidencias o están incompletas: descuento del 20%

---

## 🔄 Proceso Completo

```bash
# 1. Calificar arquitectura (automático)
./calificar_todos.sh

# 2. Revisar evidencias (manual)
git checkout student/nombre_apellido_cedula
ls -la evidencias/

# 3. Si evidencias están completas: mantener calificación
# 4. Si evidencias faltan: aplicar descuento

# 5. Volver a main
git checkout main
```

---

## 📊 Ejemplo de Uso

```bash
$ ./calificar_todos.sh

╔═══════════════════════════════════════════════════════════════╗
║    SISTEMA DE CALIFICACIÓN AUTOMÁTICA MASIVA - ABDD          ║
╚═══════════════════════════════════════════════════════════════╝

✓ Encontradas 3 rama(s)
  • student/juan_perez_0123456789
  • student/maria_lopez_0987654321
  • student/andres_cobena_1313368928

[Estudiante 1 / 3]
Calificando: juan perez (0123456789)
  ✓ Docker Compose: 30/30
  ✓ Contenedores: 25/25
  ✓ Bases de Datos: 20/20
  ✓ SymmetricDS: 25/25
Resultado: 100 / 100 pts - A - Excelente

[... más estudiantes ...]

╔═══════════════════════════════════════════════════════════════╗
║         ✓ CALIFICACIÓN COMPLETADA                            ║
╚═══════════════════════════════════════════════════════════════╝

Estudiantes: 3 | Aprobados: 3 | Reprobados: 0

📁 Resultados: resultados_20260128_133118/
```

---

## ⚡ Comandos Útiles

### Ver todas las ramas de estudiantes

```bash
git fetch --all
git branch -r | grep student/
```

### Revisar una rama específica

```bash
git checkout student/nombre_apellido_cedula
ls -la
cat docker-compose.yml
```

### Revisar evidencias

```bash
git checkout student/nombre_apellido_cedula
ls -la evidencias/
cat evidencias/README.md
```

### Ver logs de Docker (si necesitas debug)

```bash
git checkout student/nombre_apellido_cedula
docker compose up -d
docker compose logs
```

---

## 📝 Registrar Calificaciones

```bash
# Importar CSV a Excel/Google Sheets
open resultados_*/calificaciones.csv

# O procesar JSON
cat resultados_*/calificaciones.json | jq '.estudiantes[] | {nombre, cedula, total: .calificacion.total}'
```

---

## ✅ Resumen

**Para calificar a TODOS:**
```bash
./calificar_todos.sh
```

**Resultado:**
- JSON con todas las calificaciones
- CSV para importar
- Reportes individuales
- Resumen consolidado

**Tiempo:** 2-3 minutos por estudiante (automático)

---

Fecha: Enero 2026
Versión: 1.0
