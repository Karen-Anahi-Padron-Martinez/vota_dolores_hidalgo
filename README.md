# 🗳️ Vota Dolores Hidalgo — Evidencia y Pruebas TDD

> **Desarrollo Móvil Integral** — 10mo. Cuatrimestre  
> *Plebiscito Vecinal con Arquitectura TDD · Barras Animadas · Modal de Ganador*

---

## 📌 Descripción del Proyecto

**Vota Dolores Hidalgo** es una aplicación móvil desarrollada en **Flutter** para gestionar un plebiscito vecinal en el municipio de Dolores Hidalgo. El sistema implementa la metodología **TDD (Test-Driven Development)** para garantizar la integridad, consistencia y seguridad en el registro de votos y el cálculo de resultados.

### Reglas de Negocio Implementadas:
1. **Registro de Voto Válido:** Incrementa el contador de la opción seleccionada.
2. **Validación de Opción:** Rechaza votos dirigidos a opciones inexistentes (`opcionInvalida`).
3. **Voto Único por Usuario:** Impide votos duplicados por un mismo elector (`usuarioYaVoto`).
4. **Cálculo Porcentual Dinámico:** Calcula la proporción de votos para cada opción, evitando división por cero cuando no hay sufragios registrados.
5. **Determinación del Ganador:** Identifica la opción mayoritaria o detecta empates múltiples.
6. **Cierre de Votación:** Bloquea la emisión de votos una vez superada la fecha límite (`votacionCerrada`).

---

## 📊 1. Resumen de Pruebas Unitarias y de Integración

Todas las reglas de negocio están cubiertas en `test/servicio_votacion_test.dart`:

| # | Ronda / Prueba | Regla de Negocio Protegida | Resultado Esperado | Estado |
|:---:|:---|:---|:---|:---:|
| 1 | **Ronda 1** | Registrar un voto válido incrementa el contador de la opción | `ResultadoVoto.exitoso`, contador = 1 | ✅ Aprobada |
| 2 | **Ronda 2** | Votar por una opción que no existe es rechazado | `ResultadoVoto.opcionInvalida` | ✅ Aprobada |
| 3 | **Ronda 3** | Un mismo usuario no puede emitir voto duplicado | `ResultadoVoto.usuarioYaVoto`, contador intacto | ✅ Aprobada |
| 4 | **Ronda 4 (a)** | Cálculo porcentual correcto para cada opción según el total | % ponderado exacto por opción | ✅ Aprobada |
| 5 | **Ronda 4 (b)** | Caso borde con 0 votos registrados | Porcentajes en `0.0%` (sin división por cero ni NaN) | ✅ Aprobada |
| 6 | **Ronda 5** | Determinar ganador con mayor número de votos | Retorna la opción líder en votos | ✅ Aprobada |
| 7 | **Ronda 6** | Reconocimiento de empates en primer lugar sin elegir al azar | Retorna todas las opciones empatadas | ✅ Aprobada |
| 8 | **Ronda 7 (a)** | Rechazo de votos tras la fecha de cierre de la votación | `ResultadoVoto.votacionCerrada`, contador intacto | ✅ Aprobada |
| 9 | **Ronda 7 (b)** | Registro exitoso de votos cuando la votación sigue abierta | `ResultadoVoto.exitoso` | ✅ Aprobada |
| 10 | **Integración** | Simulación completa de plebiscito con múltiples vecinos | Ganador correcto y descarte de votos repetidos | ✅ Aprobada |

---

## 💻 2. Salida de Terminal (`flutter test`)

Ejecución de la suite completa de pruebas automatizadas:

```bash
$ flutter test
00:00 +0: test\servicio_votacion_test.dart: registrar un voto valido incrementa el contador de esa opcion
00:00 +1: test\servicio_votacion_test.dart: votar por una opcion que no existe regresa opcionInvalida
00:00 +2: test\servicio_votacion_test.dart: un mismo usuario no puede votar dos veces
00:00 +3: test\servicio_votacion_test.dart: calcula el porcentaje de cada opcion correctamente
00:00 +4: test\servicio_votacion_test.dart: si no hay ningun voto, todos los porcentajes son 0
00:00 +5: test\servicio_votacion_test.dart: determinarGanador regresa la opcion con mas votos
00:00 +6: test\servicio_votacion_test.dart: si hay empate, determinarGanador regresa mas de una opcion
00:00 +7: test\servicio_votacion_test.dart: no se puede votar si la votacion ya cerro
00:00 +8: test\servicio_votacion_test.dart: si la votacion sigue abierta, el voto se registra normalmente
00:01 +9: test\servicio_votacion_test.dart: simulacion completa: varios vecinos votan y se determina un ganador
00:01 +10: All tests passed!
```

---

## 📸 3. Evidencias del Proceso TDD y de la Aplicación

### 3.1. Ciclo TDD: Red 🔴 y Green 🟢 por Rondas

A continuación se documentan las fases de prueba guiadas por TDD, mostrando el fallo inicial (fase roja) y la posterior resolución (fase verde).

---

#### 📍 Ronda 1: Registro de un Voto Válido

- **🔴 Fase Roja (Código y Consola):** Se escribe la prueba antes de definir la clase `ServicioVotacion`. El editor y la consola indican que la clase no existe.

