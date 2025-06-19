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

### 2.1 Clasificación de Puntos Críticos de la Superficie $f(x,y)$

**Secuencia de Pasos:**
1.  **Definición de las Primeras Derivadas Parciales:** Iniciamos calculando las primeras derivadas parciales de la función de elevación $f(x,y)$ con respecto a $x$ y a $y$ ($f_x$ y $f_y$). Estas derivadas nos permiten identificar la "pendiente" de la superficie en cada dirección.
- Para calcular $f_x$ , derivamos $f(x,y)$ con respecto a  $x$, tratando  $y$ como una constante:

$$
\frac{\partial}{\partial x} \left[ 4e^{-0.1(x^2 + y^2)} \right] = 4e^{-0.1(x^2 + y^2)} \cdot (-0.1 \cdot 2x) = -0.8x e^{-0.1(x^2 + y^2)}
$$

$$
\frac{\partial}{\partial x} \left[ 2.5e^{-0.1((x-5)^2 + (y-5)^2)} \right] = 2.5e^{-0.1((x-5)^2 + (y-5)^2)} \cdot (-0.1 \cdot 2(x - 5)) = -0.5(x - 5) e^{-0.1((x - 5)^2 + (y - 5)^2)}
$$

Por lo tanto, la primera derivada parcial con respecto a \( **x** \) es:

$$
f_x(x, y) = -0.8x e^{-0.1(x^2 + y^2)} - 0.5(x - 5)e^{-0.1((x - 5)^2 + (y - 5)^2)}
$$

---

De manera similar, para calcular \( $f_y$ \), derivamos \( $f(x,y)$ \) con respecto a \( $y$ \), tratando \( $x$ \) como constante:

$$
\frac{\partial}{\partial y} \left[ 4e^{-0.1(x^2 + y^2)} \right] = 4e^{-0.1(x^2 + y^2)} \cdot (-0.1 \cdot 2y) = -0.8y e^{-0.1(x^2 + y^2)}
$$

$$
\frac{\partial}{\partial y} \left[ 2.5e^{-0.1((x - 5)^2 + (y - 5)^2)} \right] = 2.5e^{-0.1((x - 5)^2 + (y - 5)^2)} \cdot (-0.1 \cdot 2(y - 5)) = -0.5(y - 5) e^{-0.1((x - 5)^2 + (y - 5)^2)}
$$

Por lo tanto, la primera derivada parcial con respecto a \( y \) es:

$$
f_y(x, y) = -0.8y e^{-0.1(x^2 + y^2)} - 0.5(y - 5)e^{-0.1((x - 5)^2 + (y - 5)^2)}
$$
  

 ---  
2.  **Identificación de Puntos Críticos:** Establecimos $f_x = 0$ y $f_y = 0$. Dado que el sistema de ecuaciones no lineales resultante es complejo de resolver de forma analítica, la identificación de los puntos críticos $(x_c, y_c)$ se realizará mediante **métodos numéricos iterativos**, como el **método de Newton-Raphson para sistemas de ecuaciones**. En estos puntos, el gradiente de la función es nulo, lo que indica un "aplanamiento" local de la superficie.
---
### Implementación Numérica para la Identificación de Puntos Críticos

Recordemos el sistema de ecuaciones que debemos resolver, derivadas de igualar a cero las primeras derivadas parciales ($f_x$ y $f_y$):

1.  $f_x(x,y) = -0.8x e^{-0.1(x^2 + y^2)} - 0.5(x - 5) e^{-0.1((x - 5)^2 + (y - 5)^2)} = 0$
2.  $f_y(x,y) = -0.8y e^{-0.1(x^2 + y^2)} - 0.5(y - 5) e^{-0.1((x - 5)^2 + (y - 5)^2)} = 0$

Para resolver este sistema, se emplea la librería `SciPy` de Python, que proporciona herramientas robustas para la optimización y la búsqueda de raíces de funciones no lineales. Es recomendable proporcionar la matriz Jacobiana del sistema (compuesta por las segundas derivadas parciales $f_{xx}, f_{yy}, f_{xy}$), ya que esto acelera y mejora la estabilidad del algoritmo de Newton-Raphson subyacente.

A continuación, se muestra el código Python para este procedimiento:

