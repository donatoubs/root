# Taller Autónomo: Modelado Combinatorio y All-Pairs
**Módulo:** QA & Testing Combinatorio  
**Integrantes:** Donato Oña, Mateo Villacreses, Guillermo Paredes  

---

## 1. Matriz de Parámetros y Valores (P&V)

Abstracción de los requerimientos del configurador en una matriz formal discreta:

| ID | Parámetro (P) | Valores (V) | n (Cantidad) |
| :--- | :--- | :--- | :---: |
| **P1** | Motor | Gasolina, Híbrido, Eléctrico | 3 |
| **P2** | Transmisión | Manual, Automática, Monomarcha | 3 |
| **P3** | Frenos | Estándar, ABS, Regenerativo | 3 |
| **P4** | Mercado | América, Europa | 2 |
| **P5** | Modo Conducción | Eco, Sport, Autónomo | 3 |

---

## 2. Cálculo de la Explosión Combinatoria

- **Cálculo matemático de combinaciones puras (producto cartesiano):**
  $$v_1 \times v_2 \times v_3 \times v_4 \times v_5 = 3 \times 3 \times 3 \times 2 \times 3 = 243 \text{ combinaciones puras}$$

- **Análisis de Inviabilidad:**  
  Si ejecutar, configurar y verificar cada prueba de forma manual toma un promedio de 15 minutos, probar las 243 opciones requeriría más de 60 horas de trabajo continuo de QA. En ciclos de desarrollo ágiles con despliegues constantes, una suite exhaustiva hipertrofiada genera retrasos críticos en el time-to-market y costos financieramente inviables, por ello, es indispensable aplicar muestreos combinatorios optimizados.

---

## 3. Derivación de la Suite All-Pairs ($t=2$)

Suite reducida y ortogonal de casos de prueba para garantizar la cobertura de todas las parejas de valores con el mínimo esfuerzo (~15 casos):

| Test Case | Motor | Transmisión | Frenos | Mercado | Modo Conducción |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-01** | Gasolina | Manual | ABS | América | Eco |
| **TC-02** | Gasolina | Automática | Estándar | Europa | Sport |
| **TC-03** | Gasolina | Monomarcha | Regenerativo | América | Autónomo |
| **TC-04** | Híbrido | Manual | Estándar | Europa | Autónomo |
| **TC-05** | Híbrido | Automática | Regenerativo | América | Eco |
| **TC-06** | Híbrido | Monomarcha | ABS | Europa | Sport |
| **TC-07** | Eléctrico | Manual | Regenerativo | Europa | Sport |
| **TC-08** | Eléctrico | Automática | ABS | América | Autónomo |
| **TC-09** | Eléctrico | Monomarcha | Estándar | América | Eco |
| **TC-10** | Gasolina | Manual | Regenerativo | Europa | Eco |
| **TC-11** | Híbrido | Automática | Estándar | América | Sport |
| **TC-12** | Eléctrico | Monomarcha | ABS | Europa | Autónomo |
| **TC-13** | Gasolina | Automática | ABS | América | Autónomo |
| **TC-14** | Híbrido | Manual | Regenerativo | América | Sport |
| **TC-15** | Eléctrico | Manual | Estándar | Europa | Eco |

---

## 4. Identificación de Constraints (Restricciones Lógicas)

Reglas de negocio formales para excluir combinaciones físicamente imposibles o inválidas antes de ejecutar las pruebas en producción:

```text
// Regla 1: El motor eléctrico exige transmisión monomarcha y frenos regenerativos
IF [Motor] = "Eléctrico" THEN [Transmisión] = "Monomarcha" AND [Frenos] = "Regenerativo";

// Regla 2: Los vehículos con motor a gasolina no permiten utilizar frenos regenerativos
IF [Motor] = "Gasolina" THEN [Frenos] <> "Regenerativo";

// Regla 3: El modo de conducción autónomo no está disponible para motores de gasolina
IF [Modo Conducción] = "Autónomo" THEN [Motor] <> "Gasolina";
'''

---

## 5. Cierre Cognitivo (Preguntas de Transferencia)
Pregunta 1: ¿Qué riesgo financiero y operativo corremos si ignoramos los constraints lógicos y enviamos la matriz matemática pura directamente al equipo de automatización (QA)?
Respuesta: Si se omiten las restricciones lógicas y se ejecuta una matriz matemática pura, el sistema intentará probar configuraciones físicamente incompatibles (como un motor de gasolina con frenos regenerativos). Operativamente, esto genera falsos positivos masivos, pérdida de tiempo en depuración de errores fantasma y, en un entorno de manufactura real, podría enviar instrucciones de ensamblaje erróneas que provoquen daños críticos en el hardware o fallos catastróficos en producción, traduciéndose en pérdidas financieras millonarias.

Pregunta 2: ¿Por qué la técnica All-Pairs es matemáticamente y empíricamente superior a que un tester diseñe 20 casos de prueba basándose únicamente en su intuición?
Respuesta: La intuición humana suele estar sesgada hacia los casos de uso más comunes o nominales, dejando de lado interacciones complejas entre múltiples variables. En contraste, la técnica All-Pairs se fundamenta en las investigaciones empíricas del NIST (Kuhn et al.), las cuales demuestran estadísticamente que la gran mayoría de los fallos de software son provocados por la interacción de uno o dos parámetros simultáneos. Utilizar All-Pairs garantiza un rigor científico y matemático que maximiza la capacidad diagnóstica cubriendo el 100% de los pares de variables con la menor cantidad de pruebas posibles, algo que la subjetividad humana no puede certificar de manera sistemática.