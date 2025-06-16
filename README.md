# Informe del Proyecto ABP - Cálculo Vectorial
## Datos generales
- **Nombre del proyecto**: Diseño de ciclovia
- **Integrantes**
- Erick Rodrigo Villanueva Ayala / Codigo: 202410153
-
- 
- **Aula**: Teoría 1.03
- **Sala**: Sala  3  
- **Grupo**: Grupo 2  
- **Docente**: Nombre y Apellido

---

## 1. Comprensión del Problema

El problema consiste en diseñar una **ciclovía óptima** que conecte los puntos $A(0,0)$ y $B(5,5)$ sobre una superficie de terreno irregular definida por la función $f(x,y) = z$ , en la cual, $x$ y $y$ esta en metros y , $z$ es la elevación de la superficie . El objetivo principal es **minimizar los costos de construcción**, los cuales están directamente relacionados con la pendiente del camino y las modificaciones necesarias del terreno.

La función de elevación $f(x,y)$ sobre la cual se definirá la ciclovía es:

$$f(x,y) = 4 e^{-0.1(x^2 + y^2)} + 2.5 e^{-0.1((x - 5)^2 + (y - 5)^2)}$$

---

### 1. Modelado de la Trayectoria mediante Funciones Vectoriales

Para este análisis comparativo, se proponen dos trayectorias distintas: una **lineal** y otra **polinomial**. Ambas deben cumplir la condición inicial de conectar los puntos $A(0,0)$ y $B(5,5)$.

### Datos de las Trayectorias Propuestas:

**Trayectoria Lineal:**

Para una trayectoria lineal que conecta $A(0,0)$ y $B(5,5)$, podemos definirla paramétricamente como:

$$\mathbf{r_1}(t) = \langle x(t), y(t) \rangle = \langle 5t, 5t \rangle \quad \text{para } 0 \le t \le 1$$

Donde:
* $x(t) = 5t$
* $y(t) = 5t$

---

**Trayectoria Polinomial:**


Una posible parametrización de la **trayectoria polinomial** $\mathbf{r_2}(t)$ que conecta los puntos $A(0,0)$ y $B(5,5)$ podría ser (esto es un ejemplo, puedes ajustar los coeficientes según tu diseño):

$$\mathbf{r_2}(t) = \langle 5t, 5t^2 \rangle \quad \text{para } 0 \le t \le 1$$

Donde:
* $x(t) = 5t$
* $y(t) = 5t^2$
---
### 2. Cálculo de pendientes y costos asociados
### 3. Determinación del punto silla y distancia vertical de máxima sepación
Objetivo 1: Clasificar los puntos críticos de la superficie. ( Posibles maximos , minimos o puntos sillas en las supercie.

$$f(x,y) = 4 e^{-0.1(x^2 + y^2)} + 2.5 e^{-0.1((x - 5)^2 + (y - 5)^2)}$$

## 3. Objetivo 2: Máxima Separación Vertical y Verificación de Punto Silla

El segundo objetivo consiste en hallar la máxima separación vertical entre un puente y el terreno, y verificar si esta máxima separación ocurre en un punto silla del terreno. El puente se define como una línea recta que conecta los puntos $A(0,0,4.02)$ y $B(5,5,2.53)$.

### 3.1. Modelado de la Línea Recta (Puente)

La línea recta que representa el puente, $\mathbf{L}(t)$, se parametriza como:

$$\mathbf{L}(t) = \langle x(t), y(t), Z_{puente}(t) \rangle$$

Utilizando los puntos $A(0,0,4.02)$ y $B(5,5,2.53)$, la ecuación paramétrica del puente es:

$$\mathbf{L}(t) = \mathbf{A} + t(\mathbf{B} - \mathbf{A})$$
$$\mathbf{L}(t) = \langle 0,0,4.02 \rangle + t(\langle 5,5,2.53 \rangle - \langle 0,0,4.02 \rangle)$$
$$\mathbf{L}(t) = \langle 5t, 5t, 4.02 - 1.49t \rangle \quad \text{para } 0 \le t \le 1$$

La elevación del puente a lo largo de su trayectoria es:

$$Z_{puente}(t) = 4.02 - 1.49t$$

### 3.2. Elevación del Terreno a lo Largo de la Trayectoria del Puente

La función de elevación del terreno es $f(x,y)$. Para evaluar la elevación del terreno bajo la trayectoria del puente, sustituimos $x(t) = 5t$ y $y(t) = 5t$ en $f(x,y)$:

$$Z_{terreno}(t) = f(x(t),y(t)) = 4 e^{-0.1((5t)^2 + (5t)^2)} + 2.5 e^{-0.1(((5t) - 5)^2 + ((5t) - 5)^2)}$$
Simplificando esta expresión:
$$Z_{terreno}(t) = 4 e^{-5t^2} + 2.5 e^{-5(t - 1)^2}$$

### 3.3. Función de Separación Vertical

La separación vertical diferencial, $\Delta Z(t)$, entre el puente y el terreno a lo largo de la trayectoria es la diferencia entre la elevación del puente y la elevación del terreno:

$$\Delta Z(t) = Z_{puente}(t) - Z_{terreno}(t)$$
$$\Delta Z(t) = (4.02 - 1.49t) - \left(4 e^{-5t^2} + 2.5 e^{-5(t - 1)^2}\right)$$

Para hallar la **máxima separación vertical**, es necesario encontrar el valor máximo de $\Delta Z(t)$ en el intervalo $0 \le t \le 1$. Esto se logra calculando la primera derivada de $\Delta Z(t)$, igualándola a cero para encontrar los puntos críticos, y evaluando $\Delta Z(t)$ en estos puntos y en los extremos del intervalo ($t=0$ y $t=1$).
 
### 4. Cálculo del volumen de excavación con integrales dobles
### 5. Análisis comparativo y propuesta final. 

## 2. Desarrollo y aplicación de la solucion
### 2.1 Ejecucion de la solución
### 2.2 Uso de herramientas CAS
### 2.3 Explicación gráfica
## 3.Análisis y dicusión
## 4.Conclusiones
## 5.Bibliografía.
## 6. Reflexión sobre el trabajo(opcional).


  












