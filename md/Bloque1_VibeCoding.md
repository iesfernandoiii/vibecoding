# Bloque 1. Preparar el entorno y documentar el proyecto

> Guía práctica para 2.º de Bachillerato. Proyecto conductor: **Agenda Telefónica en Python**.

## 1. Objetivos

Al terminar este bloque podrás preparar un proyecto nuevo, organizar sus carpetas, redactar sus archivos de memoria y dejarlo listo para trabajar durante varias sesiones. Todavía no programaremos la agenda completa. Primero construiremos un contexto estable para que tanto el alumnado como los agentes sepan qué se quiere conseguir.

## 2. Qué significa programar con agentes

Un agente de programación puede leer archivos del proyecto, proponer un plan, editar código y ejecutar herramientas. Esto no convierte su respuesta en correcta. La persona continúa siendo responsable de los requisitos, de la revisión y de las pruebas.

El ciclo que usaremos en toda la unidad es:

```text
IDEA -> DOCUMENTACIÓN -> PLAN -> REVISIÓN -> ACT -> PRUEBAS -> COMMIT
```

Una petición como `hazme una agenda` es insuficiente. Una petición útil indica el objetivo, los límites, el nivel de Python y el resultado esperado.

```text
Estamos creando una agenda telefónica por consola para 2.º de Bachillerato.
Usa programación estructurada y funciones sencillas. No uses clases.
Antes de modificar archivos, lee README.md y AGENTS.md.
No programes todavía: resume los requisitos y señala cualquier duda.
```

## 3. Herramientas

- **Visual Studio Code**: editor y centro de trabajo.
- **Python**: lenguaje del proyecto.
- **Git**: historial local de versiones.
- **GitHub**: alojamiento remoto, que se abordará después.
- **GitHub Copilot Free**: ayuda integrada con límites de uso.
- **Cline con OpenRouter**: agente y proveedor de modelos. Los modelos gratuitos pueden cambiar, por lo que se elegirá uno marcado como gratuito en el momento de la clase.
- **Antigravity**: alternativa de agente o acceso gratuito cuando esté disponible en el centro.

> Captura sugerida 1: barra lateral de VS Code con Explorador, Búsqueda, Control de código fuente y extensiones.

## 4. Comprobar Python

Crea una carpeta temporal, ábrela con **Archivo > Abrir carpeta**, crea `hola.py` y escribe:

```python
print("Hola, mundo")
```

Pulsa el botón de ejecución de Python o abre el terminal integrado y ejecuta:

```bash
python hola.py
```

En algunos equipos el comando puede ser `python3 hola.py`. El resultado esperado es `Hola, mundo`.

## 5. Crear la carpeta del proyecto

Crea una carpeta llamada `agenda-telefonica` fuera de Descargas y fuera de otro repositorio Git. Ábrela directamente en VS Code. No abras solo `src`, porque Git y los agentes deben ver el proyecto completo.

Estructura inicial:

```text
agenda-telefonica/
├── README.md
├── AGENTS.md
├── TODO.md
├── SESSION_LOG.md
├── CHANGELOG.md
├── .gitignore
├── requirements.txt
├── src/
│   └── __init__.py
├── tests/
│   └── __init__.py
├── data/
└── docs/
```

`src` contendrá el programa; `tests`, las pruebas; `data`, datos creados por el programa; `docs`, material adicional. Los archivos `__init__.py` facilitan las importaciones de Python.

## 6. README.md listo para copiar

```markdown
# Agenda Telefónica

## Objetivo
Crear una aplicación de consola en Python para gestionar contactos.

## Funcionalidades de la versión 1
- Añadir contactos.
- Buscar contactos por nombre.
- Eliminar contactos.
- Mostrar todos los contactos.
- Guardar y cargar contactos desde un fichero de texto.

## Restricciones didácticas
- Programación estructurada.
- Funciones pequeñas y nombres descriptivos.
- Sin clases en la versión inicial.
- Código comprensible para alumnado de 2.º de Bachillerato.

## Estructura
- `src/`: código del programa.
- `tests/`: pruebas automáticas.
- `data/`: datos de ejecución.
- `docs/`: documentación adicional.

## Ejecución
```bash
python -m src.agenda
```

## Pruebas
```bash
python -m pytest
```

## Estado
Preparación inicial.
```

## 7. AGENTS.md listo para copiar

`AGENTS.md` no sustituye a `README.md`. El README explica el producto; AGENTS establece cómo debe trabajar un agente.

