
# Anotações

- [Configuring your ROS 2 environment
](http://docs.ros.org/en/galactic/Tutorials/Configuring-ROS2-Environment.html)

Escolher ambiente para execução
``` shell
source /opt/ros/galactic/setup.bash
# ou
source devel/setup.bash
```

Configurar source padrão
``` shell
echo "source /opt/ros/galactic/setup.bash" >> ~/.bashrc
```

Setar uma variavel de ambiente
``` shell
export ROS_DOMAIN_ID=<your_domain_id>
echo "export ROS_DOMAIN_ID=<your_domain_id>" >> ~/.bashrc
```

[Introducing turtlesim and rqt](http://docs.ros.org/en/galactic/Tutorials/Turtlesim/Introducing-Turtlesim.html)  

Instalar pacote turtlesim
``` shell 
sudo apt update
sudo apt install ros-galactic-turtlesim
``` 

Verificar executaveis do pacote turtlesim
``` shell 
ros2 pkg executables turtlesim
```

Rodar executavel ROS2
```
ros2 run turtlesim turtlesim_node
```

Rodar teleop turtlesim
```
ros2 run turtlesim turtle_teleop_key
```

Verificar nodes e seus topics, services e actions
```
ros2 node list
ros2 topic list
ros2 service list
ros2 action list
```

Instalar rqt

``` shell
sudo apt update

sudo apt install ~nros-galactic-rqt*
```

Remapear nodes(Pesquisar mais sobre remaping)

``` shell
ros2 run turtlesim turtle_teleop_key --ros-args --remap turtle1/cmd_vel:=turtle2/cmd_vel

```

Repositorio Turtlesim (/ros_tutorials/tree/galactic-devel/turtlesim)
 
``` shell
git clone https://github.com/ros/ros_tutorials.git 
```

[Understanding ROS 2 nodes](http://docs.ros.org.ros.informatik.uni-freiburg.de/en/galactic/Tutorials/Understanding-ROS2-Nodes.html#ros2-node-list)


"Cada node no ROS deve ser responsável por um único propósito de módulo (por exemplo, um node para controlar os motores das rodas, um node para controlar um telêmetro a laser, etc). Cada node pode enviar e receber dados para outros nodes por meio de topics, services, actions, ou parameters."

Executando um node ROS2

``` shell
ros2 run <package_name> <executable_name>
```

Remap "O remapeamento permite reatribuir propriedades de nó padrão, como nome de nó, nomes de tópicos, nomes de serviço etc. "
``` shell
ros2 run turtlesim turtlesim_node --ros-args --remap __node:=my_turtle
```

Vizualizar informações de um node.
``` shell
ros2 node info /my_turtle
```

``` shell
s
```
