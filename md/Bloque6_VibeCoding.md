# Bloque 6. Evolucionar el proyecto, trabajar con ramas y evaluar el aprendizaje

> Guía práctica para 2.º de Bachillerato. Proyecto conductor: **Agenda Telefónica en Python**.

## 1. Reto de evolución

La versión estable guarda datos en texto. La nueva funcionalidad almacenará contactos en una hoja de cálculo `.xlsx` mediante `openpyxl`. El cambio debe realizarse en una rama y conservar el comportamiento de la aplicación.

## 2. Preparación

En `main`:

1. ejecuta todas las pruebas;
2. actualiza la documentación si hace falta;
3. crea un commit estable;
4. crea la rama `feature/excel-storage` desde VS Code.

## 3. Solicitar un nuevo plan

```text
Lee README.md, AGENTS.md, TODO.md, SESSION_LOG.md y las pruebas actuales.
No programes todavía.

Crea un plan para sustituir el almacenamiento de texto por Excel con openpyxl.
Mantén sin cambios las funciones de alta, búsqueda y eliminación.
Incluye:
- dependencia nueva y motivo;
- funciones que cambiarán;
- compatibilidad o migración de datos;
- pruebas que deben adaptarse o añadirse;
- pasos pequeños y reversibles;
- riesgos.
Espera aprobación.
```

## 4. Plan recomendado

1. Añadir `openpyxl` a `requirements.txt`.
2. Crear `guardar_contactos_excel` y `cargar_contactos_excel` sin borrar aún las funciones de texto.
3. Escribir pruebas temporales con `tmp_path`.
4. Cambiar `RUTA_DATOS` y el programa principal.
5. Ejecutar pruebas manuales y automatizadas.
6. Documentar el nuevo formato.
7. Decidir si se conserva un script de migración o se inicia un fichero nuevo.
8. Fusionar solo cuando toda la rama esté verde.

## 5. Instalación y requirements

```bash
python -m pip install openpyxl
```

`requirements.txt`:

```text
pytest
openpyxl
```

## 6. Implementación orientativa

```python
from openpyxl import Workbook, load_workbook


def guardar_contactos_excel(agenda, ruta_fichero):
    ruta = Path(ruta_fichero)
    ruta.parent.mkdir(parents=True, exist_ok=True)

    libro = Workbook()
    hoja = libro.active
    hoja.title = "Contactos"
    hoja.append(["Nombre", "Teléfono"])

    for contacto in agenda:
        hoja.append([contacto["nombre"], contacto["telefono"]])

    libro.save(ruta)


def cargar_contactos_excel(ruta_fichero):
    ruta = Path(ruta_fichero)
    if not ruta.exists():
        return []

    libro = load_workbook(ruta)
    hoja = libro["Contactos"]
    agenda = []

    for nombre, telefono in hoja.iter_rows(min_row=2, values_only=True):
        if nombre is None or telefono is None:
            continue
        correcto, _ = añadir_contacto(agenda, str(nombre), str(telefono))
        if not correcto:
            continue

    return agenda
```

En `main`, cambia la ruta a `data/contactos.xlsx` y llama a las funciones Excel. No elimines la versión de texto hasta haber decidido la estrategia de migración.

## 7. Pruebas Excel

```python
from src.agenda import cargar_contactos_excel, guardar_contactos_excel


def test_guardar_y_cargar_excel(tmp_path):
    ruta = tmp_path / "contactos.xlsx"
    agenda = [{"nombre": "Ana", "telefono": "600123123"}]

    guardar_contactos_excel(agenda, ruta)
    cargada = cargar_contactos_excel(ruta)

    assert cargada == agenda


def test_cargar_excel_inexistente(tmp_path):
    ruta = tmp_path / "no_existe.xlsx"
    assert cargar_contactos_excel(ruta) == []
```

## 8. Revisión y fusión

Antes de fusionar:

- [ ] La rama es `feature/excel-storage`.
- [ ] No hay claves ni ficheros personales.
- [ ] `requirements.txt` está actualizado.
- [ ] Todas las pruebas pasan.
- [ ] La aplicación se ha ejecutado manualmente.
- [ ] README explica el nuevo formato.
- [ ] TODO y SESSION_LOG reflejan el estado.
- [ ] CHANGELOG incluye el cambio.

Cambia a `main`, fusiona la rama desde **Git: Merge Branch...** y ejecuta todo de nuevo.

## 9. Proyecto final evaluable

El alumnado desarrollará desde cero un proyecto diferente de la agenda. Posibles temas:

- catálogo de biblioteca;
- inventario de material;
- gestor de tareas;
- registro de películas;
- control de préstamos;
- colección de videojuegos.

El enunciado debe exigir al menos:

- menú de consola;
- altas, búsquedas, modificaciones o bajas;
- persistencia;
- validación;
- pruebas pytest;
- documentación de memoria;
- commits coherentes;
- una mejora realizada en rama.

No se evaluará copiar la agenda y cambiar nombres. La estructura del proceso puede repetirse; el modelo de datos y las decisiones deben adaptarse al nuevo problema.

## 10. Evidencias de entrega

