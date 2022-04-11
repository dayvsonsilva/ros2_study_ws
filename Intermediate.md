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

[Monitoring for parameter changes (C++)]()  

[Launch Tutorials]()  

[tf2 Tutorials]()  

[URDF Tutorials]()  