# Bloque 4. Control de versiones con Git desde Visual Studio Code

> Guía práctica para 2.º de Bachillerato. Proyecto conductor: **Agenda Telefónica en Python**.

## 1. Qué resuelve Git

Git guarda instantáneas del proyecto llamadas commits. Permite saber qué cambió, recuperar una versión anterior y trabajar en una rama sin alterar la versión estable. Git funciona localmente; GitHub se utiliza después para publicar y colaborar.

## 2. Configuración inicial

Git necesita el nombre y correo que aparecerán como autor. Esta configuración sí puede requerir el terminal una vez:

```bash
git config --global user.name "Nombre Apellidos"
git config --global user.email "correo@example.com"
```

No uses una contraseña como correo. Para comprobar la instalación:

```bash
git --version
```

## 3. Inicializar desde VS Code

1. Abre la carpeta raíz `agenda-telefonica`.
2. Abre **Control de código fuente** con el icono de rama o `Ctrl+Mayús+G`.
3. Pulsa **Inicializar repositorio**.
4. Comprueba que aparecen archivos bajo **Cambios**.

> Captura sugerida: botón Inicializar repositorio y, después, lista de cambios con letras U.

`U` significa *untracked*; `M`, modificado; `D`, eliminado. Guardar un archivo no crea un commit.

## 4. Revisar antes de preparar

Selecciona un archivo dentro de Cambios. VS Code muestra el diff. Revisa:

- que no haya claves API;
- que no aparezcan datos personales;
- que `.gitignore` funcione;
- que los cambios pertenezcan a la misma tarea;
- que no haya código temporal.

## 5. Primer commit

1. Pulsa `+` junto a cada archivo o en el encabezado de Cambios para preparar todos.
2. Verifica su paso a **Cambios almacenados provisionalmente**.
3. Escribe un mensaje: `Crear estructura y documentación inicial`.
4. Pulsa **Confirmar**.

Un commit no sube nada a Internet. Registra una instantánea en el repositorio local.

## 6. Mensajes de commit

Buenos ejemplos:

```text
Añadir validación de teléfonos
Implementar búsqueda sin distinguir mayúsculas
Guardar contactos en fichero UTF-8
Añadir pruebas de eliminación
Actualizar memoria de la sesión 4
```

Malos ejemplos:

```text
cambios
cosas
final
ahora sí
prueba 2
```

Usa un verbo de acción y describe el resultado. No mezcles diez tareas diferentes.

## 7. Commits recomendados para la agenda

```text
1. Crear estructura y documentación inicial
2. Añadir validación y alta de contactos
3. Implementar búsqueda, listado y eliminación
4. Guardar y cargar contactos desde texto
5. Integrar menú principal
6. Añadir pruebas automatizadas
```

## 8. Consultar el historial

En Control de código fuente abre el gráfico o historial disponible. También puedes abrir la línea de tiempo de un archivo desde el Explorador. Localiza:

- autor;
- fecha;
- mensaje;
- archivos incluidos;
- diferencias respecto al commit anterior.

## 9. Descartar cambios no confirmados

Si modificaste una línea y quieres recuperar la última versión confirmada:

1. Selecciona el archivo en Cambios.
2. Revisa el diff.
3. Usa **Descartar cambios** desde el menú contextual.

Esta acción puede eliminar trabajo no confirmado. No la uses sin revisar. Para cambios importantes, copia temporalmente el fragmento o consulta al profesor.

## 10. Corregir después de un commit

En esta unidad evitaremos reescribir el historial. Si un commit contiene un error:

1. corrige el archivo;
2. ejecuta las comprobaciones;
3. crea otro commit claro, por ejemplo `Corregir carga de líneas vacías`.

Más adelante se puede aprender *revert*, pero no es necesario para el flujo básico.

## 11. Publicar en GitHub

La publicación se trabajará como unidad relacionada. El flujo básico cuando el profesor lo indique es:

1. Inicia sesión en GitHub desde VS Code.
2. Abre Control de código fuente.
3. Pulsa **Publicar rama** o **Publish to GitHub**.
4. Elige repositorio privado o público según las instrucciones.
5. Comprueba en el navegador que `.env`, claves y los datos personales no aparecen.

Publicar no sustituye al commit. Primero confirmas localmente; después sincronizas.

## 12. Crear una rama desde la interfaz

Antes de una nueva funcionalidad:

1. Haz commit de la versión estable.
2. Pulsa el nombre de la rama en la barra de estado, normalmente `main`.
3. Elige **Crear nueva rama**.
4. Escribe `feature/excel-storage`.
5. Verifica que la barra de estado muestra la nueva rama.

Los commits nuevos pertenecerán a esa rama.

## 13. Volver a main

1. Guarda o confirma el trabajo de la rama.
2. Pulsa el nombre de la rama.
3. Selecciona `main`.
4. Comprueba que los cambios exclusivos de Excel ya no están visibles.

No cambies de rama con trabajo incompleto sin comprender la advertencia de VS Code.

## 14. Fusionar una rama

Cuando la funcionalidad esté probada:

1. Cambia a `main`.
2. Abre la paleta de comandos.
3. Ejecuta **Git: Merge Branch...**.
4. Selecciona `feature/excel-storage`.
5. Ejecuta las pruebas de nuevo.
6. Si todo funciona, conserva el commit de fusión o crea el commit solicitado por VS Code.

Si aparece un conflicto, no elijas botones al azar. Lee ambos cambios y consulta al profesor.

## 15. Flujo visual de una tarea

```text
main estable
   |
   +-- crear feature/nombre
          |
          +-- plan
          +-- implementación
          +-- pruebas
          +-- commit(s)
   |
volver a main -> fusionar -> volver a probar
```

## 16. Práctica guiada

1. Comprueba que la agenda funciona.
2. Crea un commit de referencia.
3. Crea `feature/ordenar-contactos`.
4. Añade o mejora el listado alfabético.
5. Revisa el diff.
6. Ejecuta el programa.
7. Confirma con `Ordenar contactos alfabéticamente`.
8. Regresa a `main` y observa la diferencia.
9. Fusiona la rama y vuelve a ejecutar.

## 17. Autoevaluación

- [ ] Distingo guardar, preparar, commit y publicar.
- [ ] Sé revisar un diff.
- [ ] Escribo mensajes descriptivos.
- [ ] Sé crear y cambiar de rama desde VS Code.
- [ ] No publico claves ni datos de ejecución.
- [ ] Ejecuto pruebas después de una fusión.


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
