# Análisis de Caja Blanca — `validar_compra`

```python
def validar_compra(edad, cuenta_activa, saldo, monto):
    if edad < 18:                          # D1
        return "Compra no permitida: edad"     # S1
    if not cuenta_activa:                  # D2
        return "Compra no permitida: cuenta"   # S2
    if saldo >= monto and monto > 0:       # D3
        return "Compra aprobada"               # S3
    return "Compra rechazada"                  # S4
```

## 1. Decisiones y condiciones simples
| Decisión | Expresión | Condiciones simples |
|---|---|---|
| D1 | `edad < 18` | C1: `edad < 18` |
| D2 | `not cuenta_activa` | C2: `cuenta_activa` (negada) |
| D3 | `saldo >= monto and monto > 0` | C3: `saldo >= monto`, C4: `monto > 0` |

**Total:** 3 decisiones y 4 condiciones simples. Hay 4 sentencias `return` (S1–S4), es decir 4 caminos posibles.

## 2. Criterio utilizado: **Cobertura de decisiones**
Cada decisión debe evaluarse al menos una vez como **Verdadera** y una vez como **Falsa**.

## 3. Mínimo de casos y cálculo
Las decisiones están en secuencia con `return` temprano: si D1 es V el programa termina, así que D2 y D3 solo se evalúan cuando las anteriores son F.

- D1 = V → 1 caso (sale por S1).
- D1 = F y D2 = V → 1 caso (S2).
- D1 = F, D2 = F, D3 = V → 1 caso (S3).
- D1 = F, D2 = F, D3 = F → 1 caso (S4).

Ningún caso puede cubrir dos resultados "V" a la vez, porque cada V termina la función. Por eso el **mínimo = 4 casos** (número de decisiones + 1 = 3 + 1, que coincide con la complejidad ciclomática V(G) = 4). Este mismo conjunto también logra la cobertura de sentencias (S1–S4).

## 4. Tres casos diseñados
| ID | edad | cuenta_activa | saldo | monto | Resultado esperado | Cubre |
|---|---|---|---|---|---|---|
| CB-01 | 16 | True | 100 | 50 | "Compra no permitida: edad" | D1 = V (S1) |
| CB-02 | 25 | False | 100 | 50 | "Compra no permitida: cuenta" | D1 = F, D2 = V (S2) |
| CB-03 | 25 | True | 100 | 50 | "Compra aprobada" | D1 = F, D2 = F, D3 = V (S3) |

## 5. ¿Alcanzan el criterio? Ajuste necesario
**No.** Con 3 casos D3 nunca toma el valor **Falso** y la sentencia S4 (`"Compra rechazada"`) no se ejecuta. Se cubren 5 de 6 resultados de decisión (83 %).

**Ajuste:** agregar un cuarto caso:

| ID | edad | cuenta_activa | saldo | monto | Resultado esperado | Cubre |
|---|---|---|---|---|---|---|
| CB-04 | 25 | True | 30 | 50 | "Compra rechazada" | D3 = F (S4) |

Con CB-01…CB-04 se logra 100 % de cobertura de decisiones y de sentencias.

## 6. Nota: diferencia con cobertura de condiciones
La cobertura de condiciones exige que **cada condición simple** (C1–C4) sea V y F. En D3, CB-03 hace C3 = V y C4 = V, y CB-04 hace C3 = F; pero como Python evalúa `and` en cortocircuito, C4 no se evalúa cuando C3 es F. Para que C4 sea Falsa haría falta un caso adicional:

| ID | edad | cuenta_activa | saldo | monto | Resultado esperado | Cubre |
|---|---|---|---|---|---|---|
| CB-05 | 25 | True | 100 | 0 | "Compra rechazada" | C3 = V, C4 = F |

Por eso, para cobertura de condiciones el mínimo sube a **5 casos**, lo que muestra que cobertura de decisiones ≠ cobertura de condiciones.
