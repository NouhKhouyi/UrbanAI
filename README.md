# UrbanAI 

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![Neo4j](https://img.shields.io/badge/Neo4j-GraphDB-green.svg)](https://neo4j.com/)
[![Ollama](https://img.shields.io/badge/LLMs-Ollama-orange.svg)](https://ollama.ai/)
[![Telegram Bot](https://img.shields.io/badge/Telegram-Bot-blue.svg)](https://core.telegram.org/bots)

*UrbanAI* es una solución desarrollada durante el Samsung Innovation Campus para facilitar la gestión colaborativa de comunidades urbanas (por ej. huertos urbanos).
Convierte mensajes de texto o voz en *tareas y alertas estructuradas, integrando **ML, LLMs, procesamiento de lenguaje natural y grafos de conocimiento*.

---

## Nota sobre el código fuente

Este repositorio *no contiene el código fuente* por motivos de *confidencialidad, ya que el proyecto fue una colaboración con **Agro4Data*.
La siguiente documentación describe la *arquitectura técnica* y los *componentes principales*, para dejar reflejado el trabajo técnico realizado.

---

## Arquitectura técnica

### 1. Interfaz de usuario: Bot de Telegram

- Reportes en *texto* o *notas de voz*.
- Funcionalidades:
  - Transcripción automática de audios (Whisper).
  - Almacenamiento de audios en *Supabase* (para un futuro fine-tuning).
  - Geolocalización vía OpenStreetMaps.
  - Consulta del clima con WeatherAPI.
- Devuelve al usuario un *resumen inmediato* de tareas y alertas detectadas, además de un mensaje enriquecido con el uso de las APIs mencionadas.

### 2. Clasificación de relevancia (ML)

- Modelo *Random Forest* filtra mensajes irrelevantes antes de enviar a los LLMs.
- Precisión: *94%* en pruebas internas.

### 3. Extracción semántica (LLMs)

- Modelos *Gemma* ejecutados vía Ollama.
- Detectan:
  - *Tareas* asignadas a roles comunitarios (por ej., Presidente, Técnico de Mantenimiento, Agricultor).
  - *Alertas* sin acción inmediata.
- Salida JSON estructurada:

  json
  {
    "tareas": [ {"tarea": "Revisar compostera", "rol": "Agricultor"} ],
    "alertas": [ "Posible fuga en sistema de riego" ]
  }
  

## 4. Almacenamiento y consultas (Neo4j)

- Base de datos de grafos con nodos de:

  - Miembros
  - Comunidades
  - Materiales
  - Especializaciones
  - Tareas
  - Alertas
- Los LLMs también generan consultas *Cypher* automáticas para Neo4j:

json
{
  "query": "MATCH (t:Tarea {Prioridad:'Alta'})-[:Realiza]->(m:Miembro) RETURN ...",
  "params": {}
}



## 5. Pipeline de integración

El módulo central **PipelineHuerto** coordina:

- Clasificación ML
- Extracción de tareas/alertas con LLMs
- Inserción en Neo4j
- Exportación opcional a CSV para histórico

---

## Flujo de datos

<img width="2740" height="1047" alt="image" src="https://github.com/user-attachments/assets/06bb51a2-3745-4cf4-b4a1-3c9e516906ba" />


## Futuras mejoras

- Procesar audios históricos en la nube con Whisper.
- Incorporar *bases de datos vectoriales* para búsquedas semánticas.
- Añadir *agentes inteligentes* que coordinen recursos adicionales.


---

## Autores

Proyecto desarrollado por:

- *Fernando Martínez Gómez*
- *Nouh Khouyi Etbar*
- *Imad Rifai*

📍 Samsung Innovation Campus · 2025
