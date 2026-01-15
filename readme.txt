La carpeta 'dockerfile-ros2-jazzy' contiene un archivo Dockerfile
con las configuraciones necesarias para crear un contenedor de Docker
con ROS2 Jazzy y otras características que facilitan su uso en un robot móvil.

1. En un terminal, acceder a la carpeta 'dockerfile-ros2-jazzy'
cd <ruta a la carpeta>/dockerfile-ros2-jazzy

2. Construir la imagen de docker a partir del archivo Dockerfile
docker build -t ros2_jazzy .

El proceso tardará bastante, dependiendo de la calidad de su conexión a internet.
Una vez concluido la imagen está lista para levantar contenedores.

3. En tu carpeta de usuario, crea una carpeta 'ros2_ws' y, de entro de ella, otra carpeta 'src'. Este será el espacio de trabajo que será compartido entre la máquina host y el 
contenedor de Docker.
cd ~ && mkdir ros2_ws && cd ros2_ws && mkdir src

4.Para lanzar un contenedor de ros2_jazzy, utilice la siguiente instrucción recomendada, reemplazando <TU_USUARIO> por el nombre de usuario de la máquina host:
docker run -it --rm --user=user --net=host --ipc=host -v /home/<TU_USUARIO>/ros2_ws:/home/user/ros2_ws -v /tmp/.X11-unix:/tmp.X11-unix:rw --env=DISPLAY -v /dev:/dev --device-cgroup-rule='c 13:* rmw' --device-cgroup-rule='c 166:* rmw' --device-cgroup-rule='c 188:* rmw' -v /dev/bus/usb:/dev/bus/usb --device-cgroup-rule='c 189:* rmw' ros2_jazzy

El usuario creado en el contenedor es 'user' y la contraseña es 'ros' (sin comillas)

Las opciones son:
-it : Mantiene abierto el terminal interactivo.
--rm : Elimina el contenedor una vez que se cierra.
--net=host --ipc=host : Realiza un puente con la conexion de red de la máquina host
-v /home/<TU_USUARIO>/ros2:/home/user/ros2_ws : comparte la carpeta de espacio de trabajo
-v /tmp/.X11-unix:/tmp.X11-unix:rw --env=DISPLAY : comparte acceso a escritorio gráfico (para turtlesim o rviz, por ejemplo)
-v /dev:/dev --device-cgroup-rule='c 13:* rmw' --device-cgroup-rule='c 166:* rmw' --device-cgroup-rule='c 188:* rmw' -v /dev/bus/usb:/dev/bus/usb --device-cgroup-rule='c 189:* rmw'
Todas estas opciones habilitan los permisos para acceder a dispositivos USB desde el interior del contenedor.
ros2_jazzy : es la imagen que utilizamos


