# Creando-Deployment-con-Kubernetes   ![image](https://github.com/user-attachments/assets/761442a3-01b0-45cb-b40a-26cf7121706b)   ![image](https://github.com/user-attachments/assets/bd5c8a79-7d38-41b0-b4a4-318b4a5ae80c)
__________________________________________________________________________________________________________________________________________________________________________________________________________________

En este Proyecto veremos cómo activar Kubernetes dentro de Docker Desktop y crear un deployment para ejecutar N8N en contenedores. Asimismo, iremos aprendiendo paso a paso a cómo habilitar Kubernetes desde la configuración de Docker, esperar a que el sistema instale los componentes necesarios y verificar que el clúster esté funcionando correctamente antes de comenzar a desplegar servicios.

Durante el proceso crearemos un archivo YAML donde definiremos la configuración del deployment, incluyendo la imagen de N8N que se ejecutará, el puerto del contenedor y el número de réplicas que permitirán mantener el servicio activo incluso si algún contenedor falla. De esta forma podremos comprender cómo Kubernetes permite mantener aplicaciones disponibles mediante balanceo de carga y auto recuperación.

También aprendermos a crear un servicio tipo NodePort para exponer tu aplicación fuera del clúster, permitiendo acceder a N8N desde el navegador utilizando localhost y un puerto específico. Finalmente comprobaremos cómo Kubernetes supervisa los contenedores: si alguno se elimina o falla, el sistema lo recrea automáticamente para mantener el servicio funcionando.

Finalmente tendremos una base clara para empezar a trabajar con Kubernetes en nuestra máquina local, desplegar contenedores de forma escalable y entender cómo esta herramienta gestiona la alta disponibilidad de tus aplicaciones. 🚀

# Pasos
___________________________________________________________________________________________________________________________________________________________________________________________________________________