```python
import numpy as np
from scipy.optimize import root

# --- Definición de las ecuaciones del sistema F(X) = [f_x, f_y] = [0, 0] ---
# X es un array [x, y]
def sistema_ecuaciones(X):
    x, y = X
    
    # f_x(x,y)
    eq1 = -0.8 * x * np.exp(-0.1 * (x**2 + y**2)) - \
          0.5 * (x - 5) * np.exp(-0.1 * ((x - 5)**2 + (y - 5)**2))
    
    # f_y(x,y)
    eq2 = -0.8 * y * np.exp(-0.1 * (x**2 + y**2)) - \
          0.5 * (y - 5) * np.exp(-0.1 * ((y - 5)**2 + (x - 5)**2))
    
    return [eq1, eq2]

# --- Definición de la matriz Jacobiana J(X) = [[f_xx, f_xy], [f_yx, f_yy]] ---
def matriz_jacobiana(X):
    x, y = X
    
    # f_xx(x,y)
    fxx_val = np.exp(-0.1 * (x**2 + y**2)) * (-0.8 + 0.16 * x**2) + \
              np.exp(-0.1 * ((x - 5)**2 + (y - 5)**2)) * (-0.5 + 0.1 * (x - 5)**2)
              
    # f_yy(x,y)
    fyy_val = np.exp(-0.1 * (x**2 + y**2)) * (-0.8 + 0.16 * y**2) + \
              np.exp(-0.1 * ((x - 5)**2 + (y - 5)**2)) * (-0.5 + 0.1 * (y - 5)**2)
              
    # f_xy(x,y) (que es igual a f_yx)
    fxy_val = 0.16 * x * y * np.exp(-0.1 * (x**2 + y**2)) + \
              0.1 * (x - 5) * (y - 5) * np.exp(-0.1 * ((x - 5)**2 + (y - 5)**2))
              
    return np.array([[fxx_val, fxy_val],
                     [fxy_val, fyy_val]])

# --- Búsqueda de Puntos Críticos ---
# Las suposiciones iniciales son cruciales para métodos numéricos y se eligen
# basándose en la forma de la función (picos en (0,0) y (5,5) y un posible punto
# silla intermedio).
initial_guesses = [
    [0.0, 0.0],  
    [5.0, 5.0],  
    [2.5, 2.5],   
    [1.0, 1.0],  
    [4.0, 4.0]    
]

encontrados_unicos = [] 
print("Iniciando la búsqueda numérica de puntos críticos:")

for i, guess in enumerate(initial_guesses):
    print(f"\nIntento {i+1} con suposición inicial: {guess}")
    try:
        # 'method='hybr'' es una buena elección para sistemas no lineales,
        # y 'jac' se usa para proporcionar la matriz Jacobiana.
        sol = root(sistema_ecuaciones, guess, jac=matriz_jacobiana, method='hybr', tol=1e-8)
        
        if sol.success:
            punto_critico = sol.x
            
            # Verificar si el punto encontrado ya está en la lista (considerando una pequeña tolerancia)
            es_nuevo = True
            for p_existente in encontrados_unicos:
                if np.allclose(punto_critico, p_existente, atol=1e-6):
                    es_nuevo = False
                    break
            
            if es_nuevo:
                encontrados_unicos.append(punto_critico)
                print(f"  Punto crítico encontrado: (x={punto_critico[0]:.6f}, y={punto_critico[1]:.6f})")
                print(f"  Verificación: f_x = {sistema_ecuaciones(punto_critico)[0]:.2e}, f_y = {sistema_ecuaciones(punto_critico)[1]:.2e}")
            else:
                print(f"  Punto encontrado ({punto_critico[0]:.6f}, {punto_critico[1]:.6f}) ya ha sido identificado.")
        else:
            print(f"  No se pudo converger para la suposición {guess}. Mensaje: {sol.message}")
    except Exception as e:
        print(f"  Ocurrió un error al procesar la suposición {guess}: {e}")

print("\n--- Resumen de Puntos Críticos Identificados Numéricamente ---")
if encontrados_unicos:
    for j, cp in enumerate(encontrados_unicos):
        print(f"Punto Crítico {j+1}: (x={cp[0]:.6f}, y={cp[1]:.6f})")
else:
    print("No se encontraron puntos críticos con las suposiciones iniciales probadas.")
```
--- Resumen de Puntos Críticos Identificados Numéricamente ---

| Punto Crítico | x          | y          | Z      |
| :------------ | :--------- | :--------- |:-------|
| 1             | 0.021903   | 0.021903   |4.02    |
| 2             | 4.939948   | 4.939948   |2.53    |
| 3             | 2.893875   | 2.893875   |1.78    |
---
4.  **Cálculo de Segundas Derivadas Parciales:** Procedimos a obtener las segundas derivadas parciales $f_{xx}$, $f_{yy}$ y $f_{xy}$. Estas derivadas nos dan información crucial sobre la concavidad de la superficie en los puntos críticos.
### Cálculo de Segundas Derivadas Parciales ($f_{xx}$, $f_{yy}$, $f_{xy}$)
---
Hemos procedido a obtener las segundas derivadas parciales, las cuales son fundamentales para determinar la concavidad de la superficie en los puntos críticos y así clasificarlos.

Dada la función de elevación:
$$f(x,y)=4e^{-0.1(x^2 + y^2)} + 2.5e^{-0.1((x-5)^2 + (y-5)^2)}$$

Y sus primeras derivadas:
* $$f_x(x,y)=−0.8xe^{-0.1(x^2 + y^2)} − 0.5(x-5)e^{-0.1((x-5)^2 + (y-5)^2)}$$
* $$f_y(x,y)=−0.8ye^{-0.1(x^2 + y^2)} − 0.5(y-5)e^{-0.1((x-5)^2 + (y-5)^2)}$$

Las segundas derivadas parciales son:

**Segunda derivada parcial con respecto a $x$ dos veces ($f_{xx}$):**
  $$f_{xx}(x,y)=\frac{\partial}{\partial x}(f_x)=e^{-0.1(x^2 + y^2)}(-0.8+0.16x^2)+e^{-0.1((x-5)^2 + (y-5)^2)}(-0.5+0.1(x-5)^2)$$

**Segunda derivada parcial con respecto a $y$ dos veces ($f_{yy}$):**
Por la simetría de la función $f(x,y)$ con respecto a $x$ y $y$, la expresión para $f_{yy}$ es análoga a $f_{xx}$, simplemente reemplazando $x$ por $y$:
  $$f_{yy}(x,y)=\frac{\partial}{\partial y}(f_y)=e^{-0.1(x^2 + y^2)}(-0.8+0.16y^2)+e^{-0.1((x-5)^2 + (y-5)^2)}(-0.5+0.1(y-5)^2)$$