1. Repositorio completo.
2. README con instalación, ejecución y decisiones.
3. AGENTS y memoria de sesiones.
4. PLAN inicial y plan de ampliación.
5. Historial de commits.
6. Rama utilizada para la mejora.
7. Pruebas automatizadas.
8. Demostración breve del programa.
9. Reflexión individual: qué hizo la IA, qué revisó el alumno y qué error corrigió.

## 11. Rúbrica de evaluación (100 puntos)

### A. Análisis y planificación: 15 puntos

- **13-15**: requisitos completos, decisiones justificadas, plan incremental con pruebas y riesgos.
- **10-12**: plan correcto con alguna omisión menor.
- **7-9**: plan genérico o parcialmente conectado con el código.
- **0-6**: se programa sin plan o el plan no responde al problema.

### B. Uso responsable de IA: 15 puntos

- **13-15**: prompts precisos, revisión crítica, rechazo de propuestas inadecuadas y explicación del código.
- **10-12**: uso adecuado con revisión suficiente.
- **7-9**: se aceptan cambios con poca justificación.
- **0-6**: dependencia total; no puede explicar el resultado o expone secretos.

### C. Calidad funcional y del código: 20 puntos

- **18-20**: funciones claras, validación consistente, nombres adecuados y comportamiento completo.
- **14-17**: funciona con defectos menores.
- **10-13**: funcionalidad parcial o código difícil de mantener.
- **0-9**: fallos graves o código no ejecutable.

### D. Pruebas: 15 puntos

- **13-15**: cubre casos normales, erróneos, límite y persistencia; todas pasan.
- **10-12**: cobertura adecuada con algunas ausencias.
- **7-9**: pocas pruebas o pruebas débiles.
- **0-6**: no hay pruebas útiles o no se ejecutan.

### E. Git y ramas: 15 puntos

- **13-15**: commits pequeños y descriptivos, rama de mejora y fusión comprobada.
- **10-12**: historial correcto con detalles mejorables.
- **7-9**: pocos commits o uso confuso de ramas.
- **0-6**: entrega sin historial útil.

### F. Documentación y continuidad: 10 puntos

- **9-10**: README, TODO, SESSION_LOG, CHANGELOG y planes están actualizados.
- **7-8**: documentación suficiente con pequeñas incoherencias.
- **5-6**: documentación incompleta o desactualizada.
- **0-4**: no permite comprender ni continuar el proyecto.

### G. Presentación y reflexión: 10 puntos

- **9-10**: demostración clara, explica decisiones, errores y límites de la IA.
- **7-8**: presentación correcta.
- **5-6**: explicación superficial.
- **0-4**: no puede demostrar o explicar el proyecto.

## 12. Plantilla final de entrega

```markdown
# Memoria de entrega

## Proyecto
Nombre y objetivo.

## Flujo seguido
Cómo se pasó de requisitos a plan, implementación, pruebas y commits.

## Uso de IA
Agentes y proveedores usados; ejemplos de prompts; propuestas rechazadas.

## Pruebas
Qué se comprobó y resultado final.

## Git
Commits principales, rama de mejora y proceso de fusión.

## Problema encontrado
Descripción, prueba que lo reprodujo y solución.

## Reflexión
Qué sé hacer ahora sin ayuda y qué debo seguir practicando.
```

## 13. Biblioteca de prompts reutilizables

### Continuar una sesión

```text
Lee README.md, AGENTS.md, TODO.md y SESSION_LOG.md.
Resume el estado en cinco puntos. No modifiques archivos.
Propón solo el siguiente paso verificable.
```

### Crear un plan

```text
No programes. Crea un plan incremental con archivos, funciones, pruebas, riesgos y criterio de finalización.
```

### Ejecutar un incremento

```text
Implementa únicamente el incremento aprobado. Antes indica los archivos que cambiarás. Después ejecuta las pruebas y resume el diff.
```

### Revisar

```text
No modifiques nada. Revisa el diff buscando incumplimientos de requisitos, complejidad innecesaria y ausencia de pruebas.
```

### Explicar

```text
Explica esta función línea a línea y muestra dos entradas normales, una errónea y una límite.
```

### Crear pruebas

```text
Propón pruebas pytest vinculadas a requisitos. No cambies producción y no escribas pruebas triviales.
```

### Corregir un fallo

```text
Analiza el mensaje de error. Formula una hipótesis, indica cómo comprobarla y espera antes de editar.
```

### Cerrar sesión

```text
Actualiza TODO.md, SESSION_LOG.md y CHANGELOG.md para reflejar solo lo realizado. No marques tareas no comprobadas. Resume pruebas y próximo paso.
```

## 14. Checklist maestro

- [ ] Puedo crear un proyecto sin copiar la agenda.
- [ ] Sé redactar README y reglas para agentes.
- [ ] Sé planificar antes de actuar.
- [ ] Comprendo el código aceptado.
- [ ] Sé ejecutar y leer pytest.
- [ ] Sé revisar cambios y crear commits desde VS Code.
- [ ] Sé trabajar en una rama y fusionarla.
- [ ] Mantengo memoria entre clases.
- [ ] No publico secretos.
- [ ] Puedo explicar qué aportó la IA y qué decidí yo.


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
