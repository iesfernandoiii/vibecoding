# Bloque 2. Planificar, revisar y autorizar el trabajo del agente

> Guía práctica para 2.º de Bachillerato. Proyecto conductor: **Agenda Telefónica en Python**.

## 1. Propósito

En este bloque aprenderás a obtener un plan útil sin permitir que el agente programe antes de tiempo. Un plan permite descubrir requisitos incompletos, elegir nombres y separar el trabajo en pasos verificables.

## 2. PLAN y ACT no son lo mismo

- **PLAN**: leer, preguntar, razonar, proponer archivos, funciones, pruebas y orden de trabajo.
- **ACT**: modificar archivos y ejecutar acciones del plan aprobado.

Aunque una herramienta no muestre botones llamados Plan y Act, podemos imponerlos mediante el prompt.

## 3. Prompt inicial de planificación

```text
Lee README.md, AGENTS.md, TODO.md y SESSION_LOG.md.
No escribas ni modifiques código todavía.

Crea un plan para la versión 1 de la Agenda Telefónica.
El plan debe indicar:
1. archivos que se crearán o modificarán;
2. funciones y responsabilidad de cada una;
3. estructura de los datos en memoria;
4. formato del fichero de texto;
5. validaciones mínimas;
6. pruebas necesarias;
7. riesgos o preguntas abiertas;
8. orden de implementación.

Usa programación estructurada y no propongas clases.
Espera aprobación antes de actuar.
```

## 4. Aspecto de un plan correcto

```markdown
# Plan de la versión 1

## Datos
Cada contacto será un diccionario con `nombre` y `telefono`.
La agenda será una lista de contactos.

## Archivos
- `src/agenda.py`: lógica y bucle principal.
- `tests/test_agenda.py`: pruebas de funciones puras.
- `data/contactos.txt`: datos creados durante la ejecución.

## Funciones
1. `normalizar_nombre(nombre)`: elimina espacios extremos.
2. `validar_telefono(telefono)`: comprueba que no esté vacío y contenga dígitos.
3. `añadir_contacto(agenda, nombre, telefono)`: añade un contacto válido.
4. `buscar_contactos(agenda, texto)`: devuelve coincidencias.
5. `eliminar_contacto(agenda, nombre)`: elimina e informa del resultado.
6. `guardar_contactos(agenda, ruta)`: escribe el fichero.
7. `cargar_contactos(ruta)`: devuelve lista vacía si no existe.
8. `mostrar_menu()` y `main()`: interacción por consola.

## Pruebas
- Alta válida e inválida.
- Búsqueda existente e inexistente.
- Eliminación existente e inexistente.
- Guardado y carga en un fichero temporal.
```

Este plan separa lógica e interacción. Es una mejora importante: las funciones que reciben parámetros y devuelven resultados son más fáciles de probar que las funciones que llaman constantemente a `input()`.

## 5. Cómo revisar un plan

Comprueba:

1. **Cobertura**: aparecen todos los requisitos del README.
2. **Comprensibilidad**: puedes explicar cada función.
3. **Alcance**: no añade interfaz gráfica, base de datos o cuentas de usuario.
4. **Dependencias**: no instala bibliotecas innecesarias.
5. **Pruebas**: cada comportamiento importante tiene comprobación.
6. **Orden**: primero datos y lógica, después consola, persistencia y pruebas.
7. **Recuperación**: el agente indica qué hará si un paso falla.

## 6. Rechazar o corregir propuestas

Si el agente propone clases:

```text
Revisa el plan. Incumple AGENTS.md porque propone una clase Contacto.
Mantén la misma funcionalidad usando listas, diccionarios y funciones.
No programes todavía. Devuelve solo el plan corregido.
```

Si el plan es demasiado grande:

```text
Divide el plan en incrementos de una sesión como máximo.
Cada incremento debe terminar con una ejecución o prueba observable.
No modifiques archivos.
```

