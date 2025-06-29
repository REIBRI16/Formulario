Amplificadores Operacionales
----------------------------

Formularios de circuitos con AO ideal (modelo de “corto virtual” y “entrada abierta virtual”) Clase de Amplificadores…

**Modelo básico (AO ideal):**

*    $v_o = A\,(v^+ - v^-)$ , con  $A\to\infty$ .
    
*    $v^+ = v^-$  (corto virtual).
    
*    $i^+=i^-=0$  (entrada abierta virtual).
    

* * *

### 1\. Amplificador inversor

$$
H(s)=\frac{V_o}{V_{in}} =-\frac{R_f}{R_1}
$$

**Origen:**  
KCL en el nodo  $v^-$ :

$$
\frac{v^- - v_{in}}{R_1}+\frac{v^- - v_o}{R_f}=0,\quad v^-=0
$$

∴  $-V_{in}/R_1 + V_o/R_f=0$ .

* * *

### 2\. Amplificador no-inversor

$$
H(s)=1+\frac{R_2}{R_1}
$$

**Origen:**  
KCL en el nodo inversor:

$$
\frac{v_o - v^-}{R_2}+\frac{v^-}{R_1}=0,\quad v^+=v^-=V_{in}
$$

∴  $(V_o - V_{in})/R_2 + V_{in}/R_1=0$ .

* * *

### 3\. Sumador inversor

$$
V_o = -R_f\Bigl(\frac{V_{a}}{R_a}+\frac{V_{b}}{R_b}+\cdots\Bigr)
$$

**Origen:**  
KCL en  $v^- = 0$ :  
 $\sum (0 - V_i)/R_i + (0 - V_o)/R_f=0$ .

* * *

### 4\. Restador (diferenciador de señales)

$$
V_o = \frac{R_2}{R_1}(V_b - V_a)
$$

**Origen:**  
Combina dos sumadores y dos divisores de tensión en configuración de puente, aplicando KCL y virtual short.

* * *

### 5\. Integrador

$$
V_o(t) = -\frac{1}{R\,C}\int_0^t V_{in}(\tau)\,d\tau + V_o(0)
$$

**Origen:**  
Reemplaza  $R_f$  por  $1/(sC)$  en el inversor:  
   $\;H(s)=-\frac{1/(sC)}{R_1}=-\frac{1}{R_1C}\,\frac1s$ .

* * *

### 6\. Derivador

$$
V_o(t) = -R\,C\;\frac{d}{dt}V_{in}(t)
$$

**Origen:**  
Reemplaza  $R_1$  por  $1/(sC)$  en el inversor:  
   $\;H(s)=-R_f\,sC$ .

* * *

Redes de Dos Puertos
--------------------

Parámetros Z, Y, h y transformaciones π↔T Clase de Redes de dos p…

* * *

### 1\. Parámetros Z (impedancias)

$$
\begin{aligned} V_1 &= Z_{11}I_1 + Z_{12}I_2,\\ V_2 &= Z_{21}I_1 + Z_{22}I_2. \end{aligned}
$$

**Medición:**

*    $Z_{11}=V_1/I_1$  con  $I_2=0$ .
    
*    $Z_{12}=V_1/I_2$  con  $I_1=0$ .
    
*   Análogo para  $Z_{21}$ ,  $Z_{22}$ .
    

* * *

### 2\. Parámetros Y (admitancias)

$$
\begin{aligned} I_1 &= Y_{11}V_1 + Y_{12}V_2,\\ I_2 &= Y_{21}V_1 + Y_{22}V_2. \end{aligned}
$$

**Medición:**

*    $Y_{11}=I_1/V_1$  con  $V_2=0$ .
    
*    $Y_{12}=I_1/V_2$  con  $V_1=0$ .
    
*   Análogo para  $Y_{21}$ ,  $Y_{22}$ .
    

* * *

### 3\. Parámetros híbridos h

$$
\begin{aligned} V_1 &= h_{11}I_1 + h_{12}V_2,\\ I_2 &= h_{21}I_1 + h_{22}V_2. \end{aligned}
$$

*    $h_{11}=V_1/I_1$  con  $V_2=0$ .
    
*    $h_{12}=V_1/V_2$  con  $I_1=0$ .
    
*   Etc.
    

* * *

### 4\. Transformaciones π↔T

Para una red π con ramas  $Z_A,Z_B,Z_C$ , la red T equivalente tiene:

$$
\begin{aligned} Z_1 &= \frac{Z_A Z_C}{Z_A+Z_B+Z_C},\quad Z_2 = Z_B,\quad Z_3 = \frac{Z_A Z_C}{Z_A+Z_B+Z_C}. \end{aligned}
$$

* * *

Respuesta en Frecuencia y Bode
------------------------------

Funciones de transferencia, polos, ceros y filtros Clase de Respuesta en f…

* * *

### 1\. Función de transferencia

$$
H(j\omega)=\frac{V_{out}(j\omega)}{V_{in}(j\omega)}.
$$

* * *

### 2\. Ganancia y fase

$$
G(\omega)=|H(j\omega)|,\quad \phi(\omega)=\arg H(j\omega).
$$

* * *

### 3\. Diagrama de Bode

*   **Magnitud:**  $20\log_{10}G(\omega)$  vs.  $\log_{10}\omega$ .
    
*   **Fase:**  $\phi(\omega)$  vs.  $\log_{10}\omega$ .
    
*   Cada polo simple aporta  $-20$  dB/década y  $-90°$ .
    
*   Cada cero simple aporta  $+20$  dB/década y  $+90°$ .
    

* * *

### 4\. Filtros de primer orden

*   **Pasa bajos:**  
     $\displaystyle H(s)=\frac{1}{1+s/\omega_c},\;\omega_c=1/RC.$   
     $|H(j\omega)|=1/\sqrt{1+(\omega/\omega_c)^2},\;\phi=-\arctan(\omega/\omega_c).$ 
    
*   **Pasa altos:**  
     $\displaystyle H(s)=\frac{s/\omega_c}{1+s/\omega_c}.$ 
    
*   **Pasa banda (2.º orden):**  
     $\displaystyle H(s)=\frac{\omega_0/Q\;s}{s^2+( \omega_0/Q)s+\omega_0^2},\;Q=\frac{\omega_0}{2\alpha}.$ 
    

* * *

Acoplamiento Magnético y Transformadores
----------------------------------------

Ley de Faraday,  $M$ , coeficiente  $k$ , transformador ideal Clase de Acoplamiento m…

* * *

### 1\. Inductancias acopladas

$$
v_1 = L_1\frac{di_1}{dt} + M\frac{di_2}{dt},\quad v_2 = M\frac{di_1}{dt} + L_2\frac{di_2}{dt}.
$$
 
$$
M = k\sqrt{L_1L_2},\quad 0\le k\le1.
$$

* * *

### 2\. RSP de transformador ideal

$$
\frac{V_1}{V_2}=\frac{N_1}{N_2},\quad Z'_L=\Bigl(\frac{N_1}{N_2}\Bigr)^2Z_L.
$$

Potencia:  $S_p=V_1I_1^*,\;S_s=V_2I_2^*$ , con  $S_p=S_s$ .

* * *

### 3\. Conexión en serie/paralelo de bobinas

*   **Serie aditiva:**  $L_{eq}=L_1+L_2+2M$ .
    
*   **Serie sustitutiva:**  $L_{eq}=L_1+L_2-2M$ .
    
*   **Paralelo:**  $L_{eq}=(L_1L_2 - M^2)/(L_1+L_2 \mp 2M)$ .