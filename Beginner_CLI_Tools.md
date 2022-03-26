
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




[Understanding ROS 2 topics](http://docs.ros.org/en/galactic/Tutorials/Topics/Understanding-ROS2-Topics.html)


``` shell
rqt_graph
```
![Imagem salva a partir do rqt graph.](ros_graph.svg "rqt_graph").

Listando topicos
``` shell
ros2 topic list
```

Listando dopicos com seus respectivos tipos
``` shell
ros2 topic list -t
```

Visualisar dados de uma
``` shell
ros2 topic echo <topic_name>
```


Para ter mais detalhes de um topic
``` shell
ros2 topic info /turtle1/cmd_vel
```
Saida
```
Type: geometry_msgs/msg/Twist
Publisher count: 1
Subscription count: 1
```

Topic pub
```
ros2 topic pub <topic_name> <msg_type> '<args>'
```

Exemplo de um topi pub (Enviar comando apenas uma vez "--once")
```
ros2 topic pub --once /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```

O mesmo comando pode ser modificado de "--once" para "--rate 1" para que a mensagem seja publicado no topico com uma frequencia de 1hz até que o comando seja interrompido.

Para monitorar a frequencia em que um topico esta sendo publicado na rede.
```
ros2 topic hz /turtle1/pose
```

[Understanding ROS 2 services](http://docs.ros.org/en/galactic/Tutorials/Services/Understanding-ROS2-Services.html)


Services são outro método de comunicação para nodes no ROS graph. Os serviços são baseados em um modelo call-and-response, versus o modelo de topics’ publisher-subscriber. Enquanto os topics permitem que os nós assinem fluxos de dados e obtenham atualizações contínuas, os serviços só fornecem dados quando são especificamente chamados por um cliente. 

Verificando o tipo de um determinado service
```
ros2 service type <service_name>
```
```
ros2 service type /clear
```

Lisando os services ativos com detalhes
```
ros2 service list -t
```

Procurar por um service de um tipo especifico
```
ros2 service find <type_name>
```
Ex
```
ros2 interface show std_srvs/srv/Empty
```
```
ros2 interface show turtlesim/srv/Spawn
```

Sintax Service call
```
ros2 service call <service_name> <service_type> <arguments>
```
ex
```
ros2 service call /clear std_srvs/srv/Empty
```

```
ros2 service call /spawn turtlesim/srv/Spawn "{x: 2, y: 2, theta: 0.2, name: ''}"
```

[Understanding ROS 2 parameters](http://docs.ros.org/en/galactic/Tutorials/Parameters/Understanding-ROS2-Parameters.html)

Um parameter é um valor de configuração de um node. Você pode pensar em parameter como configurações de node. Um node pode armazenar parâmetros como inteiros, floats, booleanos, strings e listas. No ROS 2, cada nó mantém seus próprios parâmetros. Para obter mais informações sobre os parâmetros, consulte o documento conceitual. 

Listar parametros dos nodes ativos
```
ros2 param list
```

Sintax do param get
```
ros2 param get <node_name> <parameter_name>
```
Ex
```
ros2 param get /turtlesim background_g
```

Sintax param set
``` 
ros2 param set <node_name> <parameter_name> <value>
```

Ex
```
ros2 param set /turtlesim background_r 150
```
O comando param dump salva parametros de um node em arquivos.yaml

ros2 param dump sintax
```
ros2 param dump <node_name>
```
ex
```
ros2 param dump /turtlesim
```

Asaida do comando será um arquivo com os dados abaixo

``` yaml
/turtlesim:
  ros__parameters:
    background_b: 255
    background_g: 86
    background_r: 150
    qos_overrides:
      /parameter_events:
        publisher:
          depth: 1000
          durability: volatile
          history: keep_last
          reliability: reliable
    use_sim_time: false
```

Assim como é possivel salvar os parametros, tambem podemos carregar dados de um arquivo .yaml

Sintax param load
```
ros2 param load <node_name> <parameter_file>
```
Ex
```
ros2 param load /turtlesim ./turtlesim.yaml
```

Tambem é possivel carregar arquivos no momento de rodar um node.

Sintax
```
ros2 run <package_name> <executable_name> --ros-args --params-file <file_name>
```
Ex
```  
ros2 run turtlesim turtlesim_node --ros-args --params-file ./turtlesim.yaml
```
