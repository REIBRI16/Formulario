# Ayuda examen programación avanzada

**User:** Ibarra Riffo (losibarrariffo@gmail.com)  
**Created:** 6/30/2025 12:27  
**Updated:** 6/30/2025 18:02  
**Exported:** 6/30/2025 18:07  
**Link:** [https://chatgpt.com/c/6862baf6-d6b4-800c-8d8e-49c86c553953](https://chatgpt.com/c/6862baf6-d6b4-800c-8d8e-49c86c553953)  

## Response:
Pensó durante un par de segundos

El usuario quiere un resumen muy detallado de las semanas 10 a 15, basado en los archivos que compartió. Me pide que sea

Listo

````markdown
# Semana 10: Interfaces gráficas con PyQt5  

## Objetivo  
Aprender a construir aplicaciones de escritorio con ventanas, botones y eventos usando la biblioteca PyQt5, y entender cómo separar la lógica de la interfaz (UI) de la lógica de negocio.

---

## 1. Estructura básica de una app PyQt5  
- **QApplication**: objeto principal que controla el ciclo de eventos de la aplicación.  
- **QMainWindow**: ventana principal con barras de menús, barras de herramientas y zonas de estado.  
- **QWidget**: clase base para todos los elementos gráficos (botones, etiquetas, cuadros de texto).  

```python
from PyQt5.QtWidgets import QApplication, QMainWindow

app = QApplication([])                   # Crea el entorno de la aplicación
window = QMainWindow()                   # Instancia la ventana principal
window.setWindowTitle("Mi App")          # Título de la ventana
window.show()                            # Muestra la ventana en pantalla
app.exec_()                              # Entra en el bucle de eventos
````

* * *

2\. Layouts y organización de widgets
-------------------------------------

*   **QVBoxLayout** y **QHBoxLayout**: organizan widgets en columna y en fila, respectivamente.
*   **QGridLayout**: organiza widgets en una cuadrícula (filas × columnas).
*   **Métodos clave**:
    *   `addWidget(widget, row, col)` en grid
    *   `setSpacing(n)` para espacio interno
    *   `setContentsMargins(left, top, right, bottom)` para márgenes exteriores
        

```python
from PyQt5.QtWidgets import QWidget, QVBoxLayout, QPushButton

container = QWidget()
layout = QVBoxLayout()
btn1 = QPushButton("Aceptar")
btn2 = QPushButton("Cancelar")
layout.addWidget(btn1)
layout.addWidget(btn2)
container.setLayout(layout)
```

* * *

3\. Widgets esenciales y propiedades
------------------------------------

| Widget | Descripción | Métodos principales |
| --- | --- | --- |
| QPushButton | Botón clicable | `setText()`, `clicked.connect()` |
| QLabel | Muestra texto o imagen | `setText()`, `setPixmap()` |
| QLineEdit | Campo de texto de una sola línea | `text()`, `setText()`, `setEchoMode()` |
| QComboBox | Desplegable de selección | `addItem()`, `currentText()` |
| QCheckBox | Casilla de verificación | `isChecked()`, `setChecked()` |

```python
label = QLabel("Ingrese su nombre:")
edit = QLineEdit()
print(edit.text())  # Obtiene el texto actual
```

* * *

4\. Señales y slots (eventos)
-----------------------------

*   Una **señal** es un mensaje emitido por un widget (por ejemplo, al hacer clic).
*   Un **slot** es una función que responde a esa señal.
*   Conectar señal a slot:
    

```python
def saludar():
    print("¡Hola!")

button = QPushButton("Saludar")
button.clicked.connect(saludar)    # Al hacer clic, se llama a saludar()
```

*   Se puede usar una **lambda** para pasar argumentos:
    

```python
button.clicked.connect(lambda: saludar_con_nombre("Marlene"))
```

* * *

5\. Ventanas múltiples y paso de datos
--------------------------------------

*   Para abrir una ventana hija desde la principal, instanciar otra clase derivada de `QDialog` o `QWidget`:
    

```python
dialog = MiDialogo(parent=self)
dialog.data_submitted.connect(self.recibir_datos)
dialog.exec_()   # Abre la ventana como modal
```

*   En la clase hija, definir una señal:
    

```python
from PyQt5.QtCore import pyqtSignal

class MiDialogo(QDialog):
    data_submitted = pyqtSignal(str)      # Señal que emite un string

    def enviar(self):
        texto = self.line_edit.text()
        self.data_submitted.emit(texto)   # Emite la señal
        self.accept()                     # Cierra el diálogo
```

* * *

Semana 11: Concurrencia con `threading`
=======================================

Objetivo
--------

Ejecutar tareas de entrada/salida en paralelo para mejorar la capacidad de respuesta, y aprender a proteger datos compartidos entre hilos.

* * *

1\. Creación de hilos
---------------------

*   Importar y configurar un hilo:
    

```python
import threading

def tarea(id):
    print(f"Hilo {id} iniciando")

hilo = threading.Thread(target=tarea, args=(1,))
hilo.start()    # Lanza el hilo en background
hilo.join()     # Espera a que termine antes de continuar
```

* * *

2\. Daemon threads vs hilos normales
------------------------------------

*   **Hilos normales**: el programa espera a que terminen antes de cerrar.
*   **Daemon threads**: marcados como `daemon=True`; si sólo quedan daemons vivos, la aplicación se cierra inmediatamente.
*   Cuándo usar:
    *   Tareas de background que no deben bloquear el cierre de la app (logs, monitoreo).
    *   No críticos; no garantizan terminación limpia.
        

```python
hilo = threading.Thread(target=tarea, args=(2,))
hilo.setDaemon(True)  # O hilo.daemon = True antes de start()
hilo.start()
```

* * *

3\. Sincronización y problemas comunes
--------------------------------------

*   **Lock**: evita condiciones de carrera al proteger secciones críticas.
    

```python
lock = threading.Lock()
counter = 0

def incrementar():
    global counter
    with lock:
        temp = counter
        temp += 1
        counter = temp
```

*   Olvidar `lock.release()` al manejar manualmente puede bloquear otros hilos.
*   Siempre usar `with lock:` para asegurar liberación.
    

* * *

4\. Patrones avanzados
----------------------

*   **Producer–Consumer** con `deque` y `Event`:
    

```python
from collections import deque
from threading import Event, Thread

queue = deque()
event = Event()

def producer():
    queue.append("dato")
    event.set()         # Señala al consumidor

def consumer():
    event.wait()        # Espera hasta que producer llame set()
    item = queue.popleft()
    print(item)
```

*   **ThreadPoolExecutor** para gestionar múltiples hilos más fácilmente:
    

```python
from concurrent.futures import ThreadPoolExecutor

with ThreadPoolExecutor(max_workers=5) as executor:
    executor.map(tarea, range(10))
```

* * *

Semana 12: Serialización y Networking TCP/UDP
=============================================

Objetivo
--------

Guardar y cargar estructuras complejas en disco, y comunicar procesos locales o remotos mediante sockets.

* * *

1\. Serialización de datos
--------------------------

*   **pickle**
    *   Convierte objetos Python arbitrarios en bytes.
    *   **Precaución**: riesgo de ejecución de código malicioso al cargar datos no confiables.
        

```python
import pickle

data = {'x': [1,2,3], 'y': "texto"}
with open('data.pkl','wb') as f:
    pickle.dump(data, f)

with open('data.pkl','rb') as f:
    data_cargada = pickle.load(f)
```

*   **json**
    *   Almacena sólo tipos básicos: listas, diccionarios, strings, números, booleans, `None`.
    *   Texto legible y portable.
        

```python
import json

with open('data.json','w') as f:
    json.dump(data, f, indent=2)

with open('data.json','r') as f:
    data_cargada = json.load(f)
```

* * *

2\. Sockets TCP
---------------

1.  **Servidor TCP**
    ```python
    import socket
    srv = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    srv.bind(('localhost', 5000))
    srv.listen(1)                     # Máximo 1 cliente en cola
    conn, addr = srv.accept()        # Bloquea hasta que un cliente se conecte
    datos = conn.recv(1024)          # Lee hasta 1024 bytes
    conn.sendall(b"Hola cliente")    # Envía respuesta
    conn.close()
    srv.close()
    ```
2.  **Cliente TCP**
    ```python
    import socket
    cli = socket.socket()
    cli.connect(('localhost', 5000))
    cli.sendall(b"Hola servidor")
    respuesta = cli.recv(1024)
    print(respuesta.decode())
    cli.close()
    ```
    *   **Timeouts**: `socket.settimeout(segundos)` para evitar bloqueos indefinidos.
*   **shutdown()** vs **close()**: `shutdown()` cierra canal de envío o recepción antes de liberar recurso.
    

* * *

3\. Sockets UDP
---------------

*   **Sin conexión**; se envía a una dirección sin “handshake”.
*   No garantizado: orden o entrega.
    

```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.sendto(b"Hola UDP", ('localhost', 5001))
data, addr = sock.recvfrom(1024)
print(data.decode(), "de", addr)
sock.close()
```

* * *

Semana 13: Web Services con Flask y consumo con `requests`
==========================================================

Objetivo
--------

Diseñar APIs RESTful simples con Flask y consumirlas desde scripts Python usando la librería `requests`.

* * *

1\. Cliente HTTP con `requests`
-------------------------------

*   Enviar **GET** y procesar JSON:
    

```python
import requests

resp = requests.get("https://api.example.com/items")
if resp.status_code == 200:
    items = resp.json()           # Convierte cuerpo en dict/list
```

*   Enviar **POST** con JSON:
    

```python
payload = {'nombre': 'María', 'edad': 30}
resp = requests.post("https://api.example.com/users", json=payload)
```

*   Manejo de errores:
    ```python
    try:
        resp.raise_for_status()    # Lanza HTTPError si status ≥400
    except requests.exceptions.RequestException as e:
        print("Error de red o HTTP:", e)
    ```
    

* * *

2\. Servidor REST con Flask
---------------------------

1.  **Setup básico**
    ```python
    from flask import Flask, request, jsonify, abort
    app = Flask(__name__)
    items = []
    ```
2.  **Rutas y métodos**
    ```python
    @app.route('/items', methods=['GET'])
    def listar_items():
        return jsonify(items), 200
    @app.route('/items', methods=['POST'])
    def crear_item():
        data = request.get_json()
        if not data or 'nombre' not in data:
            abort(400, "Falta campo 'nombre'")
        items.append(data)
        return jsonify(data), 201
    ```
3.  **Ejecución**
    ```bash
    export FLASK_APP=mi_app.py
    flask run --host=0.0.0.0 --port=5000
    ```
    

* * *

Semana 14: PyQt5 Avanzado – Threads y Networking en la UI
=========================================================

Objetivo
--------

Integrar operaciones de red y cómputo paralelo en una interfaz Qt sin bloquear la experiencia de usuario.

* * *

1\. Usar `QThread` en lugar de `threading.Thread`
-------------------------------------------------

*   `QThread` se integra con el bucle de eventos de Qt y permite emitir señales de vuelta al hilo principal.
    

```python
from PyQt5.QtCore import QThread, pyqtSignal

class Worker(QThread):
    data_received = pyqtSignal(bytes)

    def run(self):
        # Operaciones de red o cómputo
        result = b"datos"
        self.data_received.emit(result)
```

*   En la UI:
    

```python
worker = Worker()
worker.data_received.connect(self.actualizar_ui)
worker.start()
```

* * *

2\. Comunicación seguro con señales
-----------------------------------

*   Evitar llamar métodos de widgets desde hilos externos; en su lugar, emitir señales y actualizar la UI en el hilo principal.
    

* * *

3\. Ejemplo completo de cliente TCP en Qt
-----------------------------------------

*   **Backend**: `QThread` gestiona socket y emite cada mensaje.
*   **Frontend**: recibe señal y muestra texto en un `QTextEdit`.
    

* * *

Semana 15: Tópicos avanzados – Regex, Grafos, NumPy y pandas
============================================================

Objetivo
--------

Introducir herramientas para procesamiento de texto, estructuras de datos complejas y análisis de datos.

* * *

1\. Expresiones regulares (`re`)
--------------------------------

*   **Compilar patrón**:
    ```python
    import re
    pat = re.compile(r'\d{3}-\d{2}-\d{4}')
    ```
*   **Buscar y extraer**:
    ```python
    m = pat.search("ID: 123-45-6789")
    if m:
        print("Encontrado:", m.group())
    ```
*   **Reemplazar**:
    ```python
    text = re.sub(r'\s+', ' ', texto)  # Normaliza espacios
    ```
    

* * *

2\. Grafos y búsqueda
---------------------

*   **Representación**:
    ```python
    grafo = {
        'A': ['B','C'],
        'B': ['A','D'],
        'C': ['A','D'],
        'D': ['B','C']
    }
    ```
*   **BFS**:
    ```python
    from collections import deque
    def bfs(grafo, inicio):
        visitados = set([inicio])
        cola = deque([inicio])
        while cola:
            nodo = cola.popleft()
            for vecino in grafo[nodo]:
                if vecino not in visitados:
                    visitados.add(vecino)
                    cola.append(vecino)
        return visitados
    ```
    

* * *

3\. NumPy
---------

*   **Crear array**:
    ```python
    import numpy as np
    arr = np.arange(10)           # [0,1,...,9]
    mat = arr.reshape(2,5)        # 2 filas, 5 columnas
    ```
*   **Operaciones vectorizadas**:
    ```python
    arr2 = arr * 2                # Multiplica cada elemento por 2
    mask = arr % 2 == 0           # Máscara booleana
    ```
    

* * *

4\. pandas
----------

*   **Series y DataFrame**:
    ```python
    import pandas as pd
    df = pd.DataFrame({'a':[1,2], 'b':[3,4]})
    ```
*   **Selección**:
    ```python
    df.loc[0,'a']                 # Primera fila, columna 'a'
    df[df['b']>3]                 # Filas donde b>3
    ```
*   **GroupBy**:
    ```python
    df.groupby('a').sum()
    ```
*   **Merge**:
    ```python
    pd.merge(df1, df2, on='clave')
    ```
    

* * *

> **Guarda este documento como tu guía de referencia**. Si necesitas ejemplos más extensos o aclarar algún punto, avísame.



---
Powered by [ChatGPT Exporter](https://www.chatgptexporter.com)