## 7. Preguntas que debe resolver el alumnado

Antes de aprobar:

- ¿Se permiten nombres duplicados?
- ¿El teléfono puede contener espacios o el signo `+`?
- ¿La búsqueda distingue mayúsculas?
- ¿Qué ocurre si el fichero no existe?
- ¿Qué ocurre si una línea está dañada?
- ¿Cuándo se guardan los cambios?

Para esta guía decidimos:

- Se permiten nombres duplicados.
- El teléfono admite dígitos, espacios y `+`, pero debe contener al menos un dígito.
- La búsqueda no distingue mayúsculas.
- Si no existe el fichero, se empieza con agenda vacía.
- Las líneas incorrectas se ignoran y se informa al usuario.
- Se guarda al salir y después de cada modificación importante.

Registra estas decisiones en README o TODO para que no queden enterradas en el chat.

## 8. Aprobar el plan y pasar a ACT

```text
Plan aprobado con las decisiones registradas en README.md.
Implementa únicamente el primer incremento:
- crear `src/agenda.py`;
- definir las funciones de normalización, validación y alta;
- crear pruebas básicas para esas funciones.

No implementes todavía el menú ni el almacenamiento.
Antes de editar, resume los archivos que tocarás.
Después, ejecuta las pruebas y comunica el resultado.
```

La aprobación no es ilimitada. Autoriza un incremento pequeño, no todo el proyecto si aún estás aprendiendo.

## 9. Revisar propuestas de edición

Antes de aceptar cambios del agente:

- Lee el diff, no solo el mensaje del chat.
- Rechaza cambios fuera del objetivo.
- Comprueba que no haya claves, rutas personales o código copiado de fuentes desconocidas.
- Pide explicación de cualquier sintaxis que no conozcas.

Prompt de explicación:

```text
No modifiques nada. Explica la función validar_telefono línea a línea.
Indica entradas válidas, entradas inválidas y qué devuelve.
Usa ejemplos adecuados para 2.º de Bachillerato.
```

## 10. Prompt de revisión del plan por un segundo agente

```text
Actúa como revisor, no como implementador.
Compara PLAN.md con README.md y AGENTS.md.
Identifica requisitos omitidos, complejidad innecesaria y pruebas que falten.
No modifiques archivos. Clasifica cada observación como crítica, recomendable u opcional.
```

Nunca aceptes automáticamente la crítica del segundo agente. Dos modelos pueden equivocarse del mismo modo.

## 11. Registrar la planificación

Guarda el plan aprobado en `docs/PLAN_V1.md` y actualiza `TODO.md`:

```markdown
## En curso
- [ ] Incremento 1: validación y alta.

## Pendientes
- [ ] Incremento 2: búsqueda, listado y eliminación.
- [ ] Incremento 3: almacenamiento de texto.
- [ ] Incremento 4: menú y programa principal.
- [ ] Incremento 5: conjunto completo de pruebas.
```

## 12. Actividad evaluable corta

El profesor entrega un requisito nuevo: "La agenda debe poder listar los contactos ordenados alfabéticamente". Redacta un prompt que pida actualizar el plan sin programar. Después revisa la respuesta con los siete criterios del apartado 5.

## 13. Errores frecuentes

- Aprobar un plan sin leerlo.
- Mantener decisiones solo en el chat.
- Pedir "hazlo todo".
- Confundir comentarios abundantes con código claro.
- Permitir que el agente instale dependencias sin necesidad.
- Autorizar un cambio aunque las pruebas propuestas sean vagas.

## 14. Autoevaluación

- [ ] Distingo PLAN de ACT.
- [ ] Sé pedir un plan que incluya funciones, ficheros y pruebas.
- [ ] Puedo detectar funciones demasiado complejas.
- [ ] Registro decisiones en archivos del proyecto.
- [ ] Autorizo incrementos pequeños y verificables.


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
