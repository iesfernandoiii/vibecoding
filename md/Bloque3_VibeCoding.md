# Bloque 3. Implementar la Agenda Telefónica versión 1

> Guía práctica para 2.º de Bachillerato. Proyecto conductor: **Agenda Telefónica en Python**.

## 1. Objetivo

Transformaremos el plan aprobado en una aplicación funcional, pero mantendremos la lógica separada de la consola. Esta separación facilita entender, reutilizar y probar el código.

## 2. Primer incremento: normalización, validación y alta

Crea `src/agenda.py`:

```python
def normalizar_nombre(nombre):
    """Elimina espacios exteriores y pone en mayúscula la inicial."""
    return nombre.strip().title()


def validar_telefono(telefono):
    """Devuelve True si hay al menos un dígito y solo símbolos permitidos."""
    telefono = telefono.strip()
    if telefono == "":
        return False

    permitidos = "0123456789 +"
    for caracter in telefono:
        if caracter not in permitidos:
            return False

    return any(caracter.isdigit() for caracter in telefono)


def añadir_contacto(agenda, nombre, telefono):
    nombre = normalizar_nombre(nombre)
    telefono = telefono.strip()

    if nombre == "":
        return False, "El nombre no puede estar vacío."
    if not validar_telefono(telefono):
        return False, "El teléfono no es válido."

    agenda.append({"nombre": nombre, "telefono": telefono})
    return True, "Contacto añadido."
```

Observa que `añadir_contacto` no usa `input()`. Recibe datos, modifica la lista y devuelve un resultado. La interfaz preguntará los datos más adelante.

## 3. Comprobación manual

Crea temporalmente este bloque al final del archivo o ejecútalo en el terminal interactivo:

```python
agenda = []
resultado, mensaje = añadir_contacto(agenda, "  ana pérez ", "+34 600 123 123")
print(resultado)
print(mensaje)
print(agenda)
```

Resultado esperado:

```text
True
Contacto añadido.
[{'nombre': 'Ana Pérez', 'telefono': '+34 600 123 123'}]
```

Después elimina el código temporal para evitar que se ejecute al importar el módulo.

## 4. Segundo incremento: buscar, listar y eliminar

```python
def buscar_contactos(agenda, texto):
    texto = texto.strip().lower()
    encontrados = []

    for contacto in agenda:
        if texto in contacto["nombre"].lower():
            encontrados.append(contacto)

    return encontrados


def obtener_contactos_ordenados(agenda):
    return sorted(agenda, key=lambda contacto: contacto["nombre"].lower())


def eliminar_contacto(agenda, nombre):
    nombre_normalizado = normalizar_nombre(nombre)

    for contacto in agenda:
        if contacto["nombre"].lower() == nombre_normalizado.lower():
            agenda.remove(contacto)
            return True, "Contacto eliminado."

    return False, "No se encontró el contacto."
```

Si `lambda` todavía no se ha explicado, puede reemplazarse por una función auxiliar:

```python
def clave_ordenacion(contacto):
    return contacto["nombre"].lower()
```

Y después:

```python
return sorted(agenda, key=clave_ordenacion)
```

## 5. Tercer incremento: persistencia en texto

Usaremos `|` como separador para reducir conflictos con teléfonos que contienen espacios. Cada línea tendrá `nombre|telefono`.

```python
from pathlib import Path


def guardar_contactos(agenda, ruta_fichero):
    ruta = Path(ruta_fichero)
    ruta.parent.mkdir(parents=True, exist_ok=True)

    with ruta.open("w", encoding="utf-8") as fichero:
        for contacto in agenda:
            nombre = contacto["nombre"]
            telefono = contacto["telefono"]
            fichero.write(f"{nombre}|{telefono}\n")


def cargar_contactos(ruta_fichero):
    ruta = Path(ruta_fichero)
    agenda = []

    if not ruta.exists():
        return agenda

    with ruta.open("r", encoding="utf-8") as fichero:
        for linea in fichero:
            partes = linea.rstrip("\n").split("|", maxsplit=1)
            if len(partes) != 2:
                continue

            nombre, telefono = partes
            correcto, _ = añadir_contacto(agenda, nombre, telefono)
            if not correcto:
                continue

    return agenda
```

`Path` hace más clara la gestión de rutas. `encoding="utf-8"` permite conservar tildes y eñes.

## 6. Cuarto incremento: interfaz de consola