| Fase Roja (Editor) | Fase Roja (Consola) |
| :---: | :---: |
| ![Ronda 1 - Test inicial en rojo](assets/Captura%20de%20pantalla%202026-10-05%20171958.png) | ![Ronda 1 - Error en terminal](assets/Captura%20de%20pantalla%202026-10-05%20172405.png) |

- **🟢 Fase Verde (Consola):** Se crea el servicio mínimo y la prueba pasa exitosamente (`+1: All tests passed!`).

<div align="center">
  <img src="assets/Captura%20de%20pantalla%202026-10-05%20172446.png" alt="Ronda 1 - Test Aprobado" width="800" />
</div>

---

#### 📍 Ronda 2: Voto por Opción Inválida

- **🔴 Fase Roja (Consola):** Al votar por un ID inexistente, el sistema falla por `Null check operator used on a null value`.
- **🟢 Fase Verde (Editor y Consola):** Se implementa la comprobación de nulidad retornando `ResultadoVoto.opcionInvalida` (`+2: All tests passed!`).

| 🔴 Fase Roja (Fallo por Null) | 🟢 Fase Verde (Test Aprobado) |
| :---: | :---: |
| ![Ronda 2 - Fallo en terminal](assets/Captura%20de%20pantalla%202026-10-05%20172610.png) | ![Ronda 2 - Test y Terminal Verde](assets/Captura%20de%20pantalla%202026-10-07%20165250.png) |

---

#### 📍 Ronda 3: Prevención de Voto Duplicado

- **🔴 Fase Roja (Consola):** La prueba falla (`+2 -1`) porque el servicio aún no registra el historial de votantes y permite el segundo voto.
- **🟢 Fase Verde (Editor y Consola):** Se agrega el conjunto `votantes` y la validación `votacion.votantes.contains(idUsuario)` (`+3: All tests passed!`).

| 🔴 Fase Roja (Fallo de duplicado) | 🟢 Fase Verde (Validación exitosa) |
| :---: | :---: |
| ![Ronda 3 - Fallo Voto Duplicado](assets/Captura%20de%20pantalla%202026-10-07%20165335.png) | ![Ronda 3 - Código y Terminal Verde](assets/Captura%20de%20pantalla%202026-10-07%20165424.png) |

---

#### 📍 Ronda 4: Cálculo de Porcentajes y Casos Borde

- **🔴 Fase Roja:** Se escribe la prueba para calcular porcentajes y caso borde de 0 votos. La consola reporta métodos y modelos no definidos (`obtenerResultados` y `ResultadoOpcion`).

| 🔴 Método no definido | 🔴 Modelo no definido |
| :---: | :---: |
| ![Ronda 4 - Método no definido](assets/Captura%20de%20pantalla%202026-10-07%20165506.png) | ![Ronda 4 - Ajuste de modelo](assets/Captura%20de%20pantalla%202026-10-07%20165654.png) |

---

### 3.2. Suite Completa y Prueba de Integración (10/10 Pasadas)

Validación integral del flujo del plebiscito simulando varios vecinos, descarte de votos repetidos y determinación del ganador legítimo.

<div align="center">
  <img src="assets/Captura%20de%20pantalla%202026-10-07%20171048.png" alt="Suite Completa 10 de 10 pruebas aprobadas" width="850" />
  <p><em>Prueba de integración en editor y terminal con los 10 tests aprobados exitosamente.</em></p>
</div>

---

### 3.3. Interfaz Gráfica en Ejecución

Capturas de la aplicación en funcionamiento interactuando con las reglas de negocio, animaciones de progreso y modal de resultados.

| Votación: Jardín Principal (100%) | Modal: Revelación del Ganador | Votación: Alumbrado Analco (100%) |
| :---: | :---: | :---: |
| <img src="assets/Captura%20de%20pantalla%202026-10-07%20171446.png" width="280" alt="Votación Jardín Principal" /> | <img src="assets/Captura%20de%20pantalla%202026-10-07%20171453.png" width="280" alt="Modal Resultado Ganador" /> | <img src="assets/Captura%20de%20pantalla%202026-10-07%20171601.png" width="280" alt="Votación Alumbrado" /> |
| *Visualización de barra porcentual animada* | *Diálogo animado con el ganador del plebiscito* | *Interacción reactiva con cambio de opción* |

---

## ✅ 4. Checklist de Cumplimiento TDD

- [x] **Ronda 1:** Registro de voto válido con incremento de contador.
- [x] **Ronda 2:** Rechazo controlado de opciones inexistentes (`ResultadoVoto.opcionInvalida`).
- [x] **Ronda 3:** Restricción de voto único por elector (`ResultadoVoto.usuarioYaVoto`).
- [x] **Ronda 4:** Cálculo dinámico de porcentajes y prevención de división entre cero cuando el total es 0.
- [x] **Ronda 5:** Detección de ganador con mayor cantidad de sufragios.
- [x] **Ronda 6:** Gestión de empates múltiples en primer lugar.
- [x] **Ronda 7:** Control estricto de fecha de cierre de la votación (`ResultadoVoto.votacionCerrada`).
- [x] **Integración:** Simulación completa de plebiscito vecinal con 10/10 pruebas automatizadas aprobadas.
- [x] **UI Reactiva:** La interfaz gráfica delega todas las reglas a `ServicioVotacion` sin duplicar lógica de negocio.

---

## 🚀 5. Ejecución del Proyecto

### Ejecutar las pruebas automatizadas:
```bash
flutter test
```

### Ejecutar la aplicación:
```bash
flutter run
```
