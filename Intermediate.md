# Anotações: Intermediate


[Creating an action](http://docs.ros.org/en/galactic/Tutorials/Actions/Creating-an-Action.html)  

"Você aprendeu sobre ações anteriormente no tutorial Entendendo as ações do ROS 2. Assim como os outros tipos de comunicação e suas respectivas interfaces (tópicos/msg e serviços/srv), você também pode definir ações personalizadas em seus pacotes. Este tutorial mostra como definir e construir uma ação que você pode usar com o servidor de ação e o cliente de ação que você escreverá no próximo tutorial."


o principal arquivo deste pacote é o Fibonacci.action.
```
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
```
ros2 pkg create --dependencies action_tutorials_interfaces rclcpp rclcpp_action rclcpp_components -- action_tutorials_cpp
``` 

Arquivos criados/editados:
action_tutorials_cpp/CMakeLists.txt
action_tutorials_cpp/package.xml
action_tutorials_cpp/include/action_tutorials_cpp/visibility_control.h
action_tutorials_cpp/src/fibonacci_action_server.cpp
action_tutorials_cpp/src/fibonacci_action_client.cpp


Execute os comados abaixo em terminais distintos:
``` shell
ros2 run action_tutorials_cpp fibonacci_action_server
```
``` shell
ros2 run action_tutorials_cpp fibonacci_action_client
```



[Writing an action server and client (Python)](http://docs.ros.org/en/galactic/Tutorials/Actions/Writing-a-Py-Action-Server-Client.html)  

"As ações são uma forma de comunicação assíncrona no ROS 2. Clientes de ação enviam solicitações de meta para servidores de ação. Os servidores de ação enviam feedback de metas e resultados para clientes de ação."

Criando o pacote
```
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

```
ros2 component types
```
Retorna:
```
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
```
mkdir -p ~/ros2_study_ws/src
cd ~/ros2_study_ws
```

Após criar o diretorio, os pacotes de trabalho devem ser adicionados ao diretorio src:
obs: clonar repo git clone https://github.com/ros2/examples src/examples -b galactic
```
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

```
.
├── build
├── install
├── log
└── src

4 directories, 0 files
```

Ainda é possivel realizar o teste do ambiente.
```
colcon test
```

[Monitoring for parameter changes (C++)](http://docs.ros.org/en/galactic/Tutorials/Monitoring-For-Parameter-Changes-CPP.html)  

"Muitas vezes, um nó precisa responder a alterações em seus próprios parâmetros ou nos parâmetros de outro nó. A classe ParameterEventHandler facilita a escuta de alterações de parâmetro para que seu código possa responder a elas. Este tutorial mostrará como usar a versão C++ da classe ParameterEventHandler para monitorar alterações nos parâmetros de um nó, bem como alterações nos parâmetros de outro nó."


```
ros2 pkg create --build-type ament_cmake cpp_parameter_event_handler --dependencies rclcpp
```
Arquivos criados/editados:

package.xml
parameter_event_handler1.cpp
parameter_event_handler.cpp 

Primeira parte do tutorial
Terminal 1
```
ros2 run cpp_parameter_event_handler parameter_event_handler
```
Terminal 2
```
ros2 param set node_with_parameters an_int_param 43
```
retorno 
```
[INFO] [1606950498.422461764] [node_with_parameters]: cb: Received an update to parameter "an_int_param" of type integer: "43"
```  

Segunda parte
Terminal 1
```
ros2 run cpp_parameter_event_handler parameter_event_handler
```
Terminal 2
```
ros2 run demo_nodes_cpp parameter_blackboard
```
Terminal 3
```
ros2 param set parameter_blackboard a_double_param 3.45
```

Retorno
```
[INFO] [1606952588.237531933] [node_with_parameters]: cb2: Received an update to parameter "a_double_param" of type: double: "3.45"
```

[Launch Tutorials](http://docs.ros.org/en/galactic/Tutorials/Launch/Launch-Main.html)  

[Creating a ROS 2 Launch File.](http://docs.ros.org/en/galactic/Tutorials/Launch/Creating-Launch-Files.html) 

Criando um arquivo de launch para ros2

Arquivo criado no turorial
src/ros_tutorials/launch/turtlesim_mimic_launch.py


```
cd launch
ros2 launch turtlesim_mimic_launch.py
```

O comando ira abrir duas instancias do turtlesim.

No segundo terminal executar o comando abaixo:
```
ros2 topic pub -r 1 /turtlesim1/turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: -1.8}}"
```

Para visualizar a interligação entre nós basta rodar o comando:
```
rqt_graph
```  


[Launching and Monitor Multiple Nodes with Launch.](http://docs.ros.org/en/galactic/Tutorials/Launch/Launch-system.html) 

"O sistema de lançamento no ROS 2 é responsável por ajudar o usuário a descrever a configuração de seu sistema e então executá-lo conforme descrito. A configuração do sistema inclui quais programas executar, onde executá-los, quais argumentos transmiti-los e convenções específicas do ROS que facilitam a reutilização de componentes em todo o sistema, fornecendo a cada um configurações diferentes. Também é responsável por monitorar o estado dos processos iniciados, reportar e/ou reagir a mudanças no estado desses processos."  

Para este turorial foi utilizado o pacote my_package_py.

Arquivos criados/editados:  
setup.py
launch/my_script.launch.py


```
ros2 launch my_package_py my_script.launch.py
```

[Using substitutions in launch files](http://docs.ros.org/en/galactic/Tutorials/Launch/Using-Substitutions.html) 

"Os arquivos de inicialização são usados para iniciar nós, serviços e executar processos. Esse conjunto de ações pode ter argumentos, que afetam seu comportamento. As substituições podem ser usadas em argumentos para fornecer mais flexibilidade ao descrever arquivos de inicialização reutilizáveis. As substituições são variáveis que são avaliadas apenas durante a execução da descrição de execução e podem ser usadas para adquirir informações específicas como uma configuração de execução, uma variável de ambiente ou para avaliar uma expressão arbitrária do Python."

 Para o Tutorial foram criados dois arquivos:

 launch/example_main.launch.py
 launch/example_substitutions.launch.py

[Using Event Handlers.]() 
[Using ROS 2 Launch For Large Projects.]() 









[tf2 Tutorials]()  

[URDF Tutorials]()  