```markdown
# Instrucciones para agentes

## Contexto
Proyecto educativo de 2.º de Bachillerato. El alumnado conoce programación estructurada en Python.

## Reglas obligatorias
1. Lee `README.md`, `TODO.md` y `SESSION_LOG.md` antes de proponer cambios.
2. No programes si se solicita un plan.
3. Usa funciones sencillas, nombres descriptivos y constantes cuando proceda.
4. No uses clases, bases de datos, decoradores ni técnicas avanzadas sin autorización.
5. No añadas dependencias sin explicar su necesidad.
6. No cambies requisitos por iniciativa propia.
7. No borres archivos ni reescribas todo el proyecto para corregir un problema pequeño.
8. Explica los cambios con lenguaje adecuado para Bachillerato.
9. Añade o actualiza pruebas para cada comportamiento nuevo.
10. Al terminar, indica archivos modificados, pruebas ejecutadas y resultado.

## Flujo
ANALIZAR -> PLANIFICAR -> ESPERAR APROBACIÓN -> IMPLEMENTAR -> PROBAR -> DOCUMENTAR

## Criterio de finalización
Una tarea solo está terminada si el código se entiende, las pruebas pasan y la documentación refleja el estado real.
```

## 8. Memoria entre sesiones

### TODO.md

```markdown
# Plan y tareas

## En curso
- [ ] Acordar el plan de la versión 1.

## Pendientes
- [ ] Crear el menú.
- [ ] Añadir contactos.
- [ ] Buscar contactos.
- [ ] Eliminar contactos.
- [ ] Mostrar contactos.
- [ ] Guardar y cargar datos.
- [ ] Crear pruebas.

## Terminadas
- [x] Crear estructura inicial.
- [x] Redactar documentación básica.
```

### SESSION_LOG.md

```markdown
# Diario de sesiones

## Sesión 1 - AAAA-MM-DD
### Objetivo
Preparar la estructura y la memoria del proyecto.

### Realizado
- Creadas las carpetas y plantillas iniciales.

### Comprobaciones
- `hola.py` se ejecuta correctamente.

### Pendiente
- Inicializar Git y crear el plan de la versión 1.

### Primer paso de la próxima sesión
Leer README, AGENTS y TODO; después solicitar un plan sin programar.
```

### CHANGELOG.md

```markdown
# Historial de cambios

## [Sin publicar]
### Añadido
- Estructura inicial del proyecto.
- Documentación de contexto y continuidad.
```

## 9. .gitignore y requirements.txt

```gitignore
__pycache__/
*.py[cod]
.pytest_cache/
.venv/
venv/
.env
.vscode/
data/contactos.txt
data/contactos.xlsx
```

En `requirements.txt` escribe inicialmente:

```text
pytest
```

Más adelante añadiremos `openpyxl` cuando la agenda use una hoja de cálculo.

## 10. Configurar agentes sin exponer secretos

En Cline abre ajustes, selecciona OpenRouter como proveedor, pega la clave en el campo de configuración y elige el modelo permitido. No escribas la clave dentro de `.env` si el centro usa un gestor seguro; si se usa `.env`, debe permanecer ignorado por Git.

En Copilot inicia sesión con la cuenta de GitHub. El plan gratuito tiene límites, así que reserva las peticiones largas para planificación o revisión y usa prompts concisos para dudas pequeñas.

> Captura sugerida 2: panel de Cline con el proveedor seleccionado, ocultando completamente la clave.

## 11. Actividad del bloque

Crea desde cero la estructura anterior. Completa las plantillas con tu nombre de proyecto y fecha. Pide al agente que revise únicamente la coherencia documental:

```text
Lee README.md, AGENTS.md, TODO.md, SESSION_LOG.md y CHANGELOG.md.
No modifiques archivos y no programes.
Señala contradicciones, requisitos ambiguos y tareas que falten.
Responde con una lista breve y justificada.
```

Decide qué observaciones aceptar. Actualiza manualmente los archivos necesarios.

## 12. Lista de comprobación

- [ ] VS Code abre la carpeta raíz.
- [ ] Python ejecuta un programa simple.
- [ ] La estructura coincide con la propuesta.
- [ ] README define objetivo, funciones y límites.
- [ ] AGENTS contiene reglas de trabajo.
- [ ] TODO distingue pendiente, en curso y terminado.
- [ ] SESSION_LOG permite continuar mañana.
- [ ] `.gitignore` evita publicar secretos y datos de ejecución.
- [ ] Ninguna clave API está dentro del proyecto.


---

## Rutina de cierre de sesión

Antes de terminar una clase de 55-60 minutos:

1. Guarda todos los archivos.
2. Ejecuta el programa y las pruebas disponibles.
3. Revisa los cambios desde **Control de código fuente**.
4. Actualiza `TODO.md`, `SESSION_LOG.md` y, cuando proceda, `CHANGELOG.md`.
5. Realiza un commit pequeño y descriptivo si el proyecto queda en un estado coherente.
6. Deja escrito el primer paso de la sesión siguiente.

## Regla de seguridad y responsabilidad

No compartas contraseñas ni claves API en el chat, en el código o en Git. Guarda los secretos fuera del repositorio y añade sus nombres a `.gitignore`. No ejecutes comandos propuestos por un agente hasta entender qué hacen. Si un agente solicita borrar muchos archivos, instalar software inesperado o acceder fuera de la carpeta del proyecto, detén la acción y consulta al profesor.
