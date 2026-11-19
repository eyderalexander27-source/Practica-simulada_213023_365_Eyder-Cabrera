## 1. Descripción

Aplicación de consola en **Python 3** que gestiona clientes, servicios y
reservas de la empresa *Software FJ*. **No usa base de datos**: toda la
información vive en objetos y listas en memoria. La única persistencia es el
archivo **`registro.log`**, donde se registran cronológicamente eventos y
errores (con traza completa).

## 2. Cómo ejecutarlo

```bash
python3 Actividad.py
```

Requisito: Python 3.10 o superior. No requiere librerías externas (solo
biblioteca estándar: `abc`, `enum`, `re`, `logging`, `os`, `datetime`,
`traceback`, `typing`).

Al finalizar imprime un **reporte del sistema** y genera/actualiza
`registro.log`.

## 3. Estructura del código (un solo archivo `Actividad.py`, en orden)

| Sección | Contenido |
|---|---|
| 1 | **9 excepciones personalizadas**, todas hijas de `SistemaGestionError` |
| 2 | **`Entidad`** — clase abstracta general (`ABC`) con método abstracto `describir()` |
| 3 | **`Cliente`** — hereda de `Entidad`; datos **privados** (`__email`, `__telefono`, `__reservas`) con validación mediante `@property` y setters |
| 4 | **`GestorLogs`** — registro cronológico en `registro.log` (eventos, advertencias, errores con traza) |
| 5 | **`Servicio`** (abstracta) + **3 derivadas polimórficas**: `ReservaSalas`, `AlquilerEquipos`, `AsesoriaEspecializada` |
| 6 | **`Reserva`** — máquina de estados `PENDIENTE → CONFIRMADA → FINALIZADA` (o `CANCELADA`); usa `try/except/else/finally` y encadenamiento `raise ... from` |
| 7 | **`SistemaGestion`** + simulación de **17 operaciones** secuenciales + `main()` |

## 4. Principios POO aplicados

- **Abstracción:** `Entidad` y `Servicio` son clases abstractas.
- **Herencia:** `Cliente` y `Servicio` heredan de `Entidad`; los 3 servicios
  heredan de `Servicio`; todas las excepciones heredan de `SistemaGestionError`.
- **Polimorfismo:** cada servicio sobrescribe `_costo_base()`,
  `validar_parametros()` y `describir()`; la `Reserva` calcula el costo sin
  saber qué tipo de servicio es.
- **Encapsulación:** atributos privados/protegidos; acceso mediante
  `@property` con validación; el estado de la reserva solo cambia con
  `confirmar()`, `cancelar()` y `procesar()`.
- **Sobrecarga (simulada):** `calcular_costo()` con parámetros opcionales
  (`impuesto`, `descuento`) y variante `calcular_costo_total()` con extras.

## 5. Manejo avanzado de excepciones

- Múltiples bloques `try/except`; `try/except/else/finally` en
  `Reserva.procesar()` y `GestorLogs.registrar_con_estado()`.
- **Encadenamiento** (`raise ... from`) para preservar la causa original.
- **Resiliencia:** ningún error detiene la aplicación; cada fallo se registra
  y la simulación continúa.

## 6. Simulación de 17 operaciones (mínimo 10 requeridas)

Operaciones válidas e inválidas: registro de clientes (con email y teléfono
mal formados), registro de servicios (capacidad faltante, nivel de asesoría
inválido), reservas exitosas (sala con descuento, alquiler con extras,
asesoría), reservas que fallan (servicio no disponible, cliente inexistente,
medio día de alquiler), cancelación de reserva finalizada y demostración de
sobrecarga de `calcular_costo`.

## 7. Verificación

El código se ejecuta sin errores (`exit code 0`), genera el reporte final y
escribe `registro.log` con marca de tiempo en cada entrada.
