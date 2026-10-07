# Vota Dolores Hidalgo — Evidencia de las Pruebas

> **Desarrollo Móvil Integral** — Proyecto complementario de práctica TDD  
> 🗳️ Votación · 📊 Barras animadas · 🏆 Revelación del ganador con animación

---

# Evidencia de las Pruebas

En este documento se presenta la evidencia de la ejecución, validación y cobertura de las pruebas unitarias y de integración desarrolladas mediante la metodología **TDD (Test-Driven Development)** para el proyecto **Vota Dolores Hidalgo**.

---

## 1. Resumen de Pruebas Unitarias y de Integración

Todas las reglas de negocio del plebiscito se encuentran protegidas mediante pruebas automatizadas en `test/servicio_votacion_test.dart`:

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

## 2. Salida de Terminal (`flutter test`)

Ejecución de la suite completa de pruebas unitarias y de integración:

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

## 3. Capturas de Pantalla de Evidencia

### 📸 Evidencia de Pruebas Automatizadas
> *Captura de la terminal ejecutando `flutter test` con todas las pruebas aprobadas:*
<img width="1110" height="653" alt="Captura de pantalla 2026-10-07 171048" src="https://github.com/user-attachments/assets/ed95b3a1-0d73-4566-b94c-cb3e98dee1a8" />

![Pruebas en Terminal](docs/evidencias/terminal_tests.png)

---

### 📱 Evidencia de la Aplicación en Ejecución
> *Capturas de la interfaz gráfica animada interactuando con las reglas probadas:*

| Barras de Porcentajes Animadas | Revelación del Ganador (Modal Animado) |
| :---: | :---: |
| (<img width="617" height="682" alt="Captura de pantalla 2026-10-07 171446" src="https://github.com/user-attachments/assets/df74fc6c-af0c-4857-aed7-00624bf49311" />
) |(<img width="623" height="907" alt="Captura de pantalla 2026-10-07 171453" src="https://github.com/user-attachments/assets/0044c87b-1e53-4563-90fb-02002322898b" />
) |

---

## 4. Checklist de Cumplimiento TDD

- [x] Cada regla de negocio (voto único, opción válida, fecha de cierre, empates) cuenta con su prueba correspondiente.
- [x] `ResultadoVoto` gestiona los casos de negocio esperados sin recurrir a excepciones no controladas.
- [x] La función `obtenerResultados()` previene divisiones entre cero cuando el total de votos es 0.
- [x] La interfaz gráfica delega todas las reglas a `ServicioVotacion` sin duplicar lógica de negocio.
- [x] Prueba de integración que valida el flujo completo de un plebiscito vecinal y descarta votos duplicados.
