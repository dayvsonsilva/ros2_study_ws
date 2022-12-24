
# Anotações - Beginner: CLI Tools

- [Configuring your ROS 2 environment
](http://docs.ros.org/en/humble/Tutorials/Configuring-ROS2-Environment.html)

Escolher ambiente para execução
``` shell
source /opt/ros/humble/setup.bash
# ou
source devel/setup.bash
```

Configurar source padrão
``` shell
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
```

Setar uma variavel de ambiente
``` shell
export ROS_DOMAIN_ID=<your_domain_id>
echo "export ROS_DOMAIN_ID=<your_domain_id>" >> ~/.bashrc
```

[Introducing turtlesim and rqt](http://docs.ros.org/en/humble/Tutorials/Turtlesim/Introducing-Turtlesim.html)  

Instalar pacote turtlesim  

``` shell 
sudo apt update
sudo apt install ros-humble-turtlesim
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

sudo apt install ~nros-humble-rqt*
```

Remapear nodes(Pesquisar mais sobre remaping)

``` shell
ros2 run turtlesim turtle_teleop_key --ros-args --remap turtle1/cmd_vel:=turtle2/cmd_vel

```

Repositorio Turtlesim (/ros_tutorials/tree/humble-devel/turtlesim)
 
``` shell
git clone https://github.com/ros/ros_tutorials.git 
```

[Understanding ROS 2 nodes](http://docs.ros.org.ros.informatik.uni-freiburg.de/en/humble/Tutorials/Understanding-ROS2-Nodes.html#ros2-node-list)


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




[Understanding ROS 2 topics](http://docs.ros.org/en/humble/Tutorials/Topics/Understanding-ROS2-Topics.html)


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
```shell
Type: geometry_msgs/msg/Twist
Publisher count: 1
Subscription count: 1
```

Topic pub
```shell
ros2 topic pub <topic_name> <msg_type> '<args>'
```

Exemplo de um topi pub (Enviar comando apenas uma vez "--once")
```shell
ros2 topic pub --once /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```

O mesmo comando pode ser modificado de "--once" para "--rate 1" para que a mensagem seja publicado no topico com uma frequencia de 1hz até que o comando seja interrompido.

Para monitorar a frequencia em que um topico esta sendo publicado na rede.
```shell
ros2 topic hz /turtle1/pose
```

[Understanding ROS 2 services](http://docs.ros.org/en/humble/Tutorials/Services/Understanding-ROS2-Services.html)


Services são outro método de comunicação para nodes no ROS graph. Os serviços são baseados em um modelo call-and-response, versus o modelo de topics’ publisher-subscriber. Enquanto os topics permitem que os nós assinem fluxos de dados e obtenham atualizações contínuas, os serviços só fornecem dados quando são especificamente chamados por um cliente. 

Verificando o tipo de um determinado service
```shell
ros2 service type <service_name>
```
```shell
ros2 service type /clear
```

Lisando os services ativos com detalhes
```shell
ros2 service list -t
```

Procurar por um service de um tipo especifico
```shell
ros2 service find <type_name>
```
Ex
```shell
ros2 interface show std_srvs/srv/Empty
```
```shell
ros2 interface show turtlesim/srv/Spawn
```

Sintax Service call
```shell
ros2 service call <service_name> <service_type> <arguments>
```
ex
```shell
ros2 service call /clear std_srvs/srv/Empty
```

```shell
ros2 service call /spawn turtlesim/srv/Spawn "{x: 2, y: 2, theta: 0.2, name: ''}"
```

[Understanding ROS 2 parameters](http://docs.ros.org/en/humble/Tutorials/Parameters/Understanding-ROS2-Parameters.html)

Um parameter é um valor de configuração de um node. Você pode pensar em parameter como configurações de node. Um node pode armazenar parâmetros como inteiros, floats, booleanos, strings e listas. No ROS 2, cada nó mantém seus próprios parâmetros. Para obter mais informações sobre os parâmetros, consulte o documento conceitual. 

Listar parametros dos nodes ativos
```shell
ros2 param list
```

Sintax do param get
```shell
ros2 param get <node_name> <parameter_name>
```
Ex
```shell
ros2 param get /turtlesim background_g
```

Sintax param set
``` shell
ros2 param set <node_name> <parameter_name> <value>
```

Ex
```shell
ros2 param set /turtlesim background_r 150
```
O comando param dump salva parametros de um node em arquivos.yaml

ros2 param dump sintax
```shell
ros2 param dump <node_name>
```
ex
``` shell
ros2 param dump /turtlesim
```

Asaida do comando será um arquivo com os dados abaixo

```yaml
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
```shell
ros2 param load <node_name> <parameter_file>
```
Ex
```shell
ros2 param load /turtlesim ./turtlesim.yaml
```

Tambem é possivel carregar arquivos no momento de rodar um node.

Sintax
```shell
ros2 run <package_name> <executable_name> --ros-args --params-file <file_name>
```
Ex
```shell
ros2 run turtlesim turtlesim_node --ros-args --params-file ./turtlesim.yaml
```

[Understanding ROS 2 actions](http://docs.ros.org/en/humble/Tutorials/Understanding-ROS2-Actions.html)

"As ações são um dos tipos de comunicação no ROS 2 e destinam-se a tarefas de longa duração. Eles consistem em três partes: um objetivo, feedback e um resultado.  

As ações são construídas em tópicos e serviços. Sua funcionalidade é semelhante aos serviços, exceto que as ações podem ser canceladas. Eles também fornecem feedback constante, ao contrário de serviços que retornam uma única resposta.  

As ações usam um modelo cliente-servidor, semelhante ao modelo editor-assinante (descrito no tutorial de tópicos). Um nó “cliente de ação” envia uma meta para um nó “servidor de ação” que reconhece a meta e retorna um fluxo de feedback e um resultado. "  


O node info apresenta tos os subscribers, publishers, services, actions servers e clients
```
ros2 node info /turtlesim
```

Retorno:
```shell
/turtlesim
  Subscribers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /turtle1/cmd_vel: geometry_msgs/msg/Twist
  Publishers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /rosout: rcl_interfaces/msg/Log
    /turtle1/color_sensor: turtlesim/msg/Color
    /turtle1/pose: turtlesim/msg/Pose
  Service Servers:
    /clear: std_srvs/srv/Empty
    /kill: turtlesim/srv/Kill
    /reset: std_srvs/srv/Empty
    /spawn: turtlesim/srv/Spawn
    /turtle1/set_pen: turtlesim/srv/SetPen
    /turtle1/teleport_absolute: turtlesim/srv/TeleportAbsolute
    /turtle1/teleport_relative: turtlesim/srv/TeleportRelative
    /turtlesim/describe_parameters: rcl_interfaces/srv/DescribeParameters
    /turtlesim/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
    /turtlesim/get_parameters: rcl_interfaces/srv/GetParameters
    /turtlesim/list_parameters: rcl_interfaces/srv/ListParameters
    /turtlesim/set_parameters: rcl_interfaces/srv/SetParameters
    /turtlesim/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
  Service Clients:

  Action Servers:
    /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
  Action Clients:
```

Para listar actions ativos
```shell
ros2 action list
```

Para listar actions ativos com seus tipos
```shell
ros2 actions list -t
```
retorno:
```shell
/turtle1/rotate_absolute
```


Verificando inforções do action
``` shell
ros2 action info /turtle1/rotate_absolute
```

Retorno

``` shell
Action: /turtle1/rotate_absolute
Action clients: 1
    /teleop_turtle
Action servers: 1
    /turtlesim
```

"Isso nos diz o que aprendemos anteriormente ao executar as informações do nó ros2 em cada nó: O nó /teleop_turtle tem um cliente de ação e o nó /turtlesim tem um servidor de ação para a ação /turtle1/rotate_absolute."


Para verificar a extrutura de um action
``` shell
ros2 interface show turtlesim/action/RotateAbsolute
```

retorna
```
The desired heading in radians
float32 theta
---
The angular displacement in radians to the starting position
float32 delta
---
The remaining rotation in radians
float32 remaining
```

Sintaxros2 action send_goal
```
ros2 action send_goal <action_name> <action_type> <values>
```

<values> precisam estar no formato YAML. 

Ex
```
ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: 1.57}"
```

retorna:
```
Waiting for an action server to become available...
Sending goal:
   theta: 1.57

Goal accepted with ID: f8db8f44410849eaa93d3feb747dd444

Result:
  delta: -1.568000316619873

Goal finished with status: SUCCEEDED
```

ex2, adicionando feedback ao terminal
```
ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: -1.57}" --feedback
```

retorna
```
Sending goal:
   theta: -1.57

Goal accepted with ID: e6092c831f994afda92f0086f220da27

Feedback:
  remaining: -3.1268222332000732

Feedback:
  remaining: -3.1108222007751465

…

Result:
  delta: 3.1200008392333984

Goal finished with status: SUCCEEDED]
```


[Using rqt_console](http://docs.ros.org/en/humble/Tutorials/Rqt-Console/Using-Rqt-Console.html)

"rqt_console é uma ferramenta GUI usada para introspectar mensagens de log no ROS 2. Normalmente, as mensagens de log aparecem em seu terminal. Com o rqt_console, você pode coletar essas mensagens ao longo do tempo, visualizá-las de perto e de forma mais organizada, filtrá-las, salvá-las e até recarregar os arquivos salvos para introspecção em um momento diferente."


Abrir rqt_console
```
ros2 run rqt_console rqt_console
```

Tipos de mensagens de log disponiveis 
```
Fatal
Error
Warn
Info
Debug
```

"Mensagens fatais indicam que o sistema será encerrado para tentar se proteger de prejuízos.

As mensagens de erro indicam problemas significativos que não necessariamente danificam o sistema, mas o impedem de funcionar corretamente.

As mensagens de aviso indicam atividade inesperada ou resultados não ideais que podem representar um problema mais profundo, mas não prejudicam totalmente a funcionalidade.

As mensagens de informação indicam atualizações de eventos e status que servem como uma verificação visual de que o sistema está funcionando conforme o esperado.

As mensagens de depuração detalham todo o processo passo a passo da execução do sistema. "


Altere o nivel de mensagem
```
ros2 run turtlesim turtlesim_node --ros-args --log-level WARN
```


[Introducing ROS 2 launch](http://docs.ros.org/en/humble/Tutorials/Launch/CLI-Intro.html)

"Os arquivos de inicialização permitem que você inicie e configure vários executáveis contendo nós ROS 2 simultaneamente."


Rodar launch de exemplo
```
ros2 launch turtlesim multisim.launch.py
```

multisim.launch.py
``` py
# turtlesim/launch/multisim.launch.py

from launch import LaunchDescription
import launch_ros.actions

def generate_launch_description():
    return LaunchDescription([
        launch_ros.actions.Node(
            namespace= "turtlesim1", package='turtlesim', executable='turtlesim_node', output='screen'),
        launch_ros.actions.Node(
            namespace= "turtlesim2", package='turtlesim', executable='turtlesim_node', output='screen'),
    ])
```  

Esse tutorial é introdutorio, na seção Intermediate o tema sera abordado com mais detalhes.


[Recording and playing back data](http://docs.ros.org/en/humble/Tutorials/Ros2bag/Recording-And-Playing-Back-Data.html)

"ros2 bag é uma ferramenta de linha de comando para gravar dados publicados em tópicos em seu sistema. Ele acumula os dados passados em qualquer número de tópicos e os salva em um banco de dados. Você pode então reproduzir os dados para reproduzir os resultados de seus testes e experimentos. Gravar tópicos também é uma ótima maneira de compartilhar seu trabalho e permitir que outras pessoas o recriem."


Instalar ros2 bag
```
sudo apt-get install ros-humble-ros2bag \
                     ros-humble-rosbag2-storage-default-plugins
```

Sintax ros2 bag
```
ros2 bag record <topic_name>
```

Ex (salvar dados no arquivo record)
```
ros2 bag record /turtle1/cmd_vel
```

Salvando mais de um topic no arquivo subset
```
ros2 bag record -o subset /turtle1/cmd_vel /turtle1/pose
```

Verificar informações do bag
```
ros2 bag info <bag_file_name>
```

Reproduzindo dados de um bag
```
ros2 bag play <bag_file_name>
```
Ex
```
ros2 bag play subset
```