```python
RUTA_DATOS = "data/contactos.txt"


def mostrar_menu():
    print("\nAGENDA TELEFÓNICA")
    print("1. Añadir contacto")
    print("2. Buscar contacto")
    print("3. Eliminar contacto")
    print("4. Mostrar contactos")
    print("5. Salir")
    return input("Elige una opción: ").strip()


def imprimir_contactos(contactos):
    if not contactos:
        print("No hay contactos para mostrar.")
        return

    for contacto in contactos:
        print(f"{contacto['nombre']}: {contacto['telefono']}")


def main():
    agenda = cargar_contactos(RUTA_DATOS)

    while True:
        opcion = mostrar_menu()

        if opcion == "1":
            nombre = input("Nombre: ")
            telefono = input("Teléfono: ")
            correcto, mensaje = añadir_contacto(agenda, nombre, telefono)
            print(mensaje)
            if correcto:
                guardar_contactos(agenda, RUTA_DATOS)

        elif opcion == "2":
            texto = input("Texto que deseas buscar: ")
            imprimir_contactos(buscar_contactos(agenda, texto))

        elif opcion == "3":
            nombre = input("Nombre exacto que deseas eliminar: ")
            correcto, mensaje = eliminar_contacto(agenda, nombre)
            print(mensaje)
            if correcto:
                guardar_contactos(agenda, RUTA_DATOS)

        elif opcion == "4":
            imprimir_contactos(obtener_contactos_ordenados(agenda))

        elif opcion == "5":
            guardar_contactos(agenda, RUTA_DATOS)
            print("Datos guardados. Hasta pronto.")
            break

        else:
            print("Opción no válida.")


if __name__ == "__main__":
    main()
```

## 7. Ejecutar el programa

Desde la raíz:

```bash
python -m src.agenda
```

Prueba al menos esta secuencia:

1. Mostrar la agenda vacía.
2. Añadir Ana y Luis.
3. Buscar `an`.
4. Cerrar y volver a abrir.
5. Comprobar que los contactos se cargan.
6. Eliminar uno.
7. Escribir una opción no válida.

## 8. Prompts de implementación por incrementos

```text
Lee el plan aprobado. Implementa solo las funciones puras de alta y validación.
No añadas el menú. Explica cada cambio y ejecuta una comprobación mínima.
```

```text
Revisa el diff del incremento actual. No añadas nuevas funciones.
Busca errores de casos límite y sintaxis demasiado avanzada para el nivel.
```

```text
Implementa el almacenamiento en texto con UTF-8.
Si el fichero no existe, devuelve una agenda vacía.
Ignora líneas dañadas sin detener el programa.
No cambies las funciones ya aprobadas salvo que sea imprescindible.
```

## 9. Revisar código generado

Para cada función responde:

- ¿Qué recibe?
- ¿Qué devuelve?
- ¿Modifica una lista o un fichero?
- ¿Qué ocurre con una entrada vacía?
- ¿Qué errores puede producir?
- ¿Podría probarse sin usar el teclado?

No des por correcto un código solo porque "parece funcionar". Prueba casos normales, erróneos y límite.

## 10. Ejemplos de errores del agente

### Mezclar toda la lógica con input

```python
def añadir_contacto():
    nombre = input("Nombre: ")
```

Esta versión es cómoda al principio, pero difícil de probar. Pide separar entrada y lógica.

### Capturar cualquier excepción

```python
try:
    ...
except:
    pass
```

Oculta problemas. Captura excepciones concretas o evita el error comprobando previamente.

### Reescribir el fichero al importar

Nunca debe ejecutarse `guardar_contactos` al importar el módulo. El punto de entrada debe estar protegido por `if __name__ == "__main__":`.

## 11. Actualización documental

En `TODO.md`, mueve a terminadas las tareas realizadas. En `SESSION_LOG.md`, registra qué secuencias manuales se probaron. En `CHANGELOG.md`:

```markdown
## [0.1.0] - AAAA-MM-DD
### Añadido
- Alta, búsqueda, listado y eliminación de contactos.
- Persistencia en fichero de texto UTF-8.
- Interfaz de consola.
```

## 12. Actividad de ampliación

Solicita un plan, sin programar, para impedir contactos exactamente duplicados. Decide si se considera duplicado el mismo nombre, el mismo teléfono o ambos. Registra la decisión y autoriza un incremento pequeño.

## 13. Autoevaluación

- [ ] Puedo explicar todas las funciones.
- [ ] La lógica está separada de `input()` y `print()` cuando es razonable.
- [ ] Los datos persisten después de cerrar.
- [ ] El programa soporta una agenda vacía.
- [ ] No se han añadido dependencias externas.
- [ ] La documentación coincide con el código real.


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