**Segunda derivada parcial mixta ($f_{xy}$ o $f_{yx}$):**
* $$f_{xy}(x,y)=\frac{\partial}{\partial y}(f_x)=0.16xye^{-0.1(x^2 + y^2)}+0.1(x-5)(y-5)e^{-0.1((x-5)^2 + (y-5)^2)}$$

Estas expresiones serán evaluadas en los puntos críticos identificados numéricamente para determinar la naturaleza de cada uno (máximo, mínimo o punto silla) mediante el criterio del Hessiano.

---
6.  **Aplicación del Criterio de la Segunda Derivada (Hessiano):** Para cada punto crítico $(x_c, y_c)$ identificado (posiblemente a través del método de Newton-Raphson), calculamos el determinante del Hessiano, $D = f_{xx}f_{yy} - (f_{xy})^2$, y aplicamos los criterios de clasificación estándares para determinar si el punto es un máximo local, un mínimo local o un punto silla.
---
Hemos evaluado las segundas derivadas parciales y el determinante del Hessiano en cada uno de los puntos críticos identificados.

**6.1. Análisis para el Punto Crítico 1: $(x=0.021903, y=0.021903)$**

* $f_{xx}(0.021903, 0.021903) \approx -0.7993$
* $f_{yy}(0.021903, 0.021903) \approx -0.7993$
* $f_{xy}(0.021903, 0.021903) \approx 0.0008$
* **Determinante del Hessiano ($D$):**
    $$D = f_{xx}f_{yy} - (f_{xy})^2 \approx (-0.7993)(-0.7993) - (0.0008)^2 \approx 0.6389$$

    Dado que $D > 0$ y $f_{xx} < 0$, el Punto Crítico 1 es un **Máximo Local**.
    Este punto representa la cima principal o más alta del terreno, cercana al origen.

**6.2. Análisis para el Punto Crítico 2: $(x=4.939948, y=4.939948)$**

* $f_{xx}(4.939948, 4.939948) \approx -0.4998$
* $f_{yy}(4.939948, 4.939948) \approx -0.4998$
* $f_{xy}(4.939948, 4.939948) \approx 0.0012$
* **Determinante del Hessiano ($D$):**
    $$D = f_{xx}f_{yy} - (f_{xy})^2 \approx (-0.4998)(-0.4998) - (0.0012)^2 \approx 0.2498$$

    Dado que $D > 0$ y $f_{xx} < 0$, el Punto Crítico 2 es un **Máximo Local**.
    Este punto representa la segunda cima importante del terreno, cercana al punto (5,5).

**6.3. Análisis para el Punto Crítico 3: $(x=2.893875, y=2.893875)$**

* $f_{xx}(2.893875, 2.893875) \approx -0.0625$
* $f_{yy}(2.893875, 2.893875) \approx -0.0625$
* $f_{xy}(2.893875, 2.893875) \approx 0.0768$
* **Determinante del Hessiano ($D$):**
    $$D = f_{xx}f_{yy} - (f_{xy})^2 \approx (-0.0625)(-0.0625) - (0.0768)^2 \approx 0.0039 - 0.0059 \approx -0.0020$$

    Dado que $D < 0$, el Punto Crítico 3 es un **Punto Silla**.
    Este punto indica una zona de la superficie donde se puede subir en una dirección y bajar en otra, actuando como un "collado" entre las dos cimas.

---

**Resumen de la Clasificación Final:**

| Punto Crítico | Coordenadas $(x,y)$      | Tipo de Punto |
| :------------ | :----------------------- | :------------ |
| 1             | $(0.021903, 0.021903)$   | Máximo Local  |
| 2             | $(4.939948, 4.939948)$   | Máximo Local  |
| 3             | $(2.893875, 2.893875)$   | Punto Silla   |

**Representación de los puntos críticos del parque natural en GEOGEBRA**

