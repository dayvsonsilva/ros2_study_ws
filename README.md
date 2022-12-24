<!-- cSpell:enable -->
# ros2_study(Em construção)

## Introdução
<div style="text-align: justify"> 
<!-- De acordo com a própria documentação, o Robot Operating System(ROS) é um conjunto de bibliotecas e ferramentas para construir aplicações com robôs. Desde drives até algoritmos no estado da arte, e possui poderosas ferramentas de desenvolvimento, o ROS tem o que é preciso para projetos de robótica. E é tudo código aberto. -->
 
Repositório com registros e códigos de tutoriais realizados seguindo a documentação do ROS2 humble. Durante os estudos foi utilizado o SO Ubuntu 20.04. 
 
O objetivo deste repositório é registrar informações importantes e ou códigos em formato original ou modificados, sendo exclusivamente para uso próprio e didático, não sendo recomendado a utilização de seus códigos e informações para aplicações que possam vir a gerar prejuízos de qualquer natureza.
</div>


## Requisitos
<div style="text-align: justify"> 
Para melhor aproveitamento dos tutoriais é importante que se tenha conhecimento em Linux, C/C++ e Python, porém não ter conhecimento prévio nestes assuntos não faz com que seja impossível aprender sobre ROS. Inclusive o estudo da Robótica traz oportunidades para que com a orientação correta você possa estudar desde a eletrônica da leitura de sensores e acionamento de atuadores até a utilização de algoritmos de aprendizado de máquinas aplicados à robótica. 
</div>



## Preparação do Ambiente
 
### Instalação do ROS2 humble via pacotes Debian
<div style="text-align: justify">  

