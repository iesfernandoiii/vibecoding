# Bloque 5. Pruebas automáticas con pytest

> Guía práctica para 2.º de Bachillerato. Proyecto conductor: **Agenda Telefónica en Python**.

## 1. Por qué probar

Una prueba automática ejecuta una función con datos conocidos y compara el resultado con lo esperado. No demuestra que el programa sea perfecto, pero detecta regresiones rápidamente y obliga a concretar el comportamiento.

## 2. Instalar pytest

Se recomienda un entorno virtual por proyecto:

```bash
python -m venv .venv
```

Actívalo según el sistema y selecciona su intérprete desde VS Code. Instala:

```bash
python -m pip install -U pytest
```

Comprueba:

```bash
python -m pytest --version
```

## 3. Convenciones

Pytest descubre habitualmente archivos `test_*.py` o `*_test.py` y funciones cuyo nombre comienza por `test_`. Usaremos `tests/test_agenda.py`.

```python
from src.agenda import normalizar_nombre


def test_normalizar_nombre_elimina_espacios():
    assert normalizar_nombre("  ana pérez  ") == "Ana Pérez"
```

Ejecuta desde la raíz:

```bash
python -m pytest
```

## 4. Patrón Preparar, Actuar, Comprobar

```python
def test_añadir_contacto_valido():
    # Preparar
    agenda = []

    # Actuar
    correcto, mensaje = añadir_contacto(agenda, "Ana", "600123123")

    # Comprobar
    assert correcto is True
    assert mensaje == "Contacto añadido."
    assert agenda == [{"nombre": "Ana", "telefono": "600123123"}]
```

## 5. Pruebas completas de lógica

```python
from src.agenda import (
    añadir_contacto,
    buscar_contactos,
    eliminar_contacto,
    normalizar_nombre,
    validar_telefono,
)


def test_normalizar_nombre():
    assert normalizar_nombre("  ana pérez ") == "Ana Pérez"


def test_telefono_vacio_no_es_valido():
    assert validar_telefono("") is False


def test_telefono_con_letras_no_es_valido():
    assert validar_telefono("600ABC123") is False


def test_telefono_con_prefijo_es_valido():
    assert validar_telefono("+34 600 123 123") is True


def test_alta_valida_modifica_agenda():
    agenda = []
    correcto, _ = añadir_contacto(agenda, "Ana", "600123123")
    assert correcto is True
    assert len(agenda) == 1


def test_alta_invalida_no_modifica_agenda():
    agenda = []
    correcto, _ = añadir_contacto(agenda, "Ana", "ABC")
    assert correcto is False
    assert agenda == []


def test_busqueda_no_distingue_mayusculas():
    agenda = [{"nombre": "Ana Pérez", "telefono": "600123123"}]
    assert buscar_contactos(agenda, "ANA") == agenda


def test_busqueda_inexistente_devuelve_lista_vacia():
    agenda = [{"nombre": "Ana", "telefono": "600123123"}]
    assert buscar_contactos(agenda, "Luis") == []


def test_eliminar_contacto_existente():
    agenda = [{"nombre": "Ana", "telefono": "600123123"}]
    correcto, _ = eliminar_contacto(agenda, "Ana")
    assert correcto is True
    assert agenda == []


def test_eliminar_contacto_inexistente_no_modifica():
    agenda = [{"nombre": "Ana", "telefono": "600123123"}]
    correcto, _ = eliminar_contacto(agenda, "Luis")
    assert correcto is False
    assert len(agenda) == 1
```

## 6. Probar ficheros sin tocar los datos reales

Pytest proporciona `tmp_path`, una carpeta temporal para cada prueba.

```python
from src.agenda import cargar_contactos, guardar_contactos


def test_guardar_y_cargar_contactos(tmp_path):
    ruta = tmp_path / "contactos.txt"
    agenda_original = [
        {"nombre": "Ana", "telefono": "600123123"},
        {"nombre": "Luis", "telefono": "+34 611 222 333"},
    ]

    guardar_contactos(agenda_original, ruta)
    agenda_cargada = cargar_contactos(ruta)

    assert agenda_cargada == agenda_original


def test_cargar_fichero_inexistente(tmp_path):
    ruta = tmp_path / "no_existe.txt"
    assert cargar_contactos(ruta) == []


def test_cargar_ignora_linea_dañada(tmp_path):
    ruta = tmp_path / "contactos.txt"
    ruta.write_text("linea dañada\nAna|600123123\n", encoding="utf-8")

    agenda = cargar_contactos(ruta)

    assert agenda == [{"nombre": "Ana", "telefono": "600123123"}]
```

## 7. Leer un fallo

Un fallo muestra el nombre de la prueba, la línea y los valores comparados. No cambies inmediatamente la expectativa para conseguir verde. Primero pregunta: ¿está mal el código o está mal la prueba?

Comandos útiles:

```bash
python -m pytest -q
python -m pytest -x
python -m pytest tests/test_agenda.py
python -m pytest tests/test_agenda.py::test_normalizar_nombre
```

## 8. Pedir pruebas a un agente

```text
Lee README.md, AGENTS.md y src/agenda.py.
No modifiques el código de producción.
Propón pruebas pytest para comportamientos normales, erróneos y límite.
Explica qué requisito cubre cada prueba.
No pruebes detalles internos que no formen parte del comportamiento esperado.
```

Después autoriza solo las pruebas aprobadas. Una prueba generada por IA también debe revisarse.

## 9. Pruebas engañosas

Esta prueba no sirve:

```python
def test_algo():
    assert True
```

Tampoco sirve duplicar el algoritmo dentro de la prueba. La prueba debe expresar un resultado esperado claro.

## 10. Diseñar código comprobable

Las funciones con `input()` y `print()` son más difíciles de probar. Por eso la lógica principal recibe parámetros y devuelve valores. La consola queda como una capa delgada.

## 11. Prueba de regresión

Si encuentras un error:

1. escribe una prueba que falle por ese error;
2. confirma que falla por el motivo correcto;
3. corrige el código;
4. ejecuta todo el conjunto;
5. crea un commit que mencione la corrección.

Ejemplo de mensaje:

```text
Corregir validación de teléfonos vacíos
```

## 12. Criterio mínimo del proyecto

Deben existir pruebas para:

- normalización;
- teléfono válido e inválido;
- alta válida e inválida;
- búsqueda con mayúsculas y sin resultados;
- eliminación existente e inexistente;
- ordenación;
- guardado y carga;
- fichero inexistente;
- línea dañada.

## 13. Integración con VS Code

Instala la extensión oficial de Python, abre el icono de pruebas y configura pytest cuando VS Code lo solicite. Aun así, aprende a ejecutar `python -m pytest`, porque el comando es reproducible y fácil de documentar.

> Captura sugerida: árbol de pruebas de VS Code con pruebas aprobadas y una prueba fallida.

## 14. Actividad

Introduce deliberadamente un error en `buscar_contactos` para que distinga mayúsculas. Ejecuta las pruebas, interpreta el fallo, corrige el código y vuelve a ejecutar. No confirmes el error deliberado. Confirma solo la corrección final.

## 15. Autoevaluación

- [ ] Sé instalar y ejecutar pytest.
- [ ] Sé escribir una prueba con `assert`.
- [ ] Distingo caso normal, error y límite.
- [ ] Uso `tmp_path` para ficheros.
- [ ] No cambio una prueba solo para ocultar un fallo.
- [ ] Ejecuto todas las pruebas antes del commit.


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
