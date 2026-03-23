# 🤖 Control de Robot Diferencial con micro-ROS

## 🧠 Descripción

Este proyecto explora la integración de **micro-ROS** en un sistema embebido basado en Arduino para el control de un **robot diferencial**.

El objetivo principal fue implementar comunicación entre un microcontrolador y el ecosistema ROS 2, permitiendo:

* Controlar motores en tiempo real
* Leer encoders
* Publicar información del robot
* Recibir comandos desde un nodo externo

Este trabajo se desarrolló como parte de la exploración en robótica móvil y sistemas embebidos.

---

## ⚙️ Características

* 🤖 Control de robot diferencial (dos motores)
* 📡 Comunicación con ROS 2 mediante micro-ROS
* 📈 Lectura de encoders en tiempo real
* 🎯 Control de velocidad con PID
* 📤 Publicación de datos (posición de encoders)
* 📥 Suscripción a comandos de velocidad
* 🌐 Comunicación vía WiFi con agente micro-ROS

---

## 🧩 Arquitectura del sistema

El sistema está compuesto por:

* **Microcontrolador (Arduino / ESP32)**
* **Motores DC con driver**
* **Encoders**
* **Agente micro-ROS (en PC)**
* **Nodo ROS 2 externo**

---

## 🔄 Flujo de funcionamiento

1. El nodo en ROS 2 envía velocidades deseadas
2. El microcontrolador recibe comandos (`/diffrobot_motor`)
3. Se ajusta la velocidad con control PID
4. Los encoders miden el movimiento real
5. Se publican datos de posición:

   * `/diffrobot_left_position`
   * `/diffrobot_right_position`

---

## 📡 Comunicación ROS

### 📥 Suscripción

```text
/diffrobot_motor  (std_msgs/Int64MultiArray)
```

* `data[0]` → velocidad motor izquierdo
* `data[1]` → velocidad motor derecho

---

### 📤 Publicaciones

```text
/diffrobot_left_position   (std_msgs/Int64)
/diffrobot_right_position  (std_msgs/Int64)
```

---

## ⚙️ Configuración de red

En el código:

```cpp id="netcfg01"
IPAddress agent_ip(127, 0, 0, 0); // IP del agente
size_t port = 8888;
char ssid[] = "SSID";
char psk[] = "PASSWORD";
```

⚠️ Debes cambiar:

* `SSID`
* `PASSWORD`
* `agent_ip`

---

## 🧠 Control PID

Se implementa control PID para cada motor:

```cpp id="pid01"
diffrobot::Pid left_pid_controller(...);
diffrobot::Pid right_pid_controller(...);
```

Permite:

* Estabilizar velocidad
* Corregir errores del encoder
* Mejorar respuesta del sistema

---

## ⏱️ Ejecución del sistema

El control se ejecuta mediante un **timer periódico**:

```cpp id="timer01"
RCCHECK(rclc_timer_init_default(...))
```

En cada ciclo:

* Se ajusta la velocidad de los motores
* Se leen encoders
* Se publican datos

---

## 🔌 Componentes principales

* Microcontrolador (Arduino / ESP32)
* Driver de motores
* Encoders
* Red WiFi
* PC con ROS 2 (agente micro-ROS)

---

## 🚀 Cómo usar

### 1. Configurar micro-ROS agent en PC

```bash id="runagent01"
ros2 run micro_ros_agent micro_ros_agent udp4 --port 8888
```

---

### 2. Configurar red en el código

Editar:

```cpp id="netcfg02"
char ssid[] = "TU_WIFI";
char psk[] = "TU_PASSWORD";
```

---

### 3. Subir código al microcontrolador

Desde PlatformIO o Arduino IDE.

---

### 4. Enviar comandos desde ROS 2

```bash id="pubcmd01"
ros2 topic pub /diffrobot_motor std_msgs/msg/Int64MultiArray "{data: [100, 100]}"
```

---

## 📚 Conceptos aplicados

* Sistemas embebidos
* micro-ROS
* ROS 2
* Control PID
* Comunicación distribuida
* Interrupciones (encoders)
* Tiempo real

---

## ⚠️ Limitaciones

* Configuración de red puede ser inestable
* Dependencia del agente micro-ROS
* Latencia en comunicación
* Ajuste manual del PID

---

## 🧪 Estado del proyecto

⚠️ Proyecto experimental

Se logró:

* Comunicación básica con ROS 2
* Control de motores
* Lectura de encoders

Pendiente:

* Mejorar estabilidad
* Optimizar control
* Integrar navegación

---

## 🧠 Futuras mejoras

* 🚗 Odometry completa
* 📍 SLAM
* 🧭 Navegación autónoma
* 🔵 Integración con ROS Navigation Stack
* 📊 Visualización en RViz

---

## 👨‍💻 Autor

* Jose Esteban Robayo - Ingenieria Mecatronica 
* Proyecto académico – Pontificia Universidad Javeriana

---

## 💡 Notas

* Se utilizan interrupciones para lectura de encoders
* El control PID depende de la frecuencia (`kPidRate`)
* La comunicación depende de la conexión WiFi

---

## ✨ Agradecimientos

Proyecto enfocado en la exploración de robótica moderna con ROS 2 y sistemas embebidos 🚀
