# Anotações - Beginner: Client Libraries

- [Creating a workspace](http://docs.ros.org/en/galactic/Tutorials/Workspace/Creating-A-Workspace.html)
  
<div style="text-align: justify"> 
"Um espaço de trabalho é um diretório que contém pacotes do ROS 2. Antes de usar o ROS 2, é necessário fornecer seu espaço de trabalho de instalação do ROS 2 no terminal em que você planeja trabalhar. Isso torna os pacotes do ROS 2 disponíveis para você usar nesse terminal.

Você também tem a opção de fornecer um “overlay” – um espaço de trabalho secundário onde você pode adicionar novos pacotes sem interferir no espaço de trabalho ROS 2 existente que você está estendendo, ou “underlay”. Seu underlay deve conter as dependências de todos os pacotes em seu overlay. Os pacotes em sua sobreposição substituirão os pacotes na subjacência. Também é possível ter várias camadas de subjacências e sobreposições, com cada sobreposição sucessiva usando os pacotes de suas subjacências pai. "
</div>

Para cada novo terminal aberto deve ser definido o ambiente de execução
``` shell
source /opt/ros/galactic/setup.bash
```

``` shell
mkdir -p ~/dev_ws/src
cd ~/dev_ws/src
```
<div style="text-align: justify"> 
Para o repositorio atual o ambiente de trabalho é chamado de ros2_study_ws, os pacotes utilizados no ambiente de trabalho devem estar no diretorio /src.  
</div>
Como referencia será utilizado o repositorio ros_tutorials.

``` shell
cd ros2_study_ws/src
git clone https://github.com/ros/ros_tutorials.git -b galactic-devel
```

Instalando depedencias
``` shell
# cd if you're still in the ``src`` directory with the ``ros_tutorials`` clone
cd ..
rosdep install -i --from-path src --rosdistro galactic -y
```
<div style="text-align: justify"> 
"Os pacotes declaram suas dependências no arquivo package.xml (você aprenderá mais sobre pacotes no próximo tutorial). Este comando percorre essas declarações e instala as que estão faltando. Você pode aprender mais sobre o rosdep em outro tutorial (em breve)."
</div>

Build workspace
``` shell
source /opt/ros/galactic/setup.bash
cd ros2_study_ws
colcon build
```

Outros argumentos úteis para a compilação do colcon:

--packages-up-to compila o pacote que você deseja, além de todas as suas dependências, mas não todo o espaço de trabalho (economiza tempo)

--symlink-install evita que você tenha que reconstruir toda vez que ajustar scripts python

--event-handlers console_direct+ mostra a saída do console durante a compilação (pode ser encontrado no diretório de log) 



Source o ambiente de rabalho(overlay)
``` shell
source /opt/ros/galactic/setup.bash
cd ~/ros2_study_ws
. install/local_setup.bash
```

Agora ao rodar o pacote turtlesim, será utilizado o pacote do ambiente ros2_study_ws(overlay)
``` shell
ros2 run turtlesim turtlesim_node
```

PESQUISAR DIFERENÇA ENTRE:
``` shell
cd ~/ros2_study_ws
. install/local_setup.bash
```
E
``` shell
cd ~/ros2_study_ws
source install/setup.bash
```

- [Creating your first ROS 2 package](http://docs.ros.org/en/galactic/Tutorials/Creating-Your-First-ROS2-Package.html)

<div style="text-align: justify"> 
"1 O que é um pacote ROS 2?
Um pacote pode ser considerado um contêiner para seu código ROS 2. Se você quiser instalar seu código ou compartilhá-lo com outras pessoas, precisará organizá-lo em um pacote. Com os pacotes, você pode liberar seu trabalho do ROS 2 e permitir que outros o construam e usem facilmente.

A criação de pacotes no ROS 2 usa ament como seu sistema de compilação e colcon como sua ferramenta de compilação. Você pode criar um pacote usando CMake ou Python, que são oficialmente suportados, embora existam outros tipos de compilação. "
</div>

Tutorial abordando uso do CMake

Estrutura de um pacote ROS2
my_package/
     CMakeLists.txt
     package.xml

Onde:

package.xml - arquivo contendo meta-informações sobre o pacote

CMakeLists.txt - arquivo que descreve como construir o código dentro do pacote 

NÂO UTILIZAR PACOTES ANINHADOS!!!!

my_package_1/
     CMakeLists.txt
     package.xml
     my_package_2/
        CMakeLists.txt
        package.xml


Padrão de organização de pacotes

workspace_folder/
    src/
      package_1/
          CMakeLists.txt
          package.xml

      package_2/
          setup.py
          package.xml
          resource/package_2
      ...
      package_n/
          CMakeLists.txt
          package.xml

Criando um pacote CMake
``` shell
ros2 pkg creat -- build-type ament_cmake <package_name>
```

Utilizando o parametro  --node-name para gerar um executavel de teste
``` shell
ros2 pkg create --build-type ament_cmake --node-name my_node my_package
```

Após criar pacote deve ser realizado a compilação
```
colcon bulid
```

Executando o arquivo hello_world gerado com o parametro --node-name
``` shell
ros2 run my_package my_node
```
Retorno:
``` shell
CMakeLists.txt  include  package.xml  src
```


Tutorial abordando uso do Python

Estrutura d epacote ROS2 em Python
my_package/
      setup.py
      package.xml
      resource/my_package

package.xml - arquivo contendo meta-informações sobre o pacote.

setup.py - contendo instruções de como instalar o pacote.

setup.cfg -  é necessário quando um pacote tem executáveis, então ros2 run pode encontrá-los.

/<package_name> - um diretório com o mesmo nome do seu pacote, usado pelas ferramentas do ROS 2 para encontrar seu pacote, contém __init__.py.

Sintax do comando para criação de pacote utilizando Python
``` shell
ros2 pkg create --build-type ament_python <package_name>
```

``` shell
ros2 pkg create --build-type ament_python --node-name my_node my_package_python
```

Testando o pacote
``` shell
ros2 run my_package_python my_node
```


[Writing a simple publisher and subscriber (C++)](http://docs.ros.org/en/galactic/Tutorials/Writing-A-Simple-Cpp-Publisher-And-Subscriber.html)

Criando pacote ROS2 com cpp
``` shell
ros2 pkg create --build-type ament_cmake cpp_pubsub
```

O codigo utilizado nesta etapa pode ser baixado a patir do repositorio abaixo
```
wget -O publisher_member_function.cpp https://raw.githubusercontent.com/ros2/examples/master/rclcpp/topics/minimal_publisher/member_function.cpp
```

Para o tutorial foram criados os arquivos:

publisher_member_function.cpp
subscriber_member_function.cpp

Alem da criação dos nós, foram realizadas alterações nos arquivos:  

package.xml 
CMakeLists.txt  
 
<div style="text-align: justify"> 
As alterações do arquivo package.xml diz respeito a edição de informações basicas do pacote(Descrição, nome do mantenedor e tipo de licença), e tambem nele é adicionado as dependencias do pacote. 

</div>

``` xml
<depend>rclcpp</depend>
<depend>std_msgs</depend>
```

No arquivo CmakeLists.txt tambem deve ser adicionado as dependencias:
```
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
```

Alem das dependencias deve ser adicionado os executaveis do pacote:

add_executable(talker src/publisher_member_function.cpp)
ament_target_dependencies(talker rclcpp std_msgs)

add_executable(listener src/subscriber_member_function.cpp)
ament_target_dependencies(listener rclcpp std_msgs)

Para finalizar deve ser adicionado ao arquivo a tag install, para que o comando ros2 run possa encontrar os arquivos.
```
install(TARGETS
  talker
  listener
  DESTINATION lib/${PROJECT_NAME})
```

Após a criação e edição dos arquivos realise a instalação das dependencias.
```
rosdep install -i --from-path src --rosdistro galactic -y
```
Compile o pacote
```
colcon build --packages-select cpp_pubsub
```

Rode em terminais separados os dois executaveis


```
ros2 run cpp_pubsub talker

```
Retorno:
```
[INFO] [minimal_publisher]: Publishing: "Hello World: 0"
[INFO] [minimal_publisher]: Publishing: "Hello World: 1"
[INFO] [minimal_publisher]: Publishing: "Hello World: 2"
[INFO] [minimal_publisher]: Publishing: "Hello World: 3"
[INFO] [minimal_publisher]: Publishing: "Hello World: 4"
```

```
ros2 run cpp_pubsub listener

```

Retorno:
```
[INFO] [minimal_subscriber]: I heard: "Hello World: 10"
[INFO] [minimal_subscriber]: I heard: "Hello World: 11"
[INFO] [minimal_subscriber]: I heard: "Hello World: 12"
[INFO] [minimal_subscriber]: I heard: "Hello World: 13"
[INFO] [minimal_subscriber]: I heard: "Hello World: 14"
```


[Writing a simple publisher and subscriber (Python)](http://docs.ros.org/en/galactic/Tutorials/Writing-A-Simple-Py-Publisher-And-Subscriber.html)


Criando pacote ROS2 com Python
```
ros2 pkg create --build-type ament_python py_pubsub
```

Para o tutorial foram criados os arquivos:

publisher_member_function.py
subscriber_member_function.py

Alem da criação dos nós, foram realizadas alterações nos arquivos:  

setup.py
setup.cfg 
package.xml

As alterações do arquivo package.xml diz respeito a edição de informações basicas do pacote(Descrição, nome do mantenedor e tipo de licença), e tambem nele é adicionado as dependencias do pacote. 

``` xml
<exec_depend>rclpy</exec_depend>
<exec_depend>std_msgs</exec_depend>
```

Abra o arquivo setup.py. Novamente, combine os campos maintainer, maintainer_email, description e license identico ao package.xml: 

```
maintainer='YourName',
maintainer_email='you@email.com',
description='Examples of minimal publisher/subscriber using rclpy',
license='Apache License 2.0',
```

Alem disso deve ser adidionado as seguintes linhas
``` python
entry_points={
        'console_scripts': [
                'talker = py_pubsub.publisher_member_function:main',
                'listener = py_pubsub.subscriber_member_function:main',
        ],
},
```
[Writing a simple service and client (C++)](http://docs.ros.org/en/galactic/Tutorials/Writing-A-Simple-Cpp-Service-And-Client.html)


<div style="text-align: justify"> >
Quando os nós se comunicam usando serviços, o nó que envia uma solicitação de dados é chamado de nó cliente e aquele que responde à solicitação é o nó de serviço. A estrutura da solicitação e resposta é determinada por um arquivo .srv.
</div>

No tutorial foram criados 2 arquivos:

add_two_ints_server.cpp
add_two_ints_client.cpp

Para rodar o exemplo rode os dois comandos abaixo em terminais diferentes
```
ros2 run cpp_srvcli server
```
Retorno:
```
[INFO] [rclcpp]: Ready to add two ints.
```

```
ros2 run cpp_srvcli client 2 3

```
Retorno:
```
[INFO] [rclcpp]: Sum: 5
```


[Writing a simple service and client (Python)](http://docs.ros.org/en/galactic/Tutorials/Writing-A-Simple-Py-Service-And-Client.html) 


Criando um pacote
```
ros2 pkg create --build-type ament_python py_srvcli --dependencies rclpy example_interfaces
```

Após a criação do pacote atualizar os arquivos package.xml e setup.py.

Para o service e o client foram criados os arquivos:  

service_member_function.py
client_member_function.py


Apos a criação dos arquivos basta rodar os dois comando abaixo em terminais diferentes

```
ros2 run py_srvcli service
```

```
ros2 run py_srvcli client 2 3
```

O cliente passa os dados "2 e 3" para o server que retorna o resultado da soma "5"


VERIFICAR SE É POSSIVEL UTILIZAR UM SRV DE UM PACOTE PYTHON AMENT_PYTHON.


<!--  -->
Proximos  

[Writing a simple service and client (C++)]()  
[Writing a simple service and client (Python)]()  
[Creating custom ROS 2 msg and srv files]()  
  - C++
  - Python 
[Expanding on ROS 2 interfaces]()  
[Using parameters in a class (C++)]()  
[Using parameters in a class (Python)]()  
[Getting started with ros2doctor]()  
[Creating and Using Plugins (C++)]()
