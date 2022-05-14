# Anotações: Intermediate


[Creating an action](http://docs.ros.org/en/galactic/Tutorials/Actions/Creating-an-Action.html)  

"Você aprendeu sobre ações anteriormente no tutorial Entendendo as ações do ROS 2. Assim como os outros tipos de comunicação e suas respectivas interfaces (tópicos/msg e serviços/srv), você também pode definir ações personalizadas em seus pacotes. Este tutorial mostra como definir e construir uma ação que você pode usar com o servidor de ação e o cliente de ação que você escreverá no próximo tutorial."


o principal arquivo deste pacote é o Fibonacci.action.
```msg
int32 order # Request
---
int32[] sequence # Result
---
int32[] partial_sequence # Feedback
```  

Arquivos editados

CMakeLists.txt
package.xml


[Writing an action server and client (C++)](http://docs.ros.org/en/galactic/Tutorials/Actions/Writing-a-Cpp-Action-Server-Client.html)  

"As ações são uma forma de comunicação assíncrona no ROS. Os clientes de ação enviam solicitações de meta para servidores de ação. Os servidores de ação enviam feedback de metas e resultados para clientes de ação.
"

Criando o pacote
```sh
ros2 pkg create --dependencies action_tutorials_interfaces rclcpp rclcpp_action rclcpp_components -- action_tutorials_cpp
``` 

Arquivos criados/editados:
action_tutorials_cpp/CMakeLists.txt
action_tutorials_cpp/package.xml
action_tutorials_cpp/include/action_tutorials_cpp/visibility_control.h
action_tutorials_cpp/src/fibonacci_action_server.cpp
action_tutorials_cpp/src/fibonacci_action_client.cpp


Execute os comados abaixo em terminais distintos:
```sh
ros2 run action_tutorials_cpp fibonacci_action_server
```

```sh
ros2 run action_tutorials_cpp fibonacci_action_client
```



[Writing an action server and client (Python)](http://docs.ros.org/en/galactic/Tutorials/Actions/Writing-a-Py-Action-Server-Client.html)  

"As ações são uma forma de comunicação assíncrona no ROS 2. Clientes de ação enviam solicitações de meta para servidores de ação. Os servidores de ação enviam feedback de metas e resultados para clientes de ação."

Criando o pacote
```sh
ros2 pkg create --build-type ament_python action_tutorials_py  --dependencies rclpy action_tutorials_interfaces
```

Arquivos criados/editados:

action_tutorials_py/setup.py
action_tutorials_py/package.xml

action_tutorials_py/action_tutorials_py/fibonacci_action_server1.py # Arquivo inicial do tutorial
action_tutorials_py/action_tutorials_py/fibonacci_action_client1.py # Arquivo inicial do tutorial
action_tutorials_py/action_tutorials_py/fibonacci_action_server.py # Arquivo final do tutorial
action_tutorials_py/action_tutorials_py/fibonacci_action_client.py # Arquivo final do tutorial

Na primeira etapa do tutorial é abordado um formato mais simples do server e client, onde o feedback é passado ao final do processamento, no formato final é implementado um formato de feedback incremental.




[Composing multiple nodes in a single process](http://docs.ros.org/en/galactic/Tutorials/Composition.html)  REFAZER

"Para ver quais componentes estão registrados e disponíveis no espaço de trabalho, execute o seguinte em um shell:"

```sh
ros2 component types
```

Retorna:
```sh
(... components of other packages here)
composition
  composition::Talker
  composition::Listener
  composition::NodeLikeListener
  composition::Server
  composition::Client
(... components of other packages here)
```  

[Using colcon to build packages](http://docs.ros.org/en/galactic/Tutorials/Colcon-Tutorial.html)  


Underlay - Instalação /opt/ros/galactic/setup.bash  
Overlay - Workspace ~/ros2_study_ws

Para criar um ambiente de trabalho(Workspace ou ws):
```sh
mkdir -p ~/ros2_study_ws/src
cd ~/ros2_study_ws
```

Após criar o diretorio, os pacotes de trabalho devem ser adicionados ao diretorio src:
obs: clonar repo git clone https://github.com/ros2/examples src/examples -b galactic
```sh
.
└── src
    └── examples
        ├── CONTRIBUTING.md
        ├── LICENSE
        ├── rclcpp
        ├── rclpy
        └── README.md

4 directories, 3 files
```

"Na raiz da área de trabalho, execute colcon build. Como tipos de compilação como ament_cmake não suportam o conceito de espaço devel e requerem que o pacote seja instalado, colcon suporta a opção --symlink-install. Isso permite que os arquivos instalados sejam alterados alterando os arquivos no espaço de origem (por exemplo, arquivos Python ou outros não compilados com recursos) para uma iteração mais rápida."

```
colcon build --symlink-install
```

Após o build do workspace tera os seguintes diretorios

```sh
.
├── build
├── install
├── log
└── src

4 directories, 0 files
```

Ainda é possivel realizar o teste do ambiente.
```sh
colcon test
```

[Monitoring for parameter changes (C++)](http://docs.ros.org/en/galactic/Tutorials/Monitoring-For-Parameter-Changes-CPP.html)  

"Muitas vezes, um nó precisa responder a alterações em seus próprios parâmetros ou nos parâmetros de outro nó. A classe ParameterEventHandler facilita a escuta de alterações de parâmetro para que seu código possa responder a elas. Este tutorial mostrará como usar a versão C++ da classe ParameterEventHandler para monitorar alterações nos parâmetros de um nó, bem como alterações nos parâmetros de outro nó."


```sh
ros2 pkg create --build-type ament_cmake cpp_parameter_event_handler --dependencies rclcpp
```
Arquivos criados/editados:

package.xml
parameter_event_handler1.cpp
parameter_event_handler.cpp 

Primeira parte do tutorial
Terminal 1
```sh
ros2 run cpp_parameter_event_handler parameter_event_handler
```
Terminal 2
```sh
ros2 param set node_with_parameters an_int_param 43
```
retorno 
```sh
[INFO] [1606950498.422461764] [node_with_parameters]: cb: Received an update to parameter "an_int_param" of type integer: "43"
```  

Segunda parte
Terminal 1
```sh
ros2 run cpp_parameter_event_handler parameter_event_handler
```
Terminal 2
```sh
ros2 run demo_nodes_cpp parameter_blackboard
```
Terminal 3
```sh
ros2 param set parameter_blackboard a_double_param 3.45
```

Retorno
```sh
[INFO] [1606952588.237531933] [node_with_parameters]: cb2: Received an update to parameter "a_double_param" of type: double: "3.45"
```

[Launch Tutorials](http://docs.ros.org/en/galactic/Tutorials/Launch/Launch-Main.html)  

[Creating a ROS 2 Launch File.](http://docs.ros.org/en/galactic/Tutorials/Launch/Creating-Launch-Files.html) 

Criando um arquivo de launch para ros2

Arquivo criado no turorial
src/ros_tutorials/launch/turtlesim_mimic_launch.py


```sh
cd launch
ros2 launch turtlesim_mimic_launch.py
```

O comando ira abrir duas instancias do turtlesim.

No segundo terminal executar o comando abaixo:
```sh
ros2 topic pub -r 1 /turtlesim1/turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: -1.8}}"
```

Para visualizar a interligação entre nós basta rodar o comando:
```sh
rqt_graph
```  


[Launching and Monitor Multiple Nodes with Launch.](http://docs.ros.org/en/galactic/Tutorials/Launch/Launch-system.html) 

"O sistema de lançamento no ROS 2 é responsável por ajudar o usuário a descrever a configuração de seu sistema e então executá-lo conforme descrito. A configuração do sistema inclui quais programas executar, onde executá-los, quais argumentos transmiti-los e convenções específicas do ROS que facilitam a reutilização de componentes em todo o sistema, fornecendo a cada um configurações diferentes. Também é responsável por monitorar o estado dos processos iniciados, reportar e/ou reagir a mudanças no estado desses processos."  

Para este turorial foi utilizado o pacote my_package_py.

Arquivos criados/editados:  
setup.py
launch/my_script.launch.py


```sh
ros2 launch my_package_py my_script.launch.py
```

[Using substitutions in launch files](http://docs.ros.org/en/galactic/Tutorials/Launch/Using-Substitutions.html) 

"Os arquivos de inicialização são usados para iniciar nós, serviços e executar processos. Esse conjunto de ações pode ter argumentos, que afetam seu comportamento. As substituições podem ser usadas em argumentos para fornecer mais flexibilidade ao descrever arquivos de inicialização reutilizáveis. As substituições são variáveis que são avaliadas apenas durante a execução da descrição de execução e podem ser usadas para adquirir informações específicas como uma configuração de execução, uma variável de ambiente ou para avaliar uma expressão arbitrária do Python."

 Para o Tutorial foram criados dois arquivos:

 launch/example_main.launch.py
 launch/example_substitutions.launch.py

Terminal 1
```sh
ros2 launch launch_tutorial example_substitutions.launch.py --show-args
```

Terminal 2
```sh
ros2 launch launch_tutorial example_substitutions.launch.py turtlesim_ns:='turtlesim3' use_provided_red:='True' new_background_r:=200
```

[Using Event Handlers.](http://docs.ros.org/en/galactic/Tutorials/Launch/Using-Event-Handlers.html) 

"Launch in ROS 2 é um sistema que executa e gerencia processos definidos pelo usuário. Ele é responsável por monitorar o estado dos processos que lançou, bem como relatar e reagir às mudanças no estado desses processos. Essas alterações são chamadas de eventos e podem ser tratadas registrando um manipulador de eventos no sistema de inicialização. Os manipuladores de eventos podem ser registrados para eventos específicos e podem ser úteis para monitorar o estado dos processos. Além disso, eles podem ser usados para definir um conjunto complexo de regras que podem ser usadas para modificar dinamicamente o arquivo de inicialização."


Para o tutorial foi criado o arquivo launch/tutorial/launch/example_event_handlers.launch.py

Executando o launch
```sh
ros2 launch launch_tutorial example_event_handlers.launch.py turtlesim_ns:='turtlesim3' use_provided_red:='True' new_background_r:=200
```
Passos executados pelo launch
This will do the following:

    Start a turtlesim node with a blue background

    Spawn the second turtle

    Change the color to purple

    Change the color to pink after two seconds if the provided background_r argument is 200 and use_provided_red argument is True

    Shutdown the launch file when the turtlesim window is closed

Additionally, it will log messages to the console when:

    The turtlesim node starts

    The spawn action is executed

    The change_background_r action is executed

    The change_background_r_conditioned action is executed

    The turtlesim node exits

    The launch process is asked to shutdown.


[Using ROS 2 Launch For Large Projects.](http://docs.ros.org/en/galactic/Tutorials/Launch/Using-ROS2-Launch-For-Large-Projects.html) 


"Este tutorial descreve algumas dicas para escrever arquivos de inicialização para projetos grandes. O foco está em como estruturar os arquivos de inicialização para que possam ser reutilizados o máximo possível em diferentes situações. Além disso, abrange exemplos de uso de diferentes ferramentas de inicialização do ROS 2, como parâmetros, arquivos YAML, remapeamentos, namespaces, argumentos padrão e configurações RViz."

Arquivos criados no tutorial:

launch/launch_turtlesim.launch.py
launch/turtlesim_world_1.launch.py
launch/turtlesim_world_2.launch.py
launch/turtlesim_world_3.launch.py
launch/broadcaster_listener.launch.py
launch/mimic.launch.py
launch/turtlesim_rviz.launch.py
launch/fixed_broadcaster.launch.py
config/turtlesim.yaml


ros2 launch launch_tutorial launch_turtlesim.launch.py  

* Nescessario instalar pacote
sudo apt install ros-galactic-turtle-tf2-py


[tf2 Tutorials](http://docs.ros.org/en/galactic/Tutorials/Tf2/Tf2-Main.html)  

- [ ] Learning tf2   

[1. Introduction to tf2](http://docs.ros.org/en/galactic/Tutorials/Tf2/Introduction-To-Tf2.html)

"Objetivo: Executar uma demonstração do turtlesim e ver um pouco do poder do tf2 em um exemplo multi-robô usando o turtlesim."

Para este tutorial o pacote a seguir deve ser instalado:
```sh
sudo apt-get install ros-galactic-turtle-tf2-py ros-galactic-tf2-tools ros-galactic-tf-transformations
```

```sh
pip3 install transforms3d
```
tf2 tools

1 Using view_frames
```sh
ros2 run tf2_tools view_frames
```

2 Using tf2_echo
```sh
ros2 run tf2_ros tf2_echo turtle2 turtle1
```

rviz and tf2
```sh
ros2 run rviz2 rviz2 -d $(ros2 pkg prefix --share turtle_tf2_py)/rviz/turtle_rviz.rviz
```

[2. Writing a tf2 static broadcaster]() 
- [Python](http://docs.ros.org/en/galactic/Tutorials/Tf2/Writing-A-Tf2-Static-Broadcaster-Py.html)

Pacote utilizado no tutorial
```sh
ros2 pkg create --build-type ament_python learning_tf2_py
```

Arquivos criados no tutorial:
learning_tf2_py/learning_tf2_py/static_turtle_tf2_broadcaster.py

Terminal 1
```sh
ros2 run learning_tf2_py static_turtle_tf2_broadcaster mystaticturtle 0 0 1 0 0 0
```

Terminal 2
```
ros2 topic echo --qos-reliability reliable --qos-durability transient_local /tf_static
```
Retorna
```sh
transforms:
- header:
   stamp:
      sec: 1622908754
      nanosec: 208515730
   frame_id: world
child_frame_id: mystaticturtle
transform:
   translation:
      x: 0.0
      y: 0.0
      z: 1.0
   rotation:
      x: 0.0
      y: 0.0
      z: 0.0
      w: 1.0
```

Pesar de ser possivel escrever o porpio codigo para trabalhar com transformadas, é sugerido que seja utlizado o comando apropiado para isso:

```sh
ros2 run tf2_ros static_transform_publisher x y z yaw pitch roll frame_id child_frame_id
```
ou
```sh
ros2 run tf2_ros static_transform_publisher x y z yaw pitch roll frame_id child_frame_id
```

Para adicionar esse comando a um arquivo de launch:
```py
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
   return LaunchDescription([
      Node(
            package='tf2_ros',
            executable='static_transform_publisher',
            arguments = ['0', '0', '1', '0', '0', '0', 'world', 'mystaticturtle']
      ),
   ])
```

- [C++](http://docs.ros.org/en/galactic/Tutorials/Tf2/Writing-A-Tf2-Static-Broadcaster-Cpp.html)

Pacote utilizado no tutorial
```sh
ros2 pkg create --build-type ament_cmake learning_tf2_cpp
```

Arquivos criados no tutorial:
src/static_turtle_tf2_broadcaster.cpp



[3. Writing a tf2 broadcaster]()
- [Python](http://docs.ros.org/en/galactic/Tutorials/Tf2/Writing-A-Tf2-Broadcaster-Py.html)  

"Nos próximos dois tutoriais, escreveremos o código para reproduzir a demonstração do tutorial Introdução ao tf2. Depois disso, os tutoriais a seguir se concentram em estender a demonstração com recursos tf2 mais avançados, incluindo o uso de tempos limite em pesquisas de transformação e viagens no tempo."  


Arquivos criados no tutorial:
learning_tf2_py/learning_tf2_py/turtle_tf2_broadcaster.py
learning_tf2_py/launch/turtle_tf2_demo.launch.py

Update:
setup.py
package.xml

Rodar tutorial
Terminal 1
```sh
ros2 launch learning_tf2_py turtle_tf2_demo.launch.py
``` 

Terminal 2
```
ros2 run turtlesim turtle_teleop_key
```   
Terminal 3
```
ros2 run tf2_ros tf2_echo world turtle1
```

Resposta:
```sh
At time 1651597783.967794171
- Translation: [6.971, 6.643, 0.000]
- Rotation: in Quaternion [0.000, 0.000, -0.774, 0.633]
```
- [C++](http://docs.ros.org/en/galactic/Tutorials/Tf2/Writing-A-Tf2-Broadcaster-Cpp.html)
    
Arquivos criados e ou editados no tutorial:
learning_f2_cpp/src/turtle_tf2_broadcaster.cpp
learning_f2_cpp/launch/turtle_tf2_demo.launch.py
CMaKeList.txt
package.xml

Execurar o tutorial

Terminal 1
```sh
ros2 launch learning_tf2_cpp turtle_tf2_demo.launch.py
```

Terminal 2
```sh
ros2 run turtlesim turtle_teleop_key
```
Terminal 3
```sh
ros2 run tf2_ros tf2_echo world turtle1
```

Resposta:
```sh
[INFO] [1652213515.939368866] [tf2_echo]: Waiting for transform world ->  turtle1: Invalid frame ID "world" passed to canTransform argument target_frame - frame does not exist
At time 1652213516.929271505
- Translation: [5.544, 5.544, 0.000]
- Rotation: in Quaternion [0.000, 0.000, 0.000, 1.000]
At time 1652213517.937710474
- Translation: [5.544, 5.544, 0.000]
- Rotation: in Quaternion [0.000, 0.000, 0.000, 1.000]
At time 1652213518.929866449
- Translation: [5.544, 5.544, 0.000]
- Rotation: in Quaternion [0.000, 0.000, 0.000, 1.000]

```


[4. Writing a tf2 listener.](docs.ros.org/en/galactic/Tutorials/Tf2/Writing-A-Tf2-Broadcaster-Py.html)
- [Python](http://docs.ros.org/en/galactic/Tutorials/Tf2/Writing-A-Tf2-Broadcaster-Py.html) 

Arquivos criados no tutorial:
learning_tf2_py/learning_tf2_py/turtle_tf2_listener.py

Update:
setup.py
package.xml

Adicionar a linha a seguir no arquivo setup.py:
```py
'console_scripts': [
...
'turtle_tf2_listener = learning_tf2_py.turtle_tf2_listener:main',
...
],
```

Terminal 1
```sh
ros2 launch learning_tf2_py turtle_tf2_demo.launch.py
```
Terminal 2
```
ros2 run turtlesim turtle_teleop_key
```

- [C++](http://docs.ros.org/en/galactic/Tutorials/Tf2/Writing-A-Tf2-Listener-Cpp.html)

Arquivos criados no tutorial:
learning_tf2_cpp/src/turtle_tf2_listener.cpp
learning_tf2_cpp/launch/turtle_tf2_demo.launch.py # Editado


```sh
ros2 launch learning_tf2_cpp turtle_tf2_demo.launch.py

```

```sh
ros2 run turtlesim turtle_teleop_key
```



[5. Adding a frame](http://docs.ros.org/en/galactic/Tutorials/Tf2/Adding-A-Frame-Py.html)  
- Python  

Arquivos criados no tutorial:
learning_tf2_py/learning_tf2_py/fixed_frame_tf2_broadcaster.py
learning_tf2_py/launch/turtle_tf2_dynamic_frame_demo.launch.py

Formatos para envio de argumentos 

1 - Via terminal: 
```sh
ros2 launch learning_tf2_py turtle_tf2_fixed_frame_demo.launch.py **target_frame:=carrot1**
```

2 - Via launch file:
```py
def generate_launch_description():
   demo_nodes = IncludeLaunchDescription(
      ...,
      launch_arguments={'target_frame': 'carrot1'}.items(),
      )
```

- [C++](http://docs.ros.org/en/galactic/Tutorials/Tf2/Adding-A-Frame-Cpp.html)

Arquivos criados no tutorial:
learning_tf2_cpp/learning_tf2_py/fixed_frame_tf2_broadcaster.cpp
learning_tf2_py/launch/turtle_tf2_fixed_frame_demo.launch.py
learning_tf2_cpp/src/dynamic_frame_tf2_broadcaster.cpp
learning_tf2_py/launch/turtle_tf2_dynamic_frame_demo.launch.py




Exemplo 1
Terminal 1
```sh
ros2 launch learning_tf2_cpp turtle_tf2_fixed_frame_demo.launch.py target_frame:=carrot1
```

Terminal 2
```sh
ros2 run turtlesim turtle_teleop_key
```

Exemplo 2
Terminal unico
```
ros2 launch learning_tf2_cpp turtle_tf2_dynamic_frame_demo.launch.py
``` 






[6. Learning about tf2 and time]()

- [Python](http://docs.ros.org/en/galactic/Tutorials/Tf2/Learning-About-Tf2-And-Time-Py.html)  

"Nos tutoriais anteriores, recriamos a demonstração da tartaruga escrevendo um transmissor tf2 e um ouvinte tf2. Também aprendemos como adicionar um novo quadro à árvore de transformação. Agora vamos aprender mais sobre o argumento timeout que faz o lookup_transform esperar pela transformação especificada até a duração especificada antes de lançar uma exceção. Essa ferramenta pode ser útil para ouvir transformações publicadas em taxas variadas ou fontes de entrada com rede não confiável e latência não desprezível. Este tutorial ensinará como usar o timeout na função lookup_transform para esperar que uma transformação esteja disponível na árvore tf2"

Arquivos editados no tutorial:
src/learning_tf2_py/learning_tf2_py/turtle_tf2_listener.py

Para correto funcionamento foi nescessario adicionar a linha 
```python
from rclpy.duration import Duration
```

Executando o tutorial
Terminal 1
```
ros2 launch learning_tf2_py turtle_tf2_demo.launch.py
```

Terminal 2
```sh
ros2 run turtlesim turtle_teleop_key
```

A tartatura 2 tera a posição da tartaruga 1 um segundo atrás como referencia de posicionamentos. 

- [C++]()

[7. Time travel with tf2.]()
- [Python](http://docs.ros.org/en/galactic/Tutorials/Tf2/Time-Travel-With-Tf2-Py.html)  

"No tutorial anterior, discutimos os conceitos básicos de tf2 e tempo. Este tutorial nos levará um passo adiante e exporá um poderoso truque do tf2: a viagem no tempo. Em suma, uma das principais características da biblioteca tf2 é que ela é capaz de transformar dados no tempo e no espaço.

Esse recurso de viagem no tempo do tf2 pode ser útil para várias tarefas, como monitorar a pose do robô por um longo período de tempo ou construir um robô seguidor que seguirá os “passos” do líder. Usaremos esse recurso de viagem no tempo para procurar transformações de volta no tempo e programar turtle2 para seguir 5 segundos atrás de cenoura1."

- C++

[Debugging tf2]()
- [ ] Quaternion fundamentals.
- [ ] Debugging tf2 problems.

[Using sensor messages with tf2]()

[URDF Tutorials]()  
