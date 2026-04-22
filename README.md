                                                                       # Creando-Deployment-en-Kubernetes   

![image](https://github.com/user-attachments/assets/761442a3-01b0-45cb-b40a-26cf7121706b)   ![image](https://github.com/user-attachments/assets/bd5c8a79-7d38-41b0-b4a4-318b4a5ae80c)
__________________________________________________________________________________________________________________________________________________________________________________________________________________

En este Proyecto veremos cómo activar Kubernetes dentro de Docker Desktop y crear un deployment para ejecutar N8N en contenedores. Asimismo, iremos aprendiendo paso a paso a cómo habilitar Kubernetes desde la configuración de Docker, esperar a que el sistema instale los componentes necesarios y verificar que el clúster esté funcionando correctamente antes de comenzar a desplegar servicios.

Durante el proceso crearemos un archivo YAML donde definiremos la configuración del deployment, incluyendo la imagen de N8N que se ejecutará, el puerto del contenedor y el número de réplicas que permitirán mantener el servicio activo incluso si algún contenedor falla. De esta forma podremos comprender cómo Kubernetes permite mantener aplicaciones disponibles mediante balanceo de carga y auto recuperación.

También aprendermos a crear un servicio tipo NodePort para exponer tu aplicación fuera del clúster, permitiendo acceder a N8N desde el navegador utilizando localhost y un puerto específico. Finalmente comprobaremos cómo Kubernetes supervisa los contenedores: si alguno se elimina o falla, el sistema lo recrea automáticamente para mantener el servicio funcionando.

Finalmente tendremos una base clara para empezar a trabajar con Kubernetes en nuestra máquina local, desplegar contenedores de forma escalable y entender cómo esta herramienta gestiona la alta disponibilidad de tus aplicaciones. 🚀

# Pasos
_____________________________________________________________________________________________________________________________________________________________________________________________________________

CREANDO DEPLOYMENT EN KUBERNETES(N8N)

•	Primero vamos a Docker Desktop y hacemos click en icono de configuraciones
 
![image](https://github.com/user-attachments/assets/dfa56947-e049-4974-8140-ec07b694dc56)

•	Y luego damos click en donde dice kubernetes, lo habilitamos y, damos click en aplicar e instalar.

![image](https://github.com/user-attachments/assets/033cb8e7-030d-422e-a13f-b9e90a42b500) 

•	Luego verificamos que este activado

 ![image](https://github.com/user-attachments/assets/5a0e8880-2e73-4d46-9c21-f1f5df5b6e70)

•	Ahora salimos del área de configuración y nos vamos a imágenes.

![image](https://github.com/user-attachments/assets/f2b428be-7d90-409d-8220-2a7371a88ca0)
 
•	En nuestros documentos creamos una carpeta llamada Kubernetes y dentro de la carpeta creamos un archivo llamado n8n.yaml

 ![image](https://github.com/user-attachments/assets/130c4534-d529-437e-98cc-2a12e8aa8ee4)

•	Luego damos click derecho al archivo y pegamos el siguiente código: 

apiVersion: apps/v1             # versión 1
kind: Deployment                 #Construye el contenedor.
metadata:
  name: n8n                           # nombre del contenedor.
spec:
  replicas: 1                            #cantidadde contenedores que deseas correr.
  selector:
    matchLabels:
      app: n8n                          #jala del deployment todo lo de n8n.
  template:
    metadata:
      labels:
        app: n8n                      #el contenedor se llamará n8n.
    spec:
      containers:
      - name: n8n                  # <-- OJO: Empieza con guion y 2 espacios
        image: n8nio/n8n:latest    # <-- 4 espacios desde el margen izquierdo
        ports:
        - containerPort: 5678      # <-- OJO: Empieza con guion y 4 espacios
---
apiVersion: v1
kind: Service                               # Ejecuta el contenedor.
metadata:
  name: n8n-service
spec:
  type: NodePort      # lee los contenedores y saca la información a través de un puerto.
  selector:
    app: n8n
  ports:
  - port: 5678                    # <-- OJO: Empieza con guion y 2 espacios
    targetPort: 5678         #puerto de servicio que va alimentar al NodePort.
    nodePort: 30007


•	Guardamos el archivo y abrimos el powershell (terminal), para abrir el archivo kubernetes directo a la terminal tendremos que ir primero a la carpeta y hacemos click en la parte superior donde esta la ruta de la carpeta y luego escribimos cmd y presionamos enter y, así abrirá el terminal directo en la carpeta de kubernetes.

 ![image](https://github.com/user-attachments/assets/bfb97620-817d-4c5c-b7e8-574b21551053)

 ![image](https://github.com/user-attachments/assets/33468367-5587-46aa-a88f-a359aed3f5b2)

•	Ahora en la terminal escribimos el siguiente comando: kubectl apply -f n8n.yaml
1.	Kubectl -- llama al ejecutable de Kubernetes.
2.	Apply  -f   -- vamos aplicar las configuraciones que tenemos en el archivo n8n.yaml
 
![image](https://github.com/user-attachments/assets/8f5f0fb6-54c5-4213-918f-137f1d67e42b)

•	Eso indica que ya esta creado y lo podemos ver en el Pods (Kubernetes) del Docker Desktop.

 ![image](https://github.com/user-attachments/assets/51466acb-0e45-4e84-a649-510bb1d12507)

•	Por último, podemos ver en el localhost:30007. 
•	Algunas veces en Windows (Docker Desktop), el tipo de servicio NodePort (el puerto 30007) tiene problemas de enrutamiento de red con WSL2. Además, Kubernetes tarda uno o dos minutos en "activar" ese puerto, para ello haremos los siguientes pasos:
1.	Verifica que el Pod esté vivo (Espera a que diga "Running")
Escribe esto en tu terminal: kubectl get pods

Si ves la palabra Running en la columna STATUS, continúa. Si ves ContainerCreating, espera 30 segundos y vuelve a escribir el comando hasta que diga Running (n8n pesa unos 700MB y tarda en descargar).

2.	Abre otra ventana de terminal (la anterior déjala así) y escribe exactamente esto:

kubectl port-forward service/n8n-service 5678:5678

(Verás un mensaje que dice "Forwarding from 127.0.0.1:5678..."). Esta terminal se quedará "congelada" o mostrando logs. No la cierres. Mientras esa terminal esté abierta, el túnel está activo).

3.	Ahora, en tu navegador web, NO vayas al 30007. Ve a:
http://localhost:5678

4.	¡Ahí deberías ver la pantalla de bienvenida de n8n!

![image](https://github.com/user-attachments/assets/e35f02e9-5054-4b4d-b199-12fcd295f606)
