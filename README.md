# 🤠 Momias de Guanajuato: La Leyenda 🧟

> **Desarrollo Móvil Integral — Proyecto Complementario de Práctica TDD (10mo Cuatrimestre)**  
> *Un videojuego de plataformas 2D con física real, construido 100% guiado por pruebas (Test-Driven Development).*

![Flutter](https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white)
![Dart](https://img.shields.io/badge/Dart-0175C2?style=for-the-badge&logo=dart&logoColor=white)
![TDD](https://img.shields.io/badge/TDD-100%25_Tested-success?style=for-the-badge)
![Tests](https://img.shields.io/badge/Tests-23%20Passed-brightgreen?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-purple?style=for-the-badge)

---

## 📖 Tabla de Contenidos
1. [Descripción del Proyecto](#-descripción-del-proyecto)
2. [Mecánicas del Juego](#-mecánicas-del-juego)
3. [Arquitectura y Estructura del Código](#-arquitectura-y-estructura-del-código)
4. [Metodología TDD Paso a Paso (Rondas 1 a 10)](#-metodología-tdd-paso-a-paso-rondas-1-a-10)
   - [Ronda 1: Colisión AABB](#ronda-1--colisión-entre-rectángulos-aabb)
   - [Ronda 2: Gravedad y Salto](#ronda-2--gravedad-y-salto)
   - [Ronda 3: Aterrizaje sobre Plataformas](#ronda-3--aterrizar-sobre-una-plataforma)
   - [Ronda 4: Movimiento Horizontal y Límites](#ronda-4--movimiento-horizontal-con-límites-del-nivel)
   - [Ronda 5: Patrullaje de Momias (IA Simple)](#ronda-5--la-momia-patrulla-entre-dos-límites)
   - [Ronda 6: Motor de Combate (Stomp vs Golpe)](#ronda-6--combate-saltar-encima-derrota-chocar-de-lado-golpea)
   - [Ronda 7: Recolección de Calaveritas](#ronda-7--recolectar-calaveritas)
   - [Ronda 8: Detección de la Meta](#ronda-8--detectar-la-meta-el-callejón-del-beso)
   - [Ronda 9: Sistema de Vidas y Game Over](#ronda-9--vidas-perder-vida-y-game-over)
   - [Ronda 10: Motor del Juego Integrado](#ronda-10--motorjuego-el-bucle-que-integra-todo)
5. [Prueba de Integración](#-prueba-de-integración)
6. [Resultados de la Suite de Pruebas](#-resultados-de-la-suite-de-pruebas)
7. [Interfaz Gráfica y Capturas en Ejecución](#-interfaz-gráfica-y-capturas-en-ejecución)
8. [Checklist de Calidad TDD](#-checklist-de-calidad-tdd)
9. [Instrucciones de Instalación y Ejecución](#-instrucciones-de-instalación-y-ejecución)
10. [Retos de Extensión](#-retos-de-extensión)

---

## 🎮 Descripción del Proyecto

En **Momias de Guanajuato: La Leyenda**, un intrépido explorador debe recorrer los legendarios y angostos callejones empedrados de la ciudad de Guanajuato. Para lograr su objetivo, debe saltar entre plataformas de cantera rosa, esquivar y aplastar a las momias que patrullan las calles, recolectar dulces calaveritas de azúcar para sumar puntos y finalmente llegar al emblemático **Callejón del Beso**.

A diferencia de proyectos por turnos o con movimientos discretos en cuadrículas, un juego de plataformas corre a **60 cuadros por segundo (FPS)** con cálculo continuo de cinemática y detección de colisiones. Para garantizar robustez total, **el 100% de la física, combate, lógica de patrullaje y reglas de juego fueron diseñadas como funciones y clases puras en Dart**, blindadas mediante pruebas unitarias con TDD antes de implementar los widgets de Flutter.

---

## 🕹 Mecánicas del Juego

- **🤠 Explorador (Personaje):** Cae por gravedad constante, salta con impulso vertical al estar sobre el suelo y se traslada horizontalmente.
- **🧱 Plataformas:** Superficies sólidas donde el personaje puede aterrizar desde arriba gracias a detección temporal basada en su posición previa (`yAnterior`).
- **🧟 Momias Patrulleras:** Enemigos con IA determinista de patrullaje entre dos puntos de control fijos, invirtiendo su dirección automáticamente.
- **💥 Sistema de Combate:**
  - *Stomp:* Saltar y caer sobre la mitad superior de una momia la derrota de inmediato y le otorga al jugador un rebote ascendente.
  - *Golpe:* Chocar lateralmente con una momia reduce una vida y reubica al explorador en su punto de partida.
- **💀 Calaveritas de Azúcar:** Ítems coleccionables repartidos por el nivel que otorgan **50 puntos** cada una.
- **💋 Callejón del Beso (Meta):** Alcanzar la coordenada final del nivel otorga la victoria; perder todas las vidas provoca el *Game Over*.

---

## 🏗 Arquitectura y Estructura del Código

El proyecto mantiene una estricta separación de responsabilidades: la carpeta `lib/logica/` no tiene dependencias de Flutter ni de la interfaz gráfica, permitiendo que la suite de pruebas unitarias corra a máxima velocidad sobre la máquina virtual de Dart.

```text
momias_guanajuato/
├── lib/
│   ├── logica/
│   │   ├── calaverita.dart         # Entidad y dimensiones de calaveritas
│   │   ├── estado_juego.dart       # Enum (jugando, ganado, perdido)
│   │   ├── momia.dart              # Entidad momia e IA de patrullaje
│   │   ├── motor_combate.dart      # Reglas de colisión: derrota vs daño
│   │   ├── motor_fisica.dart       # Cinemática, gravedad, salto, colisión con piso
│   │   ├── motor_juego.dart        # Orquestador del bucle y reglas globales
│   │   ├── personaje.dart          # Estado y física del explorador
│   │   └── rectangulo.dart         # Algoritmo de colisión geométrica AABB
│   ├── presentation/
│   │   └── juego_screen.dart       # Renderizado, cámara dinámica y controles táctiles
│   └── main.dart                   # Punto de entrada de la aplicación Flutter
├── test/
│   ├── momia_test.dart             # Pruebas de patrullaje e inversión de marcha
│   ├── motor_combate_test.dart     # Pruebas de stomp y colisión lateral
│   ├── motor_fisica_test.dart      # Pruebas de gravedad, salto, piso y límites
│   ├── motor_juego_test.dart       # Pruebas de vidas, calaveritas y meta
│   └── rectangulo_test.dart        # Pruebas de intersección AABB pura
└── assets/                         # Evidencias fotográficas del ciclo TDD y UI
```

---

## 🧪 Metodología TDD Paso a Paso (Rondas 1 a 10)

El desarrollo siguió estrictamente la metodología **Red-Green-Refactor**:
1. 🔴 **ROJO:** Escribir una prueba unitaria que describa el comportamiento esperado y verificar que falle.
2. 🟢 **VERDE:** Escribir la mínima cantidad de código necesario en Dart para que la prueba pase.
3. 🔵 **REFACTORIZAR:** Pulir el diseño garantizando que toda la suite permanezca en verde.

---

### Ronda 1 — Colisión entre rectángulos (AABB)

**Regla de negocio:** Dos cajas alineadas en los ejes $X$ e $Y$ se intersectan si y solo si se traslapan en ambos ejes simultáneamente. Esta primitiva geométrica sustenta todas las colisiones del juego.

- **Prueba (`test/rectangulo_test.dart`):**
```dart
test('dos rectangulos que se traslapan se detectan como interseccion', () {
  const a = Rectangulo(x: 0, y: 0, ancho: 10, alto: 10);
  const b = Rectangulo(x: 5, y: 5, ancho: 10, alto: 10);
  expect(a.intersectaCon(b), true);
});

test('dos rectangulos separados no se intersectan', () {
  const a = Rectangulo(x: 0, y: 0, ancho: 10, alto: 10);
  const b = Rectangulo(x: 20, y: 20, ancho: 10, alto: 10);
  expect(a.intersectaCon(b), false);
});
```

- **Solución (`lib/logica/rectangulo.dart`):**
```dart
class Rectangulo {
  final double x, y, ancho, alto;
  const Rectangulo({required this.x, required this.y, required this.ancho, required this.alto});

  bool intersectaCon(Rectangulo otro) {
    return x < otro.x + otro.ancho &&
        x + ancho > otro.x &&
        y < otro.y + otro.alto &&
        y + alto > otro.y;
  }
}
```

#### 🟢 Evidencia Ronda 1 en Verde:
![Ronda 1 Verde](assets/Captura%20de%20pantalla%202026-10-12%20175900.png)

---

### Ronda 2 — Gravedad y salto

**Regla de negocio:** La gravedad debe incrementar la velocidad vertical del personaje en cada cuadro. El salto solo debe ser posible si el personaje se encuentra apoyado en el suelo.

- **Prueba (`test/motor_fisica_test.dart`):**
```dart
test('aplicarGravedad aumenta la velocidad vertical y mueve al personaje hacia abajo', () {
  final personaje = Personaje(x: 0, y: 0);
  MotorFisica.aplicarGravedad(personaje);
  expect(personaje.velocidadY, MotorFisica.gravedad);
  expect(personaje.y, MotorFisica.gravedad);
});

test('saltar solo funciona si el personaje esta en el suelo', () {
  final personaje = Personaje(x: 0, y: 0)..enElSuelo = true;
  MotorFisica.saltar(personaje);
  expect(personaje.velocidadY, MotorFisica.impulsoSalto);
  expect(personaje.enElSuelo, false);
});
```

#### 🔴 Evidencia Ronda 2 en Rojo:
Fallo inicial por clases y métodos inexistentes (`Personaje`, `MotorFisica`):
![Ronda 2 Rojo](assets/Captura%20de%20pantalla%202026-10-12%20180025.png)

#### 🟢 Evidencia Ronda 2 en Verde:
Implementación de la cinemática básica superando las pruebas con éxito:
![Ronda 2 Verde](assets/Captura%20de%20pantalla%202026-10-12%20180247.png)

---

### Ronda 3 — Aterrizar sobre una plataforma

**Regla de negocio:** Para evitar el efecto de atravesar plataformas (*tunneling*), se utiliza la "costura testable" de la posición anterior `yAnterior`. Si el pie del personaje venía por encima de la plataforma y ahora la cruza cayendo (`velocidadY >= 0`), aterriza sobre su superficie y se cancela la velocidad vertical.

- **Prueba (`test/motor_fisica_test.dart`):**
```dart
test('el personaje aterriza sobre una plataforma al caer sobre ella', () {
  final personaje = Personaje(x: 10, y: 70)..velocidadY = 5;
  const plataforma = Rectangulo(x: 0, y: 100, ancho: 100, alto: 20);

  MotorFisica.resolverColisionConPiso(personaje, [plataforma], 65);

  expect(personaje.enElSuelo, true);
  expect(personaje.y, 100 - personaje.alto);
  expect(personaje.velocidadY, 0);
});
```

#### 🔴 Evidencia Ronda 3 en Rojo:
Fallo de ejecución durante la adición de los nuevos casos de prueba:
![Ronda 3 Rojo](assets/Captura%20de%20pantalla%202026-10-12%20180423.png)

#### 🟢 Evidencia Ronda 3 en Verde:
Lógica de aterrizaje con `resolverColisionConPiso` validada (5 pruebas pasando):
![Ronda 3 Verde](assets/Captura%20de%20pantalla%202026-10-12%20180948.png)

---

### Ronda 4 — Movimiento horizontal con límites del nivel

**Regla de negocio:** El personaje debe desplazarse horizontalmente según el valor delta, impidiéndole salir del rango $[0, anchoNivel - ancho]$.

- **Prueba (`test/motor_fisica_test.dart`):**
```dart
test('moverHorizontal no deja salir al personaje por el borde izquierdo', () {
  final personaje = Personaje(x: 2, y: 0);
  MotorFisica.moverHorizontal(personaje, -10, 1000);
  expect(personaje.x, 0);
});

test('moverHorizontal no deja salir al personaje por el borde derecho', () {
  final personaje = Personaje(x: 990, y: 0);
  MotorFisica.moverHorizontal(personaje, 20, 1000);
  expect(personaje.x, 1000 - personaje.ancho);
});
```

#### 🔴 Evidencia Ronda 4 en Rojo:
Fallo al ejecutar las pruebas antes de implementar el método de contención horizontal:
![Ronda 4 Rojo](assets/Captura%20de%20pantalla%202026-10-12%20181054.png)

#### 🟢 Evidencia Ronda 4 en Verde:
Método `MotorFisica.moverHorizontal` operativo y verificado (8 pruebas pasando):
![Ronda 4 Verde](assets/Captura%20de%20pantalla%202026-10-12%20181119.png)

---

### Ronda 5 — La momia patrulla entre dos límites

**Regla de negocio:** La momia se mueve continuamente en línea recta. Al alcanzar o rebasar su límite derecho o izquierdo, invierte automáticamente el signo de su velocidad. Si fue derrotada, permanece estática.

- **Prueba (`test/momia_test.dart`):**
```dart
test('la momia invierte direccion al llegar al limite derecho', () {
  final momia = Momia(x: 165, y: 0, limiteIzquierdo: 50, limiteDerecho: 200, velocidadX: 2);
  momia.patrullar();
  momia.patrullar();
  expect(momia.velocidadX, -2);
});

test('una momia derrotada no se mueve', () {
  final momia = Momia(x: 100, y: 0, limiteIzquierdo: 50, limiteDerecho: 200)..derrotada = true;
  momia.patrullar();
  expect(momia.x, 100);
});
```

#### 🔴 Evidencia Ronda 5 en Rojo:
Error de compilación al requerir la clase `Momia`:
![Ronda 5 Rojo](assets/Captura%20de%20pantalla%202026-10-12%20181221.png)

#### 🟢 Evidencia Ronda 5 en Verde:
Patrullaje determinista de enemigos en verde (3 pruebas pasando):
![Ronda 5 Verde](assets/Captura%20de%20pantalla%202026-10-12%20181259.png)

---

### Ronda 6 — Combate: saltar encima derrota, chocar de lado golpea

**Regla de negocio:** Para lograr un *stomp* auténtico (al estilo de los clásicos de plataformas), el explorador debe cumplir dos condiciones simultáneas: ir con velocidad vertical descendente (`velocidadY > 0`) y que la parte inferior de su cuerpo esté en la mitad superior de la momia. Cualquier otro impacto se considera daño al jugador.

- **Prueba (`test/motor_combate_test.dart`):**
```dart
test('caer sobre la mitad superior de una momia la derrota', () {
  final personaje = Personaje(x: 100, y: 68)..velocidadY = 5;
  final momia = Momia(x: 100, y: 100, limiteIzquierdo: 0, limiteDerecho: 200);

  final resultado = MotorCombate.resolverColision(personaje, momia);

  expect(resultado, ResultadoColisionMomia.momiaDerrotada);
  expect(momia.derrotada, true);
});

test('chocar de lado con una momia golpea al jugador', () {
  final personaje = Personaje(x: 90, y: 100)..velocidadY = 0;
  final momia = Momia(x: 100, y: 100, limiteIzquierdo: 0, limiteDerecho: 200);

  final resultado = MotorCombate.resolverColision(personaje, momia);

  expect(resultado, ResultadoColisionMomia.jugadorGolpeado);
  expect(momia.derrotada, false);
});
```

#### 🟢 Evidencia Ronda 6 en Verde:
Resolución de colisiones de combate verificada en verde:
![Ronda 6 Verde](assets/Captura%20de%20pantalla%202026-10-12%20181521.png)

---

### Ronda 7 — Recolectar calaveritas

**Regla de negocio:** Al entrar en contacto con una calaverita no recolectada, esta cambia su bandera a `recolectada = true` y suma 50 puntos al marcador. No puede recolectarse dos veces.

- **Prueba (`test/motor_juego_test.dart`):**
```dart
test('recolectar una calaverita suma puntos y la marca como recolectada', () {
  final personaje = Personaje(x: 100, y: 100);
  final calaverita = Calaverita(x: 105, y: 105);

  final puntos = MotorJuego.recolectarSiCorresponde(personaje, [calaverita]);

  expect(puntos, 50);
  expect(calaverita.recolectada, true);
});
```

#### 🔴 Evidencia Ronda 7 en Rojo:
Falta de definición de `Calaverita` y `MotorJuego.recolectarSiCorresponde`:
![Ronda 7 Rojo](assets/Captura%20de%20pantalla%202026-10-12%20183007.png)

#### 🟢 Evidencia Ronda 7 en Verde:
Pruebas de recolección de coleccionables pasando:
![Ronda 7 Verde](assets/Captura%20de%20pantalla%202026-10-12%20183126.png)

---

### Ronda 8 — Detectar la meta (el Callejón del Beso)

**Regla de negocio:** El juego detecta que el jugador ha ganado cuando su coordenada $X$ es igual o mayor a la coordenada de la meta configurada en el nivel.

- **Prueba (`test/motor_fisica_test.dart`):**
```dart
test('haLlegadoALaMeta es verdadero cuando el personaje alcanza la posicion', () {
  final personaje = Personaje(x: 1900, y: 0);
  expect(MotorFisica.haLlegadoALaMeta(personaje, 1900), true);
});

test('haLlegadoALaMeta es falso si aun no llega', () {
  final personaje = Personaje(x: 500, y: 0);
  expect(MotorFisica.haLlegadoALaMeta(personaje, 1900), false);
});
```

#### 🔴 Evidencia Ronda 8 en Rojo:
Fallo por ausencia del método `haLlegadoALaMeta` en `MotorFisica`:
![Ronda 8 Rojo](assets/Captura%20de%20pantalla%202026-10-12%20183229.png)

#### 🟢 Evidencia Ronda 8 en Verde:
Meta detectada con precisión (10 pruebas de física en verde):
![Ronda 8 Verde](assets/Captura%20de%20pantalla%202026-10-12%20183300.png)
![Física Completa Verde](assets/Captura%20de%20pantalla%202026-10-12%20215456.png)

---

### Ronda 9 — Vidas: perder vida y Game Over

**Regla de negocio:** Recibir un golpe lateral descuenta una vida y reinicia al personaje en su posición inicial (`_xInicial`, `_yInicial`). Si las vidas llegan a cero, el estado cambia a `EstadoJuego.perdido`.

- **Prueba (`test/motor_juego_test.dart`):**
```dart
test('al chocar lateralmente con una momia se pierde una vida y el personaje regresa al inicio', () {
  final personaje = Personaje(x: 95, y: 100);
  final momia = Momia(x: 100, y: 100, limiteIzquierdo: 100, limiteDerecho: 200, velocidadX: 0);
  final motor = MotorJuego(
    personaje: personaje,
    plataformas: const [Rectangulo(x: 0, y: 300, ancho: 1000, alto: 20)],
    momias: [momia],
    calaveritas: const [],
    posicionMeta: 500,
    anchoNivel: 1000,
  );

  motor.mover(0);

  expect(motor.vidas, 2);
  expect(personaje.x, 95);
});
```

---

### Ronda 10 — MotorJuego: el bucle que integra todo

El método `MotorJuego.mover(double deltaX)` orquesta las reglas en el orden riguroso necesario para la correcta simulación de 60 FPS:
1. Mover horizontalmente respetando los límites del nivel.
2. Aplicar aceleración por gravedad.
3. Resolver colisión con plataformas según `yAnterior`.
4. Actualizar patrullaje de momias y evaluar colisiones de combate.
5. Recolectar calaveritas intersectadas.
6. Evaluar condición de victoria al llegar a la meta.

```dart
void mover(double deltaX) {
  if (estado != EstadoJuego.jugando) return;
  final yAnterior = personaje.y;

  MotorFisica.moverHorizontal(personaje, deltaX, anchoNivel);
  MotorFisica.aplicarGravedad(personaje);
  MotorFisica.resolverColisionConPiso(personaje, plataformas, yAnterior);

  for (final momia in momias) {
    momia.patrullar();
    final resultado = MotorCombate.resolverColision(personaje, momia);
    if (resultado == ResultadoColisionMomia.jugadorGolpeado) {
      _perderVida();
      return;
    }
  }

  puntaje += recolectarSiCorresponde(personaje, calaveritas);

  if (MotorFisica.haLlegadoALaMeta(personaje, posicionMeta)) {
    estado = EstadoJuego.ganado;
  }
}
```

---

## 🎯 Prueba de Integración

Para validar que todos los motores (física, combate, recolección y meta) coexisten armoniosamente, se implementó la prueba de integración completa en `test/motor_juego_test.dart`:

```dart
test('llegar a la meta cambia el estado a ganado', () {
  final personaje = Personaje(x: 495, y: 268);
  final motor = MotorJuego(
    personaje: personaje,
    plataformas: const [Rectangulo(x: 0, y: 300, ancho: 1000, alto: 20)],
    momias: const [],
    calaveritas: const [],
    posicionMeta: 500,
    anchoNivel: 1000,
  );

  motor.mover(10); // 495 + 10 = 505 (supera los 500 de meta)

  expect(motor.estado, EstadoJuego.ganado);
});
```

---

## 🏆 Resultados de la Suite de Pruebas

Al ejecutar el comando `flutter test` en la raíz del proyecto, los **5 archivos de prueba** pasan al 100% sin advertencias ni fallos:

| Archivo de Prueba | Casos de Prueba | Estado |
| :--- | :---: | :---: |
| `test/rectangulo_test.dart` | 2 | 🟢 Pasó |
| `test/momia_test.dart` | 3 | 🟢 Pasó |
| `test/motor_combate_test.dart` | 3 | 🟢 Pasó |
| `test/motor_fisica_test.dart` | 10 | 🟢 Pasó |
| `test/motor_juego_test.dart` | 5 | 🟢 Pasó |
| **Total General** | **23 pruebas** | **🟢 100% Verde** |

### 📸 Captura de la Suite Completa en Verde (`23 tests passed`):
![Suite Completa 23 Pruebas](assets/Captura%20de%20pantalla%202026-10-12%20215518.png)

---

## 🎨 Interfaz Gráfica y Capturas en Ejecución

La capa visual construida en Flutter (`lib/presentation/juego_screen.dart`) se encarga únicamente de traducir las entidades de Dart puro a píxeles en pantalla:

- **Cámara dinámica:** Sigue la posición horizontal del explorador manteniendo un encuadre centrado (`camaraX`).
- **Paleta de Guanajuato:** Fondos cálidos inspirados en el atardecer guanajuatense y plataformas decoradas con el característico **rosa mexicano** de las fachadas coloniales.
- **HUD interactivo:** Visualizador superior de vidas restantes y calaveritas recolectadas en tiempo real.
- **Controles táctiles:** Botones inferiores de tacto continuo para desplazarse a la izquierda/derecha y saltar hacia arriba.

### 🎮 El juego en acción:
A continuación se muestran capturas del juego en ejecución sobre Google Chrome / Flutter Web:

#### 1. Explorando el nivel y esquivando momias (Vidas: 2, Plataformas y Calaveritas):
![Juego en Ejecución 1](assets/Captura%20de%20pantalla%202026-10-12%20215750.png)

#### 2. Vista panorámica inicial con HUD completo (Vidas: 3, Puntaje: 0):
![Juego en Ejecución 2](assets/Captura%20de%20pantalla%202026-10-12%20215951.png)

---

## ✅ Checklist de Calidad TDD

- [x] **AABB aislado:** La colisión geométrica `Rectangulo` se probó independientemente antes de integrarla con el resto de entidades.
- [x] **Costura temporal:** `resolverColisionConPiso` utiliza la posición previa `yAnterior` para distinguir caídas legítimas de penetraciones laterales o inferiores.
- [x] **Criterio estricto de Stomp:** Caer sobre una momia exige `velocidadY > 0` y contacto sobre la mitad superior del enemigo, evitando derrotas accidentales en colisiones laterales.
- [x] **Determinismo:** El patrullaje de las momias y el avance cinemático no dependen de temporizadores del sistema en las pruebas unitarias.
- [x] **Orquestación centralizada:** `MotorJuego.mover()` es la única autoridad sobre el orden secuencial de física, combate, recolección y condiciones de fin de partida.
- [x] **Cero lógica en UI:** `JuegoScreen` no toma decisiones de colisión ni calcula puntuaciones; solo refleja el estado puro del motor.

---

## 🚀 Instrucciones de Instalación y Ejecución

### Prerrequisitos
- [Flutter SDK](https://flutter.dev/docs/get-started/install) (versión 3.13 o superior)
- Dart SDK integrado
- Google Chrome o un emulador de Android/iOS configurado

### Pasos

1. Clonar el repositorio o ingresar a la carpeta del proyecto:
   ```bash
   cd momias_guanajuato
   ```

2. Descargar paquetes y dependencias:
   ```bash
   flutter pub get
   ```

3. **Ejecutar la suite completa de pruebas unitarias (TDD):**
   ```bash
   flutter test
   ```

4. **Lanzar la aplicación:**
   ```bash
   # En navegador Chrome (Flutter Web)
   flutter run -d chrome

   # O en tu dispositivo / emulador conectado
   flutter run
   ```

---

## 🌟 Retos de Extensión

- [ ] **Colisión lateral con plataformas:** Impedir que el explorador atraviese los muros laterales de las plataformas elevadas aplicando el patrón de costura testable.
- [ ] **Vidas visuales con corazones:** Representar las vidas en el HUD con iconos `❤️` en lugar de un indicador numérico simple.
- [ ] **Momias voladoras / patrulla vertical:** Extender la clase `Momia` para soportar patrullaje en el eje vertical entre `limiteArriba` y `limiteAbajo`.
- [ ] **Persistencia del récord:** Guardar la mejor puntuación de calaveritas alcanzada usando el paquete `shared_preferences`.
- [ ] **Nivel 2 extendido:** Diseñar un segundo nivel con saltos más desafiantes reutilizando la misma arquitectura de `MotorJuego`.

---

<p align="center">
  <b>🏆 Insignia Desbloqueada: Cinturón Negro de Proyecto TDD</b><br>
  <i>Plataformas 2D con física, combate e IA de patrullaje, 100% guiado por pruebas.</i>
</p>