A documentação original aborda formatos diferentes de instalação, incluindo instalação em vários sistemas operacionais. Para os estudos é sugerido utilizar a instalação via pacotes debian pela facilidade, visto que no primeiro momento o foco é o estudos dos pacotes ROS2 disponíveis para diversas aplicações, em casos onde o ROS2 será utilizado em uma versão do Linux que não tenha suporte nativo será necessário revisitar a página oficial da documentação do [ROS2 Galatic Installation](http://docs.ros.org/en/humble/Installation.html#installation).

O bloco de codigo abaixo tras um resumo com os comando para instalação do ROS 2 humble, para entender o passo-a-passo da instalação é sugerido que seja realizado uma visita a pagina [Installing ROS 2 via Debian Packages](http://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html#installing-ros-2-via-debian-packages) 

</div>  

```shell  
# Set locale
locale  # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings

# Setup Sources

apt-cache policy | grep universe
 500 http://us.archive.ubuntu.com/ubuntu focal/universe amd64 Packages
     release v=20.04,o=Ubuntu,a=focal,n=focal,l=Ubuntu,c=universe,b=amd64

sudo apt install software-properties-common
sudo add-apt-repository universe

sudo apt update && sudo apt install curl gnupg lsb-release
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

```

Instalando os Pacotes do ROS2  
```shell
sudo apt update
sudo apt install ros-humble-desktop
```

Após a instalação é recomendado realizar o teste do ROS2 através da execução dos comandos abaixo.

```shell
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_cpp talker
```

```shell
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_py listener
```


## Tutoriais 
A partir da instalação do ROS2 é possível iniciar a execução dos tutoriais.

OBS: Para a execução dos tutoriais criar uma pasta/pacote para cada tutorial e registrar informações importantes em arquivos Markdown.


### Beginner

[Beginner: CLI Tools](http://docs.ros.org/en/humble/Tutorials.html#beginner-cli-tools) <!--10-->

- [x] Configuring your ROS 2 environment  
- [x] Introducing turtlesim and rqt  
- [x] Understanding ROS 2 nodes  
- [X] Understanding ROS 2 topics  
- [x] Understanding ROS 2 services  
- [x] Understanding ROS 2 parameters  
- [x] Understanding ROS 2 actions  
- [x] Using rqt_console  
- [x] Introducing ROS 2 launch  
- [x] Recording and playing back data  


[Beginner: Client Libraries](http://docs.ros.org/en/humble/Tutorials.html#beginner-client-libraries) <!--16-->

- [x] Creating a workspace  
- [x] Creating your first ROS 2 package  
  - [x] CMake
  - [x] Python 
- [x] Writing a simple publisher and subscriber (C++)  
- [x] Writing a simple publisher and subscriber (Python)  
- [x] Writing a simple service and client (C++)  
- [x] Writing a simple service and client (Python)  
- [x] Creating custom ROS 2 msg and srv files  
  - [x] C++
  - [x] Python 
- [x] Expanding on ROS 2 interfaces  
- [x] Using parameters in a class (C++)  
- [x] Using parameters in a class (Python)  
- [x] Getting started with ros2doctor  
- [x] Creating and Using Plugins (C++) 


### Intermediate
[Intermediate](http://docs.ros.org/en/humble/Tutorials.html#intermediate) <!--32-->

- [x] Creating an action  
- [x] Writing an action server and client (C++)  
- [x] Writing an action server and client (Python)  
- [ ] Composing multiple nodes in a single process  **REFAZER**
- [x] Using colcon to build packages  
- [x] Monitoring for parameter changes (C++)  
- [x] [Launch Tutorials](http://docs.ros.org/en/humble/Tutorials/Launch/Launch-Main.html)  
  - [x] Creating a ROS 2 Launch File.
  - [x] Launching and Monitor Multiple Nodes with Launch.
  - [x] Using Substitutions.
  - [x] Using Event Handlers.
  - [x] Using ROS 2 Launch For Large Projects.
- [ ] [tf2 Tutorials](http://docs.ros.org/en/humble/Tutorials/Tf2/Tf2-Main.html)
  - [x] Learning tf2   
    - [x] Introduction to tf2.
    - [x] Writing a tf2 static broadcaster 
      - [x] (Python) 
      - [x] (C++)
    - [x] Writing a tf2 broadcaster
      - [x] (Python) 
      - [x] (C++)
    - [x] Writing a tf2 listener.
      - [x] (Python) 
      - [x] (C++)
    - [x] Adding a frame  
      - [x] (Python) 
      - [x] (C++)
    - [x] Learning about tf2 and time
      - [x] (Python)
      - [x] (C++) 
    - [x] Time travel with tf2.
      - [x] (Python) 
      - [x] (C++)
  - [ ] Debugging tf2
    - [x] Quaternion fundamentals.
    - [ ] Debugging tf2 problems.**REFAZER**
  - [ ] Using sensor messages with tf2 **REFAZER**
- [ ] [URDF Tutorials](http://docs.ros.org/en/humble/Tutorials/URDF/URDF-Main.html)
  - [ ] Building a Visual Robot Model with URDF from Scratch
  - [ ] Building a Movable Robot Model with URDF
  - [ ] Adding Physical and Collision Properties to a URDF Model
  - [ ] Using Xacro to Clean Up a URDF File
  - [ ] Using URDF with robot_state_publisher

### Advanced
[Advanced](http://docs.ros.org/en/humble/Tutorials.html#intermediate) <!--6-->

- [ ] ROS 2 Topic Statistics Tutorial (C++)  
- [ ] Using Fast DDS Discovery Server as discovery protocol [community-contributed]  
- [ ] Implement a custom memory allocator  
- [ ] Unlock all the potential of Fast DDS as ROS 2 middleware [community-contributed]  
- [ ] Recording a bag from your own node (C++)  
- [ ] Recording a bag from your own node (Python)  

### Simulation
[Simulation](http://docs.ros.org/en/humble/Tutorials.html#intermediate)<!--1-->

- [ ] Simulation Tutorials  

### Miscellaneous
[Miscellaneous](http://docs.ros.org/en/humble/Tutorials.html#miscellaneous) <!--4-->

- [ ] ROS2 on IBM Cloud Kubernetes [community-contributed]  
- [ ] Eclipse Oxygen with ROS 2 and rviz2 [community-contributed]  
- [ ] Building realtime Linux for ROS 2 [community-contributed]  
- [ ] Building ROS 2 Package with eclipse 2021-06  


### Demos<!--15-->
[Demos](http://docs.ros.org/en/humble/Tutorials.html#demos)

- [ ] Use quality-of-service settings to handle lossy networks.  
- [ ] Management of nodes with managed lifecycles.  
- [ ] Efficient intra-process communication.  
- [ ] Bridge communication between ROS 1 and ROS 2.  
- [ ] Recording and playback of topic data with rosbag using the ROS 1 bridge.  
- [ ] Turtlebot 2 demo using ROS 2.  
- [ ] TurtleBot 3 demo using ROS 2. [community-contributed]  
- [ ] Simulate the TurtleBot 3 on ROS [community-contributed].  
- [ ] Navigate TurtleBot 3 in simulation. [community-contributed]  
- [ ] SLAM with TurtleBot3 in simulation. [community-contributed]  
- [ ] MoveIt 2 arm motion planning demo.  
- [ ] Write real-time safe code that uses the ROS 2 APIs.  
- [ ] Use the robot state publisher to publish joint states and TF.  
- [ ] Use DDS-Security.  
- [ ] Logging and logger configuration.  

### Examples <!--1-->
[Examples](http://docs.ros.org/en/humble/Tutorials.html#examples) 

- [ ] Python and C++ minimal examples.  


## Referências

- [Documentação ROS Galatic](http://docs.ros.org/en/humble/index.html)
- [A Sutileza dos Quatérnions no Movimento de Rotação de Corpos Rígidos](https://www.scielo.br/j/rbef/a/XvBQPp9F5SsWwQCTGX9pk5C/abstract/?lang=pt)
- [O que são números Quaternions?](https://www.youtube.com/watch?v=8WeatzzMT_A)
- [Dinâmica de Sistemas Mecânicos, Professor Samuel da Silva](https://www.youtube.com/watch?v=x4GhXt7yvks&list=PLHtj55JNGdtmqJUZud-Vsdc68Q4Ez21By)
