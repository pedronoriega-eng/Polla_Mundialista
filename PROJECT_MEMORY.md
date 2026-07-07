# Memoria del Proyecto - Sistema de Apuestas (Quiniela) Colombia vs. Suiza

Este documento registra las especificaciones de diseño, estructura de datos y arquitectura para la aplicación web de apuestas deportivas.

## Estructura de la Base de Datos (JSON Schema)

La aplicación utiliza un servicio backend Serverless (Supabase o Firebase) para registrar cada apuesta realizada por los usuarios. A continuación se detalla el esquema de datos para la colección o tabla `apuestas`.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Apuesta",
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "description": "Identificador único de la apuesta (autogenerado por el BaaS)."
    },
    "nombre_apostador": {
      "type": "string",
      "description": "Nombre completo del usuario que realiza la apuesta.",
      "minLength": 3
    },
    "pronostico": {
      "type": "object",
      "description": "El marcador exacto pronosticado para el partido.",
      "properties": {
        "goles_colombia": {
          "type": "integer",
          "minimum": 0,
          "description": "Goles pronosticados para Colombia"
        },
        "goles_congo": {
          "type": "integer",
          "minimum": 0,
          "description": "Goles pronosticados para República del Congo (Mantenido para compatibilidad de registros previos)"
        },
        "goles_portugal": {
          "type": "integer",
          "minimum": 0,
          "description": "Goles pronosticados para Portugal (Mantenido para compatibilidad de registros previos)"
        },
        "goles_suiza": {
          "type": "integer",
          "minimum": 0,
          "description": "Goles pronosticados para Suiza"
        },
        "ganador": {
          "type": "string",
          "enum": ["Colombia", "Suiza", "Portugal", "República del Congo", "Empate"],
          "description": "Cálculo directo del equipo ganador según los goles."
        }
      },
      "required": ["goles_colombia", "goles_congo", "goles_portugal", "goles_suiza", "ganador"]
    },
    "fecha_apuesta": {
      "type": "string",
      "format": "date-time",
      "description": "Marca de tiempo (ISO 8601) de cuándo se registró la apuesta."
    }
  },
  "required": ["nombre_apostador", "pronostico", "fecha_apuesta"]
}
```

### Ejemplo de Registro Completo

```json
{
  "id": "apuesta_78a2d1f9-03b5-4cde-a178-e8cb9b6a98fb",
  "nombre_apostador": "Pedro Pérez",
  "pronostico": {
    "goles_colombia": 3,
    "goles_congo": 1,
    "goles_portugal": 1,
    "goles_suiza": 1,
    "ganador": "Colombia"
  },
  "fecha_apuesta": "2026-06-18T08:05:00-05:00"
}
```

## Arquitectura del Proyecto

```
sistema-apuestas/
├── PROJECT_MEMORY.md       # Memoria técnica y especificación del JSON
├── index.html              # Gate de pago e ingreso (Acceso con contraseña Apuesta_123)
├── app.html                # Formulario principal de apuesta (Tarjeta de partido)
├── dashboard.html          # Visualización de apuestas y liquidación (Contraseña Pnoriega_123)
└── css/
    └── style.css           # Estilos patrios unificados (Colombia/Suiza Theme)
```