![image](https://github.com/user-attachments/assets/b30dcee6-62bc-4375-bce4-e7216b0aa8f7)

---
**Aplicación de Conceptos Matemáticos:**
* **Derivadas Parciales:** Esenciales para encontrar la tasa de cambio de la elevación en direcciones específicas y localizar puntos donde la superficie no tiene pendiente localmente.
* **Puntos Críticos:** Representan ubicaciones clave donde la superficie puede alcanzar sus valores extremos o exhibir comportamientos de "silla de montar".
* **Métodos Numéricos (Newton-Raphson):**  Indispensables para resolver sistemas complejos de ecuaciones no lineales. Su aplicación se sustenta en el uso de librerías especializadas de Python, como **SciPy**, que proporcionan herramientas robustas para la búsqueda numérica de raíces y la optimización, permitiendo encontrar las coordenadas de los puntos críticos.
* **Criterio del Hessiano:** Un pilar del cálculo multivariable que utiliza las segundas derivadas para determinar la naturaleza de los puntos críticos, clasificándolos rigurosamente.
  
**Justificación:**

La clasificación de los puntos críticos nos proporciona una **comprensión profunda de la topografía del terreno**. Conocer la ubicación de máximos (cumbres), mínimos (valles) y puntos silla (pasos o collados) es crucial para la **planificación de rutas**. Esta información influye directamente en la **pendiente de la ciclovía** y los **costos asociados a los movimientos de tierra**, permitiéndonos diseñar de manera más eficiente y económica.


### 2.1.2 Análisis de Separación Vertical entre Puente y Terreno

---
1.  **Modelado del Puente:** Parametrizamos la línea recta que define el puente desde $A(0,0,4.02)$ hasta $B(5,5,2.53)$ como una función vectorial tridimensional $\mathbf{L}(t) = \langle x(t), y(t), Z_{puente}(t) \rangle$. Esta parametrización describe la posición y elevación del puente en cualquier punto de su recorrido.
---
Se parametriza la línea recta que define el puente desde el punto $A(0,0,4.02)$ hasta el punto $B(5,5,2.53)$ como una función vectorial tridimensional $\mathbf{L}(t) = \langle x(t), y(t), Z_{\text{puente}}(t) \rangle$. Esta parametrización describe la posición y elevación del puente en cualquier punto de su recorrido.

Desarrollo del Modelado del Puente
Para desarrollar la parametrización de la línea recta que define el puente, seguimos el método general para parametrizar un segmento de línea entre dos puntos en el espacio tridimensional.

Dados los puntos extremos del puente:

Punto inicial: $A = P_0 = (x_0, y_0, z_0) = (0, 0, 4.02)$
Punto final: $B = P_1 = (x_1, y_1, z_1) = (5, 5, 2.53)$
La función vectorial de una línea recta que une dos puntos $P_0$ y $P_1$ se puede expresar de la siguiente forma:
$$\mathbf{L}(t) = P_0 + t(P_1 - P_0) \quad \text{para } 0 \le t \le 1$$

Primero, calculamos el vector direccional del segmento, que es la diferencia entre el punto final y el punto inicial:
$$P_1 - P_0 = (5 - 0, 5 - 0, 2.53 - 4.02)$$
$$P_1 - P_0 = (5, 5, -1.49)$$

Ahora, sustituimos el punto inicial $P_0$ y el vector direccional calculado en la ecuación de la función vectorial $\mathbf{L}(t)$:
$$\mathbf{L}(t) = (0, 0, 4.02) + t(5, 5, -1.49)$$

Expandiendo esta expresión, obtenemos las ecuaciones paramétricas para cada una de las coordenadas ($x$, $y$, y $Z_{\text{puente}}$):
* $$x(t) = 0 + 5t \implies x(t) = 5t$$
* $$y(t) = 0 + 5t \implies y(t) = 5t$$
* $$Z_{\text{puente}}(t) = 4.02 + t(-1.49) \implies Z_{\text{puente}}(t) = 4.02 - 1.49t$$

Por lo tanto, la parametrización de la línea recta que define el puente es:
* $$\mathbf{L}(t) = \langle 5t, 5t, 4.02 - 1.49t \rangle$$
donde el parámetro $t$ varía en el intervalo cerrado de $0$ a $1$ ($0 \le t \le 1$).

Con esta parametrización:

* Cuando $t=0$, $\mathbf{L}(0) = \langle 0, 0, 4.02 \rangle$, lo que corresponde al punto de inicio $A$.
* Cuando $t=1$, $\mathbf{L}(1) = \langle 5(1), 5(1), 4.02 - 1.49(1) \rangle = \langle 5, 5, 2.53 \rangle$, lo que corresponde al punto final $B$.
* Esta función vectorial $\mathbf{L}(t)$ nos permite determinar la posición tridimensional $(x(t), y(t), Z_{\text{puente}}(t))$ de cualquier punto a lo largo del  recorrido del puente. 
---
3.  **Elevación del Terreno Bajo el Puente:** Sustituimos las componentes horizontales $x(t)$ y $y(t)$ del puente en la función de elevación del terreno $f(x,y)$ para obtener $Z_{terreno}(t)$. Esto nos da la altura de la superficie terrestre directamente debajo del puente.
---
### Definición de la Función de Separación: Desarrollo Detallado

Para desarrollar la función de separación $\Delta Z(t)$, simplemente restamos la elevación del terreno bajo el puente, $Z_{\text{terreno}}(t)$, de la elevación del puente, $Z_{\text{puente}}(t)$ .

**1. Recordar las Funciones de Elevación:**

* **Elevación del Puente ( $Z_{\text{puente}}(t)$ ) : **
    Esta función proviene de la parametrización de la línea recta del puente:
    $$Z_{\text{puente}}(t) = 4.02 - 1.49t$$

* **Elevación del Terreno Bajo el Puente ( $Z_{\text{terreno}}(t)$ ):**
    Esta función se obtuvo al sustituir $x(t)$ e $y(t)$ del puente en la función de elevación del terreno $f(x,y)$:
    $$Z_{\text{terreno}}(t) = 4e^{-5t^2} + 2.5e^{-5(t - 1)^2}$$

**2. Definir la Función de Separación ( $\Delta Z(t)$ ):**

La función de separación $\Delta Z(t)$ se define como la diferencia vertical entre la altura del puente y la altura del terreno en cada punto a lo largo del recorrido del puente (parametrizado por $t$):
$$\Delta Z(t) = Z_{\text{puente}}(t) - Z_{\text{terreno}}(t)$$

**3. Sustituir las expresiones de $Z_{\text{puente}}(t)$ y $Z_{\text{terreno}}(t)$:**

Sustituimos las fórmulas obtenidas para cada una de las elevaciones en la definición de $\Delta Z(t)$:
$$\Delta Z(t) = (4.02 - 1.49t) - (4e^{-5t^2} + 2.5e^{-5(t - 1)^2})$$

Finalmente, la función de separación $\Delta Z(t)$ es:
* ### $$\Delta Z(t) = 4.02 - 1.49t - 4e^{-5t^2} - 2.5e^{-5(t - 1)^2}$$

Esta función $\Delta Z(t)$ representa la separación vertical del puente sobre el terreno en cualquier punto a lo largo de su extensión (para $0 \le t \le 1$). El objetivo principal de definir esta función es poder encontrar el punto de máxima separación.

Para obtener la altura de la superficie terrestre directamente debajo de cada punto del puente, sustituimos las ecuaciones paramétricas de las coordenadas horizontales del puente, $x(t)$ y $y(t)$, en la función de elevación del terreno $f(x,y)$.

---
5.  **Definición de la Función de Separación:** Creamos una nueva función, $\Delta Z(t) = Z_{puente}(t) - Z_{terreno}(t)$, que representa la diferencia de elevación vertical entre el puente y el terreno para cada valor del parámetro $t$.
---   
**5.1. Función de Elevación del Terreno:**
La función de elevación del terreno es:
$$f(x,y) = 4e^{-0.1(x^2 + y^2)} + 2.5e^{-0.1((x - 5)^2 + (y - 5)^2)}$$

**5.2. Parámetros Horizontales del Puente:**
Las componentes paramétricas del puente para las coordenadas horizontales son:
$$x(t) = 5t$$
$$y(t) = 5t$$
donde $t$ varía en el rango $[0, 1]$.

**5.3. Sustitución de $x(t)$ y $y(t)$ en $f(x,y)$ para obtener $Z_{\text{terreno}}(t)$:**
Ahora, reemplazamos $x$ por $x(t)$ y $y$ por $y(t)$ en la expresión de $f(x,y)$:

$$Z_{\text{terreno}}(t) = f(x(t), y(t))$$
$$Z_{\text{terreno}}(t) = f(5t, 5t)$$

Sustituyendo explícitamente $5t$ por $x$ y $5t$ por $y$ en la fórmula de $f(x,y)$:

$$Z_{\text{terreno}}(t) = 4e^{-0.1((5t)^2 + (5t)^2)} + 2.5e^{-0.1((5t - 5)^2 + (y - 5)^2)}$$

Procedemos a simplificar esta expresión:

**Simplificación del primer término:**
$$4e^{-0.1((5t)^2 + (5t)^2)} = 4e^{-0.1(25t^2 + 25t^2)}$$
$$= 4e^{-0.1(50t^2)}$$
$$= 4e^{-5t^2}$$

**Simplificación del segundo término:**
$$2.5e^{-0.1((5t - 5)^2 + (5t - 5)^2)} = 2.5e^{-0.1(2(5t - 5)^2)}$$
$$= 2.5e^{-0.2(5(t - 1))^2}$$
$$= 2.5e^{-0.2 \cdot 25 (t - 1)^2}$$
$$= 2.5e^{-5(t - 1)^2}$$

**5.4. Expresión Final para la Elevación del Terreno Bajo el Puente:**
Combinando los términos simplificados, obtenemos la función $Z_{\text{terreno}}(t)$:
$$Z_{\text{terreno}}(t) = 4e^{-5t^2} + 2.5e^{-5(t - 1)^2}$$

Esta función $Z_{\text{terreno}}(t)$ representa la elevación del terreno para cualquier punto $(x(t), y(t))$ directamente debajo del puente, a medida que $t$ varía de $0$ a $1$. Es crucial para calcular la distancia vertical entre el puente y el terreno.

---
6.  **Optimización para Máxima Separación (Método Numérico con Python):** Para identificar la **máxima separación vertical** aplicamos **métodos de optimización numérica**. Esto se realiza mediante la **programación en Python** para encontrar el valor de $t$ que maximiza $\Delta Z(t)$ dentro del intervalo relevante $[0,1]$.
---  
  ### Optimización para Máxima Separación (Método Numérico con Python)

Para identificar la **máxima separación vertical** entre el puente y el terreno, aplicamos métodos de optimización numérica. Esto se realiza mediante la programación en Python para encontrar el valor de $t$ que maximiza $\Delta Z(t)$ dentro del intervalo relevante $[0,1]$. Se evaluarán los valores de $\Delta Z(t)$ en el máximo encontrado numéricamente y en los extremos del intervalo ($t=0$ y $t=1$) para confirmar el valor global.

Primero, implementamos la función $\Delta Z(t)$ que derivamos previamente en Python. Esta función tomará el parámetro `t` y devolverá la diferencia de elevación.
Dado que queremos encontrar el *máximo* de una función en un intervalo ($t \in [0,1]$), y las herramientas de optimización numérica de `SciPy` (como `scipy.optimize.minimize_scalar`) para hallar el maximo valor tiempo t dentro del intervalo cerrado $[0,1]$, que representa el inicio y el final del puente

El valor más grande de $\Delta Z(t)$ entre el máximo local encontrado numéricamente y los valores en los extremos del intervalo será la máxima separación vertical global.
```python
import numpy as np
import matplotlib
matplotlib.use('TkAgg')  # O 'Qt5Agg' si tienes Qt instalado
import matplotlib.pyplot as plt

# Definimos la función del parque
def f(x, y):
    return 4 * np.exp(-0.1 * (x**2 + y**2)) + 2.5 * np.exp(-0.1 * ((x - 5)**2 + (y - 5)**2))

# Parametrización de la recta entre A = (0,0) y B = (5,5)
def x_t(t): return 5 * t
def y_t(t): return 5 * t

# Evaluamos los extremos del puente
fA = f(0, 0)
fB = f(5, 5)

# Altura del puente en función de t (interpolación lineal)
def z_puente(t): return (1 - t) * fA + t * fB

# Altura del parque en cada punto t sobre la recta
def z_parque(t): return f(x_t(t), y_t(t))

# Diferencia vertical
def D(t): return z_puente(t) - z_parque(t)

# Valores de t entre 0 y 1
t_vals = np.linspace(0, 1, 200)
D_vals = D(t_vals)

# Encontramos el máximo de la diferencia
max_diff = np.max(D_vals)
t_max = t_vals[np.argmax(D_vals)]
x_max, y_max = x_t(t_max), y_t(t_max)

# Mostrar el resultado
print(f"Punto de máxima separación: ({x_max:.4f}, {y_max:.4f})")
print(f"Máxima diferencia vertical: {max_diff:.4f}")

# Gráfica de la diferencia D(t)
plt.plot(t_vals, D_vals, label='Diferencia vertical (Puente - Parque)')
plt.axvline(t_max, color='red', linestyle='--', label='Máximo')
plt.xlabel("t (a lo largo del puente)")
plt.ylabel("Diferencia vertical")
plt.title("Máxima separación vertical entre el puente y el parque")
plt.legend()
plt.grid(True)
plt.show()
```
$$Figure 1  **Maxima \ separación \ vertical \ entre \ el \ puente \ y \ el \ parque**$$

![maxima separacion vertical entre el puente y el parque](https://github.com/user-attachments/assets/6b82bf4d-e451-4872-af18-51eb7373a76a)

| t    | x(t) | y(t) | z_puente(t) | f(x,y) | Δz(t)  |
| :--- | :--- | :--- | :---------- | :----- | :----- |
| 0.00 | 0.00 | 0.00 | 4.0168      | 4.0168 | 0.0000 |
| 0.01 | 0.05 | 0.05 | 4.0019      | 4.0166 | -0.0147|
| 0.02 | 0.10 | 0.10 | 3.9870      | 4.0125 | -0.0255|
| ...  | ...  | ...  | ...         | ...    | ...    |
| 0.52 | 2.60 | 2.60 | 3.2715      | 1.8541 | **1.4174** |
| ...  | ...  | ...  | ...         | ...    | ...    |
| 1.00 | 5.00 | 5.00 | 2.5270      | 2.5270 | 0.0000 |

### Según el análisis numérico, la máxima distancia vertical entre el puente recto y la superficie del terreno es de aproximadamente 1.417 metros. Esta separación ocurre cuando el parámetro t tiene un valor de 0.523 aproximadamete a la mitadad de su recorrido.
---
9.  **Verificación de Punto Silla en el Terreno:** Una vez identificada la ubicación (x,y) en el terreno donde se produce esta máxima separación, aplicamos el Criterio del Hessiano a $f(x,y)$ en ese punto específico para determinar si corresponde a un punto silla.
### Máxima Separación Vertical entre el Puente y el Terreno
---
Según el análisis numérico, la **máxima distancia vertical** entre el puente recto y la superficie del terreno es de aproximadamente $\bf{1.417 \ metros}$. Esta separación ocurre cuando el parámetro $t$ tiene un valor de $\bf{0.523}$, aproximadamente a la mitad de su recorrido.

### Verificación de Punto Silla en el Terreno

Una vez identificada la ubicación $(x,y)$ en el terreno donde se produce esta máxima separación, aplicamos el Criterio del Hessiano a $f(x,y)$ en ese punto específico para determinar si corresponde a un punto silla.

**1. Determinar las Coordenadas $(x,y)$ en el Terreno para $t=0.523$:**

Utilizamos las ecuaciones paramétricas del puente para obtener las coordenadas horizontales correspondientes a $t=0.523$:
$$x(t) = 5t \implies x(0.523) = 5 \times 0.523 = 2.615$$
$$y(t) = 5t \implies y(0.523) = 5 \times 0.523 = 2.615$$
Así, el punto en el terreno donde se produce la máxima separación es aproximadamente $(2.615, 2.615)$.

**2. Evaluar las Segundas Derivadas Parciales en el Punto $(2.615, 2.615)$:**

Ahora, sustituimos $x=2.615$ y $y=2.615$ en las expresiones de las segundas derivadas parciales $f_{xx}$, $f_{yy}$, y $f_{xy}$:

* **Segunda derivada parcial con respecto a $x$ dos veces ($f_{xx}$):**
    $$f_{xx}(x,y) = e^{-0.1(x^2 + y^2)} (-0.8 + 0.16x^2) + e^{-0.1((x - 5)^2 + (y - 5)^2)} (-0.5 + 0.1(x - 5)^2)$$
    Evaluando en $(2.615, 2.615)$:
    $$f_{xx}(2.615, 2.615) \approx -0.0163$$

* **Segunda derivada parcial con respecto a $y$ dos veces ($f_{yy}$):**
    $$f_{yy}(x,y) = e^{-0.1(x^2 + y^2)} (-0.8 + 0.16y^2) + e^{-0.1((x - 5)^2 + (y - 5)^2)} (-0.5 + 0.1(y - 5)^2)$$
    Evaluando en $(2.615, 2.615)$:
    $$f_{yy}(2.615, 2.615) \approx -0.0163$$

* **Segunda derivada parcial mixta ($f_{xy}$ o $f_{yx}$):**
    $$f_{xy}(x,y) = 0.16xy e^{-0.1(x^2 + y^2)} + 0.1(x - 5)(y - 5) e^{-0.1((x - 5)^2 + (y - 5)^2)}$$
    Evaluando en $(2.615, 2.615)$:
    $$f_{xy}(2.615, 2.615) \approx 0.0818$$

**3. Calcular el Determinante del Hessiano ($D$) en $(2.615, 2.615)$:**

El determinante del Hessiano se calcula como:
$$D(x,y) = f_{xx}(x,y) \cdot f_{yy}(x,y) - (f_{xy}(x,y))^2$$
Sustituyendo los valores calculados:
$$D(2.615, 2.615) \approx (-0.0163)(-0.0163) - (0.0818)^2$$
$$D(2.615, 2.615) \approx 0.00026569 - 0.00669124$$
$$D(2.615, 2.615) \approx -0.00642555$$

**4. Aplicar el Criterio del Hessiano:**

Dado que $D(2.615, 2.615) \approx -0.0064 < 0$, el criterio del Hessiano indica que el punto $(2.615, 2.615)$ es un **Punto Silla** en la superficie del terreno.

**Conclusión de la Verificación:**

El punto en el terreno donde el puente alcanza su máxima separación vertical, $(2.615, 2.615)$, efectivamente corresponde a un **punto silla** en la topografía modelada. Esto es coherente con el análisis de los puntos críticos de la función de elevación, donde se identificó un punto silla cercano en $(2.893875, 2.893875)$. La ligera diferencia se debe a que la máxima separación no necesariamente coincide exactamente con un punto crítico de la superficie, sino con un punto a lo largo de la trayectoria del puente donde la distancia es máxima.


**Aplicación de Conceptos Matemáticos:**
* **Funciones Vectoriales y Parametrización de Curvas:** Esenciales para modelar con precisión tanto el puente como la trayectoria de la superficie que pasa por debajo del puente.
* **Optimización Numérica y Programación (Python):** Fundamentales para encontrar el máximo de funciones complejas cuando las soluciones analíticas son inviables. Permiten aproximar eficientemente el valor óptimo con librerias como Scipy y Matplotlib. 
* **Criterio del Hessiano:** Se emplea para clasificar el punto de interés en el terreno, confirmando si su topografía local es un punto silla.
* **Evaluación de Funciones Multivariables:** Necesario para obtener la elevación del terreno en cualquier punto bajo el puente.
* **Criterio del Hessiano (reaplicado):** Se emplea nuevamente para clasificar el punto de interés en el terreno, confirmando si su topografía local es un punto silla.

**Justificación:**
Este análisis es crucial para la **seguridad y la viabilidad estructural del puente**. Identificar la máxima separación vertical nos permite entender la holgura del diseño o, por el contrario, señalar posibles puntos de conflicto donde el puente podría acercarse demasiado o incluso chocar con el terreno. Verificar si este punto es un **punto silla** es relevante porque los puntos silla pueden ser **áreas topográficamente inestables o puntos de convergencia de drenajes**, lo que podría afectar tanto la ingeniería del puente como el diseño de la ciclovía adyacente.

---
### 2.1.3 Determinación del Parámetro de Trayectoria 'n'

---
1.  **Definición de la Trayectoria:** Partimos de la forma general de la trayectoria polinomial de la ciclovía $\mathbf{r}(t) = \langle 5t, 5t^n \rangle$.
--- 

La trayectoria de la ciclovía está definida por la siguiente forma polinomial:

 $$\vec{r}(t) = \langle 5t,\ 5t^n \rangle$$

---
2.  **Cálculo de Derivadas de Componentes:** Obtuvimos las derivadas de las componentes $x(t)$ y $y(t)$ con respecto a $t$ ($\frac{dx}{dt}$ y $\frac{dy}{dt}$). Estas derivadas describen la velocidad de cambio en cada coordenada a lo largo de la trayectoria.
---
Las componentes de la trayectoria son:

- $x(t) = 5t$
- $y(t) = 5t^n$

Sus derivadas con respecto a $t$ son:

- $\frac{dx}{dt} = 5$
- $\frac{dy}{dt} = 5n t^{n-1}$

Estas derivadas describen la velocidad de cambio en cada coordenada a lo largo de la trayectoria.

---
4.**Formulación de la Integral de Longitud de Arco:** Establecimos la integral de longitud de arco para la trayectoria proyectada en el plano $XY$. Esta integral suma infinitesimalmente la longitud de pequeños segmentos de la curva.

---

La fórmula para la longitud de arco $L$ de una curva paramétrica es:

$$
L = \int_a^b \sqrt{\left( \frac{dx}{dt} \right)^2 + \left( \frac{dy}{dt} \right)^2} \, dt
$$

Dado que el recorrido de la ciclovía se define en el intervalo $t \in [0, 1]$, los límites de integración son $a=0$ y $b=1$.

Sustituyendo las derivadas en la fórmula de la longitud de arco:

$$
\begin{aligned}
L &= \int_0^{1} \sqrt{(5)^2 + (5n t^{n-1})^2} \, dt \\
  &= \int_0^{1} \sqrt{25 + 25n^2 t^{2n - 2}} \, dt \\
  &= \int_0^{1} \sqrt{25\left(1 + n^2 t^{2n - 2} \right)} \, dt \\
  &= 5 \int_0^{1} \sqrt{1 + n^2 t^{2n - 2}} \, dt
\end{aligned}
$$

---
6.  **Establecimiento de la Ecuación:** Igualamos el resultado de la integral de longitud de arco a la longitud proyectada requerida de 8 metros. Esto nos da una ecuación cuya única incógnita es $n$.
---
Para que la longitud de la ciclovía sea de 8 metros en este intervalo, igualamos la integral a 8:

$$
\begin{aligned}
8 &= 5 \int_0^{1} \sqrt{1 + n^2 t^{2n - 2}} \, dt \\
\Rightarrow \quad \frac{8}{5} &= \int_0^{1} \sqrt{1 + n^2 t^{2n - 2}} \, dt \\
\Rightarrow \quad 1.6 &= \int_0^{1} \sqrt{1 + n^2 t^{2n - 2}} \, dt
\end{aligned}
$$

Esta es la ecuación final, con el nuevo intervalo, cuya única incógnita es $n$.

---
7.  **Resolución Numérica:** Dada la complejidad analítica de la integral, la ecuación se resuelve utilizando métodos numéricos en python para determinar el valor específico de $n$ que cumpla con la condición  de una longitud de 8 metros.
---

El procedimiento del método numérico tiene como finalidad encontrar el valor de $n$ que satisface la ecuación definida por la integral, de manera que se cumpla con la condición geométrica deseada: una longitud total de 8 metros para la trayectoria. Esto se traduce en resolver la ecuación:

$$
\int_0^1 \sqrt{1 + n^2 t^{2n - 2}}\, dt = 1.6
$$

donde el valor 1.6 proviene de dividir la longitud total requerida (8 metros) entre el factor de escala horizontal (5) que aparece en la parametrización. El objetivo es hallar el valor de $n$ tal que la función:

$$
f(n) = \int_0^1 \sqrt{1 + n^2 t^{2n - 2}}\, dt - 1.6
$$

sea igual a cero. De esta manera, se garantiza que el valor encontrado de $n$ genera una trayectoria cuya longitud cumple exactamente con el requerimiento del problema.

    
```python
import matplotlib
matplotlib.use('TkAgg')  # Usa un backend compatible con PyCharm (evita 'tostring_rgb' error)

# --- Librerías necesarias ---
import numpy as np
from scipy.integrate import quad
import matplotlib.pyplot as plt
from tqdm import tqdm

# --- Función dentro de la integral ---
def integrand(t, n):
    if t == 0 and 2 * n - 2 <= 0:
        return np.inf
    return np.sqrt(1 + n**2 * t**(2 * n - 2))

# --- Función f(n) = integral - 1.6 ---
def equation_to_solve(n):
    integral_value, _ = quad(integrand, 0, 1, args=(n,), limit=100)
    return integral_value - 1.6

# --- Rango de valores para 'n' ---
n_values = np.linspace(0.6, 5.0, 100)

print("Calculando los valores de la función para el gráfico...")
f_values = [equation_to_solve(n) for n in tqdm(n_values)]

# --- Punto especial para marcar: n=1 ---
f_at_1 = equation_to_solve(1)

# --- Crear gráfico ---
plt.style.use('seaborn-v0_8-whitegrid')
fig, ax = plt.subplots(figsize=(10, 6))

# Curva f(n)
ax.plot(n_values, f_values, label=r'$f(n) = \int_0^1 \sqrt{1+n^2 t^{2n-2}} dt - 1.6$', color='dodgerblue', linewidth=2)

# Línea horizontal en y=0 (condición de longitud deseada)
ax.axhline(0, color='red', linestyle='--', label='Objetivo (f(n) = 0)')

# Punto especial: n = 1 (línea recta)
ax.plot(1, f_at_1, 'o', color='darkgreen', markersize=8, label='Caso n = 1 (Línea Recta)')

# Configuraciones
ax.set_title('Gráfico de la Función para Determinar $n$', fontsize=16)
ax.set_xlabel('Valor de $n$', fontsize=12)
ax.set_ylabel('Valor de $f(n)$', fontsize=12)
ax.set_ylim(-1.0, 1.5)
ax.legend(fontsize=10)

# Mostrar gráfico en ventana externa
plt.show()

```
![Función n](https://github.com/user-attachments/assets/a5dba136-eadc-4312-a4df-e173f1f0d6b4)


El valor de $n$ que satisface la condición es:  
**$n \approx 3.994972$**

---

### 🔎 Verificación

El valor de la integral con $n = 3.994972$ es:  
**$\int_0^1 \sqrt{1 + n^2 t^{2n - 2}}\, dt = 1.600000$**

El valor objetivo era:  
**$1.6$**

La longitud total de la ciclovía sería:  
**$L = 5 \cdot 1.6 = 8.0000$ metros** (Objetivo: 8 metros)

---

**Justificación:**
La determinación de $n$ nos permite **ajustar la forma de la ciclovía** para cumplir con una **restricción de longitud específica**. Esto es de vital importancia en la planificación de infraestructuras, ya que las longitudes de las rutas pueden estar limitadas por factores como el presupuesto disponible, los tiempos de recorrido deseados o las características del terreno. Al controlar el parámetro $n$, podemos diseñar una curva que se adapte con precisión a estas necesidades del proyecto, optimizando el equilibrio entre la accesibilidad y los objetivos económicos/operativos.



  












